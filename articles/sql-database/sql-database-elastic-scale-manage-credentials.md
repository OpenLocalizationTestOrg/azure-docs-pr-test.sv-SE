---
title: "Hantera autentiseringsuppgifter i klientbibliotek för elastisk databas | Microsoft Docs"
description: "Hur du anger rätt nivå av autentiseringsuppgifter, admin till skrivskyddat läge, för appar för elastisk databas"
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
ms.openlocfilehash: 46908be2846062a0520d21e06db3091a4d711b0b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="credentials-used-to-access-the-elastic-database-client-library"></a><span data-ttu-id="bcb1d-103">Autentiseringsuppgifter som används för att komma åt klientbibliotek för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="bcb1d-103">Credentials used to access the Elastic Database client library</span></span>
<span data-ttu-id="bcb1d-104">Den [klientbibliotek för elastisk databas](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) använder tre olika typer av autentiseringsuppgifter för åtkomst till den [Fragmentera kartan manager](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="bcb1d-104">The [Elastic Database client library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) uses three different kinds  of credentials to access the [shard map manager](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="bcb1d-105">Beroende på behov, använda autentiseringsuppgifter med den lägsta nivån av åtkomst som möjligt.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-105">Depending on the need, use the credential with  the lowest level of access possible.</span></span>

* <span data-ttu-id="bcb1d-106">**Hantering av autentiseringsuppgifter**: för att skapa eller ändra en Fragmentera kartan manager.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-106">**Management credentials**: for creating or manipulating a shard map manager.</span></span> <span data-ttu-id="bcb1d-107">(Se den [ordlista](sql-database-elastic-scale-glossary.md).)</span><span class="sxs-lookup"><span data-stu-id="bcb1d-107">(See the [glossary](sql-database-elastic-scale-glossary.md).)</span></span> 
* <span data-ttu-id="bcb1d-108">**Åtkomst till autentiseringsuppgifterna för**: åtkomst till en befintlig Fragmentera kartan manager för att få information om hur du delar.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-108">**Access credentials**: to access an existing shard map manager to obtain information about shards.</span></span>
* <span data-ttu-id="bcb1d-109">**Autentiseringsuppgifter för anslutning**: att ansluta till delar.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-109">**Connection credentials**: to connect to shards.</span></span> 

<span data-ttu-id="bcb1d-110">Se även [hantera databaser och inloggningar i Azure SQL Database](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="bcb1d-110">See also [Managing databases and logins in Azure SQL Database](sql-database-manage-logins.md).</span></span> 

## <a name="about-management-credentials"></a><span data-ttu-id="bcb1d-111">Om hantering av autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="bcb1d-111">About management credentials</span></span>
<span data-ttu-id="bcb1d-112">Hantering av autentiseringsuppgifter används för att skapa en [ **ShardMapManager** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) objekt för program som hanterar Fragmentera maps.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-112">Management credentials are used to create a [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) object for applications that manipulate shard maps.</span></span> <span data-ttu-id="bcb1d-113">(Till exempel se [att lägga till en Fragmentera med elastiska Databasverktyg](sql-database-elastic-scale-add-a-shard.md) och [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md)) användaren av klientbiblioteket elastisk skalbarhet skapar SQL-användare och SQL-inloggningar och ser till att varje beviljas Läs-/ skrivbehörigheter på globala Fragmentera kartan databasen och alla Fragmentera databaser samt.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-113">(For example, see [Adding a shard using Elastic Database tools](sql-database-elastic-scale-add-a-shard.md) and [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md)) The user of the elastic scale client library creates the SQL users and SQL logins and makes sure each is granted the read/write permissions on the global shard map database and all shard databases as well.</span></span> <span data-ttu-id="bcb1d-114">Dessa autentiseringsuppgifter används för att underhålla globala Fragmentera kartan och lokala Fragmentera maps när ändringar i kartan Fragmentera utförs.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-114">These credentials are used to maintain the global shard map and the local shard maps when changes to the shard map are performed.</span></span> <span data-ttu-id="bcb1d-115">Till exempel använda autentiseringsuppgifter hantering för att skapa Fragmentera kartan manager-objekt (med hjälp av [ **GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx):</span><span class="sxs-lookup"><span data-stu-id="bcb1d-115">For instance, use the management credentials to create the shard map manager object (using [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx):</span></span> 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

<span data-ttu-id="bcb1d-116">Variabeln **smmAdminConnectionString** är en anslutningssträng som innehåller autentiseringsuppgifter för hantering.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-116">The variable **smmAdminConnectionString** is a connection string that contains the management credentials.</span></span> <span data-ttu-id="bcb1d-117">Användar-ID och lösenord ger läsning och skrivning åtkomst till både Fragmentera kartan databas och enskilda delar.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-117">The user ID and password provides read/write access to both shard map database and individual shards.</span></span> <span data-ttu-id="bcb1d-118">Management-anslutningssträngen innehåller också servernamnet och databasnamnet att identifiera globala Fragmentera kartan databasen.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-118">The management connection string also includes the server name and database name to identify the global shard map database.</span></span> <span data-ttu-id="bcb1d-119">Här är en typisk anslutningssträng för detta ändamål:</span><span class="sxs-lookup"><span data-stu-id="bcb1d-119">Here is a typical connection string for that purpose:</span></span>

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

<span data-ttu-id="bcb1d-120">Använd inte värden i form av ”username@server”, i stället använda värdet ”användarnamn”.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-120">Do not use values in the form of "username@server"—instead just use the "username" value.</span></span>  <span data-ttu-id="bcb1d-121">Det beror på att autentiseringsuppgifter måste arbeta mot både Fragmentera kartan manager-databasen och enskilda delar som kan vara på olika servrar.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-121">This is because credentials must work against both the shard map manager database and individual shards, which may be on different servers.</span></span>

## <a name="access-credentials"></a><span data-ttu-id="bcb1d-122">Autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="bcb1d-122">Access credentials</span></span>
<span data-ttu-id="bcb1d-123">När du skapar en Fragmentera kartan manager i ett program som inte administrerar Fragmentera maps Använd autentiseringsuppgifter som har skrivskyddad behörighet på kartan globala Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-123">When creating a shard map manager in an application that does not administer shard maps, use credentials that have read-only permissions on the global shard map.</span></span> <span data-ttu-id="bcb1d-124">Informationen som hämtas från global Fragmentera kartan under dessa autentiseringsuppgifter används för [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) och för att fylla i Fragmentera kartan cachen på klienten.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-124">The information retrieved from the global shard map under these credentials are used for [data-dependent routing](sql-database-elastic-scale-data-dependent-routing.md) and to populate the shard map cache on the client.</span></span> <span data-ttu-id="bcb1d-125">Autentiseringsuppgifterna som tillhandahålls via samma mönster för anrop till **GetSqlShardMapManager** som ovan:</span><span class="sxs-lookup"><span data-stu-id="bcb1d-125">The credentials are provided through the same call pattern to **GetSqlShardMapManager** as shown above:</span></span> 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

<span data-ttu-id="bcb1d-126">Observera användningen av den **smmReadOnlyConnectionString** återspeglar användningen av olika autentiseringsuppgifter för åtkomst på uppdrag av **icke-administratörer** användare: autentiseringsuppgifterna bör inte ge skrivbehörighet för globala Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-126">Note the use of the **smmReadOnlyConnectionString** to reflect the use of different credentials for this access on behalf of **non-admin** users: these credentials should not provide write permissions on the global shard map.</span></span> 

## <a name="connection-credentials"></a><span data-ttu-id="bcb1d-127">Autentiseringsuppgifter för anslutning</span><span class="sxs-lookup"><span data-stu-id="bcb1d-127">Connection credentials</span></span>
<span data-ttu-id="bcb1d-128">Ytterligare autentiseringsuppgifter behövs när du använder den [ **OpenConnectionForKey** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) metod för att komma åt en Fragmentera som är associerade med en nyckel för horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-128">Additional credentials are needed when using the [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) method to access a shard associated with a sharding key.</span></span> <span data-ttu-id="bcb1d-129">Dessa autentiseringsuppgifter måste du ange behörigheter för skrivskyddad åtkomst till lokala Fragmentera kartan tabellerna som finns på Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-129">These credentials need to provide permissions for read-only access to the local shard map tables residing on the shard.</span></span> <span data-ttu-id="bcb1d-130">Detta behövs för att utföra anslutningsverifiering för omdirigering av data beroende på Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-130">This is needed to perform connection validation for data-dependent routing on the shard.</span></span> <span data-ttu-id="bcb1d-131">Det här kodstycket tillåter åtkomst till data i samband med data beroende routning:</span><span class="sxs-lookup"><span data-stu-id="bcb1d-131">This code snippet allows data access in the context of data dependent routing:</span></span> 

    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

<span data-ttu-id="bcb1d-132">I det här exemplet **smmUserConnectionString** innehåller anslutningssträngen för autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-132">In this example, **smmUserConnectionString** holds the connection string for the user credentials.</span></span> <span data-ttu-id="bcb1d-133">Här är en typisk anslutningssträng för autentiseringsuppgifter för Azure SQL DB:</span><span class="sxs-lookup"><span data-stu-id="bcb1d-133">For Azure SQL DB, here is a typical connection string for user credentials:</span></span> 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

<span data-ttu-id="bcb1d-134">Precis som med administratörsautentiseringsuppgifter, inte värden i form av ”username@server”.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-134">As with the admin credentials, do not values in the form of "username@server".</span></span> <span data-ttu-id="bcb1d-135">Använd ”användarnamn” istället bara.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-135">Instead, just use "username".</span></span>  <span data-ttu-id="bcb1d-136">Observera också att anslutningssträngen inte innehåller ett servernamn och databasnamn.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-136">Also note that the connection string does not contain a server name and database name.</span></span> <span data-ttu-id="bcb1d-137">Det beror på den **OpenConnectionForKey** anropet kommer automatiskt att dirigera anslutningen till rätt Fragmentera baserat på nyckeln.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-137">That is because the **OpenConnectionForKey** call will automatically direct the connection to the correct shard based on the key.</span></span> <span data-ttu-id="bcb1d-138">Därför tillhandahålls inte databasnamnet och namn på servern.</span><span class="sxs-lookup"><span data-stu-id="bcb1d-138">Hence, the database name and server name are not provided.</span></span> 

## <a name="see-also"></a><span data-ttu-id="bcb1d-139">Se även</span><span class="sxs-lookup"><span data-stu-id="bcb1d-139">See also</span></span>
[<span data-ttu-id="bcb1d-140">Hantera databaser och inloggningar i Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="bcb1d-140">Managing databases and logins in Azure SQL Database</span></span>](sql-database-manage-logins.md)

[<span data-ttu-id="bcb1d-141">Säkra din SQL Database</span><span class="sxs-lookup"><span data-stu-id="bcb1d-141">Securing your SQL Database</span></span>](sql-database-security-overview.md)

[<span data-ttu-id="bcb1d-142">Komma igång med jobb för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="bcb1d-142">Getting started with Elastic Database jobs</span></span>](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

