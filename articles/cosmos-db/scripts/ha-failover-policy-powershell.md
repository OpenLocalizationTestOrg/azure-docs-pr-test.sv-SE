---
title: "PowerShell Script-skapa en princip för Azure Cosmos DB redundans aaaAzure | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en princip för Azure Cosmos DB växling vid fel"
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
ms.openlocfilehash: 750127385164cd16837b6e29c506d2b44146a629
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-cosmos-db-failover-policy-for-high-availability-using-powershell"></a><span data-ttu-id="fe24d-103">Skapa en princip för Azure Cosmos DB redundanskluster för hög tillgänglighet med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe24d-103">Create an Azure Cosmos DB failover policy for high availability using PowerShell</span></span>

<span data-ttu-id="fe24d-104">PowerShell-exempelskriptet skapar en princip för växling vid fel för hög tillgänglighet för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe24d-104">This sample PowerShell script creates a failover policy for high availability for Azure Cosmos DB.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="fe24d-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="fe24d-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/modify-failover-priority/modify-failover-priority.ps1?highlight=36-39,42-47 "Create an Azure Cosmos DB DocumentDB API account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="fe24d-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="fe24d-106">Clean up deployment</span></span>

<span data-ttu-id="fe24d-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="fe24d-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="fe24d-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="fe24d-108">Script explanation</span></span>

<span data-ttu-id="fe24d-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="fe24d-109">This script uses hello following commands.</span></span> <span data-ttu-id="fe24d-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="fe24d-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="fe24d-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="fe24d-111">Command</span></span> | <span data-ttu-id="fe24d-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="fe24d-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fe24d-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fe24d-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="fe24d-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="fe24d-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fe24d-115">Ny AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="fe24d-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="fe24d-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="fe24d-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="fe24d-117">Anropa AzureRmResourceAction</span><span class="sxs-lookup"><span data-stu-id="fe24d-117">Invoke-AzureRmResourceAction</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | <span data-ttu-id="fe24d-118">Anropar en åtgärd på hello Azure CosmosDB konto.</span><span class="sxs-lookup"><span data-stu-id="fe24d-118">Invokes an action on hello Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="fe24d-119">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fe24d-119">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="fe24d-120">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="fe24d-120">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="fe24d-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe24d-121">Next steps</span></span>

<span data-ttu-id="fe24d-122">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="fe24d-122">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="fe24d-123">Ytterligare Azure Cosmos DB PowerShell skriptexempel finns i hello [Azure Cosmos DB PowerShell-skript](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="fe24d-123">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
