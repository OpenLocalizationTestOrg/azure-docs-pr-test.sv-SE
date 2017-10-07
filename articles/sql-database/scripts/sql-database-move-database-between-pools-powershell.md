---
title: aaaPowerShell exempel flytta Azure SQL database-elastisk SQL-pool | Microsoft Docs
description: "Azure PowerShell exempel skriptet toomove en SQL-databas mellan elastiska pooler med hjälp av PowerShell"
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
ms.openlocfilehash: 501e82ce93a31264d0625fb0243b4e44dcb2d007
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-elastic-pools-and-move-databases-between-elastic-pools"></a><span data-ttu-id="945cc-103">Använd PowerShell toocreate elastiska pooler och flytta databaser mellan elastiska pooler</span><span class="sxs-lookup"><span data-stu-id="945cc-103">Use PowerShell toocreate elastic pools and move databases between elastic pools</span></span>

<span data-ttu-id="945cc-104">Exempel på detta PowerShell-skript skapar två elastiska pooler flyttar en databas från en elastisk pool till en annan elastisk pool och flyttar en databas utanför en elastisk pool tooa enskild databas prestandanivå.</span><span class="sxs-lookup"><span data-stu-id="945cc-104">This PowerShell script example creates two elastic pools and moves a database from one elastic pool into another elastic pool, and then moves a database out of an elastic pool tooa single database performance level.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="945cc-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="945cc-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=17-18 "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="945cc-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="945cc-106">Clean up deployment</span></span>

<span data-ttu-id="945cc-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="945cc-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="945cc-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="945cc-108">Script explanation</span></span>

<span data-ttu-id="945cc-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="945cc-109">This script uses hello following commands.</span></span> <span data-ttu-id="945cc-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="945cc-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="945cc-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="945cc-111">Command</span></span> | <span data-ttu-id="945cc-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="945cc-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="945cc-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="945cc-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="945cc-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="945cc-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="945cc-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="945cc-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="945cc-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="945cc-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="945cc-117">Ny AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="945cc-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="945cc-118">Skapar en elastisk pool i en logisk server.</span><span class="sxs-lookup"><span data-stu-id="945cc-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="945cc-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="945cc-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="945cc-120">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="945cc-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="945cc-121">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="945cc-121">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="945cc-122">Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="945cc-122">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="945cc-123">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="945cc-123">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="945cc-124">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="945cc-124">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="945cc-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="945cc-125">Next steps</span></span>

<span data-ttu-id="945cc-126">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="945cc-126">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="945cc-127">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="945cc-127">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
