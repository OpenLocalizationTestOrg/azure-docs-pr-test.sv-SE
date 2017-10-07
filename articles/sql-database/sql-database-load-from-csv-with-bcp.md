---
title: "aaaLoad data från CSV-filen till Azure SQL Database (bcp) | Microsoft Docs"
description: "För små datastorlekar, använder du bcp tooimport data till Azure SQL Database."
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
ms.workload: data-services
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 9350e459aa844223820fbbd849a830cf0354d4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a>Läsa in data från CSV till Azure SQL Database (flat-filer)
Du kan använda hello bcp kommandoradsverktyget tooimport data från en CSV-fil till Azure SQL Database.

## <a name="before-you-begin"></a>Innan du börjar
### <a name="prerequisites"></a>Krav
toocomplete hello stegen i den här artikeln, måste du:

* Skapa en logisk Azure SQL Database-server och -databas
* hello kommandoradsverktyget BCP installerat
* hello kommandoradsverktyget sqlcmd installerat

Du kan hämta verktygen bcp och sqlcmd för hello från hello [Microsoft Download Center][Microsoft Download Center].

### <a name="data-in-ascii-or-utf-16-format"></a>Data i ASCII- eller UTF-16-format
Om du provar den här självstudiekursen med dina egna data behöver data toouse hello ASCII- eller UTF-16-kodning eftersom bcp inte stöder UTF-8. 

## <a name="1-create-a-destination-table"></a>1. Skapa en måltabell
Definiera en tabell i SQL-databas som hello måltabellen. hello kolumner i tabellen hello måste överensstämma med toohello data i varje rad i datafilen.

toocreate en tabell, öppna en kommandotolk och använder sqlcmd.exe toorun hello följande kommando:

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

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-hello-data"></a>3. Läs in hello data
tooload hello data, öppna en kommandotolk och kör hello följande kommando, ersätter hello värdena för servernamn, databasen namn, användarnamn och lösenord med din egen information.

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

Använd det här kommandot tooverify hello data har lästs in korrekt

```bcp
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

## <a name="next-steps"></a>Nästa steg
toomigrate en SQL Server-databas finns [SQL Server-Databasmigrering](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
