---
title: "Läs in data från CSV-fil till Azure SQL Database (bcp) | Microsoft Docs"
description: "För mindre datastorlekar används bcp för att importera data till Azure SQL Database."
services: sql-database
documentationcenter: NA
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 875f9b8d-f1a1-4895-b717-f45570fb7f80
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 2f00a740b3cc59c4e7b3b378c06cfa8cb05e8380
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a>Läsa in data från CSV till Azure SQL Database (flat-filer)
Du kan använda kommandoradsverktyget bcp för att importera data från en CSV-fil till Azure SQL Database.

## <a name="before-you-begin"></a>Innan du börjar
### <a name="prerequisites"></a>Krav
Du behöver följande för att slutföra stegen i den här artikeln:

* Skapa en logisk Azure SQL Database-server och -databas
* Kommandoradsverktyget bcp installerat
* Kommandoradsverktyget sqlcmd installerat

Du kan hämta verktygen bcp och sqlcmd från [Microsoft Download Center][Microsoft Download Center].

### <a name="data-in-ascii-or-utf-16-format"></a>Data i ASCII- eller UTF-16-format
Om du använder egna data i självstudierna, måste de använda sig av ASCII- eller UTF-16-kodning eftersom bcp inte stöder UTF-8. 

## <a name="1-create-a-destination-table"></a>1. Skapa en måltabell
Definiera en tabell i SQL Database som måltabellen. Kolumnerna i tabellen måste motsvara data i varje rad i din datafil.

För att skapa en tabell, öppnar du en kommandotolk och använder sqlcmd.exe för att köra följande kommando:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a>2. Skapa en källdatafil
Öppna Anteckningar och kopiera följande datarader till en ny textfil. Spara sedan filen till din lokala temp-katalog, C:\Temp\DimDate2.txt. Den här datan är i ASCII-format.

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

(Valfritt) Om du vill exportera dina egna data från en SQL Server-databas, öppnar du en kommandotolk och kör följande kommando. Ersätt TableName, ServerName, DatabaseName, Username och Password med din egen information.

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-the-data"></a>3. Läs in data
För att läsa in data, öppnar du en kommandotolk och kör följande kommando, där du ersätter värdena för servernamn, databasnamn, användarnamn och lösenord med din egen information.

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

Använd det här kommandot för att verifiera att data har lästs in korrekt

```bcp
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Resultatet borde se ut så här:

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

## <a name="next-steps"></a>Nästa steg
Om du vill migrera en SQL Server-databas hittar du mer information i [SQL Server-databasmigrering](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
