---
title: aaaCreate ett SQL Data Warehouse med TSQL | Microsoft Docs
description: "Lär dig hur toocreate en Azure SQL Data Warehouse med TSQL"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 81ef59a66c61452ff8a2aca29837f155e87d017d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Skapa en SQL Data Warehouse-databas med hjälp av Transact-SQL (TSQL)
> [!div class="op_single_selector"]
> * [Azure-portalen](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Den här artikeln visar hur toocreate en SQL Data Warehouse med hjälp av T-SQL.

## <a name="prerequisites"></a>Krav
tooget igång, behöver du:

* **Azure-konto**: Besök [kostnadsfri utvärderingsversion av Azure] [ Azure Free Trial] eller [MSDN Azure-krediter] [ MSDN Azure Credits] toocreate ett konto.
* **Azure SQL-servern**: Se [skapa en logisk Azure SQL Database-server med hello Azure-portalen] [skapa en logisk Azure SQL Database-server med hello Azure-portalen] eller [skapa en logisk Azure SQL Database-server med PowerShell] [skapa en Azure SQL Logisk Database-server med PowerShell] för mer information.
* **Resursgruppen**: antingen använda hello samma resurs grupp som din Azure SQL-server eller se [hur toocreate en resursgrupp][how toocreate a resource group].
* **Miljö tooexecute T-SQL**: du kan använda [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], eller [SSMS] [ SSMS] tooexecute T-SQL.

> [!NOTE]
> Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.  Mer information om priser finns i [Priser för SQL Data Warehouse][SQL Data Warehouse pricing].
>
>

## <a name="create-a-database-with-visual-studio"></a>Skapa en databas med Visual Studio
Om du är ny tooVisual Studio finns i artikeln hello [frågan Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].  toostart, Öppna SQL Server Object Explorer i Visual Studio och Anslut toohello-server som är värd för SQL Data Warehouse-databas.  När du är ansluten, kan du skapa ett SQL Data Warehouse genom att köra följande SQL-kommando mot hello hello **master** databas.  Det här kommandot skapar hello databasen MySqlDwDb med en Tjänstmålet DW400 och Tillåt hello databasen toogrow tooa maximal storlek på 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Skapa en databas med sqlcmd
Du kan också köra hello samma kommando med sqlcmd genom att köra hello följande i Kommandotolken.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

hello Standardsortering när inte anges är SQL_Latin1_General_CP1_CI_AS för sortering.  Hej `MAXSIZE` kan vara mellan 250 GB och 240 TB.  Hej `SERVICE_OBJECTIVE` kan vara mellan DW100 och DW2000 [DWU][DWU].  En lista över alla giltiga värden finns hello MSDN-dokumentationen för [CREATE DATABASE][CREATE DATABASE].  Både hello MAXSIZE och SERVICE_OBJECTIVE kan ändras med en [ALTER DATABASE] [ ALTER DATABASE] T-SQL-kommandot.  hello sorteringen för en databas kan inte ändras när den har skapats.   Varning ska användas när ändra hello SERVICE_OBJECTIVE ändras DWU orsakar en omstart av tjänster som avbryter alla frågor som rör sig.  En ändring av MAXSIZE startar inte om tjänsterna eftersom det bara är en enkel metadata-åtgärd.

## <a name="next-steps"></a>Nästa steg
När ditt SQL Data Warehouse är färdigetablerat, kan du [läsa in exempeldata] [ load sample data] eller kolla hur för[utveckla][develop], [ladda][load], eller [migrera][migrate].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[how toocreate a SQL Data Warehouse from hello Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL database with hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
