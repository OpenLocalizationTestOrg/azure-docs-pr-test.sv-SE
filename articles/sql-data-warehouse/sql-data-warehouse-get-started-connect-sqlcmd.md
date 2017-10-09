---
title: aaaConnect tooAzure SQL Data Warehouse sqlcmd | Microsoft Docs
description: "Använd [sqlcmd] [sqlcmd] kommandoradsverktyget tooconnect tooand fråga en Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 0334df7b969da1966ba29c97f835a2dc9e383e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sqlcmd"></a>Ansluta tooSQL Data Warehouse med sqlcmd
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Använd [sqlcmd] [ sqlcmd] kommandoradsverktyget tooconnect tooand fråga en Azure SQL Data Warehouse.  

## <a name="1-connect"></a>1. Anslut
tooget igång med [sqlcmd][sqlcmd], öppna hello-kommandotolk och ange **sqlcmd** följt av hello anslutningssträngen för SQL Data Warehouse-databas. hello anslutningssträngen kräver hello följande parametrar:

* **Server (-S):** Server i form av hello `<`servernamn`>`. database.windows.net
* **Databas (-d):** Databasens namn.
* **Aktivera identifierare inom citattecken (-I):** identifierare inom citattecken måste vara aktiverade tooconnect tooa SQL Data Warehouse-instans.

toouse SQL Server-autentisering, behöver du tooadd hello användarnamn/lösenord parametrar:

* **Användare (-U):** serveranvändare i formatet hello `<`användare`>`
* **Lösenord (-P):** lösenord som är associerat med hello användare.

Anslutningssträngen kan till exempel se ut hello följande:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

toouse Azure Active Directory-integrerad autentisering, behöver du tooadd hello Azure Active Directory parametrar:

* **Azure Active Directory-autentisering (-G):** använder Azure Active Directory för autentisering

Anslutningssträngen kan till exempel se ut hello följande:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Du behöver för[aktivera Azure Active Directory Authentication](sql-data-warehouse-authentication.md) tooauthenticate med Active Directory.
> 
> 

## <a name="2-query"></a>2. Fråga
Du kan utfärda alla stöds Transact-SQL-instruktioner mot hello instansen efter anslutning.  I det här exemplet skickas frågor i interaktivt läge.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Dessa nästa exempel visar hur du kan köra dina frågor i batchläge hello -Q alternativet eller skicka SQL-toosqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Nästa steg
Se [sqlcmd-dokumentationen] [ sqlcmd] mer information om information om hello alternativen i sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
