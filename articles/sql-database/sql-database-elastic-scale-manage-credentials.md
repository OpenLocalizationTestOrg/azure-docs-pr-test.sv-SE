---
title: "aaaManaging autentiseringsuppgifter i hello klientbibliotek för elastisk databas | Microsoft Docs"
description: "Hur tooset hello rätt nivå av autentiseringsuppgifter, admin tooread endast för appar för elastisk databas"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 72e0edaf-795e-4856-84a5-6594f735fb7e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 218783ca2a07e3c0a4b089aa92634f32c41386e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="credentials-used-tooaccess-hello-elastic-database-client-library"></a>Autentiseringsuppgifterna används tooaccess hello-klientbibliotek för elastisk databas
Hej [klientbibliotek för elastisk databas](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) använder tre olika typer av autentiseringsuppgifter tooaccess hello [Fragmentera kartan manager](sql-database-elastic-scale-shard-map-management.md). Beroende på hello behovet att använda hello credential med hello lägsta nivå av möjliga åtkomst.

* **Hantering av autentiseringsuppgifter**: för att skapa eller ändra en Fragmentera kartan manager. (Se hello [ordlista](sql-database-elastic-scale-glossary.md).) 
* **Åtkomst till autentiseringsuppgifterna för**: tooaccess en befintlig Fragmentera mappa manager tooobtain information om hur du delar.
* **Autentiseringsuppgifter för anslutning**: tooconnect tooshards. 

Se även [hantera databaser och inloggningar i Azure SQL Database](sql-database-manage-logins.md). 

## <a name="about-management-credentials"></a>Om hantering av autentiseringsuppgifter
Hantering av autentiseringsuppgifter är används toocreate en [ **ShardMapManager** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) objekt för program som hanterar Fragmentera maps. (Till exempel se [att lägga till en Fragmentera med elastiska Databasverktyg](sql-database-elastic-scale-add-a-shard.md) och [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md)) hello användare av klientbiblioteket för hello elastisk skalbarhet skapar hello SQL-användare och SQL-inloggningar och ser till att varje är hello Skriv-och läsbehörighet på hello globala Fragmentera mappa beviljas databasen och alla Fragmentera databaser samt. Autentiseringsuppgifterna är används toomaintain hello globala Fragmentera kartan och hello lokala Fragmentera maps när ändringar toohello Fragmentera mappningen ska utföras. Till exempel använda hello autentiseringsuppgifter toocreate hello Fragmentera kartan manager hanteringsobjektet (med hjälp av [ **GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

hello variabeln **smmAdminConnectionString** är en anslutningssträng som innehåller autentiseringsuppgifter för hantering av hello. ger läsning och skrivning åtkomst tooboth Fragmentera kartan databasen och enskilda delar hello användar-ID och lösenord. hello management-anslutningssträngen innehåller också hello namnet och databasen namn tooidentify hello globala Fragmentera kartan serverdatabasen. Här är en typisk anslutningssträng för detta ändamål:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Använd inte värden i hello form av ”username@server”, i stället använda hello ”användarnamn” värde.  Det beror på att autentiseringsuppgifter måste arbeta mot både hello Fragmentera kartan manager-databasen och enskilda delar som kan vara på olika servrar.

## <a name="access-credentials"></a>Autentiseringsuppgifter
När du skapar en Fragmentera kartan manager i ett program som inte administrerar Fragmentera maps Använd autentiseringsuppgifter som har skrivskyddad behörighet på hello globala Fragmentera karta. hello information som hämtas från hello globala Fragmentera kartan under dessa autentiseringsuppgifter används för [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) och toopopulate hello Fragmentera mappa cache på hello-klienten. hello autentiseringsuppgifter tillhandahålls via hello samma anropa mönster för**GetSqlShardMapManager** som ovan: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Observera hello användning av hello **smmReadOnlyConnectionString** tooreflect hello använder olika autentiseringsuppgifter för åtkomst på uppdrag av **icke-administratörer** användare: autentiseringsuppgifterna bör inte ge skrivning behörigheter på hello globala Fragmentera karta. 

## <a name="connection-credentials"></a>Autentiseringsuppgifter för anslutning
Ytterligare autentiseringsuppgifter behövs när du använder hello [ **OpenConnectionForKey** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) metoden tooaccess en Fragmentera som är associerade med en nyckel för horisontell partitionering. Dessa autentiseringsuppgifter måste tooprovide behörigheter för skrivskyddad åtkomst toohello lokala Fragmentera kartan tabeller på hello Fragmentera. Det här är nödvändig tooperform anslutningsverifiering för data-beroende routning på hello Fragmentera. Det här kodstycket tillåter åtkomst till data i hello gäller data beroende routing: 

    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

I det här exemplet **smmUserConnectionString** innehåller hello anslutningssträngen för hello användarens autentiseringsuppgifter. Här är en typisk anslutningssträng för autentiseringsuppgifter för Azure SQL DB: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Precis som med hello administratörsautentiseringsuppgifter inte värden i hello form av ”username@server”. Använd ”användarnamn” istället bara.  Observera också att hello anslutningssträngen inte innehåller ett servernamn och databasnamn. Det beror på hello **OpenConnectionForKey** anropet kommer automatiskt att dirigera hello anslutning toohello rätt Fragmentera baserat på hello nyckel. Därför tillhandahålls inte hello databasens namn och namn på servern. 

## <a name="see-also"></a>Se även
[Hantera databaser och inloggningar i Azure SQL Database](sql-database-manage-logins.md)

[Säkra din SQL Database](sql-database-security-overview.md)

[Komma igång med jobb för elastisk databas](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

