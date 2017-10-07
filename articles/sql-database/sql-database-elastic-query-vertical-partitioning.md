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
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Fråga på molnet databaser med olika scheman (förhandsgranskning)
![Fråga på tabeller i olika databaser][1]

Lodrätt partitionerad-databaser använder olika uppsättningar med tabeller på olika databaser. Det innebär att hello schema skiljer sig på olika databaser. Till exempel finns alla tabeller för lager på en databas när alla redovisning-relaterade tabeller är på en andra databas. 

## <a name="prerequisites"></a>Krav
* hello användare måste ha behörigheten ALTER ANY extern DATAKÄLLA. Den här behörigheten ingår i hello ALTER DATABASE-behörighet.
* ALTER ANY extern DATAKÄLLA behörigheter är nödvändiga toorefer toohello underliggande datakällan.

## <a name="overview"></a>Översikt

> [!NOTE]
> Till skillnad från med horisontell partitionering är dessa DDL-satser inte beroende definierar en datanivå med en Fragmentera karta via hello klientbibliotek för elastisk databas.
>

1. [SKAPA HUVUDNYCKEL](https://msdn.microsoft.com/library/ms174382.aspx)
2. [SKAPA DATABASBEGRÄNSADE AUTENTISERINGSUPPGIFTER](https://msdn.microsoft.com/library/mt270260.aspx)
3. [SKAPA EXTERN DATAKÄLLA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [SKAPA EXTERN TABELL](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>Skapa huvudnyckel för databasen omfång och autentiseringsuppgifter
hello autentiseringsuppgifter används av hello elastisk frågan tooconnect tooyour fjärranslutna databaser.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Se till att hello `<username>` innehåller inte några **”@servername”** suffix. 
>

## <a name="create-external-data-sources"></a>Skapa externa datakällor
Syntax:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> hello typparameter måste anges för**RDBMS**. 
>

### <a name="example"></a>Exempel
hello visar följande exempel hello användningen av hello instruktionen CREATE för externa datakällor. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

tooretrieve hello lista över aktuella externa datakällor: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externa tabeller
Syntax:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Exempel
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

hello som följande exempel visar hur tooretrieve hello lista med externa tabeller från databasen för hello: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Kommentarer
Elastisk frågan utökar hello befintliga extern tabell syntax toodefine externa tabeller som använder externa datakällor av typen RDBMS. En extern tabelldefinition för vertikal partitionering omfattar följande aspekter hello: 

* **Schemat**: hello extern tabell DDL definierar ett schema som kan använda för dina frågor. hello schema i den externa tabelldefinitionen måste toomatch hello schemat för hello tabeller i hello fjärrdatabasen där hello faktiska data lagras. 
* **Fjärrdatabasen referens**: hello externa DDL-tabellen hänvisar tooan extern datakälla. hello extern datakälla anger hello logiska servernamnet och databasnamnet på hello fjärrdatabasen där hello faktiska tabelldata lagras. 

Med hjälp av en extern datakälla som beskrivs i föregående avsnitt i hello, hello syntax toocreate externa tabeller är följande: 

Hej DATA_SOURCE satsen definierar hello extern datakälla (d.v.s. hello fjärrdatabasen vid vertikal partitionering) som används för hello extern tabell.  

Hej SCHEMA_NAME och OBJECT_NAME satser ger respektive hello möjlighet toomap hello extern tabell definition tooa tabell i ett annat schema för hello fjärrdatabasen eller tooa tabell med ett annat namn. Detta är användbart om du vill toodefine en extern tabell tooa katalogvyn eller DMV på fjärrdatabasen- eller någon annan situation där hello fjärrtabell namn är upptaget lokalt.  

hello utelämnar följande DDL-instruktion en externa tabelldefinitionen från hello lokala katalog. Det påverkar inte hello fjärrdatabasen. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Behörighet för CREATE/DROP extern tabell**: ALTER ANY extern DATAKÄLLA behövs för extern tabell DDL som också behövs toorefer toohello underliggande data.  

## <a name="security-considerations"></a>Säkerhetsöverväganden
Användare med åtkomst toohello extern tabell få automatiskt åtkomst toohello underliggande fjärrtabeller under hello autentiseringsuppgifter som anges i hello externa definitionen av datakällan. Du bör noggrant hantera åtkomst toohello extern tabell i ordning tooavoid områdena höjning av privilegier genom hello autentiseringsuppgifterna för hello extern datakälla. Vanlig SQL behörigheter kan vara används tooGRANT eller ÅTERKALLA åtkomst tooan extern tabell precis som om det vore en vanlig tabell.  

## <a name="example-querying-vertically-partitioned-databases"></a>Exempel: frågar lodrätt partitionerad databaser
hello utför följande fråga en trevägs koppling mellan hello två lokala tabeller för rader och order och hello fjärrtabell för kunder. Detta är ett exempel på hello referens data användningsfall för elastiska frågan: 

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
Du kan använda vanliga SQL Server-anslutningen strängar tooconnect din BI och data integration verktyg toodatabases på hello SQL DB-server som har elastisk fråga aktiverad och externa tabeller har definierats. Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver. Referera toohello elastisk fråga databas och dess externa tabeller precis som alla andra SQL Server-databas som du ska ansluta toowith verktyget du behöver. 

## <a name="best-practices"></a>Bästa praxis
* Kontrollera hello elastisk frågan endpoint databasen har fått åtkomst toohello fjärrdatabasen genom att aktivera åtkomst för Azure-tjänster i dess brandväggskonfigurationen för SQL-databas. Se också till att hello autentiseringsuppgifter tillhandahålls i hello externa data definition av datakällan kan logga in hello fjärrdatabasen och har hello behörigheter tooaccess hello fjärrtabell.  
* Elastisk frågan fungerar bäst för frågor där de flesta av hello beräkningsresurser kan göras på fjärranslutna hello-databaser. Du får vanligtvis hello bästa frågeprestanda med selektiv filter-predikat som kan utvärderas på hello fjärranslutna databaser eller kopplingar som kan utföras helt på hello fjärrdatabasen. Andra frågemönster behöva tooload stora mängder data från hello fjärrdatabasen och kan fungera dåligt. 

## <a name="next-steps"></a>Nästa steg

* En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).
* Vertikal partitionering, finns [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).
* Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).
* Syntax och exempel frågor för vågrätt partitionerade finns [frågar vågrätt partitionerad data)](sql-database-elastic-query-horizontal-partitioning.md)
* Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
