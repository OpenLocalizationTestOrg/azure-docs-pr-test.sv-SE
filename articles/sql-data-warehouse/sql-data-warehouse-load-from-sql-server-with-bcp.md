---
title: "aaaLoad data från SQL Server till Azure SQL Data Warehouse (bcp) | Microsoft Docs"
description: "För små datastorlekar, använder du bcp tooexport data från SQL Server tooflat filer och importera hello data direkt i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: a03b5403d123e8814ae73a7cce8e6851c8b264a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Läs in data från SQL Server till Azure SQL Data Warehouse (flat-filer)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Du kan använda hello bcp kommandoradsverktyget tooexport data från SQL Server och läsa in den direkt tooAzure SQL Data Warehouse för små datauppsättningar.

I de här självstudierna kommer du att använda bcp för att:

* Exportera en tabell från från SQL Server med hjälp av hello bcp out-kommandot (eller skapa en enkel exempelfil)
* Importera hello tabell från en flat fil tooSQL Data Warehouse.
* Skapa statistik på hello läsa in data.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a>Innan du börjar
### <a name="prerequisites"></a>Krav
toostep igenom den här kursen behöver du:

* En SQL Data Warehouse-databas
* hello kommandoradsverktyget BCP installerat
* hello kommandoradsverktyget sqlcmd installerat

Du kan hämta verktygen bcp och sqlcmd för hello från hello [Microsoft Download Center][Microsoft Download Center].

### <a name="data-in-ascii-or-utf-16-format"></a>Data i ASCII- eller UTF-16-format
Om du provar den här självstudiekursen med dina egna data behöver data toouse hello ASCII- eller UTF-16-kodning eftersom bcp inte stöder UTF-8. 

PolyBase stöder UTF-8 men har inte än stöd för UTF-16. Observera att om du vill toocombine bcp med PolyBase måste tootransform hello data tooUTF-8 efter att de exporterats från SQL Server. 

## <a name="1-create-a-destination-table"></a>1. Skapa en måltabell
Definiera en tabell i SQL Data Warehouse som kommer att hello måltabell för hello belastningen. hello kolumner i tabellen hello måste överensstämma med toohello data i varje rad i datafilen.

toocreate en tabell, öppna en kommandotolk och använder sqlcmd.exe toorun hello följande kommando:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a>2. Skapa en källdatafil
Öppna Anteckningar och kopiera hello följande rader med data i en ny textfil och sedan spara den här filen tooyour lokala temp-katalog, C:\Temp\DimDate2.txt. Den här datan är i ASCII-format.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Valfritt) tooexport dina egna data från en SQL Server-databas, öppna en kommandotolk och kör följande kommando hello. Ersätt TableName, ServerName, DatabaseName, Username och Password med din egen information.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-hello-data"></a>3. Läs in hello data
tooload hello data, öppna en kommandotolk och kör hello följande kommando, ersätter hello värdena för servernamn, databasen namn, användarnamn och lösenord med din egen information.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Använd det här kommandot tooverify hello data har lästs in korrekt

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

hello resultat bör se ut så här:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

## <a name="4-create-statistics"></a>4. Skapa statistik
SQL Data Warehouse stöder inte automatiskt skapande eller uppdatering av statistik ännu. tooget hello bästa frågeprestanda, det är viktigt toocreate statistik på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data. En detaljerad förklaring av statistik finns [statistik][Statistics]. 

Kör följande kommando toocreate statistik på din nya inlästa tabell hello.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. Exportera data från SQL Data Warehouse
För skojs skull kan du exportera hello data som du just läste in, tillbaks ut ur SQL Data Warehouse.  hello kommandot tooexport är exakt hello samma som för att exportera från SQLServer.

Det är dock något annorlunda hello resultat. Eftersom hello data lagras på distribuerade platser inom SQL Data Warehouse när du exporterar data skriver varje Compute-nod den data toohello utdatafilen. hello är hello data i hello utdatafil sannolikt toobe annat än hello hello data i hello indatafilen.

### <a name="export-a-table-and-compare-exported-results"></a>Exportera en tabell och jämför exporterade resultat
toosee hello exporterade data, öppna en kommandotolk och kör det här kommandot med dina egna parametrar. ServerName är hello namnet på din logiska Azure SQL Server.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Du kan kontrollera hello data exporterats korrekt genom att öppna nya hello-filen. hello data i hello filen bör matcha hello texten nedan, men kommer antagligen vara sorterad i en annan ordning:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-hello-results-of-a-query"></a>Exportera hello resultatet av en fråga
Du kan använda hello **queryout** funktionen i bcp tooexport hello resultatet av en fråga istället för att exportera hello hela tabellen. 

## <a name="next-steps"></a>Nästa steg
Se [Läs in data till SQL Data Warehouse][Load data into SQL Data Warehouse] för en översikt över inläsning.
För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].
Se [tabell översikt] [ Table Overview] eller [CREATE TABLE-syntax] [ CREATE TABLE syntax] för mer information om hur du skapar en tabell i SQL Data Warehouse.

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
