---
title: "aaaReporting över databaser som skalats ut molntjänster | Microsoft Docs"
description: "hur tooset elastisk frågor via horisontella partitioner"
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
ms.openlocfilehash: 78986c2040bf308195bf7c77e64d4f37273fcf36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="30c5e-103">Rapportering över databaser som skalats ut molnet (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="30c5e-103">Reporting across scaled-out cloud databases (preview)</span></span>
![Fråga på shards][1]

<span data-ttu-id="30c5e-105">Delat databaser distribuera rader i en skaländras ut data nivå.</span><span class="sxs-lookup"><span data-stu-id="30c5e-105">Sharded databases distribute rows across a scaled out data tier.</span></span> <span data-ttu-id="30c5e-106">hello-schemat är identiska för alla deltagande databaser, även kallat horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="30c5e-106">hello schema is identical on all participating databases, also known as horizontal partitioning.</span></span> <span data-ttu-id="30c5e-107">Du kan skapa rapporter som omfattar alla databaser i en delat databas med en elastisk fråga.</span><span class="sxs-lookup"><span data-stu-id="30c5e-107">Using an elastic query, you can create reports that span all databases in a sharded database.</span></span>

<span data-ttu-id="30c5e-108">En Snabbstart Se [rapportering över databaser som skalats ut molnet](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="30c5e-108">For a quick start, see [Reporting across scaled-out cloud databases](sql-database-elastic-query-getting-started.md).</span></span>

<span data-ttu-id="30c5e-109">Icke-delat databaser finns [fråga över moln databaser med olika scheman](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="30c5e-109">For non-sharded databases, see [Query across cloud databases with different schemas](sql-database-elastic-query-vertical-partitioning.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="30c5e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="30c5e-110">Prerequisites</span></span>
* <span data-ttu-id="30c5e-111">Skapa en Fragmentera karta med hello klientbibliotek för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="30c5e-111">Create a shard map using hello elastic database client library.</span></span> <span data-ttu-id="30c5e-112">Se [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="30c5e-112">see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="30c5e-113">Eller Använd hello exempelapp i [Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="30c5e-113">Or use hello sample app in [Get started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="30c5e-114">Du kan också se [migrera befintliga databaser tooscaled ut databaser](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="30c5e-114">Alternatively, see [Migrate existing databases tooscaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>
* <span data-ttu-id="30c5e-115">hello användare måste ha behörigheten ALTER ANY extern DATAKÄLLA.</span><span class="sxs-lookup"><span data-stu-id="30c5e-115">hello user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="30c5e-116">Den här behörigheten ingår i hello ALTER DATABASE-behörighet.</span><span class="sxs-lookup"><span data-stu-id="30c5e-116">This permission is included with hello ALTER DATABASE permission.</span></span>
* <span data-ttu-id="30c5e-117">ALTER ANY extern DATAKÄLLA behörigheter är nödvändiga toorefer toohello underliggande datakällan.</span><span class="sxs-lookup"><span data-stu-id="30c5e-117">ALTER ANY EXTERNAL DATA SOURCE permissions are needed toorefer toohello underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="30c5e-118">Översikt</span><span class="sxs-lookup"><span data-stu-id="30c5e-118">Overview</span></span>
<span data-ttu-id="30c5e-119">De här uttrycken skapa hello metadata representation av shardade data-tier i hello elastisk fråga databas.</span><span class="sxs-lookup"><span data-stu-id="30c5e-119">These statements create hello metadata representation of your sharded data tier in hello elastic query database.</span></span> 

1. [<span data-ttu-id="30c5e-120">SKAPA HUVUDNYCKEL</span><span class="sxs-lookup"><span data-stu-id="30c5e-120">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="30c5e-121">SKAPA DATABASBEGRÄNSADE AUTENTISERINGSUPPGIFTER</span><span class="sxs-lookup"><span data-stu-id="30c5e-121">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="30c5e-122">SKAPA EXTERN DATAKÄLLA</span><span class="sxs-lookup"><span data-stu-id="30c5e-122">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="30c5e-123">SKAPA EXTERN TABELL</span><span class="sxs-lookup"><span data-stu-id="30c5e-123">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="30c5e-124">1.1 Skapa huvudnyckel för databasen omfång och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="30c5e-124">1.1 Create database scoped master key and credentials</span></span>
<span data-ttu-id="30c5e-125">hello autentiseringsuppgifter används av hello elastisk frågan tooconnect tooyour fjärranslutna databaser.</span><span class="sxs-lookup"><span data-stu-id="30c5e-125">hello credential is used by hello elastic query tooconnect tooyour remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="30c5e-126">Kontrollera att hello *”\<användarnamn\>”* innehåller inte några *”@servername”* suffix.</span><span class="sxs-lookup"><span data-stu-id="30c5e-126">Make sure that hello *"\<username\>"* does not include any *"@servername"* suffix.</span></span> 
> 
> 

## <a name="12-create-external-data-sources"></a><span data-ttu-id="30c5e-127">1.2 Skapa externa datakällor</span><span class="sxs-lookup"><span data-stu-id="30c5e-127">1.2 Create external data sources</span></span>
<span data-ttu-id="30c5e-128">Syntax:</span><span class="sxs-lookup"><span data-stu-id="30c5e-128">Syntax:</span></span>

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a><span data-ttu-id="30c5e-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="30c5e-129">Example</span></span>
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

<span data-ttu-id="30c5e-130">Hämta hello lista över aktuella externa datakällor:</span><span class="sxs-lookup"><span data-stu-id="30c5e-130">Retrieve hello list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

<span data-ttu-id="30c5e-131">hello extern datakälla refererar till Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="30c5e-131">hello external data source references your shard map.</span></span> <span data-ttu-id="30c5e-132">En elastisk fråga använder sedan hello extern datakälla och hello underliggande Fragmentera kartan tooenumerate hello databaser som ingår i hello datanivå.</span><span class="sxs-lookup"><span data-stu-id="30c5e-132">An elastic query then uses hello external data source and hello underlying shard map tooenumerate hello databases that participate in hello data tier.</span></span> <span data-ttu-id="30c5e-133">hello samma autentiseringsuppgifter är används tooread hello Fragmentera kartan och tooaccess hello data på hello shards under hello bearbetningen av en elastisk fråga.</span><span class="sxs-lookup"><span data-stu-id="30c5e-133">hello same credentials are used tooread hello shard map and tooaccess hello data on hello shards during hello processing of an elastic query.</span></span> 

## <a name="13-create-external-tables"></a><span data-ttu-id="30c5e-134">1.3 skapa externa tabeller</span><span class="sxs-lookup"><span data-stu-id="30c5e-134">1.3 Create external tables</span></span>
<span data-ttu-id="30c5e-135">Syntax:</span><span class="sxs-lookup"><span data-stu-id="30c5e-135">Syntax:</span></span>  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

<span data-ttu-id="30c5e-136">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="30c5e-136">**Example**</span></span>

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

<span data-ttu-id="30c5e-137">Hämta hello lista med externa tabeller från databasen för hello:</span><span class="sxs-lookup"><span data-stu-id="30c5e-137">Retrieve hello list of external tables from hello current database:</span></span> 

    SELECT * from sys.external_tables; 

<span data-ttu-id="30c5e-138">toodrop externa tabeller:</span><span class="sxs-lookup"><span data-stu-id="30c5e-138">toodrop external tables:</span></span>

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a><span data-ttu-id="30c5e-139">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="30c5e-139">Remarks</span></span>
<span data-ttu-id="30c5e-140">hello DATA\_SOURCE-satsen definierar hello extern datakälla (en Fragmentera karta) som används för hello extern tabell.</span><span class="sxs-lookup"><span data-stu-id="30c5e-140">hello DATA\_SOURCE clause defines hello external data source (a shard map) that is used for hello external table.</span></span>  

<span data-ttu-id="30c5e-141">hello schemat\_namn och OBJEKTET\_namn satser mappa hello extern tabell definition tooa tabell i ett annat schema.</span><span class="sxs-lookup"><span data-stu-id="30c5e-141">hello SCHEMA\_NAME and OBJECT\_NAME clauses map hello external table definition tooa table in a different schema.</span></span> <span data-ttu-id="30c5e-142">Om det utelämnas antas hello schemat för hello fjärrobjektet toobe ”dbo” och dess namn antas toobe identiska toohello externa tabellnamn som definieras.</span><span class="sxs-lookup"><span data-stu-id="30c5e-142">If omitted, hello schema of hello remote object is assumed toobe “dbo” and its name is assumed toobe identical toohello external table name being defined.</span></span> <span data-ttu-id="30c5e-143">Detta är användbart om hello namnet på din fjärrtabell redan är upptaget i hello databasen där du vill att toocreate hello extern tabell.</span><span class="sxs-lookup"><span data-stu-id="30c5e-143">This is useful if hello name of your remote table is already taken in hello database where you want toocreate hello external table.</span></span> <span data-ttu-id="30c5e-144">Till exempel du vill toodefine en extern tabell tooget en aggregerad vy över katalogvyer eller nivån av DMV: er på dina data som skalats ut.</span><span class="sxs-lookup"><span data-stu-id="30c5e-144">For  example, you want toodefine an external table tooget an aggregate view of catalog views or DMVs on your scaled out data tier.</span></span> <span data-ttu-id="30c5e-145">Eftersom katalogvyer och av DMV: er redan finns lokalt och kan inte du använda namnen för hello externa tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="30c5e-145">Since catalog views and DMVs already exist locally, you cannot use their names for hello external table definition.</span></span> <span data-ttu-id="30c5e-146">I stället använda ett annat namn och använda vyn hello-katalog eller hello DMVS namn i hello schemat\_namn och/eller OBJEKTET\_namn-satser.</span><span class="sxs-lookup"><span data-stu-id="30c5e-146">Instead, use a different name and use hello catalog view’s or hello DMV’s name in hello SCHEMA\_NAME and/or OBJECT\_NAME clauses.</span></span> <span data-ttu-id="30c5e-147">(Se hello exemplet nedan.)</span><span class="sxs-lookup"><span data-stu-id="30c5e-147">(See hello example below.)</span></span> 

<span data-ttu-id="30c5e-148">hello DISTRIBUTION satsen anger hello Datadistribution används för den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="30c5e-148">hello DISTRIBUTION clause specifies hello data distribution used for this table.</span></span> <span data-ttu-id="30c5e-149">hello frågeprocessorn använder hello informationen i hello DISTRIBUTION satsen toobuild hello effektivaste frågeplaner.</span><span class="sxs-lookup"><span data-stu-id="30c5e-149">hello query processor utilizes hello information provided in hello DISTRIBUTION clause toobuild hello most efficient query plans.</span></span>  

1. <span data-ttu-id="30c5e-150">**DELAT** innebär data vågrätt partitionerad över hello databaser.</span><span class="sxs-lookup"><span data-stu-id="30c5e-150">**SHARDED** means data is horizontally partitioned across hello databases.</span></span> <span data-ttu-id="30c5e-151">hello partitionering nyckel för hello Datadistribution är hello **< sharding_column_name >** parameter.</span><span class="sxs-lookup"><span data-stu-id="30c5e-151">hello partitioning key for hello data distribution is hello **<sharding_column_name>** parameter.</span></span>
2. <span data-ttu-id="30c5e-152">**REPLIKERADE** innebär att identiska kopior av hello tabellen finns på varje databas.</span><span class="sxs-lookup"><span data-stu-id="30c5e-152">**REPLICATED** means that identical copies of hello table are present on each database.</span></span> <span data-ttu-id="30c5e-153">Det är ditt ansvar tooensure hello repliker är identiska över hello databaser.</span><span class="sxs-lookup"><span data-stu-id="30c5e-153">It is your responsibility tooensure that hello replicas are identical across hello databases.</span></span>
3. <span data-ttu-id="30c5e-154">**AVRUNDAR\_ROBIN** innebär hello tabellen är partitionerad vågrätt med hjälp av en distributionsmetod för beroende program.</span><span class="sxs-lookup"><span data-stu-id="30c5e-154">**ROUND\_ROBIN** means that hello table is horizontally partitioned using an application-dependent distribution method.</span></span> 

<span data-ttu-id="30c5e-155">**Data tjänstnivån referens**: hello externa DDL-tabellen hänvisar tooan extern datakälla.</span><span class="sxs-lookup"><span data-stu-id="30c5e-155">**Data tier reference**: hello external table DDL refers tooan external data source.</span></span> <span data-ttu-id="30c5e-156">hello extern datakälla anger en Fragmentera karta som ger hello extern tabell med hello information behövs toolocate alla hello databaser i din datanivå.</span><span class="sxs-lookup"><span data-stu-id="30c5e-156">hello external data source specifies a shard map which provides hello external table with hello information necessary toolocate all hello databases in your data tier.</span></span> 

### <a name="security-considerations"></a><span data-ttu-id="30c5e-157">Säkerhetsöverväganden</span><span class="sxs-lookup"><span data-stu-id="30c5e-157">Security considerations</span></span>
<span data-ttu-id="30c5e-158">Användare med åtkomst toohello extern tabell få automatiskt åtkomst toohello underliggande fjärrtabeller under hello autentiseringsuppgifter som anges i hello externa definitionen av datakällan.</span><span class="sxs-lookup"><span data-stu-id="30c5e-158">Users with access toohello external table automatically gain access toohello underlying remote tables under hello credential given in hello external data source definition.</span></span> <span data-ttu-id="30c5e-159">Undvik oönskad höjning av privilegier genom hello autentiseringsuppgifterna för hello extern datakälla.</span><span class="sxs-lookup"><span data-stu-id="30c5e-159">Avoid undesired elevation of privileges through hello credential of hello external data source.</span></span> <span data-ttu-id="30c5e-160">Använd GRANT eller REVOKE för en extern tabell precis som om det vore en vanlig tabell.</span><span class="sxs-lookup"><span data-stu-id="30c5e-160">Use GRANT or REVOKE for an external table just as though it were a regular table.</span></span>  

<span data-ttu-id="30c5e-161">När du har definierat den externa datakällan och externa tabeller kan använda du nu fullständig T-SQL via externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="30c5e-161">Once you have defined your external data source and your external tables, you can now use full T-SQL over your external tables.</span></span>

## <a name="example-querying-horizontal-partitioned-databases"></a><span data-ttu-id="30c5e-162">Exempel: frågar vågräta partitionerade databaser</span><span class="sxs-lookup"><span data-stu-id="30c5e-162">Example: querying horizontal partitioned databases</span></span>
<span data-ttu-id="30c5e-163">hello följande fråga utför en trevägs koppling mellan lager order rader, och och använder flera aggregeringar och selektiv filter.</span><span class="sxs-lookup"><span data-stu-id="30c5e-163">hello following query performs a three-way join between warehouses, orders and order lines and uses several aggregates and a selective filter.</span></span> <span data-ttu-id="30c5e-164">Det förutsätts att (1) horisontell partitionering (delning) och (2) att lager order rader, och är delat av hello datalager id-kolumnen hello elastisk frågan kan samordna relaterade hello kopplingar på hello shards och bearbeta hello dyra tillhör hello frågan på hello shards parallellt.</span><span class="sxs-lookup"><span data-stu-id="30c5e-164">It assumes (1) horizontal partitioning (sharding) and (2) that warehouses, orders and order lines are sharded by hello warehouse id column, and that hello elastic query can co-locate hello joins on hello shards and process hello expensive part of hello query on hello shards in parallel.</span></span> 

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

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="30c5e-165">Lagrade proceduren för fjärrkörning av T-SQL: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="30c5e-165">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="30c5e-166">Elastisk frågan introducerar också en lagrad procedur som ger direktåtkomst toohello delar.</span><span class="sxs-lookup"><span data-stu-id="30c5e-166">Elastic query also introduces a stored procedure that provides direct access toohello shards.</span></span> <span data-ttu-id="30c5e-167">hello lagrade proceduren anropas [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) och kan vara används tooexecute remote lagrade procedurer eller T-SQL-kod på fjärranslutna hello-databaser.</span><span class="sxs-lookup"><span data-stu-id="30c5e-167">hello stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used tooexecute remote stored procedures or T-SQL code on hello remote databases.</span></span> <span data-ttu-id="30c5e-168">Det tar hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="30c5e-168">It takes hello following parameters:</span></span> 

* <span data-ttu-id="30c5e-169">Namn på datakälla (nvarchar): hello namnet på hello extern datakälla av typen RDBMS.</span><span class="sxs-lookup"><span data-stu-id="30c5e-169">Data source name (nvarchar): hello name of hello external data source of type RDBMS.</span></span> 
* <span data-ttu-id="30c5e-170">Fråga (nvarchar): hello T-SQL-fråga toobe utförs på varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="30c5e-170">Query (nvarchar): hello T-SQL query toobe executed on each shard.</span></span> 
* <span data-ttu-id="30c5e-171">Parameterdeklaration (nvarchar) - valfritt: strängen med datatypdefinitioner för hello parametrar som används i hello frågeparameter (till exempel sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="30c5e-171">Parameter declaration (nvarchar) - optional: String with data type definitions for hello parameters used in hello Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="30c5e-172">Värdet parameterlista - valfritt: kommaavgränsad lista över parametervärden (till exempel sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="30c5e-172">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="30c5e-173">hello sp\_köra\_fjärråtkomst använder hello extern datakälla i hello anrop parametrar tooexecute hello ges T-SQL-instruktion för fjärranslutna hello-databaser.</span><span class="sxs-lookup"><span data-stu-id="30c5e-173">hello sp\_execute\_remote uses hello external data source provided in hello invocation parameters tooexecute hello given T-SQL statement on hello remote databases.</span></span> <span data-ttu-id="30c5e-174">Den använder hello autentiseringsuppgifterna för hello externa data tooconnect toohello shardmap manager källdatabasen och hello fjärranslutna databaser.</span><span class="sxs-lookup"><span data-stu-id="30c5e-174">It uses hello credential of hello external data source tooconnect toohello shardmap manager database and hello remote databases.</span></span>  

<span data-ttu-id="30c5e-175">Exempel:</span><span class="sxs-lookup"><span data-stu-id="30c5e-175">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a><span data-ttu-id="30c5e-176">Anslutning för verktyg</span><span class="sxs-lookup"><span data-stu-id="30c5e-176">Connectivity for tools</span></span>
<span data-ttu-id="30c5e-177">Använda reguljära SQL Server-anslutningen strängar tooconnect ditt program, data och BI integration verktyg toohello databasen med definitionerna extern tabell.</span><span class="sxs-lookup"><span data-stu-id="30c5e-177">Use regular SQL Server connection strings tooconnect your application, your BI and data integration tools toohello database with your external table definitions.</span></span> <span data-ttu-id="30c5e-178">Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver.</span><span class="sxs-lookup"><span data-stu-id="30c5e-178">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="30c5e-179">Referera hello elastisk fråga databas som andra SQL Server database anslutna toohello verktyg och använda externa tabeller från verktyget eller program som om de vore lokala tabeller.</span><span class="sxs-lookup"><span data-stu-id="30c5e-179">Then reference hello elastic query database like any other SQL Server database connected toohello tool, and use external tables from your tool or application as if they were local tables.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="30c5e-180">Bästa praxis</span><span class="sxs-lookup"><span data-stu-id="30c5e-180">Best practices</span></span>
* <span data-ttu-id="30c5e-181">Kontrollera hello elastisk frågan endpoint databasen har fått åtkomst toohello shardmap databasen och alla shards via hello SQL DB brandväggar.</span><span class="sxs-lookup"><span data-stu-id="30c5e-181">Ensure that hello elastic query endpoint database has been given access toohello shardmap database and all shards through hello SQL DB firewalls.</span></span>  
* <span data-ttu-id="30c5e-182">Validera eller tillämpa hello Datadistribution definieras av hello extern tabell.</span><span class="sxs-lookup"><span data-stu-id="30c5e-182">Validate or enforce hello data distribution defined by hello external table.</span></span> <span data-ttu-id="30c5e-183">Om din distribution för faktiska data skiljer sig från hello-distribution som anges i din tabelldefinitionen kan dina frågor ge oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="30c5e-183">If your actual data distribution is different from hello distribution specified in your table definition, your queries may yield unexpected results.</span></span> 
* <span data-ttu-id="30c5e-184">Elastisk frågan för närvarande inte att utföra Fragmentera eliminering när predikat över hello horisontell partitionering nyckeln skulle låta den toosafely undanta vissa delar från bearbetning.</span><span class="sxs-lookup"><span data-stu-id="30c5e-184">Elastic query currently does not perform shard elimination when predicates over hello sharding key would allow it toosafely exclude certain shards from processing.</span></span>
* <span data-ttu-id="30c5e-185">Elastisk frågan fungerar bäst för frågor där de flesta av hello beräkningsresurser kan göras på hello delar.</span><span class="sxs-lookup"><span data-stu-id="30c5e-185">Elastic query works best for queries where most of hello computation can be done on hello shards.</span></span> <span data-ttu-id="30c5e-186">Det uppstår vanligtvis hello bästa frågeprestanda med selektiv filter-predikat som kan utvärderas på hello shards eller kopplingar över hello partitionering nycklar som kan utföras på ett partitionsjusterade sätt på alla delar.</span><span class="sxs-lookup"><span data-stu-id="30c5e-186">You typically get hello best query performance with selective filter predicates that can be evaluated on hello shards or joins over hello partitioning keys that can be performed in a partition-aligned way on all shards.</span></span> <span data-ttu-id="30c5e-187">Andra frågemönster behöva tooload stora mängder data från delar hello toohello huvudnod och kan fungera dåligt</span><span class="sxs-lookup"><span data-stu-id="30c5e-187">Other query patterns may need tooload large amounts of data from hello shards toohello head node and may perform poorly</span></span>

## <a name="next-steps"></a><span data-ttu-id="30c5e-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30c5e-188">Next steps</span></span>

* <span data-ttu-id="30c5e-189">En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="30c5e-189">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="30c5e-190">Vertikal partitionering, finns [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="30c5e-190">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="30c5e-191">Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="30c5e-191">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="30c5e-192">Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="30c5e-192">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="30c5e-193">Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="30c5e-193">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
