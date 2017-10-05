---
title: "Återställa en Azure SQL Data Warehouse (PowerShell) | Microsoft Docs"
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
ms.openlocfilehash: 6286c0e682bae2d3bf0435a25b8077a53b117b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="a9444-103">Återställa en Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="a9444-103">Restore an Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="a9444-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="a9444-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="a9444-105">[Portal][Portal]</span><span class="sxs-lookup"><span data-stu-id="a9444-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="a9444-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="a9444-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="a9444-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="a9444-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="a9444-108">I den här artikeln får du lära dig hur du återställer en Azure SQL Data Warehouse med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a9444-108">In this article you will learn how to restore an Azure SQL Data Warehouse using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a9444-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a9444-109">Before you begin</span></span>
<span data-ttu-id="a9444-110">**Kontrollera DTU-kapacitet.**</span><span class="sxs-lookup"><span data-stu-id="a9444-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="a9444-111">Varje SQL Data Warehouse finns på en SQLServer (t.ex. myserver.database.windows.net) som har en standard DTU-kvot.</span><span class="sxs-lookup"><span data-stu-id="a9444-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="a9444-112">Innan du kan återställa en SQL Data Warehouse, kontrollera att den SQL-servern har tillräckligt med återstående DTU-kvot för databasen som återställs.</span><span class="sxs-lookup"><span data-stu-id="a9444-112">Before you can restore a SQL Data Warehouse, verify that the your SQL server has enough remaining DTU quota for the database being restored.</span></span> <span data-ttu-id="a9444-113">Information om hur du beräkna DTU behövs eller begära mer DTU finns [begär en ändring för DTU-kvot][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="a9444-113">To learn how to calculate DTU needed or to request more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="a9444-114">Installera PowerShell</span><span class="sxs-lookup"><span data-stu-id="a9444-114">Install PowerShell</span></span>
<span data-ttu-id="a9444-115">För att kunna använda Azure PowerShell med SQL Data Warehouse, behöver du installera Azure PowerShell version 1.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a9444-115">In order to use Azure PowerShell with SQL Data Warehouse, you will need to install Azure PowerShell version 1.0 or greater.</span></span>  <span data-ttu-id="a9444-116">Du kan kontrollera din version genom att köra **Get-Module - ListAvailable-Name AzureRM**.</span><span class="sxs-lookup"><span data-stu-id="a9444-116">You can check your version by running **Get-Module -ListAvailable -Name AzureRM**.</span></span>  <span data-ttu-id="a9444-117">Den senaste versionen kan installeras från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="a9444-117">The latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="a9444-118">Mer information om hur du installerar den senaste versionen finns i [Installera och konfigurera Azure PowerShell][How to install and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="a9444-118">For more information on installing the latest version, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="a9444-119">Återställa en databas för aktiva eller pausats</span><span class="sxs-lookup"><span data-stu-id="a9444-119">Restore an active or paused database</span></span>
<span data-ttu-id="a9444-120">Att återställa en databas från en ögonblicksbild används den [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9444-120">To restore a database from a snapshot use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] PowerShell cmdlet.</span></span>

1. <span data-ttu-id="a9444-121">Öppna Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a9444-121">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="a9444-122">Lista över alla prenumerationer som är kopplade till ditt konto och ansluta till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a9444-122">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="a9444-123">Välj den prenumeration som innehåller databasen som ska återställas.</span><span class="sxs-lookup"><span data-stu-id="a9444-123">Select the subscription that contains the database to be restored.</span></span>
4. <span data-ttu-id="a9444-124">Ange återställningspunkter för databasen.</span><span class="sxs-lookup"><span data-stu-id="a9444-124">List the restore points for the database.</span></span>
5. <span data-ttu-id="a9444-125">Välj önskad återställningspunkt med hjälp av RestorePointCreationDate.</span><span class="sxs-lookup"><span data-stu-id="a9444-125">Pick the desired restore point using the RestorePointCreationDate.</span></span>
6. <span data-ttu-id="a9444-126">Återställa databasen till önskad återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="a9444-126">Restore the database to the desired restore point.</span></span>
7. <span data-ttu-id="a9444-127">Kontrollera att den återställda databasen är online.</span><span class="sxs-lookup"><span data-stu-id="a9444-127">Verify that the restored database is online.</span></span>

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> <span data-ttu-id="a9444-128">När återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="a9444-128">After the restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="a9444-129">Återställa en borttagen databas</span><span class="sxs-lookup"><span data-stu-id="a9444-129">Restore a deleted database</span></span>
<span data-ttu-id="a9444-130">Använd för att återställa en borttagen databas i [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9444-130">To restore a deleted database, use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="a9444-131">Öppna Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a9444-131">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="a9444-132">Lista över alla prenumerationer som är kopplade till ditt konto och ansluta till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a9444-132">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="a9444-133">Välj den prenumeration som innehåller den borttagna databasen återställs.</span><span class="sxs-lookup"><span data-stu-id="a9444-133">Select the subscription that contains the deleted database to be restored.</span></span>
4. <span data-ttu-id="a9444-134">Hämta specifika borttagen databas.</span><span class="sxs-lookup"><span data-stu-id="a9444-134">Get the specific deleted database.</span></span>
5. <span data-ttu-id="a9444-135">Återställa borttagna databasen.</span><span class="sxs-lookup"><span data-stu-id="a9444-135">Restore the deleted database.</span></span>
6. <span data-ttu-id="a9444-136">Kontrollera att den återställda databasen är online.</span><span class="sxs-lookup"><span data-stu-id="a9444-136">Verify that the restored database is online.</span></span>

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="a9444-137">När återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="a9444-137">After the restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a><span data-ttu-id="a9444-138">Återställa från en Azure-geografisk region</span><span class="sxs-lookup"><span data-stu-id="a9444-138">Restore from an Azure geographical region</span></span>
<span data-ttu-id="a9444-139">Om du vill återställa en databas kan använda den [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9444-139">To recover a database, use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="a9444-140">Öppna Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a9444-140">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="a9444-141">Lista över alla prenumerationer som är kopplade till ditt konto och ansluta till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a9444-141">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="a9444-142">Välj den prenumeration som innehåller databasen som ska återställas.</span><span class="sxs-lookup"><span data-stu-id="a9444-142">Select the subscription that contains the database to be restored.</span></span>
4. <span data-ttu-id="a9444-143">Hämta databasen som du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="a9444-143">Get the database you want to recover.</span></span>
5. <span data-ttu-id="a9444-144">Skapa återställningspunkt-begäran för databasen.</span><span class="sxs-lookup"><span data-stu-id="a9444-144">Create the recovery request for the database.</span></span>
6. <span data-ttu-id="a9444-145">Kontrollera statusen för geo-återställa databasen.</span><span class="sxs-lookup"><span data-stu-id="a9444-145">Verify the status of the geo-restored database.</span></span>

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="a9444-146">För att konfigurera databasen efter återställningen har slutförts, se [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="a9444-146">To configure your database after the restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

<span data-ttu-id="a9444-147">Den återställda databasen kommer att TDE-aktiverade om källdatabasen är TDE-aktiverad.</span><span class="sxs-lookup"><span data-stu-id="a9444-147">The recovered database will be TDE-enabled if the source database is TDE-enabled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9444-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9444-148">Next steps</span></span>
<span data-ttu-id="a9444-149">Mer information om funktionerna för verksamhetskontinuitet Azure SQL Database version, Läs den [översikt över verksamhetskontinuitet i Azure SQL Database][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="a9444-149">To learn about the business continuity features of Azure SQL Database editions, please read the [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
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
