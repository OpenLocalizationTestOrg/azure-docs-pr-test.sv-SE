---
title: "PowerShell exempel-övervakaren-skala-SQL elastisk pool Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell-exempelskript för att övervaka och skala en elastisk SQL-pool i Azure SQL Database"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 7b6a5bd43aadbed33882cf0736ec70436ffdb47b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="85a7e-103">Använda PowerShell för att övervaka och skala en elastisk SQL-pool i Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="85a7e-103">Use PowerShell to monitor and scale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="85a7e-104">Exempel på detta PowerShell-skript övervakar prestandamått för en elastisk pool, skalas till en högre prestandanivå och skapar en aviseringsregel på något av prestandamåtten.</span><span class="sxs-lookup"><span data-stu-id="85a7e-104">This PowerShell script example monitors the performance metrics of an elastic pool, scales it to a higher performance level, and creates an alert rule on one of the performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="85a7e-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="85a7e-105">Sample script</span></span>

<span data-ttu-id="85a7e-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Övervakare och skala en SQL-databas")]</span><span class="sxs-lookup"><span data-stu-id="85a7e-106">[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Monitor and scale single SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="85a7e-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="85a7e-107">Clean up deployment</span></span>

<span data-ttu-id="85a7e-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="85a7e-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="85a7e-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="85a7e-109">Script explanation</span></span>

<span data-ttu-id="85a7e-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="85a7e-110">This script uses the following commands.</span></span> <span data-ttu-id="85a7e-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="85a7e-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="85a7e-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="85a7e-112">Command</span></span> | <span data-ttu-id="85a7e-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="85a7e-113">Notes</span></span> |
|---|---|
 [<span data-ttu-id="85a7e-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="85a7e-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="85a7e-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="85a7e-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="85a7e-116">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="85a7e-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="85a7e-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="85a7e-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="85a7e-118">Ny AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="85a7e-118">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="85a7e-119">Skapar en elastisk pool i en logisk server.</span><span class="sxs-lookup"><span data-stu-id="85a7e-119">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="85a7e-120">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="85a7e-120">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="85a7e-121">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="85a7e-121">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="85a7e-122">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="85a7e-122">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="85a7e-123">Visar användningsinformation storlek för databasen.</span><span class="sxs-lookup"><span data-stu-id="85a7e-123">Shows the size usage information for the database.</span></span>|
| [<span data-ttu-id="85a7e-124">Lägg till AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="85a7e-124">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="85a7e-125">Lägger till eller uppdaterar en avisering mått-regel.</span><span class="sxs-lookup"><span data-stu-id="85a7e-125">Adds or updates a metric-based alert rule.</span></span> |
| [<span data-ttu-id="85a7e-126">Set-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="85a7e-126">Set-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | <span data-ttu-id="85a7e-127">Egenskaper för uppdatering elastisk pool</span><span class="sxs-lookup"><span data-stu-id="85a7e-127">Updates elastic pool properties</span></span> |
| [<span data-ttu-id="85a7e-128">Lägg till AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="85a7e-128">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="85a7e-129">Anger en varningsregeln ska övervaka automatiskt dtu: er i framtiden.</span><span class="sxs-lookup"><span data-stu-id="85a7e-129">Sets an alert rule to automatically monitor DTUs in the future.</span></span> |
| [<span data-ttu-id="85a7e-130">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="85a7e-130">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="85a7e-131">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="85a7e-131">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="85a7e-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85a7e-132">Next steps</span></span>

<span data-ttu-id="85a7e-133">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="85a7e-133">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="85a7e-134">Ytterligare exempel för SQL Database PowerShell-skript finns i den [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="85a7e-134">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
