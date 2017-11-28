---
title: "PowerShell-exempel-övervakaren-skala-enda Azure SQL-databas | Microsoft Docs"
description: "Azure PowerShell-exempelskript för att övervaka och skala en enskild Azure SQL-databas"
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
ms.openlocfilehash: 5d08335f4b1d6c5c6a120cbfb86ab2c62b79bb9f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-single-sql-database"></a><span data-ttu-id="03e7e-103">Använda PowerShell för att övervaka och skala en enskild SQL-databas</span><span class="sxs-lookup"><span data-stu-id="03e7e-103">Use PowerShell to monitor and scale a single SQL database</span></span>

<span data-ttu-id="03e7e-104">Exempel på detta PowerShell-skript övervakar prestandamått för en databas, skalas till en högre prestandanivå och skapar en aviseringsregel på något av prestandamåtten.</span><span class="sxs-lookup"><span data-stu-id="03e7e-104">This PowerShell script example monitors the performance metrics of a database, scales it to a higher performance level, and creates an alert rule on one of the performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="03e7e-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="03e7e-105">Sample script</span></span>

<span data-ttu-id="03e7e-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Övervakare och skala en SQL-databas")]</span><span class="sxs-lookup"><span data-stu-id="03e7e-106">[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Monitor and scale single SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="03e7e-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="03e7e-107">Clean up deployment</span></span>

<span data-ttu-id="03e7e-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="03e7e-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="03e7e-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="03e7e-109">Script explanation</span></span>

<span data-ttu-id="03e7e-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="03e7e-110">This script uses the following commands.</span></span> <span data-ttu-id="03e7e-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="03e7e-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="03e7e-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="03e7e-112">Command</span></span> | <span data-ttu-id="03e7e-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="03e7e-113">Notes</span></span> |
|---|---|
 [<span data-ttu-id="03e7e-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="03e7e-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="03e7e-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="03e7e-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="03e7e-116">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="03e7e-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="03e7e-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="03e7e-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="03e7e-118">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="03e7e-118">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="03e7e-119">Visar användningsinformation storlek för databasen.</span><span class="sxs-lookup"><span data-stu-id="03e7e-119">Shows the size usage information for the database.</span></span>|
| [<span data-ttu-id="03e7e-120">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="03e7e-120">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="03e7e-121">Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="03e7e-121">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="03e7e-122">Lägg till AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="03e7e-122">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="03e7e-123">Anger en varningsregeln ska övervaka automatiskt dtu: er i framtiden.</span><span class="sxs-lookup"><span data-stu-id="03e7e-123">Sets an alert rule to automatically monitor DTUs in the future.</span></span> |
| [<span data-ttu-id="03e7e-124">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="03e7e-124">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="03e7e-125">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="03e7e-125">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="03e7e-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03e7e-126">Next steps</span></span>

<span data-ttu-id="03e7e-127">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="03e7e-127">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="03e7e-128">Ytterligare exempel för SQL Database PowerShell-skript finns i den [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="03e7e-128">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
