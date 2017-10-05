---
title: "Azure PowerShell-skript-Multiregion replikering för Azure Cosmos DB | Microsoft Docs"
description: "Azure PowerShell skriptexempel - Multiregion replikering för Azure Cosmos DB"
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
ms.openlocfilehash: 3a469ba43e6c601f5eb0e13d588cd0bd4a0f8683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-powershell"></a><span data-ttu-id="cf968-103">Replikera en Azure Cosmos DB databaskonto i flera områden och konfigurera redundans prioriteringar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="cf968-103">Replicate an Azure Cosmos DB database account in multiple regions and configure failover priorities using PowerShell</span></span>

<span data-ttu-id="cf968-104">Det här exemplet replikerar alla slags Azure Cosmos DB databaskonto i flera områden, och konfigurerar redundans prioriteringar med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cf968-104">This sample replicates any kind of Azure Cosmos DB database account in multiple regions and configures failover priorities using PowerShell.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="cf968-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="cf968-105">Sample script</span></span>

<span data-ttu-id="cf968-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/cosmosdb/replicate-database-multiple-regions/replicate-database-multiple-regions.ps1?highlight=37-44,47-48,51-55 "replikera ett konto i Azure Cosmos DB över flera regioner")]</span><span class="sxs-lookup"><span data-stu-id="cf968-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/replicate-database-multiple-regions/replicate-database-multiple-regions.ps1?highlight=37-44,47-48,51-55 "Replicate an Azure Cosmos DB account across multiple regions")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="cf968-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="cf968-107">Clean up deployment</span></span>

<span data-ttu-id="cf968-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="cf968-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="cf968-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="cf968-109">Script explanation</span></span>

<span data-ttu-id="cf968-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="cf968-110">This script uses the following commands.</span></span> <span data-ttu-id="cf968-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="cf968-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="cf968-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="cf968-112">Command</span></span> | <span data-ttu-id="cf968-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="cf968-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cf968-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cf968-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="cf968-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="cf968-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cf968-116">Ny AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="cf968-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="cf968-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="cf968-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="cf968-118">Ange AzureRMResource</span><span class="sxs-lookup"><span data-stu-id="cf968-118">Set-AzureRMResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="cf968-119">Ändrar databaskontot.</span><span class="sxs-lookup"><span data-stu-id="cf968-119">Modifies the database account.</span></span> |
| [<span data-ttu-id="cf968-120">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cf968-120">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="cf968-121">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="cf968-121">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="cf968-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cf968-122">Next steps</span></span>

<span data-ttu-id="cf968-123">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="cf968-123">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="cf968-124">Ytterligare Azure Cosmos DB PowerShell skriptexempel finns i den [Azure Cosmos DB PowerShell-skript](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cf968-124">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>