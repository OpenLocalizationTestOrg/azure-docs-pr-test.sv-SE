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
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="66d5d-103">Återställa en Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="66d5d-103">Restore an Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="66d5d-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="66d5d-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="66d5d-105">[Portal][Portal]</span><span class="sxs-lookup"><span data-stu-id="66d5d-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="66d5d-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="66d5d-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="66d5d-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="66d5d-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="66d5d-108">I den här artikeln du lära dig hur toorestore en Azure SQL Data Warehouse med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="66d5d-108">In this article you will learn how toorestore an Azure SQL Data Warehouse using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="66d5d-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="66d5d-109">Before you begin</span></span>
<span data-ttu-id="66d5d-110">**Kontrollera DTU-kapacitet.**</span><span class="sxs-lookup"><span data-stu-id="66d5d-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="66d5d-111">Varje SQL Data Warehouse finns på en SQLServer (t.ex. myserver.database.windows.net) som har en standard DTU-kvot.</span><span class="sxs-lookup"><span data-stu-id="66d5d-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="66d5d-112">Innan du kan återställa en SQL Data Warehouse, kontrollera att hello din SQLServer har tillräckligt återstående DTU-kvot för hello databasen som återställs.</span><span class="sxs-lookup"><span data-stu-id="66d5d-112">Before you can restore a SQL Data Warehouse, verify that hello your SQL server has enough remaining DTU quota for hello database being restored.</span></span> <span data-ttu-id="66d5d-113">toolearn hur toocalculate DTU behövs eller toorequest mer DTU finns [begär en ändring för DTU-kvot][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="66d5d-113">toolearn how toocalculate DTU needed or toorequest more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="66d5d-114">Installera PowerShell</span><span class="sxs-lookup"><span data-stu-id="66d5d-114">Install PowerShell</span></span>
<span data-ttu-id="66d5d-115">I ordning toouse Azure PowerShell med SQL Data Warehouse behöver du tooinstall Azure PowerShell version 1.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="66d5d-115">In order toouse Azure PowerShell with SQL Data Warehouse, you will need tooinstall Azure PowerShell version 1.0 or greater.</span></span>  <span data-ttu-id="66d5d-116">Du kan kontrollera din version genom att köra **Get-Module - ListAvailable-Name AzureRM**.</span><span class="sxs-lookup"><span data-stu-id="66d5d-116">You can check your version by running **Get-Module -ListAvailable -Name AzureRM**.</span></span>  <span data-ttu-id="66d5d-117">hello senaste versionen kan installeras från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="66d5d-117">hello latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="66d5d-118">Mer information om hur du installerar hello senaste versionen finns [hur tooinstall och konfigurera Azure PowerShell][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="66d5d-118">For more information on installing hello latest version, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="66d5d-119">Återställa en databas för aktiva eller pausats</span><span class="sxs-lookup"><span data-stu-id="66d5d-119">Restore an active or paused database</span></span>
<span data-ttu-id="66d5d-120">toorestore en databas från en ögonblicksbild använda hello [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="66d5d-120">toorestore a database from a snapshot use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] PowerShell cmdlet.</span></span>

1. <span data-ttu-id="66d5d-121">Öppna Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="66d5d-121">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="66d5d-122">Lista över alla hello prenumerationer som är kopplade till ditt konto och ansluta tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="66d5d-122">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="66d5d-123">Välj hello-abonnemang som innehåller hello databasen toobe återställs.</span><span class="sxs-lookup"><span data-stu-id="66d5d-123">Select hello subscription that contains hello database toobe restored.</span></span>
4. <span data-ttu-id="66d5d-124">Lista hello Återställningspunkter för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="66d5d-124">List hello restore points for hello database.</span></span>
5. <span data-ttu-id="66d5d-125">Välj hello önskad återställningspunkt med hjälp av hello RestorePointCreationDate.</span><span class="sxs-lookup"><span data-stu-id="66d5d-125">Pick hello desired restore point using hello RestorePointCreationDate.</span></span>
6. <span data-ttu-id="66d5d-126">Återställa hello databasen toohello önskad återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="66d5d-126">Restore hello database toohello desired restore point.</span></span>
7. <span data-ttu-id="66d5d-127">Kontrollera att hello återställa databasen är online.</span><span class="sxs-lookup"><span data-stu-id="66d5d-127">Verify that hello restored database is online.</span></span>

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
> <span data-ttu-id="66d5d-128">När hello återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="66d5d-128">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="66d5d-129">Återställa en borttagen databas</span><span class="sxs-lookup"><span data-stu-id="66d5d-129">Restore a deleted database</span></span>
<span data-ttu-id="66d5d-130">toorestore en borttagen databas använda hello [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.</span><span class="sxs-lookup"><span data-stu-id="66d5d-130">toorestore a deleted database, use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="66d5d-131">Öppna Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="66d5d-131">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="66d5d-132">Lista över alla hello prenumerationer som är kopplade till ditt konto och ansluta tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="66d5d-132">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="66d5d-133">Välj hello-abonnemang som innehåller hello bort databasen toobe återställs.</span><span class="sxs-lookup"><span data-stu-id="66d5d-133">Select hello subscription that contains hello deleted database toobe restored.</span></span>
4. <span data-ttu-id="66d5d-134">Hämta hello specifika bort databasen.</span><span class="sxs-lookup"><span data-stu-id="66d5d-134">Get hello specific deleted database.</span></span>
5. <span data-ttu-id="66d5d-135">Återställa databasen hello tas bort.</span><span class="sxs-lookup"><span data-stu-id="66d5d-135">Restore hello deleted database.</span></span>
6. <span data-ttu-id="66d5d-136">Kontrollera att hello återställa databasen är online.</span><span class="sxs-lookup"><span data-stu-id="66d5d-136">Verify that hello restored database is online.</span></span>

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
> <span data-ttu-id="66d5d-137">När hello återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="66d5d-137">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a><span data-ttu-id="66d5d-138">Återställa från en Azure-geografisk region</span><span class="sxs-lookup"><span data-stu-id="66d5d-138">Restore from an Azure geographical region</span></span>
<span data-ttu-id="66d5d-139">toorecover en databas kan använda hello [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.</span><span class="sxs-lookup"><span data-stu-id="66d5d-139">toorecover a database, use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="66d5d-140">Öppna Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="66d5d-140">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="66d5d-141">Lista över alla hello prenumerationer som är kopplade till ditt konto och ansluta tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="66d5d-141">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="66d5d-142">Välj hello-abonnemang som innehåller hello databasen toobe återställs.</span><span class="sxs-lookup"><span data-stu-id="66d5d-142">Select hello subscription that contains hello database toobe restored.</span></span>
4. <span data-ttu-id="66d5d-143">Hämta hello-databas som du vill toorecover.</span><span class="sxs-lookup"><span data-stu-id="66d5d-143">Get hello database you want toorecover.</span></span>
5. <span data-ttu-id="66d5d-144">Skapa hello återställning för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="66d5d-144">Create hello recovery request for hello database.</span></span>
6. <span data-ttu-id="66d5d-145">Kontrollera hello status för hello geo återställa databasen.</span><span class="sxs-lookup"><span data-stu-id="66d5d-145">Verify hello status of hello geo-restored database.</span></span>

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
> <span data-ttu-id="66d5d-146">tooconfigure databasen efter hello återställningen har slutförts, se [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="66d5d-146">tooconfigure your database after hello restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

<span data-ttu-id="66d5d-147">hello återställda databasen kommer att TDE-aktiverade om hello källdatabasen är TDE-aktiverad.</span><span class="sxs-lookup"><span data-stu-id="66d5d-147">hello recovered database will be TDE-enabled if hello source database is TDE-enabled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66d5d-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66d5d-148">Next steps</span></span>
<span data-ttu-id="66d5d-149">Läs toolearn om funktionerna för verksamhetskontinuitet hello Azure SQL Database version hello [översikt över verksamhetskontinuitet i Azure SQL Database][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="66d5d-149">toolearn about hello business continuity features of Azure SQL Database editions, please read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

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
