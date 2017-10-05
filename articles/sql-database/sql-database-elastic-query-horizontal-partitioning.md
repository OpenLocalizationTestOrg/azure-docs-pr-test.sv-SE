---
title: "Rapportering över databaser som skalats ut molntjänster | Microsoft Docs"
description: "hur du ställer in elastisk frågor via horisontella partitioner"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: f86eccb8-6323-4ba7-8559-8a7c039049f3
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: mlandzic
ms.openlocfilehash: 62b5bcd26aa1ed219fb38970916e0e8847ceb577
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="7cc9c-103">Rapportering över databaser som skalats ut molnet (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="7cc9c-103">Reporting across scaled-out cloud databases (preview)</span></span>
![Fråga på shards][1]

<span data-ttu-id="7cc9c-105">Delat databaser distribuera rader i en skaländras ut data nivå.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-105">Sharded databases distribute rows across a scaled out data tier.</span></span> <span data-ttu-id="7cc9c-106">Schemat är identiska för alla deltagande databaser, även kallat horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-106">The schema is identical on all participating databases, also known as horizontal partitioning.</span></span> <span data-ttu-id="7cc9c-107">Du kan skapa rapporter som omfattar alla databaser i en delat databas med en elastisk fråga.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-107">Using an elastic query, you can create reports that span all databases in a sharded database.</span></span>

<span data-ttu-id="7cc9c-108">En Snabbstart Se [rapportering över databaser som skalats ut molnet](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-108">For a quick start, see [Reporting across scaled-out cloud databases](sql-database-elastic-query-getting-started.md).</span></span>

<span data-ttu-id="7cc9c-109">Icke-delat databaser finns [fråga över moln databaser med olika scheman](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-109">For non-sharded databases, see [Query across cloud databases with different schemas](sql-database-elastic-query-vertical-partitioning.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7cc9c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7cc9c-110">Prerequisites</span></span>
* <span data-ttu-id="7cc9c-111">Skapa en Fragmentera karta med hjälp av klientbiblioteket för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-111">Create a shard map using the elastic database client library.</span></span> <span data-ttu-id="7cc9c-112">Se [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-112">see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="7cc9c-113">Eller Använd exempelapp i [Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-113">Or use the sample app in [Get started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="7cc9c-114">Du kan också se [migrera befintliga databaser som skalats ut databaser](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-114">Alternatively, see [Migrate existing databases to scaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>
* <span data-ttu-id="7cc9c-115">Användaren måste ha behörigheten ALTER ANY extern DATAKÄLLA.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-115">The user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="7cc9c-116">Den här behörigheten har behörigheten ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-116">This permission is included with the ALTER DATABASE permission.</span></span>
* <span data-ttu-id="7cc9c-117">ALTER ANY extern DATAKÄLLA behörighet att referera till den underliggande datakällan.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-117">ALTER ANY EXTERNAL DATA SOURCE permissions are needed to refer to the underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="7cc9c-118">Översikt</span><span class="sxs-lookup"><span data-stu-id="7cc9c-118">Overview</span></span>
<span data-ttu-id="7cc9c-119">De här uttrycken skapa metadata-representation av shardade data-tier i elastisk fråga databas.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-119">These statements create the metadata representation of your sharded data tier in the elastic query database.</span></span> 

1. [<span data-ttu-id="7cc9c-120">SKAPA HUVUDNYCKEL</span><span class="sxs-lookup"><span data-stu-id="7cc9c-120">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="7cc9c-121">SKAPA DATABASBEGRÄNSADE AUTENTISERINGSUPPGIFTER</span><span class="sxs-lookup"><span data-stu-id="7cc9c-121">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="7cc9c-122">SKAPA EXTERN DATAKÄLLA</span><span class="sxs-lookup"><span data-stu-id="7cc9c-122">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="7cc9c-123">SKAPA EXTERN TABELL</span><span class="sxs-lookup"><span data-stu-id="7cc9c-123">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="7cc9c-124">1.1 Skapa huvudnyckel för databasen omfång och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="7cc9c-124">1.1 Create database scoped master key and credentials</span></span>
<span data-ttu-id="7cc9c-125">Autentiseringsuppgifterna används av elastisk frågan för att ansluta till din fjärranslutna databaser.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-125">The credential is used by the elastic query to connect to your remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="7cc9c-126">Se till att den *”\<användarnamn\>”* innehåller inte några *”@servername”* suffix.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-126">Make sure that the *"\<username\>"* does not include any *"@servername"* suffix.</span></span> 
> 
> 

## <a name="12-create-external-data-sources"></a><span data-ttu-id="7cc9c-127">1.2 Skapa externa datakällor</span><span class="sxs-lookup"><span data-stu-id="7cc9c-127">1.2 Create external data sources</span></span>
<span data-ttu-id="7cc9c-128">Syntax:</span><span class="sxs-lookup"><span data-stu-id="7cc9c-128">Syntax:</span></span>

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a><span data-ttu-id="7cc9c-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="7cc9c-129">Example</span></span>
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

<span data-ttu-id="7cc9c-130">Hämta listan över aktuella externa datakällor:</span><span class="sxs-lookup"><span data-stu-id="7cc9c-130">Retrieve the list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

<span data-ttu-id="7cc9c-131">Den externa datakällan refererar till Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-131">The external data source references your shard map.</span></span> <span data-ttu-id="7cc9c-132">En elastisk fråga använder sedan den externa datakällan och underliggande Fragmentera kartan att räkna upp de databaser som ingår i datanivå.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-132">An elastic query then uses the external data source and the underlying shard map to enumerate the databases that participate in the data tier.</span></span> <span data-ttu-id="7cc9c-133">Samma autentiseringsuppgifter används för att läsa Fragmentera kartan och komma åt data på shards under bearbetning av en elastisk fråga.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-133">The same credentials are used to read the shard map and to access the data on the shards during the processing of an elastic query.</span></span> 

## <a name="13-create-external-tables"></a><span data-ttu-id="7cc9c-134">1.3 skapa externa tabeller</span><span class="sxs-lookup"><span data-stu-id="7cc9c-134">1.3 Create external tables</span></span>
<span data-ttu-id="7cc9c-135">Syntax:</span><span class="sxs-lookup"><span data-stu-id="7cc9c-135">Syntax:</span></span>  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

<span data-ttu-id="7cc9c-136">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="7cc9c-136">**Example**</span></span>

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 

    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
         SCHEMA_NAME = 'orders', 
         OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

<span data-ttu-id="7cc9c-137">Hämta listan med externa tabeller från databasen:</span><span class="sxs-lookup"><span data-stu-id="7cc9c-137">Retrieve the list of external tables from the current database:</span></span> 

    SELECT * from sys.external_tables; 

<span data-ttu-id="7cc9c-138">Att släppa externa tabeller:</span><span class="sxs-lookup"><span data-stu-id="7cc9c-138">To drop external tables:</span></span>

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a><span data-ttu-id="7cc9c-139">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="7cc9c-139">Remarks</span></span>
<span data-ttu-id="7cc9c-140">DATA\_SOURCE-satsen definierar den externa datakällan (en Fragmentera karta) som används för den externa tabellen.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-140">The DATA\_SOURCE clause defines the external data source (a shard map) that is used for the external table.</span></span>  

<span data-ttu-id="7cc9c-141">SCHEMAT\_namn och OBJEKTET\_namn satser mappa den externa tabelldefinitionen till en tabell i ett annat schema.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-141">The SCHEMA\_NAME and OBJECT\_NAME clauses map the external table definition to a table in a different schema.</span></span> <span data-ttu-id="7cc9c-142">Om det utelämnas används schemat för fjärrobjektet antas vara ”dbo” och dess namn antas vara identiskt med extern tabell som definieras.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-142">If omitted, the schema of the remote object is assumed to be “dbo” and its name is assumed to be identical to the external table name being defined.</span></span> <span data-ttu-id="7cc9c-143">Detta är användbart om namnet på din fjärrtabell används redan i databasen där du vill skapa extern tabell.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-143">This is useful if the name of your remote table is already taken in the database where you want to create the external table.</span></span> <span data-ttu-id="7cc9c-144">Till exempel du vill definiera en extern tabell för att få en aggregerad vy över katalogvyer eller nivån av DMV: er på dina data som skalats ut.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-144">For  example, you want to define an external table to get an aggregate view of catalog views or DMVs on your scaled out data tier.</span></span> <span data-ttu-id="7cc9c-145">Eftersom katalogvyer och av DMV: er redan finns lokalt och kan inte du använda deras namn för den externa tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-145">Since catalog views and DMVs already exist locally, you cannot use their names for the external table definition.</span></span> <span data-ttu-id="7cc9c-146">I stället använda ett annat namn och använda katalogvyn eller DMV i schemat\_namn och/eller OBJEKTET\_namn-satser.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-146">Instead, use a different name and use the catalog view’s or the DMV’s name in the SCHEMA\_NAME and/or OBJECT\_NAME clauses.</span></span> <span data-ttu-id="7cc9c-147">(Se exemplet nedan.)</span><span class="sxs-lookup"><span data-stu-id="7cc9c-147">(See the example below.)</span></span> 

<span data-ttu-id="7cc9c-148">Instruktionen DISTRIBUTION anger fördelning data används för den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-148">The DISTRIBUTION clause specifies the data distribution used for this table.</span></span> <span data-ttu-id="7cc9c-149">Frågeprocessorn använder informationen i DISTRIBUTION-sats för att skapa de mest effektiva frågeplanerna.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-149">The query processor utilizes the information provided in the DISTRIBUTION clause to build the most efficient query plans.</span></span>  

1. <span data-ttu-id="7cc9c-150">**DELAT** innebär data vågrätt partitionerad över databaser.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-150">**SHARDED** means data is horizontally partitioned across the databases.</span></span> <span data-ttu-id="7cc9c-151">Partitionsnyckel för Datadistribution är den **< sharding_column_name >** parameter.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-151">The partitioning key for the data distribution is the **<sharding_column_name>** parameter.</span></span>
2. <span data-ttu-id="7cc9c-152">**REPLIKERADE** innebär att identiska kopior av tabellen finns på varje databas.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-152">**REPLICATED** means that identical copies of the table are present on each database.</span></span> <span data-ttu-id="7cc9c-153">Det är ditt ansvar att se till att replikerna är identiska mellan databaser.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-153">It is your responsibility to ensure that the replicas are identical across the databases.</span></span>
3. <span data-ttu-id="7cc9c-154">**AVRUNDAR\_ROBIN** innebär att tabellen vågrätt är partitionerad med hjälp av en distributionsmetod för beroende program.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-154">**ROUND\_ROBIN** means that the table is horizontally partitioned using an application-dependent distribution method.</span></span> 

<span data-ttu-id="7cc9c-155">**Data tjänstnivån referens**: den externa tabellen DDL refererar till en extern datakälla.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-155">**Data tier reference**: The external table DDL refers to an external data source.</span></span> <span data-ttu-id="7cc9c-156">Den externa datakällan anger en Fragmentera karta som ger den externa tabellen med informationen som behövs för att hitta alla databaser i din datanivå.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-156">The external data source specifies a shard map which provides the external table with the information necessary to locate all the databases in your data tier.</span></span> 

### <a name="security-considerations"></a><span data-ttu-id="7cc9c-157">Säkerhetsöverväganden</span><span class="sxs-lookup"><span data-stu-id="7cc9c-157">Security considerations</span></span>
<span data-ttu-id="7cc9c-158">Användare med åtkomst till extern tabell få automatiskt åtkomst till de underliggande fjärrtabeller under autentiseringen i definitionen av externa datakällan.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-158">Users with access to the external table automatically gain access to the underlying remote tables under the credential given in the external data source definition.</span></span> <span data-ttu-id="7cc9c-159">Undvik oönskad höjning av privilegier genom autentiseringsuppgifterna för den externa datakällan.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-159">Avoid undesired elevation of privileges through the credential of the external data source.</span></span> <span data-ttu-id="7cc9c-160">Använd GRANT eller REVOKE för en extern tabell precis som om det vore en vanlig tabell.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-160">Use GRANT or REVOKE for an external table just as though it were a regular table.</span></span>  

<span data-ttu-id="7cc9c-161">När du har definierat den externa datakällan och externa tabeller kan använda du nu fullständig T-SQL via externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-161">Once you have defined your external data source and your external tables, you can now use full T-SQL over your external tables.</span></span>

## <a name="example-querying-horizontal-partitioned-databases"></a><span data-ttu-id="7cc9c-162">Exempel: frågar vågräta partitionerade databaser</span><span class="sxs-lookup"><span data-stu-id="7cc9c-162">Example: querying horizontal partitioned databases</span></span>
<span data-ttu-id="7cc9c-163">Följande fråga utför en trevägs koppling mellan lager order rader, och och använder flera aggregeringar och selektiv filter.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-163">The following query performs a three-way join between warehouses, orders and order lines and uses several aggregates and a selective filter.</span></span> <span data-ttu-id="7cc9c-164">Det förutsätts att (1) horisontell partitionering (delning) och (2) att lager order rader, och är delat av datalager ID-kolumnen och att elastisk frågan kan samordna relaterade kopplingar på shards och bearbeta dyra delen av frågan på shards i parallellt.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-164">It assumes (1) horizontal partitioning (sharding) and (2) that warehouses, orders and order lines are sharded by the warehouse id column, and that the elastic query can co-locate the joins on the shards and process the expensive part of the query on the shards in parallel.</span></span> 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="7cc9c-165">Lagrade proceduren för fjärrkörning av T-SQL: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="7cc9c-165">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="7cc9c-166">Elastisk frågan introducerar också en lagrad procedur som ger direktåtkomst till shards.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-166">Elastic query also introduces a stored procedure that provides direct access to the shards.</span></span> <span data-ttu-id="7cc9c-167">Den lagrade proceduren anropas [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) och kan användas för att köra fjärråtkomst lagrade procedurer eller T-SQL-kod på den fjärranslutna databaser.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-167">The stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used to execute remote stored procedures or T-SQL code on the remote databases.</span></span> <span data-ttu-id="7cc9c-168">Den använder följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="7cc9c-168">It takes the following parameters:</span></span> 

* <span data-ttu-id="7cc9c-169">Namn på datakälla (nvarchar): namnet på den externa datakällan av typen RDBMS.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-169">Data source name (nvarchar): The name of the external data source of type RDBMS.</span></span> 
* <span data-ttu-id="7cc9c-170">Fråga (nvarchar): T-SQL-frågan ska utföras på varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-170">Query (nvarchar): The T-SQL query to be executed on each shard.</span></span> 
* <span data-ttu-id="7cc9c-171">Parameterdeklaration (nvarchar) - valfritt: strängen med definitioner av data för de parametrar som används i Frågeparametern (till exempel sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-171">Parameter declaration (nvarchar) - optional: String with data type definitions for the parameters used in the Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="7cc9c-172">Värdet parameterlista - valfritt: kommaavgränsad lista över parametervärden (till exempel sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-172">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="7cc9c-173">En sp\_köra\_remote använder den externa datakällan i startparametrar för att köra den angivna T-SQL-instruktionen på fjärr-databaser.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-173">The sp\_execute\_remote uses the external data source provided in the invocation parameters to execute the given T-SQL statement on the remote databases.</span></span> <span data-ttu-id="7cc9c-174">Autentiseringsuppgifterna för den externa datakällan används för att ansluta till shardmap manager-databasen och de fjärranslutna databaserna.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-174">It uses the credential of the external data source to connect to the shardmap manager database and the remote databases.</span></span>  

<span data-ttu-id="7cc9c-175">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7cc9c-175">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a><span data-ttu-id="7cc9c-176">Anslutning för verktyg</span><span class="sxs-lookup"><span data-stu-id="7cc9c-176">Connectivity for tools</span></span>
<span data-ttu-id="7cc9c-177">Använda reguljära anslutningssträngar för SQL Server för att ansluta ditt program din integreringsverktyg för BI och till databasen med definitionerna extern tabell.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-177">Use regular SQL Server connection strings to connect your application, your BI and data integration tools to the database with your external table definitions.</span></span> <span data-ttu-id="7cc9c-178">Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-178">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="7cc9c-179">Sedan referens elastisk fråga-databas som andra SQL Server-databas ansluten till verktyget och Använd externa tabeller från verktyget eller program som om de vore lokala tabeller.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-179">Then reference the elastic query database like any other SQL Server database connected to the tool, and use external tables from your tool or application as if they were local tables.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="7cc9c-180">Bästa praxis</span><span class="sxs-lookup"><span data-stu-id="7cc9c-180">Best practices</span></span>
* <span data-ttu-id="7cc9c-181">Se till att elastisk fråga endpoint databas har behörighet till shardmap databasen och alla delar genom brandväggar för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-181">Ensure that the elastic query endpoint database has been given access to the shardmap database and all shards through the SQL DB firewalls.</span></span>  
* <span data-ttu-id="7cc9c-182">Validera eller tillämpa Datadistribution som definieras av den externa tabellen.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-182">Validate or enforce the data distribution defined by the external table.</span></span> <span data-ttu-id="7cc9c-183">Om din distribution för faktiska data skiljer sig från distributionsplatsen som anges i din tabelldefinitionen kan dina frågor ge oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-183">If your actual data distribution is different from the distribution specified in your table definition, your queries may yield unexpected results.</span></span> 
* <span data-ttu-id="7cc9c-184">Elastisk frågan för närvarande inte att utföra Fragmentera eliminering när predikat över nyckeln för horisontell partitionering skulle göra det möjligt att på ett säkert sätt utesluta vissa delar bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-184">Elastic query currently does not perform shard elimination when predicates over the sharding key would allow it to safely exclude certain shards from processing.</span></span>
* <span data-ttu-id="7cc9c-185">Elastisk frågan fungerar bäst för frågor där de flesta av beräkningen kan göras på shards.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-185">Elastic query works best for queries where most of the computation can be done on the shards.</span></span> <span data-ttu-id="7cc9c-186">Det uppstår vanligtvis bästa frågeprestanda med selektiv filter-predikat som kan utvärderas på shards eller kopplingar över partitionering nycklar som kan utföras på ett partitionsjusterade sätt på alla delar.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-186">You typically get the best query performance with selective filter predicates that can be evaluated on the shards or joins over the partitioning keys that can be performed in a partition-aligned way on all shards.</span></span> <span data-ttu-id="7cc9c-187">Andra frågemönster kan behöva läsa in stora mängder data från delar till huvudnod och kan fungera dåligt</span><span class="sxs-lookup"><span data-stu-id="7cc9c-187">Other query patterns may need to load large amounts of data from the shards to the head node and may perform poorly</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cc9c-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7cc9c-188">Next steps</span></span>

* <span data-ttu-id="7cc9c-189">En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-189">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="7cc9c-190">Vertikal partitionering, finns [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-190">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="7cc9c-191">Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="7cc9c-191">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="7cc9c-192">Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="7cc9c-192">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="7cc9c-193">Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="7cc9c-193">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
