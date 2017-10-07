---
title: aaaRestore en Azure SQL Data Warehouse (PowerShell) | Microsoft Docs
description: "PowerShell-uppgifter för att återställa en Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: ac62f154-c8b0-4c33-9c42-f480808aa1d2
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: aa29a315080b1ed477cc6a051ce15a3202630cfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Återställa en Azure SQL Data Warehouse (PowerShell)
> [!div class="op_single_selector"]
> * [Översikt över][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

I den här artikeln du lära dig hur toorestore en Azure SQL Data Warehouse med hjälp av PowerShell.

## <a name="before-you-begin"></a>Innan du börjar
**Kontrollera DTU-kapacitet.** Varje SQL Data Warehouse finns på en SQLServer (t.ex. myserver.database.windows.net) som har en standard DTU-kvot.  Innan du kan återställa en SQL Data Warehouse, kontrollera att hello din SQLServer har tillräckligt återstående DTU-kvot för hello databasen som återställs. toolearn hur toocalculate DTU behövs eller toorequest mer DTU finns [begär en ändring för DTU-kvot][Request a DTU quota change].

### <a name="install-powershell"></a>Installera PowerShell
I ordning toouse Azure PowerShell med SQL Data Warehouse behöver du tooinstall Azure PowerShell version 1.0 eller senare.  Du kan kontrollera din version genom att köra **Get-Module - ListAvailable-Name AzureRM**.  hello senaste versionen kan installeras från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Mer information om hur du installerar hello senaste versionen finns [hur tooinstall och konfigurera Azure PowerShell][How tooinstall and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database"></a>Återställa en databas för aktiva eller pausats
toorestore en databas från en ögonblicksbild använda hello [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-cmdlet.

1. Öppna Windows PowerShell.
2. Lista över alla hello prenumerationer som är kopplade till ditt konto och ansluta tooyour Azure-konto.
3. Välj hello-abonnemang som innehåller hello databasen toobe återställs.
4. Lista hello Återställningspunkter för hello-databasen.
5. Välj hello önskad återställningspunkt med hjälp av hello RestorePointCreationDate.
6. Återställa hello databasen toohello önskad återställningspunkt.
7. Kontrollera att hello återställa databasen är online.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List hello last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get hello specific database toorestore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify hello status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> När hello återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Återställa en borttagen databas
toorestore en borttagen databas använda hello [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.

1. Öppna Windows PowerShell.
2. Lista över alla hello prenumerationer som är kopplade till ditt konto och ansluta tooyour Azure-konto.
3. Välj hello-abonnemang som innehåller hello bort databasen toobe återställs.
4. Hämta hello specifika bort databasen.
5. Återställa databasen hello tas bort.
6. Kontrollera att hello återställa databasen är online.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get hello deleted database toorestore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify hello status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> När hello återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>Återställa från en Azure-geografisk region
toorecover en databas kan använda hello [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.

1. Öppna Windows PowerShell.
2. Lista över alla hello prenumerationer som är kopplade till ditt konto och ansluta tooyour Azure-konto.
3. Välj hello-abonnemang som innehåller hello databasen toobe återställs.
4. Hämta hello-databas som du vill toorecover.
5. Skapa hello återställning för hello-databasen.
6. Kontrollera hello status för hello geo återställa databasen.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get hello database you want toorecover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that hello geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> tooconfigure databasen efter hello återställningen har slutförts, se [konfigurera databasen efter återställningen][Configure your database after recovery].
> 
> 

hello återställda databasen kommer att TDE-aktiverade om hello källdatabasen är TDE-aktiverad.

## <a name="next-steps"></a>Nästa steg
Läs toolearn om funktionerna för verksamhetskontinuitet hello Azure SQL Database version hello [översikt över verksamhetskontinuitet i Azure SQL Database][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
