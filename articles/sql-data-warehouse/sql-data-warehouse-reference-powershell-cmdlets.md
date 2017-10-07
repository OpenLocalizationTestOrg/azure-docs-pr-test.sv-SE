---
title: "aaaPowerShell-cmdlets för Azure SQL Data Warehouse"
description: "Hitta hello översta PowerShell-cmdlets för Azure SQL Data Warehouse hur toopause och återuppta en databas."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 6f0d5772-f05f-4cc8-9749-4adb153dfd50
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 84353b56131cf856e0724d338d7ed186fd2ceeaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-cmdletar och REST-API: er för SQL Data Warehouse
Många administrationsuppgifter för SQL Data Warehouse kan hanteras med hjälp av Azure PowerShell-cmdlets eller REST API: er.  Nedan följer några exempel på hur toouse PowerShell-kommandon tooautomate vanliga aktiviteter i ditt SQL Data Warehouse.  Några bra REST-exempel finns hello artikel [hantera skalbarhet med övriga][Manage scalability with REST].

> [!NOTE]
> I ordning toouse Azure PowerShell med SQL Data Warehouse, behöver du Azure PowerShell version 1.0.3 eller senare.  Du kan kontrollera din version genom att köra **Get-Module - ListAvailable-Name Azure**.  hello senaste versionen kan installeras från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Mer information om hur du installerar hello senaste versionen finns [hur tooinstall och konfigurera Azure PowerShell][How tooinstall and configure Azure PowerShell].
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Kom igång med Azure PowerShell-cmdlets
1. Öppna Windows PowerShell.
2. I PowerShell-Kommandotolken hello köra dessa kommandon toosign i toohello Azure Resource Manager och välja din prenumeration.
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Exempel på paus SQL Data Warehouse
Pausa en databas med namnet ”Database02” finns på en server med namnet ”Server01”.  hello-servern är i ett Azure-resursgrupp med namnet ”ResourceGroup1”.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
En variant det här exemplet kommer hello hämta objekt för[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].  Därför har hello databasen pausats. hello kommandot visar hello resultat.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Starta SQL Data Warehouse-exempel
Åtgärden återuppta av en databas med namnet ”Database02” finns på en server med namnet ”Server01”. hello-server ingår i en resursgrupp med namnet ”ResourceGroup1”.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

En variant det här exemplet hämtas en databas med namnet ”Database02” från en server med namnet ”Server01” som ingår i en resursgrupp med namnet ”ResourceGroup1”. Det kommer hello hämta objekt för[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Observera att om servern är foo.database.windows.net, Använd ”foo” som hello - servernamn i hello PowerShell-cmdlets.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Andra stöd för PowerShell-cmdlets
Dessa PowerShell cmdlets stöds med Azure SQL Data Warehouse.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [Ny AzureRmSqlDatabase][New-AzureRmSqlDatabase]
* [Ta bort-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [SELECT-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Pausa AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

## <a name="next-steps"></a>Nästa steg
Flera PowerShell-exemplen finns:

* [Skapa ett SQL Data Warehouse med hjälp av PowerShell][Create a SQL Data Warehouse using PowerShell]
* [Återställning av databasen][Database restore]

Andra uppgifter som kan automatiseras med PowerShell finns i [Cmdlets för Azure SQL Database][Azure SQL Database Cmdlets]. Observera att stöds inte alla Azure SQL Database-cmdlets för Azure SQL Data Warehouse.  En lista över aktiviteter som kan automatiseras med övriga, se [åtgärder för Azure SQL-databaser][Operations for Azure SQL Databases].

<!--Image references-->

<!--Article references-->
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points tooSelect-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
