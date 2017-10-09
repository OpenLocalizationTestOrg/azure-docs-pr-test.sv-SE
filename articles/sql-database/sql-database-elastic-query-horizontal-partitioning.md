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
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Rapportering över databaser som skalats ut molnet (förhandsgranskning)
![Fråga på shards][1]

Delat databaser distribuera rader i en skaländras ut data nivå. hello-schemat är identiska för alla deltagande databaser, även kallat horisontell partitionering. Du kan skapa rapporter som omfattar alla databaser i en delat databas med en elastisk fråga.

En Snabbstart Se [rapportering över databaser som skalats ut molnet](sql-database-elastic-query-getting-started.md).

Icke-delat databaser finns [fråga över moln databaser med olika scheman](sql-database-elastic-query-vertical-partitioning.md). 

## <a name="prerequisites"></a>Krav
* Skapa en Fragmentera karta med hello klientbibliotek för elastisk databas. Se [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md). Eller Använd hello exempelapp i [Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).
* Du kan också se [migrera befintliga databaser tooscaled ut databaser](sql-database-elastic-convert-to-use-elastic-tools.md).
* hello användare måste ha behörigheten ALTER ANY extern DATAKÄLLA. Den här behörigheten ingår i hello ALTER DATABASE-behörighet.
* ALTER ANY extern DATAKÄLLA behörigheter är nödvändiga toorefer toohello underliggande datakällan.

## <a name="overview"></a>Översikt
De här uttrycken skapa hello metadata representation av shardade data-tier i hello elastisk fråga databas. 

1. [SKAPA HUVUDNYCKEL](https://msdn.microsoft.com/library/ms174382.aspx)
2. [SKAPA DATABASBEGRÄNSADE AUTENTISERINGSUPPGIFTER](https://msdn.microsoft.com/library/mt270260.aspx)
3. [SKAPA EXTERN DATAKÄLLA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [SKAPA EXTERN TABELL](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 Skapa huvudnyckel för databasen omfång och autentiseringsuppgifter
hello autentiseringsuppgifter används av hello elastisk frågan tooconnect tooyour fjärranslutna databaser.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Kontrollera att hello *”\<användarnamn\>”* innehåller inte några *”@servername”* suffix. 
> 
> 

## <a name="12-create-external-data-sources"></a>1.2 Skapa externa datakällor
Syntax:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Exempel
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

Hämta hello lista över aktuella externa datakällor: 

    select * from sys.external_data_sources; 

hello extern datakälla refererar till Fragmentera kartan. En elastisk fråga använder sedan hello extern datakälla och hello underliggande Fragmentera kartan tooenumerate hello databaser som ingår i hello datanivå. hello samma autentiseringsuppgifter är används tooread hello Fragmentera kartan och tooaccess hello data på hello shards under hello bearbetningen av en elastisk fråga. 

## <a name="13-create-external-tables"></a>1.3 skapa externa tabeller
Syntax:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Exempel**

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

Hämta hello lista med externa tabeller från databasen för hello: 

    SELECT * from sys.external_tables; 

toodrop externa tabeller:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Kommentarer
hello DATA\_SOURCE-satsen definierar hello extern datakälla (en Fragmentera karta) som används för hello extern tabell.  

hello schemat\_namn och OBJEKTET\_namn satser mappa hello extern tabell definition tooa tabell i ett annat schema. Om det utelämnas antas hello schemat för hello fjärrobjektet toobe ”dbo” och dess namn antas toobe identiska toohello externa tabellnamn som definieras. Detta är användbart om hello namnet på din fjärrtabell redan är upptaget i hello databasen där du vill att toocreate hello extern tabell. Till exempel du vill toodefine en extern tabell tooget en aggregerad vy över katalogvyer eller nivån av DMV: er på dina data som skalats ut. Eftersom katalogvyer och av DMV: er redan finns lokalt och kan inte du använda namnen för hello externa tabelldefinitionen. I stället använda ett annat namn och använda vyn hello-katalog eller hello DMVS namn i hello schemat\_namn och/eller OBJEKTET\_namn-satser. (Se hello exemplet nedan.) 

hello DISTRIBUTION satsen anger hello Datadistribution används för den här tabellen. hello frågeprocessorn använder hello informationen i hello DISTRIBUTION satsen toobuild hello effektivaste frågeplaner.  

1. **DELAT** innebär data vågrätt partitionerad över hello databaser. hello partitionering nyckel för hello Datadistribution är hello **< sharding_column_name >** parameter.
2. **REPLIKERADE** innebär att identiska kopior av hello tabellen finns på varje databas. Det är ditt ansvar tooensure hello repliker är identiska över hello databaser.
3. **AVRUNDAR\_ROBIN** innebär hello tabellen är partitionerad vågrätt med hjälp av en distributionsmetod för beroende program. 

**Data tjänstnivån referens**: hello externa DDL-tabellen hänvisar tooan extern datakälla. hello extern datakälla anger en Fragmentera karta som ger hello extern tabell med hello information behövs toolocate alla hello databaser i din datanivå. 

### <a name="security-considerations"></a>Säkerhetsöverväganden
Användare med åtkomst toohello extern tabell få automatiskt åtkomst toohello underliggande fjärrtabeller under hello autentiseringsuppgifter som anges i hello externa definitionen av datakällan. Undvik oönskad höjning av privilegier genom hello autentiseringsuppgifterna för hello extern datakälla. Använd GRANT eller REVOKE för en extern tabell precis som om det vore en vanlig tabell.  

När du har definierat den externa datakällan och externa tabeller kan använda du nu fullständig T-SQL via externa tabeller.

## <a name="example-querying-horizontal-partitioned-databases"></a>Exempel: frågar vågräta partitionerade databaser
hello följande fråga utför en trevägs koppling mellan lager order rader, och och använder flera aggregeringar och selektiv filter. Det förutsätts att (1) horisontell partitionering (delning) och (2) att lager order rader, och är delat av hello datalager id-kolumnen hello elastisk frågan kan samordna relaterade hello kopplingar på hello shards och bearbeta hello dyra tillhör hello frågan på hello shards parallellt. 

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

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Lagrade proceduren för fjärrkörning av T-SQL: sp\_execute_remote
Elastisk frågan introducerar också en lagrad procedur som ger direktåtkomst toohello delar. hello lagrade proceduren anropas [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) och kan vara används tooexecute remote lagrade procedurer eller T-SQL-kod på fjärranslutna hello-databaser. Det tar hello följande parametrar: 

* Namn på datakälla (nvarchar): hello namnet på hello extern datakälla av typen RDBMS. 
* Fråga (nvarchar): hello T-SQL-fråga toobe utförs på varje Fragmentera. 
* Parameterdeklaration (nvarchar) - valfritt: strängen med datatypdefinitioner för hello parametrar som används i hello frågeparameter (till exempel sp_executesql). 
* Värdet parameterlista - valfritt: kommaavgränsad lista över parametervärden (till exempel sp_executesql).

hello sp\_köra\_fjärråtkomst använder hello extern datakälla i hello anrop parametrar tooexecute hello ges T-SQL-instruktion för fjärranslutna hello-databaser. Den använder hello autentiseringsuppgifterna för hello externa data tooconnect toohello shardmap manager källdatabasen och hello fjärranslutna databaser.  

Exempel: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Anslutning för verktyg
Använda reguljära SQL Server-anslutningen strängar tooconnect ditt program, data och BI integration verktyg toohello databasen med definitionerna extern tabell. Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver. Referera hello elastisk fråga databas som andra SQL Server database anslutna toohello verktyg och använda externa tabeller från verktyget eller program som om de vore lokala tabeller. 

## <a name="best-practices"></a>Bästa praxis
* Kontrollera hello elastisk frågan endpoint databasen har fått åtkomst toohello shardmap databasen och alla shards via hello SQL DB brandväggar.  
* Validera eller tillämpa hello Datadistribution definieras av hello extern tabell. Om din distribution för faktiska data skiljer sig från hello-distribution som anges i din tabelldefinitionen kan dina frågor ge oväntade resultat. 
* Elastisk frågan för närvarande inte att utföra Fragmentera eliminering när predikat över hello horisontell partitionering nyckeln skulle låta den toosafely undanta vissa delar från bearbetning.
* Elastisk frågan fungerar bäst för frågor där de flesta av hello beräkningsresurser kan göras på hello delar. Det uppstår vanligtvis hello bästa frågeprestanda med selektiv filter-predikat som kan utvärderas på hello shards eller kopplingar över hello partitionering nycklar som kan utföras på ett partitionsjusterade sätt på alla delar. Andra frågemönster behöva tooload stora mängder data från delar hello toohello huvudnod och kan fungera dåligt

## <a name="next-steps"></a>Nästa steg

* En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).
* Vertikal partitionering, finns [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).
* Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)
* Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).
* Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
