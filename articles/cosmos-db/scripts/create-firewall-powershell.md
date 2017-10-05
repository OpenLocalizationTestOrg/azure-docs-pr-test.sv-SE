---
title: "Azure PowerShell Script-skapa en brandvägg för Azure Cosmos DB | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en brandvägg för Azure Cosmos DB"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: caad9212649dd3dc47ddb21555b5b8496c3d2da1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-powershell"></a><span data-ttu-id="6da21-103">Azure Cosmos DB: Skapa en brandvägg som använder PowerShell</span><span class="sxs-lookup"><span data-stu-id="6da21-103">Azure Cosmos DB: Create a firewall using PowerShell</span></span>

<span data-ttu-id="6da21-104">PowerShell-exempelskriptet skapar en brandvägg för alla typer av Azure Cosmos DB API-kontot.</span><span class="sxs-lookup"><span data-stu-id="6da21-104">This sample PowerShell script creates a firewall for any kind of Azure Cosmos DB API account.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="6da21-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="6da21-105">Sample script</span></span>

<span data-ttu-id="6da21-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/cosmosdb/create-firewall/create-firewall.ps1?highlight=35-36,39-43 "skapa en brandvägg för Azure Cosmos DB")]</span><span class="sxs-lookup"><span data-stu-id="6da21-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-firewall/create-firewall.ps1?highlight=35-36,39-43 "Create a firewall for Azure Cosmos DB")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="6da21-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="6da21-107">Clean up deployment</span></span>

<span data-ttu-id="6da21-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="6da21-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="6da21-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="6da21-109">Script explanation</span></span>

<span data-ttu-id="6da21-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="6da21-110">This script uses the following commands.</span></span> <span data-ttu-id="6da21-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="6da21-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="6da21-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="6da21-112">Command</span></span> | <span data-ttu-id="6da21-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="6da21-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6da21-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="6da21-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="6da21-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="6da21-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6da21-116">Ny AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="6da21-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="6da21-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="6da21-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="6da21-118">Ange AzureRMResource</span><span class="sxs-lookup"><span data-stu-id="6da21-118">Set-AzureRMResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="6da21-119">Ändrar databaskontot.</span><span class="sxs-lookup"><span data-stu-id="6da21-119">Modifies the database account.</span></span> |
| [<span data-ttu-id="6da21-120">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="6da21-120">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="6da21-121">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="6da21-121">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="6da21-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6da21-122">Next steps</span></span>

<span data-ttu-id="6da21-123">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="6da21-123">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="6da21-124">Ytterligare Azure Cosmos DB PowerShell skriptexempel finns i den [Azure Cosmos DB PowerShell-skript](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="6da21-124">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>