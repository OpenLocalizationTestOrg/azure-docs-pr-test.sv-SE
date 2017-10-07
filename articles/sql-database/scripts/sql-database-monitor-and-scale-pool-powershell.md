---
title: "aaaPowerShell exempel-övervakaren-skala-SQL elastisk pool-Azure SQL Database | Microsoft Docs"
description: "Exempel på Azure PowerShell script toomonitor och skala en elastisk SQL-pool i Azure SQL Database"
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
ms.openlocfilehash: 149a45174ccb8072ea21753364196c7f98fd4101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomonitor-and-scale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="eef37-103">Använd PowerShell toomonitor och skala en elastisk SQL-pool i Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="eef37-103">Use PowerShell toomonitor and scale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="eef37-104">Exempel på detta PowerShell-skript som övervakar hello prestandamått för en elastisk pool skalas den tooa högre prestandanivå och skapar en aviseringsregel på en av hello prestandamått.</span><span class="sxs-lookup"><span data-stu-id="eef37-104">This PowerShell script example monitors hello performance metrics of an elastic pool, scales it tooa higher performance level, and creates an alert rule on one of hello performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="eef37-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="eef37-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="eef37-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="eef37-106">Clean up deployment</span></span>

<span data-ttu-id="eef37-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="eef37-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="eef37-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="eef37-108">Script explanation</span></span>

<span data-ttu-id="eef37-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="eef37-109">This script uses hello following commands.</span></span> <span data-ttu-id="eef37-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="eef37-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="eef37-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="eef37-111">Command</span></span> | <span data-ttu-id="eef37-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="eef37-112">Notes</span></span> |
|---|---|
 [<span data-ttu-id="eef37-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="eef37-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="eef37-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="eef37-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="eef37-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="eef37-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="eef37-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="eef37-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="eef37-117">Ny AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="eef37-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="eef37-118">Skapar en elastisk pool i en logisk server.</span><span class="sxs-lookup"><span data-stu-id="eef37-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="eef37-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="eef37-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="eef37-120">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="eef37-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="eef37-121">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="eef37-121">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="eef37-122">Visar information om användning av hello storlek för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="eef37-122">Shows hello size usage information for hello database.</span></span>|
| [<span data-ttu-id="eef37-123">Lägg till AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="eef37-123">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="eef37-124">Lägger till eller uppdaterar en avisering mått-regel.</span><span class="sxs-lookup"><span data-stu-id="eef37-124">Adds or updates a metric-based alert rule.</span></span> |
| [<span data-ttu-id="eef37-125">Set-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="eef37-125">Set-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | <span data-ttu-id="eef37-126">Egenskaper för uppdatering elastisk pool</span><span class="sxs-lookup"><span data-stu-id="eef37-126">Updates elastic pool properties</span></span> |
| [<span data-ttu-id="eef37-127">Lägg till AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="eef37-127">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="eef37-128">Anger en regel för varning tooautomatically Övervakare dtu: er i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="eef37-128">Sets an alert rule tooautomatically monitor DTUs in hello future.</span></span> |
| [<span data-ttu-id="eef37-129">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="eef37-129">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="eef37-130">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="eef37-130">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="eef37-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eef37-131">Next steps</span></span>

<span data-ttu-id="eef37-132">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="eef37-132">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="eef37-133">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="eef37-133">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
