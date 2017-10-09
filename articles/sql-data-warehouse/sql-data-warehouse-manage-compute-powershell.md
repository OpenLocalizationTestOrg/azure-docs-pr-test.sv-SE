---
title: "aaaManage beräkningskraft i Azure SQL Data Warehouse (PowerShell) | Microsoft Docs"
description: "PowerShell uppgifter toomanage beräkningskraft. Skala beräkningsresurser genom att justera dwu: er. Eller, pausa och återuppta beräkning resurser toosave kostnader."
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
ms.openlocfilehash: 8b379d4cf89570649767f6896d2c630d4f1111d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="f6faa-105">Hantera datorkraft i Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="f6faa-105">Manage compute power in Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f6faa-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="f6faa-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="f6faa-107">Portal</span><span class="sxs-lookup"><span data-stu-id="f6faa-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="f6faa-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f6faa-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="f6faa-109">REST</span><span class="sxs-lookup"><span data-stu-id="f6faa-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="f6faa-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="f6faa-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a><span data-ttu-id="f6faa-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="f6faa-111">Before you begin</span></span>
### <a name="install-hello-latest-version-of-azure-powershell"></a><span data-ttu-id="f6faa-112">Installera hello senaste versionen av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f6faa-112">Install hello latest version of Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="f6faa-113">toouse Azure PowerShell med SQL Data Warehouse, behöver du Azure PowerShell version 1.0.3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f6faa-113">toouse Azure PowerShell with SQL Data Warehouse, you need Azure PowerShell version 1.0.3 or greater.</span></span>  <span data-ttu-id="f6faa-114">tooverify din nuvarande version kör hello kommando **Get-Module - ListAvailable-Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="f6faa-114">tooverify your current version run hello command **Get-Module -ListAvailable -Name Azure**.</span></span> <span data-ttu-id="f6faa-115">Du kan installera hello senaste versionen från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="f6faa-115">You can install hello latest version from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="f6faa-116">Mer information finns i [hur tooinstall och konfigurera Azure PowerShell][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="f6faa-116">For more information, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a><span data-ttu-id="f6faa-117">Kom igång med Azure PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="f6faa-117">Get started with Azure PowerShell cmdlets</span></span>
<span data-ttu-id="f6faa-118">tooget igång:</span><span class="sxs-lookup"><span data-stu-id="f6faa-118">tooget started:</span></span>

1. <span data-ttu-id="f6faa-119">Öppna Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f6faa-119">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="f6faa-120">I PowerShell-Kommandotolken hello köra dessa kommandon toosign i toohello Azure Resource Manager och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f6faa-120">At hello PowerShell prompt, run these commands toosign in toohello Azure Resource Manager and select your subscription.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a><span data-ttu-id="f6faa-121">Skala datorkraft</span><span class="sxs-lookup"><span data-stu-id="f6faa-121">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="f6faa-122">toochange hello dwu: er, använda hello [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f6faa-122">toochange hello DWUs, use hello [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase] PowerShell cmdlet.</span></span> <span data-ttu-id="f6faa-123">hello följande exempel anger hello service nivån mål tooDW1000 för hello databasen MySQLDW som finns på servern MinServer.</span><span class="sxs-lookup"><span data-stu-id="f6faa-123">hello following example sets hello service level objective tooDW1000 for hello database MySQLDW which is hosted on server MyServer.</span></span>

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="f6faa-124">Pausa beräkning</span><span class="sxs-lookup"><span data-stu-id="f6faa-124">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="f6faa-125">toopause en databas kan använda hello [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f6faa-125">toopause a database, use hello [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase] cmdlet.</span></span> <span data-ttu-id="f6faa-126">hello pausar följande exempel en databas med namnet Database02 som finns på en server med namnet Server01.</span><span class="sxs-lookup"><span data-stu-id="f6faa-126">hello following example pauses a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="f6faa-127">hello-servern heter i en Azure-resursgrupp ResourceGroup1.</span><span class="sxs-lookup"><span data-stu-id="f6faa-127">hello server is in an Azure resource group named ResourceGroup1.</span></span>

> [!NOTE]
> <span data-ttu-id="f6faa-128">Observera att om servern är foo.database.windows.net, Använd ”foo” som hello - servernamn i hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="f6faa-128">Note that if your server is foo.database.windows.net, use "foo" as hello -ServerName in hello PowerShell cmdlets.</span></span>
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
<span data-ttu-id="f6faa-129">En variant exemplet nästa hämtar hello databasen till hello $database objekt.</span><span class="sxs-lookup"><span data-stu-id="f6faa-129">A variation, this next example retrieves hello database into hello $database object.</span></span> <span data-ttu-id="f6faa-130">Den sedan kommer hello-objekt för[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="f6faa-130">It then pipes hello object too[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span></span> <span data-ttu-id="f6faa-131">hello resultat lagras i hello objektet resultDatabase.</span><span class="sxs-lookup"><span data-stu-id="f6faa-131">hello results are stored in hello object resultDatabase.</span></span> <span data-ttu-id="f6faa-132">hello kommandot visar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="f6faa-132">hello final command shows hello results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="f6faa-133">Återuppta beräkning</span><span class="sxs-lookup"><span data-stu-id="f6faa-133">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="f6faa-134">toostart en databas kan använda hello [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f6faa-134">toostart a database, use hello [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] cmdlet.</span></span> <span data-ttu-id="f6faa-135">hello startar följande exempel en databas med namnet Database02 som finns på en server med namnet Server01.</span><span class="sxs-lookup"><span data-stu-id="f6faa-135">hello following example starts a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="f6faa-136">hello-servern heter i en Azure-resursgrupp ResourceGroup1.</span><span class="sxs-lookup"><span data-stu-id="f6faa-136">hello server is in an Azure resource group named ResourceGroup1.</span></span>

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

<span data-ttu-id="f6faa-137">En variant exemplet nästa hämtar hello databasen till hello $database objekt.</span><span class="sxs-lookup"><span data-stu-id="f6faa-137">A variation, this next example retrieves hello database into hello $database object.</span></span> <span data-ttu-id="f6faa-138">Den sedan kommer hello-objekt för[Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] och lagrar hello resultat i $resultDatabase.</span><span class="sxs-lookup"><span data-stu-id="f6faa-138">It then pipes hello object too[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] and stores hello results in $resultDatabase.</span></span> <span data-ttu-id="f6faa-139">hello kommandot visar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="f6faa-139">hello final command shows hello results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="f6faa-140">Kontrollera databasens status</span><span class="sxs-lookup"><span data-stu-id="f6faa-140">Check database state</span></span>

<span data-ttu-id="f6faa-141">Enligt hello exemplen ovan, kan använda [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] cmdlet tooget information på en databas, vilket kontrollerar hello status, men också toouse som ett argument.</span><span class="sxs-lookup"><span data-stu-id="f6faa-141">As shown in hello above examples, one can use [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase] cmdlet tooget information on a database, thereby checking hello status, but also toouse as an argument.</span></span> 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

<span data-ttu-id="f6faa-142">Som kommer att resultera i något att</span><span class="sxs-lookup"><span data-stu-id="f6faa-142">Which will result in something like</span></span> 

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

<span data-ttu-id="f6faa-143">Där du kan sedan kontrollera toosee hello *Status* av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-143">Where you can then check toosee hello *Status* of hello database.</span></span> <span data-ttu-id="f6faa-144">I det här fallet kan du se att den här databasen är online.</span><span class="sxs-lookup"><span data-stu-id="f6faa-144">In this case, you can see that this database is online.</span></span> 

<span data-ttu-id="f6faa-145">När du kör det här kommandot får statusvärdet antingen Online, pausa, inställd på återupptar, skalning och pausad.</span><span class="sxs-lookup"><span data-stu-id="f6faa-145">When you run this command, you should receive a Status value of either Online, Pausing, Resuming, Scaling, and Paused.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="f6faa-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f6faa-146">Next steps</span></span>
<span data-ttu-id="f6faa-147">Andra hanteringsåtgärder finns [översikt över][Management overview].</span><span class="sxs-lookup"><span data-stu-id="f6faa-147">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
