---
title: "aaaQuery över moln databaser med olika schema | Microsoft Docs"
description: "hur tooset flera databaser frågor via lodräta partitioner"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 84c261f2-9edc-42f4-988c-cf2f251f5eff
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 1134e2e608128b7a9cac47ff35a22a11e6e5bc14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a><span data-ttu-id="fec7c-103">Fråga på molnet databaser med olika scheman (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="fec7c-103">Query across cloud databases with different schemas (preview)</span></span>
![Fråga på tabeller i olika databaser][1]

<span data-ttu-id="fec7c-105">Lodrätt partitionerad-databaser använder olika uppsättningar med tabeller på olika databaser.</span><span class="sxs-lookup"><span data-stu-id="fec7c-105">Vertically-partitioned databases use different sets of tables on different databases.</span></span> <span data-ttu-id="fec7c-106">Det innebär att hello schema skiljer sig på olika databaser.</span><span class="sxs-lookup"><span data-stu-id="fec7c-106">That means that hello schema is different on different databases.</span></span> <span data-ttu-id="fec7c-107">Till exempel finns alla tabeller för lager på en databas när alla redovisning-relaterade tabeller är på en andra databas.</span><span class="sxs-lookup"><span data-stu-id="fec7c-107">For instance, all tables for inventory are on one database while all accounting-related tables are on a second database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fec7c-108">Krav</span><span class="sxs-lookup"><span data-stu-id="fec7c-108">Prerequisites</span></span>
* <span data-ttu-id="fec7c-109">hello användare måste ha behörigheten ALTER ANY extern DATAKÄLLA.</span><span class="sxs-lookup"><span data-stu-id="fec7c-109">hello user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="fec7c-110">Den här behörigheten ingår i hello ALTER DATABASE-behörighet.</span><span class="sxs-lookup"><span data-stu-id="fec7c-110">This permission is included with hello ALTER DATABASE permission.</span></span>
* <span data-ttu-id="fec7c-111">ALTER ANY extern DATAKÄLLA behörigheter är nödvändiga toorefer toohello underliggande datakällan.</span><span class="sxs-lookup"><span data-stu-id="fec7c-111">ALTER ANY EXTERNAL DATA SOURCE permissions are needed toorefer toohello underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="fec7c-112">Översikt</span><span class="sxs-lookup"><span data-stu-id="fec7c-112">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="fec7c-113">Till skillnad från med horisontell partitionering är dessa DDL-satser inte beroende definierar en datanivå med en Fragmentera karta via hello klientbibliotek för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="fec7c-113">Unlike with horizontal partitioning, these DDL statements do not depend on defining a data tier with a shard map through hello elastic database client library.</span></span>
>

1. [<span data-ttu-id="fec7c-114">SKAPA HUVUDNYCKEL</span><span class="sxs-lookup"><span data-stu-id="fec7c-114">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="fec7c-115">SKAPA DATABASBEGRÄNSADE AUTENTISERINGSUPPGIFTER</span><span class="sxs-lookup"><span data-stu-id="fec7c-115">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="fec7c-116">SKAPA EXTERN DATAKÄLLA</span><span class="sxs-lookup"><span data-stu-id="fec7c-116">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="fec7c-117">SKAPA EXTERN TABELL</span><span class="sxs-lookup"><span data-stu-id="fec7c-117">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="fec7c-118">Skapa huvudnyckel för databasen omfång och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="fec7c-118">Create database scoped master key and credentials</span></span>
<span data-ttu-id="fec7c-119">hello autentiseringsuppgifter används av hello elastisk frågan tooconnect tooyour fjärranslutna databaser.</span><span class="sxs-lookup"><span data-stu-id="fec7c-119">hello credential is used by hello elastic query tooconnect tooyour remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="fec7c-120">Se till att hello `<username>` innehåller inte några **”@servername”** suffix.</span><span class="sxs-lookup"><span data-stu-id="fec7c-120">Ensure that hello `<username>` does not include any **"@servername"** suffix.</span></span> 
>

## <a name="create-external-data-sources"></a><span data-ttu-id="fec7c-121">Skapa externa datakällor</span><span class="sxs-lookup"><span data-stu-id="fec7c-121">Create external data sources</span></span>
<span data-ttu-id="fec7c-122">Syntax:</span><span class="sxs-lookup"><span data-stu-id="fec7c-122">Syntax:</span></span>

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> <span data-ttu-id="fec7c-123">hello typparameter måste anges för**RDBMS**.</span><span class="sxs-lookup"><span data-stu-id="fec7c-123">hello TYPE parameter must be set too**RDBMS**.</span></span> 
>

### <a name="example"></a><span data-ttu-id="fec7c-124">Exempel</span><span class="sxs-lookup"><span data-stu-id="fec7c-124">Example</span></span>
<span data-ttu-id="fec7c-125">hello visar följande exempel hello användningen av hello instruktionen CREATE för externa datakällor.</span><span class="sxs-lookup"><span data-stu-id="fec7c-125">hello following example illustrates hello use of hello CREATE statement for external data sources.</span></span> 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

<span data-ttu-id="fec7c-126">tooretrieve hello lista över aktuella externa datakällor:</span><span class="sxs-lookup"><span data-stu-id="fec7c-126">tooretrieve hello list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a><span data-ttu-id="fec7c-127">Externa tabeller</span><span class="sxs-lookup"><span data-stu-id="fec7c-127">External Tables</span></span>
<span data-ttu-id="fec7c-128">Syntax:</span><span class="sxs-lookup"><span data-stu-id="fec7c-128">Syntax:</span></span>

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a><span data-ttu-id="fec7c-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="fec7c-129">Example</span></span>
    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

<span data-ttu-id="fec7c-130">hello som följande exempel visar hur tooretrieve hello lista med externa tabeller från databasen för hello:</span><span class="sxs-lookup"><span data-stu-id="fec7c-130">hello following example shows how tooretrieve hello list of external tables from hello current database:</span></span> 

    select * from sys.external_tables; 

### <a name="remarks"></a><span data-ttu-id="fec7c-131">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="fec7c-131">Remarks</span></span>
<span data-ttu-id="fec7c-132">Elastisk frågan utökar hello befintliga extern tabell syntax toodefine externa tabeller som använder externa datakällor av typen RDBMS.</span><span class="sxs-lookup"><span data-stu-id="fec7c-132">Elastic query extends hello existing external table syntax toodefine external tables that use external data sources of type RDBMS.</span></span> <span data-ttu-id="fec7c-133">En extern tabelldefinition för vertikal partitionering omfattar följande aspekter hello:</span><span class="sxs-lookup"><span data-stu-id="fec7c-133">An external table definition for vertical partitioning covers hello following aspects:</span></span> 

* <span data-ttu-id="fec7c-134">**Schemat**: hello extern tabell DDL definierar ett schema som kan använda för dina frågor.</span><span class="sxs-lookup"><span data-stu-id="fec7c-134">**Schema**: hello external table DDL defines a schema that your queries can use.</span></span> <span data-ttu-id="fec7c-135">hello schema i den externa tabelldefinitionen måste toomatch hello schemat för hello tabeller i hello fjärrdatabasen där hello faktiska data lagras.</span><span class="sxs-lookup"><span data-stu-id="fec7c-135">hello schema provided in your external table definition needs toomatch hello schema of hello tables in hello remote database where hello actual data is stored.</span></span> 
* <span data-ttu-id="fec7c-136">**Fjärrdatabasen referens**: hello externa DDL-tabellen hänvisar tooan extern datakälla.</span><span class="sxs-lookup"><span data-stu-id="fec7c-136">**Remote database reference**: hello external table DDL refers tooan external data source.</span></span> <span data-ttu-id="fec7c-137">hello extern datakälla anger hello logiska servernamnet och databasnamnet på hello fjärrdatabasen där hello faktiska tabelldata lagras.</span><span class="sxs-lookup"><span data-stu-id="fec7c-137">hello external data source specifies hello logical server name and database name of hello remote database where hello actual table data is stored.</span></span> 

<span data-ttu-id="fec7c-138">Med hjälp av en extern datakälla som beskrivs i föregående avsnitt i hello, hello syntax toocreate externa tabeller är följande:</span><span class="sxs-lookup"><span data-stu-id="fec7c-138">Using an external data source as outlined in hello previous section, hello syntax toocreate external tables is as follows:</span></span> 

<span data-ttu-id="fec7c-139">Hej DATA_SOURCE satsen definierar hello extern datakälla (d.v.s. hello fjärrdatabasen vid vertikal partitionering) som används för hello extern tabell.</span><span class="sxs-lookup"><span data-stu-id="fec7c-139">hello DATA_SOURCE clause defines hello external data source (i.e. hello remote database in case of vertical partitioning) that is used for hello external table.</span></span>  

<span data-ttu-id="fec7c-140">Hej SCHEMA_NAME och OBJECT_NAME satser ger respektive hello möjlighet toomap hello extern tabell definition tooa tabell i ett annat schema för hello fjärrdatabasen eller tooa tabell med ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="fec7c-140">hello SCHEMA_NAME and OBJECT_NAME clauses provide hello ability toomap hello external table definition tooa table in a different schema on hello remote database, or tooa table with a different name, respectively.</span></span> <span data-ttu-id="fec7c-141">Detta är användbart om du vill toodefine en extern tabell tooa katalogvyn eller DMV på fjärrdatabasen- eller någon annan situation där hello fjärrtabell namn är upptaget lokalt.</span><span class="sxs-lookup"><span data-stu-id="fec7c-141">This is useful if you want toodefine an external table tooa catalog view or DMV on your remote database - or any other situation where hello remote table name is already taken locally.</span></span>  

<span data-ttu-id="fec7c-142">hello utelämnar följande DDL-instruktion en externa tabelldefinitionen från hello lokala katalog.</span><span class="sxs-lookup"><span data-stu-id="fec7c-142">hello following DDL statement drops an existing external table definition from hello local catalog.</span></span> <span data-ttu-id="fec7c-143">Det påverkar inte hello fjärrdatabasen.</span><span class="sxs-lookup"><span data-stu-id="fec7c-143">It does not impact hello remote database.</span></span> 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

<span data-ttu-id="fec7c-144">**Behörighet för CREATE/DROP extern tabell**: ALTER ANY extern DATAKÄLLA behövs för extern tabell DDL som också behövs toorefer toohello underliggande data.</span><span class="sxs-lookup"><span data-stu-id="fec7c-144">**Permissions for CREATE/DROP EXTERNAL TABLE**: ALTER ANY EXTERNAL DATA SOURCE permissions are needed for external table DDL which is also needed toorefer toohello underlying data source.</span></span>  

## <a name="security-considerations"></a><span data-ttu-id="fec7c-145">Säkerhetsöverväganden</span><span class="sxs-lookup"><span data-stu-id="fec7c-145">Security considerations</span></span>
<span data-ttu-id="fec7c-146">Användare med åtkomst toohello extern tabell få automatiskt åtkomst toohello underliggande fjärrtabeller under hello autentiseringsuppgifter som anges i hello externa definitionen av datakällan.</span><span class="sxs-lookup"><span data-stu-id="fec7c-146">Users with access toohello external table automatically gain access toohello underlying remote tables under hello credential given in hello external data source definition.</span></span> <span data-ttu-id="fec7c-147">Du bör noggrant hantera åtkomst toohello extern tabell i ordning tooavoid områdena höjning av privilegier genom hello autentiseringsuppgifterna för hello extern datakälla.</span><span class="sxs-lookup"><span data-stu-id="fec7c-147">You should carefully manage access toohello external table in order tooavoid undesired elevation of privileges through hello credential of hello external data source.</span></span> <span data-ttu-id="fec7c-148">Vanlig SQL behörigheter kan vara används tooGRANT eller ÅTERKALLA åtkomst tooan extern tabell precis som om det vore en vanlig tabell.</span><span class="sxs-lookup"><span data-stu-id="fec7c-148">Regular SQL permissions can be used tooGRANT or REVOKE access tooan external table just as though it were a regular table.</span></span>  

## <a name="example-querying-vertically-partitioned-databases"></a><span data-ttu-id="fec7c-149">Exempel: frågar lodrätt partitionerad databaser</span><span class="sxs-lookup"><span data-stu-id="fec7c-149">Example: querying vertically partitioned databases</span></span>
<span data-ttu-id="fec7c-150">hello utför följande fråga en trevägs koppling mellan hello två lokala tabeller för rader och order och hello fjärrtabell för kunder.</span><span class="sxs-lookup"><span data-stu-id="fec7c-150">hello following query performs a three-way join between hello two local tables for orders and order lines and hello remote table for customers.</span></span> <span data-ttu-id="fec7c-151">Detta är ett exempel på hello referens data användningsfall för elastiska frågan:</span><span class="sxs-lookup"><span data-stu-id="fec7c-151">This is an example of hello reference data use case for elastic query:</span></span> 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="fec7c-152">Lagrade proceduren för fjärrkörning av T-SQL: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="fec7c-152">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="fec7c-153">Elastisk frågan introducerar också en lagrad procedur som ger direktåtkomst toohello delar.</span><span class="sxs-lookup"><span data-stu-id="fec7c-153">Elastic query also introduces a stored procedure that provides direct access toohello shards.</span></span> <span data-ttu-id="fec7c-154">hello lagrade proceduren anropas [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) och kan vara används tooexecute remote lagrade procedurer eller T-SQL-kod på fjärranslutna hello-databaser.</span><span class="sxs-lookup"><span data-stu-id="fec7c-154">hello stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used tooexecute remote stored procedures or T-SQL code on hello remote databases.</span></span> <span data-ttu-id="fec7c-155">Det tar hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="fec7c-155">It takes hello following parameters:</span></span> 

* <span data-ttu-id="fec7c-156">Namn på datakälla (nvarchar): hello namnet på hello extern datakälla av typen RDBMS.</span><span class="sxs-lookup"><span data-stu-id="fec7c-156">Data source name (nvarchar): hello name of hello external data source of type RDBMS.</span></span> 
* <span data-ttu-id="fec7c-157">Fråga (nvarchar): hello T-SQL-fråga toobe utförs på varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="fec7c-157">Query (nvarchar): hello T-SQL query toobe executed on each shard.</span></span> 
* <span data-ttu-id="fec7c-158">Parameterdeklaration (nvarchar) - valfritt: strängen med datatypdefinitioner för hello parametrar som används i hello frågeparameter (till exempel sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="fec7c-158">Parameter declaration (nvarchar) - optional: String with data type definitions for hello parameters used in hello Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="fec7c-159">Värdet parameterlista - valfritt: kommaavgränsad lista över parametervärden (till exempel sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="fec7c-159">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="fec7c-160">hello sp\_köra\_fjärråtkomst använder hello extern datakälla i hello anrop parametrar tooexecute hello ges T-SQL-instruktion för fjärranslutna hello-databaser.</span><span class="sxs-lookup"><span data-stu-id="fec7c-160">hello sp\_execute\_remote uses hello external data source provided in hello invocation parameters tooexecute hello given T-SQL statement on hello remote databases.</span></span> <span data-ttu-id="fec7c-161">Den använder hello autentiseringsuppgifterna för hello externa data tooconnect toohello shardmap manager källdatabasen och hello fjärranslutna databaser.</span><span class="sxs-lookup"><span data-stu-id="fec7c-161">It uses hello credential of hello external data source tooconnect toohello shardmap manager database and hello remote databases.</span></span>  

<span data-ttu-id="fec7c-162">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fec7c-162">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 



## <a name="connectivity-for-tools"></a><span data-ttu-id="fec7c-163">Anslutning för verktyg</span><span class="sxs-lookup"><span data-stu-id="fec7c-163">Connectivity for tools</span></span>
<span data-ttu-id="fec7c-164">Du kan använda vanliga SQL Server-anslutningen strängar tooconnect din BI och data integration verktyg toodatabases på hello SQL DB-server som har elastisk fråga aktiverad och externa tabeller har definierats.</span><span class="sxs-lookup"><span data-stu-id="fec7c-164">You can use regular SQL Server connection strings tooconnect your BI and data integration tools toodatabases on hello SQL DB server that has elastic query enabled and external tables defined.</span></span> <span data-ttu-id="fec7c-165">Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver.</span><span class="sxs-lookup"><span data-stu-id="fec7c-165">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="fec7c-166">Referera toohello elastisk fråga databas och dess externa tabeller precis som alla andra SQL Server-databas som du ska ansluta toowith verktyget du behöver.</span><span class="sxs-lookup"><span data-stu-id="fec7c-166">Then refer toohello elastic query database and its external tables just like any other SQL Server database that you would connect toowith your tool.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="fec7c-167">Bästa praxis</span><span class="sxs-lookup"><span data-stu-id="fec7c-167">Best practices</span></span>
* <span data-ttu-id="fec7c-168">Kontrollera hello elastisk frågan endpoint databasen har fått åtkomst toohello fjärrdatabasen genom att aktivera åtkomst för Azure-tjänster i dess brandväggskonfigurationen för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="fec7c-168">Ensure that hello elastic query endpoint database has been given access toohello remote database by enabling access for Azure Services in its SQL DB firewall configuration.</span></span> <span data-ttu-id="fec7c-169">Se också till att hello autentiseringsuppgifter tillhandahålls i hello externa data definition av datakällan kan logga in hello fjärrdatabasen och har hello behörigheter tooaccess hello fjärrtabell.</span><span class="sxs-lookup"><span data-stu-id="fec7c-169">Also ensure that hello credential provided in hello external data source definition can successfully log into hello remote database and has hello permissions tooaccess hello remote table.</span></span>  
* <span data-ttu-id="fec7c-170">Elastisk frågan fungerar bäst för frågor där de flesta av hello beräkningsresurser kan göras på fjärranslutna hello-databaser.</span><span class="sxs-lookup"><span data-stu-id="fec7c-170">Elastic query works best for queries where most of hello computation can be done on hello remote databases.</span></span> <span data-ttu-id="fec7c-171">Du får vanligtvis hello bästa frågeprestanda med selektiv filter-predikat som kan utvärderas på hello fjärranslutna databaser eller kopplingar som kan utföras helt på hello fjärrdatabasen.</span><span class="sxs-lookup"><span data-stu-id="fec7c-171">You typically get hello best query performance with selective filter predicates that can be evaluated on hello remote databases or joins that can be performed completely on hello remote database.</span></span> <span data-ttu-id="fec7c-172">Andra frågemönster behöva tooload stora mängder data från hello fjärrdatabasen och kan fungera dåligt.</span><span class="sxs-lookup"><span data-stu-id="fec7c-172">Other query patterns may need tooload large amounts of data from hello remote database and may perform poorly.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fec7c-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fec7c-173">Next steps</span></span>

* <span data-ttu-id="fec7c-174">En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fec7c-174">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="fec7c-175">Vertikal partitionering, finns [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="fec7c-175">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="fec7c-176">Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="fec7c-176">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="fec7c-177">Syntax och exempel frågor för vågrätt partitionerade finns [frågar vågrätt partitionerad data)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="fec7c-177">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="fec7c-178">Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="fec7c-178">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
