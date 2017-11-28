---
title: Hantera datorkraft i Azure SQL Data Warehouse (PowerShell) | Microsoft Docs
description: "PowerShell-uppgifter för hantering av beräkningskraft. Skala beräkningsresurser genom att justera dwu: er. Eller, pausa och återuppta beräkningsresurser för att spara kostnader."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 6a185d96447c2e1b0b463439dd062081e783da5f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="0c2ed-105">Hantera datorkraft i Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="0c2ed-105">Manage compute power in Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0c2ed-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="0c2ed-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="0c2ed-107">Portal</span><span class="sxs-lookup"><span data-stu-id="0c2ed-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="0c2ed-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c2ed-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="0c2ed-109">REST</span><span class="sxs-lookup"><span data-stu-id="0c2ed-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="0c2ed-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="0c2ed-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a><span data-ttu-id="0c2ed-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0c2ed-111">Before you begin</span></span>
### <a name="install-the-latest-version-of-azure-powershell"></a><span data-ttu-id="0c2ed-112">Installera den senaste versionen av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c2ed-112">Install the latest version of Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="0c2ed-113">Om du vill använda Azure PowerShell med SQL Data Warehouse, behöver du Azure PowerShell version 1.0.3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-113">To use Azure PowerShell with SQL Data Warehouse, you need Azure PowerShell version 1.0.3 or greater.</span></span>  <span data-ttu-id="0c2ed-114">Kontrollera din nuvarande version kör kommandot **Get-Module - ListAvailable-Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-114">To verify your current version run the command **Get-Module -ListAvailable -Name Azure**.</span></span> <span data-ttu-id="0c2ed-115">Du kan installera den senaste versionen från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="0c2ed-115">You can install the latest version from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="0c2ed-116">Mer information finns i [hur du installerar och konfigurerar du Azure PowerShell][How to install and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="0c2ed-116">For more information, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a><span data-ttu-id="0c2ed-117">Kom igång med Azure PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="0c2ed-117">Get started with Azure PowerShell cmdlets</span></span>
<span data-ttu-id="0c2ed-118">Att komma igång:</span><span class="sxs-lookup"><span data-stu-id="0c2ed-118">To get started:</span></span>

1. <span data-ttu-id="0c2ed-119">Öppna Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-119">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="0c2ed-120">Köra dessa kommandon för att logga in till Azure Resource Manager och välja din prenumeration i PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-120">At the PowerShell prompt, run these commands to sign in to the Azure Resource Manager and select your subscription.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a><span data-ttu-id="0c2ed-121">Skala datorkraft</span><span class="sxs-lookup"><span data-stu-id="0c2ed-121">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="0c2ed-122">Du kan ändra de dwu: er i [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-122">To change the DWUs, use the [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase] PowerShell cmdlet.</span></span> <span data-ttu-id="0c2ed-123">I följande exempel anger servicenivåmålet till DW1000 för MySQLDW som finns på servern minserver-databasen.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-123">The following example sets the service level objective to DW1000 for the database MySQLDW which is hosted on server MyServer.</span></span>

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="0c2ed-124">Pausa beräkning</span><span class="sxs-lookup"><span data-stu-id="0c2ed-124">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="0c2ed-125">Pausa en databas genom att använda den [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-125">To pause a database, use the [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase] cmdlet.</span></span> <span data-ttu-id="0c2ed-126">Följande exempel pausar en databas med namnet Database02 som finns på en server med namnet Server01.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-126">The following example pauses a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="0c2ed-127">Servern är i ett Azure-resursgrupp med namnet ResourceGroup1.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-127">The server is in an Azure resource group named ResourceGroup1.</span></span>

> [!NOTE]
> <span data-ttu-id="0c2ed-128">Observera att om din server är foo.database.windows.net, använda ”foo” - servernamn i PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-128">Note that if your server is foo.database.windows.net, use "foo" as the -ServerName in the PowerShell cmdlets.</span></span>
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
<span data-ttu-id="0c2ed-129">En variant exemplet nästa hämtar databasen till $database-objekt.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-129">A variation, this next example retrieves the database into the $database object.</span></span> <span data-ttu-id="0c2ed-130">Den kommer sedan objektet till [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="0c2ed-130">It then pipes the object to [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span></span> <span data-ttu-id="0c2ed-131">Resultatet lagras i objektet resultDatabase.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-131">The results are stored in the object resultDatabase.</span></span> <span data-ttu-id="0c2ed-132">Kommandot visas resultatet.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-132">The final command shows the results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="0c2ed-133">Återuppta beräkning</span><span class="sxs-lookup"><span data-stu-id="0c2ed-133">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="0c2ed-134">Starta en databas med den [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-134">To start a database, use the [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] cmdlet.</span></span> <span data-ttu-id="0c2ed-135">Följande exempel startar en databas med namnet Database02 som finns på en server med namnet Server01.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-135">The following example starts a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="0c2ed-136">Servern är i ett Azure-resursgrupp med namnet ResourceGroup1.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-136">The server is in an Azure resource group named ResourceGroup1.</span></span>

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

<span data-ttu-id="0c2ed-137">En variant exemplet nästa hämtar databasen till $database-objekt.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-137">A variation, this next example retrieves the database into the $database object.</span></span> <span data-ttu-id="0c2ed-138">Den kommer sedan objektet till [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] och lagrar resultatet i $resultDatabase.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-138">It then pipes the object to [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] and stores the results in $resultDatabase.</span></span> <span data-ttu-id="0c2ed-139">Kommandot visas resultatet.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-139">The final command shows the results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="0c2ed-140">Kontrollera databasens status</span><span class="sxs-lookup"><span data-stu-id="0c2ed-140">Check database state</span></span>

<span data-ttu-id="0c2ed-141">I ovanstående exempel visas en kan använda [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] cmdlet om du vill ha information om en databas, vilket kontrollerar status, men ska användas som ett argument.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-141">As shown in the above examples, one can use [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase] cmdlet to get information on a database, thereby checking the status, but also to use as an argument.</span></span> 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

<span data-ttu-id="0c2ed-142">Som kommer att resultera i något att</span><span class="sxs-lookup"><span data-stu-id="0c2ed-142">Which will result in something like</span></span> 

```powershell   
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

<span data-ttu-id="0c2ed-143">Där du kan sedan kontrollera för att se den *Status* av databasen.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-143">Where you can then check to see the *Status* of the database.</span></span> <span data-ttu-id="0c2ed-144">I det här fallet kan du se att den här databasen är online.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-144">In this case, you can see that this database is online.</span></span> 

<span data-ttu-id="0c2ed-145">När du kör det här kommandot får statusvärdet antingen Online, pausa, inställd på återupptar, skalning och pausad.</span><span class="sxs-lookup"><span data-stu-id="0c2ed-145">When you run this command, you should receive a Status value of either Online, Pausing, Resuming, Scaling, and Paused.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="0c2ed-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c2ed-146">Next steps</span></span>
<span data-ttu-id="0c2ed-147">Andra hanteringsåtgärder finns [översikt över][Management overview].</span><span class="sxs-lookup"><span data-stu-id="0c2ed-147">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
