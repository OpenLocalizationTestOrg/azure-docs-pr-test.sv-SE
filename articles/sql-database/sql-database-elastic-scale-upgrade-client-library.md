---
title: aaaUpgrade toohello senaste elastisk databas klientbiblioteket | Microsoft Docs
description: "Uppgradera appar och bibliotek med hjälp av Nuget"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 0a546510-76e7-465e-9271-f15ff0cfa959
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: cc2c9179be4c53ca59cd24d832127cf277c6e695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-app-toouse-hello-latest-elastic-database-client-library"></a>Uppgradera en app toouse hello senaste elastisk databas klientbibliotek
Nya versioner av hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md) är tillgängliga via NuGetand hello NuGetPackage Manager-gränssnittet i Visual Studio. Uppgraderingar innehåller felkorrigeringar och stöd för nya funktioner i hello klientbiblioteket.

**Senaste versionen av hello:** gå för[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Återskapar ditt program med hello nya bibliotek, samt ändra din befintliga Fragmentera kartan Manager metadata som lagras i din Azure SQL-databaser toosupport nya funktioner.

Utför stegen i ordning säkerställer att äldre versioner av klientbiblioteket hello inte längre finns i din miljö när metadataobjekt uppdateras, vilket innebär att gamla version metadataobjekt inte skapas efter uppgraderingen.   

## <a name="upgrade-steps"></a>Uppgradering
**1. Uppgradera dina program.** I Visual Studio, hämtning och referens hello senaste biblioteket klientversionen i alla dina utvecklingsprojekt som använder hello library; sedan återskapa och distribuera. 

* Välj i Visual Studio-lösning **verktyg** --> **NuGet Package Manager** -->  **hantera NuGet-paket för lösningen**. 
* (Visual Studio 2013) Markera hello vänstra panelen **uppdateringar**, och välj sedan hello **uppdatering** knappen hello paketet **Azure SQL Database-klientbiblioteket med elastisk skala** som visas i hello fönstret.
* (Visual Studio 2015) Ange hello filterfältet för**uppgradera tillgängliga**. Välj hello paketet tooupdate och klicka på hello **uppdatering** knappen.
* (Visual Studio 2017) Hello över hello dialogrutan Välj **uppdateringar**. Välj hello paketet tooupdate och klicka på hello **uppdatering** knappen.
* Skapa och distribuera. 

**2. Uppgradera ditt skript.** Om du använder **PowerShell** skript toomanage shards [hämta hello ny biblioteket version](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) och kopierar den till hello katalog där du kan köra skript. 

**3. Uppgradera din delade kopplingstjänsten.** Om du använder hello elastisk databas för delade sökvägssammanslagning tooreorganize shardade data, [hämta och distribuera hello senaste versionen av verktyget hello](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Detaljerad Uppgraderingsstegen för hello Service hittar [här](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. Uppgradera din Fragmentera kartan Manager-databaser**. Uppgradera hello metadata stöder Fragmentera-Maps i Azure SQL Database.  Det finns två sätt som du kan göra detta, med PowerShell eller C#. Båda alternativen visas nedan.

***Alternativ 1: Uppgradera metadata med hjälp av PowerShell***

1. Hämta hello senaste kommandoradsverktyg för NuGet från [här](http://nuget.org/nuget.exe) och spara tooa mapp. 
2. Öppna en kommandotolk, navigerar toohello samma mapp samt problemet hello kommando:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Navigera toohello undermapp som innehåller hello ny DLL klientversion du har precis har laddat ned, till exempel:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. Hämta hello elastisk databas klienten uppgradera skriptlet från hello [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), och spara det i hello samma mapp som innehåller hello DLL-filen.
5. Kör ”PowerShell.\upgrade.ps1” från hello kommandotolk och följ hello anvisningarna från den mappen.

***Alternativ 2: Uppgradera metadata med hjälp av C#***

Du kan också skapa ett Visual Studio-program som öppnar din ShardMapManager itererar över alla delar och utför hello metadata för uppgraderingen genom att anropa metoder hello [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) och [ UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) som i följande exempel: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Dessa tekniker för metadata uppgraderingar kan användas flera gånger utan att skada. Till exempel om en äldre klientversion skapar en Fragmentera oavsiktligt när du har uppdaterat, kan du köra uppgraderingen igen över alla delar tooensure som hello senaste metadata-versionen finns i hela infrastrukturen. 

**Obs:** nya versioner av klientbiblioteket hello publicerade hittills fortsätta toowork med tidigare versioner av hello Fragmentera kartan Manager metadata på Azure SQL DB och vice versa.   Tootake nytta av hello nya funktionerna i hello senaste klienten metadata måste dock toobe uppgraderas.   Observera att metadata uppgraderingar inte påverkar användardata och programspecifika data endast objekt som skapas och används av hello Fragmentera kartan Manager.  Och programmen toooperate via hello uppgraderingsordningen som beskrivs ovan. 

## <a name="elastic-database-client-version-history"></a>Versionshistorik för klienten för elastisk databas
Tidigare versioner, gå för[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

