---
title: aaaPowerShell exempel kopia Azure SQL ny databas server | Microsoft Docs
description: "Azure PowerShell exempel skriptet toocopy en ny server för SQL-databasen tooa"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: c08f993bd75913481b1d534857ac057263e1d02b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocopy-a-sql-database-tooa-new-server"></a><span data-ttu-id="dafa9-103">Använd PowerShell toocopy en ny server för SQL-databasen tooa</span><span class="sxs-lookup"><span data-stu-id="dafa9-103">Use PowerShell toocopy a SQL database tooa new server</span></span>

<span data-ttu-id="dafa9-104">Exempel på detta PowerShell-skript skapar en kopia av en befintlig databas i en ny server.</span><span class="sxs-lookup"><span data-stu-id="dafa9-104">This PowerShell script example creates a copy of an existing database in a new server.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="copy-a-database-tooa-new-server"></a><span data-ttu-id="dafa9-105">Kopiera en tooa ny databasserver</span><span class="sxs-lookup"><span data-stu-id="dafa9-105">Copy a database tooa new server</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "Copy database toonew server")]

## <a name="clean-up-deployment"></a><span data-ttu-id="dafa9-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="dafa9-106">Clean up deployment</span></span>

<span data-ttu-id="dafa9-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="dafa9-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="dafa9-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="dafa9-108">Script explanation</span></span>

<span data-ttu-id="dafa9-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="dafa9-109">This script uses hello following commands.</span></span> <span data-ttu-id="dafa9-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="dafa9-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="dafa9-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="dafa9-111">Command</span></span> | <span data-ttu-id="dafa9-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dafa9-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dafa9-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="dafa9-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="dafa9-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="dafa9-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="dafa9-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="dafa9-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="dafa9-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="dafa9-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="dafa9-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="dafa9-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="dafa9-118">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="dafa9-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="dafa9-119">Ny AzureRmSqlDatabaseCopy</span><span class="sxs-lookup"><span data-stu-id="dafa9-119">New-AzureRmSqlDatabaseCopy</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | <span data-ttu-id="dafa9-120">Skapar en kopia av en databas som använder hello ögonblicksbilden vid hello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="dafa9-120">Creates a copy of a database that uses hello snapshot at hello current time.</span></span> |
| [<span data-ttu-id="dafa9-121">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="dafa9-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="dafa9-122">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="dafa9-122">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="dafa9-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dafa9-123">Next steps</span></span>

<span data-ttu-id="dafa9-124">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dafa9-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="dafa9-125">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="dafa9-125">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
