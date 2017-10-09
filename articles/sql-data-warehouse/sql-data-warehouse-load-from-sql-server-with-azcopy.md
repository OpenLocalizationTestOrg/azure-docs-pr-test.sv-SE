---
title: "aaaLoad data från SQL Server till Azure SQL Data Warehouse (PolyBase) | Microsoft Docs"
description: "Använder bcp tooexport data från SQL Server tooflat filer, AZCopy tooimport data tooAzure blobblagring och PolyBase tooingest hello data till Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4d42786a-fb28-43c9-9c3b-72d19c0ecc11
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 89e2a91bc97642e9fc18545cb802b42d8dc4ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Läs in data från SQL Server till Azure SQL Data Warehouse (AZCopy)
Använd bcp och AZCopy kommandoradsverktyg tooload data från SQL Server tooAzure blob storage. Använd sedan PolyBase eller Azure Data Factory tooload hello data till Azure SQL Data Warehouse. 

## <a name="prerequisites"></a>Krav
toostep igenom den här kursen behöver du:

* En SQL Data Warehouse-databas
* hello kommandoradsverktyget bcp installerat
* hello kommandoradsverktyget SQLCMD installerat

> [!NOTE]
> Du kan hämta verktygen bcp och sqlcmd för hello från hello [Microsoft Download Center][Microsoft Download Center].
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a>Importera data till SQL Data Warehouse
I kursen får du skapa en tabell i Azure SQL Data Warehouse och importera data till hello tabell.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Steg 1: Skapa en tabell i Azure SQL Data Warehouse
Använd sqlcmd toorun hello följande fråga toocreate en tabell på din instans från en kommandotolk:

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

> [!NOTE]
> Se [tabell översikt] [ Table Overview] eller [CREATE TABLE-syntax] [ CREATE TABLE syntax] för mer information om hur du skapar en tabell i SQL Data Warehouse och hello  alternativen i hello WITH-satsen.
> 
> 

### <a name="step-2-create-a-source-data-file"></a>Steg 2: Skapa en källdatafil
Öppna Anteckningar och kopiera hello följande rader med data i en ny textfil och sedan spara den här filen tooyour lokala temp-katalog, C:\Temp\DimDate2.txt.

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

> [!NOTE]
> Det är viktigt tooremember att bcp.exe inte har stöd för hello UTF-8 Filkodning. Använd ASCII-filer eller UTF-16-kodade filer när du använder bcp.exe.
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a>Steg 3: Anslut och importera hello data
Med bcp kan du ansluta och importera hello data med hjälp av följande kommando ersätter hello värdena med lämpliga sådana hello:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Du kan verifiera hello data har lästs in genom att köra följande fråga med sqlcmd hello:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Detta bör returnera hello följande resultat:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Steg 4: Skapa statistik på dina nyinlästa data
SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik. I ordning tooget hello bästa prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data. En detaljerad förklaring av statistik finns hello [statistik] [ Statistics] -avsnittet i hello ämnesgruppen. Nedan visas ett enkelt exempel på hur toocreate statistik hello fram läses in i det här exemplet

Kör följande CREATE STATISTICS-uttryck från en sqlcmd-kommandotolk hello:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Exportera data från SQL Data Warehouse
I de här självstudierna kommer du att skapa en datafil från en tabell i SQL Data Warehouse. Vi kommer att exportera hello data vi skapade ovan tooa ny datafil som heter DimDate2_export.txt.

### <a name="step-1-export-hello-data"></a>Steg 1: Exportera hello data
Använda hello bcp-verktyget kan du ansluta och exportera data med hjälp av följande kommando ersätter hello värdena med lämpliga sådana hello:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Du kan kontrollera hello data exporterats korrekt genom att öppna nya hello-filen. hello data i hello filen bör matcha hello texten nedan:

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

> [!NOTE]
> På grund av toohello hur distribuerade system kanske inte hello dataordning hello samma över SQL Data Warehouse-databaser. Ett annat alternativ är toouse hello **queryout** funktionen i bcp toowrite en fråga extrakt istället exportera hello hela tabellen.
> 
> 

## <a name="next-steps"></a>Nästa steg
Se [Läs in data till SQL Data Warehouse][Load data into SQL Data Warehouse] för en översikt över inläsning.
För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].

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
