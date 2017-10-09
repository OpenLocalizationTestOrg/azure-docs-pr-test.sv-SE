---
title: "aaaReport över utskalat moln databaser (horisontell partitionering) | Microsoft Docs"
description: "hur toouse mellan databasfrågor för databasen"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: c81ef5e3-41e9-4fd2-8631-868f2e168147
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: mlandzic
ms.openlocfilehash: e34f398f8d408cffd91a70fc2cfbda73daec3550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="a85d5-103">Rapporter över databaser som skalats ut molnet (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="a85d5-103">Report across scaled-out cloud databases (preview)</span></span>
<span data-ttu-id="a85d5-104">Du kan skapa rapporter från flera Azure SQL-databaser från en enda anslutning med en [elastisk frågan](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a85d5-104">You can create reports from multiple Azure SQL databases from a single connection point using an [elastic query](sql-database-elastic-query-overview.md).</span></span> <span data-ttu-id="a85d5-105">hello databaser måste vara partitionerade vågrätt (även kallat ”delat”).</span><span class="sxs-lookup"><span data-stu-id="a85d5-105">hello databases must be horizontally partitioned (also known as "sharded").</span></span>

<span data-ttu-id="a85d5-106">Om du har en befintlig databas, se [migrera befintliga databaser tooscaled ut databaser](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="a85d5-106">If you have an existing database, see [Migrating existing databases tooscaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>

<span data-ttu-id="a85d5-107">toounderstand hello SQL-objekt krävs tooquery, se [fråga över vågrätt partitionerade databaser](sql-database-elastic-query-horizontal-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="a85d5-107">toounderstand hello SQL objects needed tooquery, see [Query across horizontally partitioned databases](sql-database-elastic-query-horizontal-partitioning.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a85d5-108">Krav</span><span class="sxs-lookup"><span data-stu-id="a85d5-108">Prerequisites</span></span>
<span data-ttu-id="a85d5-109">Hämta och köra hello [komma igång med elastisk databas verktyg exempel](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a85d5-109">Download and run hello [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a><span data-ttu-id="a85d5-110">Skapa en Fragmentera karta manager med hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="a85d5-110">Create a shard map manager using hello sample app</span></span>
<span data-ttu-id="a85d5-111">Här skapar du en karta Fragmentera manager tillsammans med flera delar, följt av infogning av data i hello delar.</span><span class="sxs-lookup"><span data-stu-id="a85d5-111">Here you will create a shard map manager along with several shards, followed by insertion of data into hello shards.</span></span> <span data-ttu-id="a85d5-112">Om du råkar tooalready har shards installation med shardade data, kan du hoppa över följande hello och flytta toohello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a85d5-112">If you happen tooalready have shards setup with sharded data in them, you can skip hello following steps and move toohello next section.</span></span>

1. <span data-ttu-id="a85d5-113">Skapa och köra hello **komma igång med elastiska Databasverktyg** exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a85d5-113">Build and run hello **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="a85d5-114">Gör hello förrän steg 7 i hello avsnittet [hämta och köra hello exempelapp](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="a85d5-114">Follow hello steps until step 7 in hello section [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="a85d5-115">Hello slutet av steg 7 visas hello följande kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="a85d5-115">At hello end of Step 7, you will see hello following command prompt:</span></span>

    ![kommandotolk][1]
2. <span data-ttu-id="a85d5-117">Skriv ”1” i hello-kommandotolk och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a85d5-117">In hello command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="a85d5-118">Detta skapar hello Fragmentera kartan manager och lägger till två delar toohello server.</span><span class="sxs-lookup"><span data-stu-id="a85d5-118">This creates hello shard map manager, and adds two shards toohello server.</span></span> <span data-ttu-id="a85d5-119">Sedan skriver ”3” och tryck på **RETUR**; Upprepa hello åtgärd fyra gånger.</span><span class="sxs-lookup"><span data-stu-id="a85d5-119">Then type "3" and press **Enter**; repeat hello action four times.</span></span> <span data-ttu-id="a85d5-120">Detta infogar exempel datarader i din shards.</span><span class="sxs-lookup"><span data-stu-id="a85d5-120">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="a85d5-121">Hej [Azure-portalen](https://portal.azure.com) ska visa tre nya databaser på servern:</span><span class="sxs-lookup"><span data-stu-id="a85d5-121">hello [Azure portal](https://portal.azure.com) should show three new databases in your server:</span></span>

   ![Visual Studio-bekräftelse][2]

   <span data-ttu-id="a85d5-123">Nu stöds mellan databasfrågor via hello klientbibliotek för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="a85d5-123">At this point, cross-database queries are supported through hello Elastic Database client libraries.</span></span> <span data-ttu-id="a85d5-124">Till exempel använda alternativ 4 i hello kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="a85d5-124">For example, use option 4 in hello command window.</span></span> <span data-ttu-id="a85d5-125">hello resultat från en fråga med flera Fragmentera är alltid en **UNION ALL** hello resultat från alla delar.</span><span class="sxs-lookup"><span data-stu-id="a85d5-125">hello results from a multi-shard query are always a **UNION ALL** of hello results from all shards.</span></span>

   <span data-ttu-id="a85d5-126">I nästa avsnitt om hello skapa vi en exempel-databasen slutpunkt som har stöd för bättre frågor till hello data över shards.</span><span class="sxs-lookup"><span data-stu-id="a85d5-126">In hello next section, we create a sample database endpoint that supports richer querying of hello data across shards.</span></span>

## <a name="create-an-elastic-query-database"></a><span data-ttu-id="a85d5-127">Skapa en fråga för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="a85d5-127">Create an elastic query database</span></span>
1. <span data-ttu-id="a85d5-128">Öppna hello [Azure-portalen](https://portal.azure.com) och logga in.</span><span class="sxs-lookup"><span data-stu-id="a85d5-128">Open hello [Azure portal](https://portal.azure.com) and log in.</span></span>
2. <span data-ttu-id="a85d5-129">Skapa en ny Azure SQL-databas i hello samma server som Fragmentera-installationen.</span><span class="sxs-lookup"><span data-stu-id="a85d5-129">Create a new Azure SQL database in hello same server as your shard setup.</span></span> <span data-ttu-id="a85d5-130">Databasen för hello ”ElasticDBQuery”.</span><span class="sxs-lookup"><span data-stu-id="a85d5-130">Name hello database "ElasticDBQuery."</span></span>

    ![Azure-portalen och prisnivå][3]

    > [!NOTE]
    > <span data-ttu-id="a85d5-132">Du kan använda en befintlig databas.</span><span class="sxs-lookup"><span data-stu-id="a85d5-132">you can use an existing database.</span></span> <span data-ttu-id="a85d5-133">Om du gör det, den får inte vara en hello delar som du vill att tooexecute dina frågor på.</span><span class="sxs-lookup"><span data-stu-id="a85d5-133">If you can do so, it must not be one of hello shards that you would like tooexecute your queries on.</span></span> <span data-ttu-id="a85d5-134">Den här databasen används för att skapa hello metadataobjekt för en elastisk databas-fråga.</span><span class="sxs-lookup"><span data-stu-id="a85d5-134">This database will be used for creating hello metadata objects for an elastic database query.</span></span>
    >

## <a name="create-database-objects"></a><span data-ttu-id="a85d5-135">Skapa databasobjekt</span><span class="sxs-lookup"><span data-stu-id="a85d5-135">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="a85d5-136">Databas-omfattande huvudnyckel och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="a85d5-136">Database-scoped master key and credentials</span></span>
<span data-ttu-id="a85d5-137">Dessa är används tooconnect toohello Fragmentera kartan manager och hello delar:</span><span class="sxs-lookup"><span data-stu-id="a85d5-137">These are used tooconnect toohello shard map manager and hello shards:</span></span>

1. <span data-ttu-id="a85d5-138">Öppna SQL Server Management Studio eller SQL Server Data Tools i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a85d5-138">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="a85d5-139">Ansluta tooElasticDBQuery databasen och kör följande T-SQL-kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="a85d5-139">Connect tooElasticDBQuery database and execute hello following T-SQL commands:</span></span>

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    <span data-ttu-id="a85d5-140">”användarnamn” och ”password” ska vara hello samma som de inloggningsuppgifter som används i steg 6 i [hämta och köra hello exempelapp](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) i [komma igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a85d5-140">"username" and "password" should be hello same as login information used in step 6 of [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) in [Getting started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="a85d5-141">Externa datakällor</span><span class="sxs-lookup"><span data-stu-id="a85d5-141">External data sources</span></span>
<span data-ttu-id="a85d5-142">toocreate en extern datakälla, kör följande kommando på hello ElasticDBQuery databasen hello:</span><span class="sxs-lookup"><span data-stu-id="a85d5-142">toocreate an external data source, execute hello following command on hello ElasticDBQuery database:</span></span>

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 <span data-ttu-id="a85d5-143">”CustomerIDShardMap” är hello Fragmentera kartan hello namn om du har skapat hello Fragmentera karta och Fragmentera kartan manager använder hello elastisk databas verktyg exemplet.</span><span class="sxs-lookup"><span data-stu-id="a85d5-143">"CustomerIDShardMap" is hello name of hello shard map, if you created hello shard map and shard map manager using hello elastic database tools sample.</span></span> <span data-ttu-id="a85d5-144">Om du har använt din anpassade konfiguration för det här exemplet bör sedan det dock hello Fragmentera mappningsnamn som du valde i ditt program.</span><span class="sxs-lookup"><span data-stu-id="a85d5-144">However, if you used your custom setup for this sample, then it should be hello shard map name you chose in your application.</span></span>

### <a name="external-tables"></a><span data-ttu-id="a85d5-145">Externa tabeller</span><span class="sxs-lookup"><span data-stu-id="a85d5-145">External tables</span></span>
<span data-ttu-id="a85d5-146">Skapa en extern tabell som matchar hello kunder tabellen på hello shards genom att köra följande kommando på ElasticDBQuery databasen hello:</span><span class="sxs-lookup"><span data-stu-id="a85d5-146">Create an external table that matches hello Customers table on hello shards by executing hello following command on ElasticDBQuery database:</span></span>

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="a85d5-147">Köra en exempelfråga för elastisk databas T-SQL</span><span class="sxs-lookup"><span data-stu-id="a85d5-147">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="a85d5-148">När du har definierat den externa datakällan och externa tabeller kan du nu använda fullständig T-SQL via externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="a85d5-148">Once you have defined your external data source and your external tables you can now use full T-SQL over your external tables.</span></span>

<span data-ttu-id="a85d5-149">Kör frågan på hello ElasticDBQuery databasen:</span><span class="sxs-lookup"><span data-stu-id="a85d5-149">Execute this query on hello ElasticDBQuery database:</span></span>

    select count(CustomerId) from [dbo].[Customers]

<span data-ttu-id="a85d5-150">Du kommer att märka hello frågan aggregerar resultat från alla hello shards och ger hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="a85d5-150">You will notice that hello query aggregates results from all hello shards and gives hello following output:</span></span>

![Information för utdata][4]

## <a name="import-elastic-database-query-results-tooexcel"></a><span data-ttu-id="a85d5-152">Importera elastisk databas frågan resultat tooExcel</span><span class="sxs-lookup"><span data-stu-id="a85d5-152">Import elastic database query results tooExcel</span></span>
 <span data-ttu-id="a85d5-153">Du kan importera hello resultaten från av en fråga tooan Excel-fil.</span><span class="sxs-lookup"><span data-stu-id="a85d5-153">You can import hello results from of a query tooan Excel file.</span></span>

1. <span data-ttu-id="a85d5-154">Starta Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="a85d5-154">Launch Excel 2013.</span></span>
2. <span data-ttu-id="a85d5-155">Navigera toohello **Data** menyfliksområdet.</span><span class="sxs-lookup"><span data-stu-id="a85d5-155">Navigate toohello **Data** ribbon.</span></span>
3. <span data-ttu-id="a85d5-156">Klicka på **från andra källor** och på **från SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="a85d5-156">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Excel-import från andra källor][5]
4. <span data-ttu-id="a85d5-158">I hello **Dataanslutningsguiden** skriver hello server namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="a85d5-158">In hello **Data Connection Wizard** type hello server name and login credentials.</span></span> <span data-ttu-id="a85d5-159">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a85d5-159">Then click **Next**.</span></span>
5. <span data-ttu-id="a85d5-160">I dialogrutan för hello **väljer hello-databas som innehåller hello data som du vill**väljer hello **ElasticDBQuery** databas.</span><span class="sxs-lookup"><span data-stu-id="a85d5-160">In hello dialog box **Select hello database that contains hello data you want**, select hello **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="a85d5-161">Välj hello **kunder** tabell i hello listvyn och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a85d5-161">Select hello **Customers** table in hello list view and click **Next**.</span></span> <span data-ttu-id="a85d5-162">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a85d5-162">Then click **Finish**.</span></span>
7. <span data-ttu-id="a85d5-163">I hello **importera Data** formuläret under **väljer du hur tooview dessa data i arbetsboken**väljer **tabell** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a85d5-163">In hello **Import Data** form, under **Select how you want tooview this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="a85d5-164">Alla hello rader från **kunder** tabell som sparas i olika delar fylla hello Excel-blad.</span><span class="sxs-lookup"><span data-stu-id="a85d5-164">All hello rows from **Customers** table, stored in different shards populate hello Excel sheet.</span></span>

<span data-ttu-id="a85d5-165">Du kan nu använda Excel-kraftfulla visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="a85d5-165">You can now use Excel’s powerful data visualization functions.</span></span> <span data-ttu-id="a85d5-166">Du kan använda hello anslutningssträngen med servernamnet, databasnamnet och autentiseringsuppgifter tooconnect BI och data integration verktyg toohello elastisk fråga databasen.</span><span class="sxs-lookup"><span data-stu-id="a85d5-166">You can use hello connection string with your server name, database name and credentials tooconnect your BI and data integration tools toohello elastic query database.</span></span> <span data-ttu-id="a85d5-167">Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver.</span><span class="sxs-lookup"><span data-stu-id="a85d5-167">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="a85d5-168">Du kan referera toohello elastisk fråga databas och externa tabeller precis som andra SQL Server-databas och att du vill ansluta toowith verktyget du SQL Server-tabeller.</span><span class="sxs-lookup"><span data-stu-id="a85d5-168">You can refer toohello elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect toowith your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="a85d5-169">Kostnad</span><span class="sxs-lookup"><span data-stu-id="a85d5-169">Cost</span></span>
<span data-ttu-id="a85d5-170">Det finns utan extra kostnad för hello elastisk databasfrågan funktionen.</span><span class="sxs-lookup"><span data-stu-id="a85d5-170">There is no additional charge for using hello Elastic Database Query feature.</span></span>

<span data-ttu-id="a85d5-171">Mer information om priser finns [prisinformation för SQL-databasen](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="a85d5-171">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a85d5-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a85d5-172">Next steps</span></span>

* <span data-ttu-id="a85d5-173">En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a85d5-173">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="a85d5-174">Vertikal partitionering, finns [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="a85d5-174">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="a85d5-175">Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="a85d5-175">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="a85d5-176">Syntax och exempel frågor för vågrätt partitionerade finns [frågar vågrätt partitionerad data)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="a85d5-176">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="a85d5-177">Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="a85d5-177">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
