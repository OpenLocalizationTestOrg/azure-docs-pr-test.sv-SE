---
title: PowerShell-exempel kopia Azure SQL ny databas server | Microsoft Docs
description: "Azure PowerShell-exempelskript för att kopiera en SQL-databas till en ny server"
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
ms.openlocfilehash: 005ea2e782f8e1cff29f743d9584eb2af2c77509
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-copy-a-sql-database-to-a-new-server"></a><span data-ttu-id="d0131-103">Använd PowerShell för att kopiera en SQL-databas till en ny server</span><span class="sxs-lookup"><span data-stu-id="d0131-103">Use PowerShell to copy a SQL database to a new server</span></span>

<span data-ttu-id="d0131-104">Exempel på detta PowerShell-skript skapar en kopia av en befintlig databas i en ny server.</span><span class="sxs-lookup"><span data-stu-id="d0131-104">This PowerShell script example creates a copy of an existing database in a new server.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="copy-a-database-to-a-new-server"></a><span data-ttu-id="d0131-105">Kopiera en databas till en ny server</span><span class="sxs-lookup"><span data-stu-id="d0131-105">Copy a database to a new server</span></span>

<span data-ttu-id="d0131-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "kopiera databasen till ny server")]</span><span class="sxs-lookup"><span data-stu-id="d0131-106">[!code-powershell[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "Copy database to new server")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d0131-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="d0131-107">Clean up deployment</span></span>

<span data-ttu-id="d0131-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="d0131-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="d0131-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d0131-109">Script explanation</span></span>

<span data-ttu-id="d0131-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="d0131-110">This script uses the following commands.</span></span> <span data-ttu-id="d0131-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d0131-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d0131-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="d0131-112">Command</span></span> | <span data-ttu-id="d0131-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d0131-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d0131-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d0131-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d0131-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="d0131-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d0131-116">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="d0131-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="d0131-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="d0131-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="d0131-118">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="d0131-118">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="d0131-119">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="d0131-119">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="d0131-120">Ny AzureRmSqlDatabaseCopy</span><span class="sxs-lookup"><span data-stu-id="d0131-120">New-AzureRmSqlDatabaseCopy</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | <span data-ttu-id="d0131-121">Skapar en kopia av en databas som använder ögonblicksbilden vid den aktuella tiden.</span><span class="sxs-lookup"><span data-stu-id="d0131-121">Creates a copy of a database that uses the snapshot at the current time.</span></span> |
| [<span data-ttu-id="d0131-122">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d0131-122">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="d0131-123">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="d0131-123">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="d0131-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d0131-124">Next steps</span></span>

<span data-ttu-id="d0131-125">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d0131-125">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d0131-126">Ytterligare exempel för SQL Database PowerShell-skript finns i den [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d0131-126">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
