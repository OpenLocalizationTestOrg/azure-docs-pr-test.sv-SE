---
title: "aaaPowerShell exempel-övervakaren-skala-enda Azure SQL database | Microsoft Docs"
description: "Exempel på Azure PowerShell script toomonitor och skala en enskild Azure SQL-databas"
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
ms.openlocfilehash: bd8f880fb47b1360ae4962d2b039faa742de258e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomonitor-and-scale-a-single-sql-database"></a><span data-ttu-id="81a3a-103">Använd PowerShell toomonitor och skala en enskild SQL-databas</span><span class="sxs-lookup"><span data-stu-id="81a3a-103">Use PowerShell toomonitor and scale a single SQL database</span></span>

<span data-ttu-id="81a3a-104">Exempel på detta PowerShell-skript som övervakar hello prestandamått för en databas skalas den tooa högre prestandanivå och skapar en aviseringsregel på en av hello prestandamått.</span><span class="sxs-lookup"><span data-stu-id="81a3a-104">This PowerShell script example monitors hello performance metrics of a database, scales it tooa higher performance level, and creates an alert rule on one of hello performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="81a3a-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="81a3a-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="81a3a-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="81a3a-106">Clean up deployment</span></span>

<span data-ttu-id="81a3a-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="81a3a-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="81a3a-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="81a3a-108">Script explanation</span></span>

<span data-ttu-id="81a3a-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="81a3a-109">This script uses hello following commands.</span></span> <span data-ttu-id="81a3a-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="81a3a-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="81a3a-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="81a3a-111">Command</span></span> | <span data-ttu-id="81a3a-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="81a3a-112">Notes</span></span> |
|---|---|
 [<span data-ttu-id="81a3a-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="81a3a-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="81a3a-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="81a3a-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="81a3a-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="81a3a-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="81a3a-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="81a3a-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="81a3a-117">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="81a3a-117">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="81a3a-118">Visar information om användning av hello storlek för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="81a3a-118">Shows hello size usage information for hello database.</span></span>|
| [<span data-ttu-id="81a3a-119">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="81a3a-119">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="81a3a-120">Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="81a3a-120">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="81a3a-121">Lägg till AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="81a3a-121">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="81a3a-122">Anger en regel för varning tooautomatically Övervakare dtu: er i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="81a3a-122">Sets an alert rule tooautomatically monitor DTUs in hello future.</span></span> |
| [<span data-ttu-id="81a3a-123">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="81a3a-123">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="81a3a-124">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="81a3a-124">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="81a3a-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81a3a-125">Next steps</span></span>

<span data-ttu-id="81a3a-126">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="81a3a-126">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="81a3a-127">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="81a3a-127">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
