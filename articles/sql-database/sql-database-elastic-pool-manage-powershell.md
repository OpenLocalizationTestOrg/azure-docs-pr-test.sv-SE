---
title: 'PowerShell: Skapa och hantera en elastisk pool med Azure SQL | Microsoft Docs'
description: "Lär dig hur toouse PowerShell toomanage en elastisk pool."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 61289770-69b9-4ae3-9252-d0e94d709331
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 06/06/2017
ms.author: srinia
ms.openlocfilehash: 92de2a4b243dcc74502064e9d2c31682691753d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a><span data-ttu-id="a9e00-103">Skapa och hantera en elastisk pool med PowerShell</span><span class="sxs-lookup"><span data-stu-id="a9e00-103">Create and manage an elastic pool with PowerShell</span></span>
<span data-ttu-id="a9e00-104">Det här avsnittet beskrivs hur du toocreate och hantera skalbara [elastiska pooler](sql-database-elastic-pool.md) med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a9e00-104">This topic shows you how toocreate and manage scalable [elastic pools](sql-database-elastic-pool.md) with PowerShell.</span></span>  <span data-ttu-id="a9e00-105">Du kan också skapa och hantera en Azure-elastisk pool med hello [Azure-portalen](https://portal.azure.com/), REST-API eller [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="a9e00-105">You can also create and manage an Azure elastic pool using hello [Azure portal](https://portal.azure.com/), REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="a9e00-106">Du kan också skapa och flytta databaser till och från elastiska pooler med [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="a9e00-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a><span data-ttu-id="a9e00-107">Skapa en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-107">Create an elastic pool</span></span>
<span data-ttu-id="a9e00-108">Hej [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) cmdlet skapar en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-108">hello [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) cmdlet creates an elastic pool.</span></span> <span data-ttu-id="a9e00-109">hello värdena för eDTU per pool, min och max dtu: er är begränsad av hello tjänstnivåvärdet (Basic, Standard, Premium eller Premium RS).</span><span class="sxs-lookup"><span data-stu-id="a9e00-109">hello values for eDTU per pool, min, and max DTUs are constrained by hello service tier value (Basic, Standard, Premium, or Premium RS).</span></span> <span data-ttu-id="a9e00-110">Se [eDTU och lagringsgränser för elastiska pooler och grupperade databaser](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span><span class="sxs-lookup"><span data-stu-id="a9e00-110">See [eDTU and storage limits for elastic pools and pooled databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span></span>

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="a9e00-111">Skapa en delad databas i en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="a9e00-112">Använd hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet och ange hello **ElasticPoolName** målpool för parametern toohello.</span><span class="sxs-lookup"><span data-stu-id="a9e00-112">Use hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet and set hello **ElasticPoolName** parameter toohello target pool.</span></span> <span data-ttu-id="a9e00-113">toomove en befintlig databas i en elastisk pool, se [flytta en databas till en elastisk pool](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span><span class="sxs-lookup"><span data-stu-id="a9e00-113">toomove an existing database into an elastic pool, see [Move a database into an elastic pool](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span></span>

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a><span data-ttu-id="a9e00-114">Fullständigt skript</span><span class="sxs-lookup"><span data-stu-id="a9e00-114">Complete script</span></span>
<span data-ttu-id="a9e00-115">Det här skriptet skapar en Azure-resursgrupp och en server.</span><span class="sxs-lookup"><span data-stu-id="a9e00-115">This script creates an Azure resource group and a server.</span></span> <span data-ttu-id="a9e00-116">När du uppmanas ange ett administratörsanvändarnamn och lösenord för hello ny server (inte dina Azure-autentiseringsuppgifter).</span><span class="sxs-lookup"><span data-stu-id="a9e00-116">When prompted, supply an administrator username and password for hello new server (not your Azure credentials).</span></span>

```PowerShell
$subscriptionId = '<your Azure subscription id>'
$resourceGroupName = '<resource group name>'
$location = '<datacenter location>'
$serverName = '<server name>'
$poolName = '<pool name>'
$databaseName = '<database name>'

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB
```

## <a name="create-an-elastic-pool-and-add-multiple-pooled-databases"></a><span data-ttu-id="a9e00-117">Skapa en elastisk pool och lägga till flera grupperade databaser</span><span class="sxs-lookup"><span data-stu-id="a9e00-117">Create an elastic pool and add multiple pooled databases</span></span>
<span data-ttu-id="a9e00-118">Skapandet av många databaser i en elastisk pool kan ta tid när det görs med hello portal eller PowerShell-cmdletar som skapar bara en enskild databas åt gången.</span><span class="sxs-lookup"><span data-stu-id="a9e00-118">Creation of many databases in an elastic pool can take time when done using hello portal or PowerShell cmdlets that create only a single database at a time.</span></span> <span data-ttu-id="a9e00-119">tooautomate skapande i en elastisk pool, se [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span><span class="sxs-lookup"><span data-stu-id="a9e00-119">tooautomate creation into an elastic pool, see [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="a9e00-120">Flytta en databas till en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-120">Move a database into an elastic pool</span></span>
<span data-ttu-id="a9e00-121">Du kan flytta en databas till eller från en elastisk pool med hello [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span><span class="sxs-lookup"><span data-stu-id="a9e00-121">You can move a database into or out of an elastic pool with hello [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span></span>

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="a9e00-122">Ändra inställningar för prestanda för en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-122">Change performance settings of an elastic pool</span></span>
<span data-ttu-id="a9e00-123">När sjunker prestanda, kan du ändra inställningarna för hello hello poolen tooaccommodate tillväxt.</span><span class="sxs-lookup"><span data-stu-id="a9e00-123">When performance suffers, you can change hello settings of hello pool tooaccommodate growth.</span></span> <span data-ttu-id="a9e00-124">Använd hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9e00-124">Use hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet.</span></span> <span data-ttu-id="a9e00-125">Ange hello - Dtu parametern toohello edtu: er per pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-125">Set hello -Dtu parameter toohello eDTUs per pool.</span></span> <span data-ttu-id="a9e00-126">Se [eDTU och Lagringsgränser](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) för möjliga värden.</span><span class="sxs-lookup"><span data-stu-id="a9e00-126">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-hello-storage-limit-for-an-elastic-pool"></a><span data-ttu-id="a9e00-127">Ändra hello lagringsgränsen för en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-127">Change hello storage limit for an elastic pool</span></span>

<span data-ttu-id="a9e00-128">Använd hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet tooset hello _- StorageMB_ parameter.</span><span class="sxs-lookup"><span data-stu-id="a9e00-128">Use hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet tooset hello _-StorageMB_ parameter.</span></span> <span data-ttu-id="a9e00-129">Ange hello lagringsgränsen i MB (till exempel 2097152 anger hello lagring gränsen too2 TB).</span><span class="sxs-lookup"><span data-stu-id="a9e00-129">Provide hello storage limit in MB (for example, 2097152 sets hello storage limit too2 TB).</span></span> <span data-ttu-id="a9e00-130">Se [eDTU och Lagringsgränser](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) för möjliga värden.</span><span class="sxs-lookup"><span data-stu-id="a9e00-130">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9e00-131">Hej max data standardlagring per pool för Premium-pooler med 1500 edtu: er eller flera är 750 GB.</span><span class="sxs-lookup"><span data-stu-id="a9e00-131">hello default max data storage per pool for Premium pools with 1500 eDTUs or more is 750 GB.</span></span> <span data-ttu-id="a9e00-132">tooobtain hello högre _max data lagringsstorleken per pool_, hello lagringsgräns måste anges explicit.</span><span class="sxs-lookup"><span data-stu-id="a9e00-132">tooobtain hello higher _max data storage size per pool_, hello storage limit must be explicitly set.</span></span> <span data-ttu-id="a9e00-133">Premium-pooler med mer än 750 GB lagring är för närvarande i förhandsversion i hello följande områden: östra USA 2, västra USA, oss Gov Virginia, Västeuropa, Tyskland Central, Sydostasien, östra, Östra Australien, Kanada Central och Kanada Öst.</span><span class="sxs-lookup"><span data-stu-id="a9e00-133">Premium pools with more than 750 GB of storage is currently in public preview in hello following regions: East US 2, West US, US Gov Virginia, West Europe, Germany Central, Southeast Asia, Japan East, Australia East, Canada Central, and Canada East.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-hello-status-of-pool-operations"></a><span data-ttu-id="a9e00-134">Hämta hello status för pool-åtgärder</span><span class="sxs-lookup"><span data-stu-id="a9e00-134">Get hello status of pool operations</span></span>
<span data-ttu-id="a9e00-135">Skapa en elastisk pool kan ta tid.</span><span class="sxs-lookup"><span data-stu-id="a9e00-135">Creating an elastic pool can take time.</span></span> <span data-ttu-id="a9e00-136">tootrack hello status för poolen operations inklusive skapandet och uppdateringar, Använd hello [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9e00-136">tootrack hello status of pool operations including creation and updates, use hello [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-hello-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a><span data-ttu-id="a9e00-137">Hämta hello status för att flytta en databas till och från en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-137">Get hello status of moving a database into and out of an elastic pool</span></span>
<span data-ttu-id="a9e00-138">Flytta en databas kan ta tid.</span><span class="sxs-lookup"><span data-stu-id="a9e00-138">Moving a database can take time.</span></span> <span data-ttu-id="a9e00-139">Spåra en move-status med hello [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9e00-139">Track a move status using hello [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="a9e00-140">Hämta resursanvändningsdata för en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-140">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="a9e00-141">Mått som kan hämtas som en procentandel av hello resursgräns pool:</span><span class="sxs-lookup"><span data-stu-id="a9e00-141">Metrics that can be retrieved as a percentage of hello resource pool limit:</span></span>

| <span data-ttu-id="a9e00-142">Måttnamnet</span><span class="sxs-lookup"><span data-stu-id="a9e00-142">Metric name</span></span> | <span data-ttu-id="a9e00-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9e00-143">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a9e00-144">CPU\_procent</span><span class="sxs-lookup"><span data-stu-id="a9e00-144">cpu\_percent</span></span> |<span data-ttu-id="a9e00-145">Genomsnittlig beräkning användning i procent av hello gränsen för hello pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-145">Average compute utilization in percentage of hello limit of hello pool.</span></span> |
| <span data-ttu-id="a9e00-146">fysiska\_data\_läsa\_procent</span><span class="sxs-lookup"><span data-stu-id="a9e00-146">physical\_data\_read\_percent</span></span> |<span data-ttu-id="a9e00-147">Genomsnittlig i/o-användning i procent baserat på hello gränsen för hello pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-147">Average I/O utilization in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="a9e00-148">loggen\_skriva\_procent</span><span class="sxs-lookup"><span data-stu-id="a9e00-148">log\_write\_percent</span></span> |<span data-ttu-id="a9e00-149">Genomsnittlig skriva resursutnyttjande i procent av hello gränsen för hello pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-149">Average write resource utilization in percentage of hello limit of hello pool.</span></span> |
| <span data-ttu-id="a9e00-150">DTU\_förbrukning\_procent</span><span class="sxs-lookup"><span data-stu-id="a9e00-150">DTU\_consumption\_percent</span></span> |<span data-ttu-id="a9e00-151">Genomsnittlig eDTU-användning i procent av eDTU gränsen för hello poolen</span><span class="sxs-lookup"><span data-stu-id="a9e00-151">Average eDTU utilization in percentage of eDTU limit for hello pool</span></span> |
| <span data-ttu-id="a9e00-152">lagring\_procent</span><span class="sxs-lookup"><span data-stu-id="a9e00-152">storage\_percent</span></span> |<span data-ttu-id="a9e00-153">Genomsnittlig lagringsanvändningen i procent av hello lagringsgränsen för hello pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-153">Average storage utilization in percentage of hello storage limit of hello pool.</span></span> |
| <span data-ttu-id="a9e00-154">anställda\_procent</span><span class="sxs-lookup"><span data-stu-id="a9e00-154">workers\_percent</span></span> |<span data-ttu-id="a9e00-155">Maximal samtidiga arbetare (antal begäranden) i procent baserat på hello gränsen för hello pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-155">Maximum concurrent workers (requests) in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="a9e00-156">sessioner\_procent</span><span class="sxs-lookup"><span data-stu-id="a9e00-156">sessions\_percent</span></span> |<span data-ttu-id="a9e00-157">Maximalt antal samtidiga sessioner i procent baserat på hello gränsen för hello pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-157">Maximum concurrent sessions in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="a9e00-158">eDTU_limit</span><span class="sxs-lookup"><span data-stu-id="a9e00-158">eDTU_limit</span></span> |<span data-ttu-id="a9e00-159">Aktuell elastiska poolens DTU inställning för den här elastiska poolen under intervallet.</span><span class="sxs-lookup"><span data-stu-id="a9e00-159">Current max elastic pool DTU setting for this elastic pool during this interval.</span></span> |
| <span data-ttu-id="a9e00-160">lagring\_gräns</span><span class="sxs-lookup"><span data-stu-id="a9e00-160">storage\_limit</span></span> |<span data-ttu-id="a9e00-161">Aktuella elastiska poolens lagringsgräns inställningen för den här elastiska poolen i megabyte under intervallet.</span><span class="sxs-lookup"><span data-stu-id="a9e00-161">Current max elastic pool storage limit setting for this elastic pool in megabytes during this interval.</span></span> |
| <span data-ttu-id="a9e00-162">eDTU\_används</span><span class="sxs-lookup"><span data-stu-id="a9e00-162">eDTU\_used</span></span> |<span data-ttu-id="a9e00-163">Genomsnittlig edtu: er som används av hello-pool i det här intervallet.</span><span class="sxs-lookup"><span data-stu-id="a9e00-163">Average eDTUs used by hello pool in this interval.</span></span> |
| <span data-ttu-id="a9e00-164">lagring\_används</span><span class="sxs-lookup"><span data-stu-id="a9e00-164">storage\_used</span></span> |<span data-ttu-id="a9e00-165">Genomsnittlig lagringsutrymme som används av hello-pool i det här intervallet i byte</span><span class="sxs-lookup"><span data-stu-id="a9e00-165">Average storage used by hello pool in this interval in bytes</span></span> |

<span data-ttu-id="a9e00-166">**Mått granularitet/kvarhållningsperioder:**</span><span class="sxs-lookup"><span data-stu-id="a9e00-166">**Metrics granularity/retention periods:**</span></span>

* <span data-ttu-id="a9e00-167">Data returneras på 5 minuter granularitet.</span><span class="sxs-lookup"><span data-stu-id="a9e00-167">Data is returned at 5-minute granularity.</span></span>  
* <span data-ttu-id="a9e00-168">Datakvarhållning är 35 dagar.</span><span class="sxs-lookup"><span data-stu-id="a9e00-168">Data retention is 35 days.</span></span>  

<span data-ttu-id="a9e00-169">Denna cmdlet och API begränsar hello antalet rader som kan hämtas i ett anrop too1000 rader (cirka 3 dagar data på 5 minuter granularitet).</span><span class="sxs-lookup"><span data-stu-id="a9e00-169">This cmdlet and API limits hello number of rows that can be retrieved in one call too1000 rows (about 3 days of data at 5-minute granularity).</span></span> <span data-ttu-id="a9e00-170">Men det här kommandot kan anropas flera gånger med olika börja/sluta tid intervall tooretrieve mer data</span><span class="sxs-lookup"><span data-stu-id="a9e00-170">But this command can be called multiple times with different start/end time intervals tooretrieve more data</span></span>

<span data-ttu-id="a9e00-171">tooretrieve hello mått:</span><span class="sxs-lookup"><span data-stu-id="a9e00-171">tooretrieve hello metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a><span data-ttu-id="a9e00-172">Hämta resursanvändningsdata för en databas i en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-172">Get resource usage data for a database in an elastic pool</span></span>
<span data-ttu-id="a9e00-173">Dessa API: er är hello samma som hello API: er som används för att övervaka hello resursanvändningen för en enskild databas, förutom följande semantiska skillnaden hello: mått som hämtats är uttryckt i procent av hello per databas max edtu: er (eller motsvarande fjärrskrivbordsanslutning för hello Ange underliggande måttet som processor eller I/O) för poolen.</span><span class="sxs-lookup"><span data-stu-id="a9e00-173">These APIs are hello same as hello APIs used for monitoring hello resource utilization of a single database, except for hello following semantic difference: metrics retrieved are expressed as a percentage of hello per database max eDTUs (or equivalent cap for hello underlying metric like CPU or IO) set for that pool.</span></span> <span data-ttu-id="a9e00-174">Till exempel anger 50% användning av någon av de här måtten att hello resursanvändningen för specifika är vid 50% av hello per databas cap-gränsen för den här resursen i hello överordnade poolen.</span><span class="sxs-lookup"><span data-stu-id="a9e00-174">For example, 50% utilization of any of these metrics indicates that hello specific resource consumption is at 50% of hello per database cap limit for that resource in hello parent pool.</span></span>

<span data-ttu-id="a9e00-175">tooretrieve hello mått:</span><span class="sxs-lookup"><span data-stu-id="a9e00-175">tooretrieve hello metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-tooan-elastic-pool-resource"></a><span data-ttu-id="a9e00-176">Lägg till en resurs för avisering tooan elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-176">Add an alert tooan elastic pool resource</span></span>
<span data-ttu-id="a9e00-177">Du kan lägga till Varningsregler tooan elastisk pool toosend e-postmeddelanden eller meddela strängar för[URL slutpunkter](https://msdn.microsoft.com/library/mt718036.aspx) när hello elastisk pool träffar ett tröskelvärde för användning som du skapat.</span><span class="sxs-lookup"><span data-stu-id="a9e00-177">You can add alert rules tooan elastic pool toosend email notifications or alert strings too[URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when hello elastic pool hits a utilization threshold that you set up.</span></span> <span data-ttu-id="a9e00-178">Använd hello Lägg till AzureRmMetricAlertRule cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9e00-178">Use hello Add-AzureRmMetricAlertRule cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9e00-179">Resursutnyttjande övervakning för elastiska pooler har en fördröjning på minst 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="a9e00-179">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="a9e00-180">Konfigurera aviseringar för mindre än 10 minuter för elastiska pooler stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="a9e00-180">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="a9e00-181">Alla aviseringar som angetts för elastiska pooler med en punkt (parameter med namnet ”-fönsterstorlek” i PowerShell API) på mindre än 10 minuter kan inte utlösas.</span><span class="sxs-lookup"><span data-stu-id="a9e00-181">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="a9e00-182">Kontrollera att alla aviseringar som du definierar för elastiska pooler använder en period (WindowSize) på 10 minuter eller mer.</span><span class="sxs-lookup"><span data-stu-id="a9e00-182">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>
>

<span data-ttu-id="a9e00-183">Det här exemplet lägger till en avisering för att få en avisering när en elastisk pool-eDTU förbrukning går över tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="a9e00-183">This example adds an alert for getting notified when an elastic pool’s eDTU consumption goes above certain threshold.</span></span>

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location =  '<location'                         # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

#$Target Resource ID
$ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# create a unique rule name
$alertName = $poolName + "- DTU consumption rule"

# Create an alert rule for DTU_consumption_percent
Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail
```

<span data-ttu-id="a9e00-184">Mer information finns i [skapa SQL-databas aviseringar i Azure-portalen](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a9e00-184">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="add-alerts-tooall-databases-in-an-elastic-pool"></a><span data-ttu-id="a9e00-185">Lägga till aviseringar tooall databaser i en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-185">Add alerts tooall databases in an elastic pool</span></span>
<span data-ttu-id="a9e00-186">Du kan lägga till Varningsregler tooall databas i en elastisk pool toosend e-postmeddelanden eller meddela strängar för[URL slutpunkter](https://msdn.microsoft.com/library/mt718036.aspx) när en resurs träffar ett tröskelvärdet för användning som konfigurerats av hello avisering.</span><span class="sxs-lookup"><span data-stu-id="a9e00-186">You can add alert rules tooall database in an elastic pool toosend email notifications or alert strings too[URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when a resource hits a utilization threshold set up by hello alert.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9e00-187">Resursutnyttjande övervakning för elastiska pooler har en fördröjning på minst 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="a9e00-187">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="a9e00-188">Konfigurera aviseringar för mindre än 10 minuter för elastiska pooler stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="a9e00-188">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="a9e00-189">Alla aviseringar som angetts för elastiska pooler med en punkt (parameter med namnet ”-fönsterstorlek” i PowerShell API) på mindre än 10 minuter kan inte utlösas.</span><span class="sxs-lookup"><span data-stu-id="a9e00-189">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="a9e00-190">Kontrollera att alla aviseringar som du definierar för elastiska pooler använder en period (WindowSize) på 10 minuter eller mer.</span><span class="sxs-lookup"><span data-stu-id="a9e00-190">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>

<span data-ttu-id="a9e00-191">Det här exemplet lägger till en avisering tooeach hello databaser i en elastisk pool för att få ett meddelande när att databasen DTU-förbrukning går över tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="a9e00-191">This example adds an alert tooeach of hello databases in an elastic pool for getting notified when that database’s DTU consumption goes above certain threshold.</span></span>

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location = '<location'                          # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
foreach ($db in $dbList)
{
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop hello alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
}
```

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a><span data-ttu-id="a9e00-192">Samla in och övervaka resursanvändningsdata över flera pooler i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="a9e00-192">Collect and monitor resource usage data across multiple pools in a subscription</span></span>
<span data-ttu-id="a9e00-193">När du har många databaser i en prenumeration är besvärlig toomonitor varje elastisk pool separat.</span><span class="sxs-lookup"><span data-stu-id="a9e00-193">When you have many databases in a subscription, it is cumbersome toomonitor each elastic pool separately.</span></span> <span data-ttu-id="a9e00-194">I stället kan PowerShell-cmdlets för SQL-databasen och T-SQL-frågor vara kombinerade toocollect resursanvändningsdata från flera pooler och deras databaser för övervakning och analys av Resursanvändning.</span><span class="sxs-lookup"><span data-stu-id="a9e00-194">Instead, SQL database PowerShell cmdlets and T-SQL queries can be combined toocollect resource usage data from multiple pools and their databases for monitoring and analysis of resource usage.</span></span> <span data-ttu-id="a9e00-195">En [exempel på implementering](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) av sådana en uppsättning powershell-skript finns i hello GitHub SQL Server-exempel databasen tillsammans med dokumentation om vad det gör och hur toouse den.</span><span class="sxs-lookup"><span data-stu-id="a9e00-195">A [sample implementation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) of such a set of powershell scripts can be found in hello GitHub SQL Server samples repository along with documentation on what it does and how toouse it.</span></span>

<span data-ttu-id="a9e00-196">toouse detta exempel på implementering, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="a9e00-196">toouse this sample implementation, follow these steps.</span></span>

1. <span data-ttu-id="a9e00-197">Hämta hello [skript och dokumentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span><span class="sxs-lookup"><span data-stu-id="a9e00-197">Download hello [scripts and documentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span></span>
2. <span data-ttu-id="a9e00-198">Ändra hello skript för din miljö.</span><span class="sxs-lookup"><span data-stu-id="a9e00-198">Modify hello scripts for your environment.</span></span> <span data-ttu-id="a9e00-199">Ange en eller flera servrar som elastiska pooler finns.</span><span class="sxs-lookup"><span data-stu-id="a9e00-199">Specify one or more servers on which elastic pools are hosted.</span></span>
3. <span data-ttu-id="a9e00-200">Ange en telemetri databas där hello insamlade är toobe lagras.</span><span class="sxs-lookup"><span data-stu-id="a9e00-200">Specify a telemetry database where hello collected metrics are toobe stored.</span></span>
4. <span data-ttu-id="a9e00-201">Anpassa hello skriptet toospecify hello varaktighet för körning av hello skript.</span><span class="sxs-lookup"><span data-stu-id="a9e00-201">Customize hello script toospecify hello duration of hello scripts' execution.</span></span>

<span data-ttu-id="a9e00-202">På en hög nivå hello skript hello följande:</span><span class="sxs-lookup"><span data-stu-id="a9e00-202">At a high level, hello scripts do hello following:</span></span>

* <span data-ttu-id="a9e00-203">Räknar upp alla servrar i en viss Azure-prenumeration (eller en angiven lista över servrar).</span><span class="sxs-lookup"><span data-stu-id="a9e00-203">Enumerates all servers in a given Azure subscription (or a specified list of servers).</span></span>
* <span data-ttu-id="a9e00-204">Kör ett bakgrundsjobb för varje server.</span><span class="sxs-lookup"><span data-stu-id="a9e00-204">Runs a background job for each server.</span></span> <span data-ttu-id="a9e00-205">hello jobbet körs i en slinga med jämna mellanrum och samlar in telemetridata för alla hello pooler i hello-server.</span><span class="sxs-lookup"><span data-stu-id="a9e00-205">hello job runs in a loop at regular intervals and collects telemetry data for all hello pools in hello server.</span></span> <span data-ttu-id="a9e00-206">Hello insamlade data hämtas sedan till hello angivna telemetri databasen.</span><span class="sxs-lookup"><span data-stu-id="a9e00-206">It then loads hello collected data into hello specified telemetry database.</span></span>
* <span data-ttu-id="a9e00-207">Räknar upp en lista över databaser i varje pool toocollect hello databasen resursanvändningsdata.</span><span class="sxs-lookup"><span data-stu-id="a9e00-207">Enumerates a list of databases in each pool toocollect hello database resource usage data.</span></span> <span data-ttu-id="a9e00-208">Hello insamlade data hämtas sedan till hello telemetri databasen.</span><span class="sxs-lookup"><span data-stu-id="a9e00-208">It then loads hello collected data into hello telemetry database.</span></span>

<span data-ttu-id="a9e00-209">hello kan insamlade mått i hello telemetri databasen vara analyserade toomonitor hello hälsotillståndet för elastiska pooler och hello databaser i den.</span><span class="sxs-lookup"><span data-stu-id="a9e00-209">hello collected metrics in hello telemetry database can be analyzed toomonitor hello health of elastic pools and hello databases in it.</span></span> <span data-ttu-id="a9e00-210">hello skriptet installerar också en fördefinierad tabell Value-funktion (Tabellvärdesfunktion) i hello telemetri databasen toohelp sammanställd hello mått för ett angivet tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="a9e00-210">hello script also installs a pre-defined Table-Value function (TVF) in hello telemetry database toohelp aggregate hello metrics for a specified time window.</span></span> <span data-ttu-id="a9e00-211">Resultaten av hello Tabellvärdesfunktion kan till exempel vara används tooshow ”främsta elastiska pooler med hello maximala eDTU-användning i ett angivet tidsintervall”.</span><span class="sxs-lookup"><span data-stu-id="a9e00-211">For example, results of hello TVF can be used tooshow “top N elastic pools with hello maximum eDTU utilization in a given time window.”</span></span> <span data-ttu-id="a9e00-212">Du kan också använda analytiska verktyg som Excel eller Power BI tooquery och analysera hello insamlade data.</span><span class="sxs-lookup"><span data-stu-id="a9e00-212">Optionally, use analytic tools like Excel or Power BI tooquery and analyze hello collected data.</span></span>

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a><span data-ttu-id="a9e00-213">Exempel: hämta resurs förbrukning mått för en elastisk pool och dess databaser</span><span class="sxs-lookup"><span data-stu-id="a9e00-213">Example: retrieve resource consumption metrics for an elastic pool and its databases</span></span>
<span data-ttu-id="a9e00-214">Det här exemplet hämtar hello förbrukning mått för en given elastiska poolen och alla dess databaser.</span><span class="sxs-lookup"><span data-stu-id="a9e00-214">This example retrieves hello consumption metrics for a given elastic pool and all its databases.</span></span> <span data-ttu-id="a9e00-215">Insamlade data är formaterad och skrivs tooa CSV-formaterad fil.</span><span class="sxs-lookup"><span data-stu-id="a9e00-215">Collected data is formatted and written tooa .csv formatted file.</span></span> <span data-ttu-id="a9e00-216">hello filen går att bläddra i Excel.</span><span class="sxs-lookup"><span data-stu-id="a9e00-216">hello file can be browsed with Excel.</span></span>

```PowerShell
$subscriptionId = '<Azure subscription id>'          # Azure subscription ID
$resourceGroupName = '<resource group name>'             # Resource Group
$serverName = <server name>                              # server name
$poolName = <elastic pool name>                          # pool name

# Login tooAzure account and select hello subscription.
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Get resource usage metrics for an elastic pool for hello specified time interval.
$startTime = '4/27/2016 00:00:00'  # start time in UTC
$endTime = '4/27/2016 01:00:00'    # end time in UTC

# Construct hello pool resource ID and retrive pool metrics at 5-minute granularity.
$poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
$poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
$dbMetrics = @()
foreach ($db in $dbList)
{
     $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
     $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
}

#Optionally you can format hello metrics and output as .csv file using hello following script block.
$command = {
param($metricList, $outputFile)

# Format metrics into a table.
$table = @()
foreach($metric in $metricList) {
   foreach($metricValue in $metric.MetricValues) {
      $sx = New-Object PSObject -Property @{
      Timestamp = $metricValue.Timestamp.ToString()
      MetricName = $metric.Name;
      Average = $metricValue.Average;
      ResourceID = $metric.ResourceId
   }$table = $table += $sx
   }
}

# Output hello metrics into a .csv file.
write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
}

# Format and output pool metrics
Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

# Format and output database metrics
Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv
```

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="a9e00-217">Svarstiden för åtgärder elastisk pool</span><span class="sxs-lookup"><span data-stu-id="a9e00-217">Latency of elastic pool operations</span></span>
* <span data-ttu-id="a9e00-218">Ändra hello min edtu: er per databas eller max edtu: er per databas vanligtvis har slutförts i 5 minuter eller mindre.</span><span class="sxs-lookup"><span data-stu-id="a9e00-218">Changing hello min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="a9e00-219">Ändra hello edtu: er per pool beror på hello totala mängden diskutrymme som används av alla databaser i hello pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-219">Changing hello eDTUs per pool depends on hello total amount of space used by all databases in hello pool.</span></span> <span data-ttu-id="a9e00-220">Ändringarna tar i genomsnitt 90 minuter eller mindre per 100 GB.</span><span class="sxs-lookup"><span data-stu-id="a9e00-220">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="a9e00-221">Om hello totalt utrymme som används av alla databaser i poolen hello är exempelvis 200 GB, och sedan hello förväntade svarstid för att ändra hello pool-eDTU per pool är tre timmar eller mindre.</span><span class="sxs-lookup"><span data-stu-id="a9e00-221">For example, if hello total space used by all databases in hello pool is 200 GB, then hello expected latency for changing hello pool eDTU per pool is 3 hours or less.</span></span>



## <a name="next-steps"></a><span data-ttu-id="a9e00-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9e00-222">Next steps</span></span>
* <span data-ttu-id="a9e00-223">[Skapa elastiska jobb](sql-database-elastic-jobs-overview.md) elastiska jobb låter dig köra T-SQL-skript mot valfritt antal databaser i hello pool.</span><span class="sxs-lookup"><span data-stu-id="a9e00-223">[Create elastic jobs](sql-database-elastic-jobs-overview.md) Elastic jobs let you run T-SQL scripts against any number of databases in hello pool.</span></span>
* <span data-ttu-id="a9e00-224">Se [skala ut med Azure SQL Database](sql-database-elastic-scale-introduction.md): Använd elastisk verktyg tooscale ut, flytta data, fråga eller skapa transaktioner.</span><span class="sxs-lookup"><span data-stu-id="a9e00-224">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic tools tooscale out, move data, query, or create transactions.</span></span>
