---
title: Azure PowerShell Script-skapa ett Azure DB-API Cosmos DocumentDB-konto | Microsoft Docs
description: "Azure PowerShell-skript exempel – skapa ett Azure DB-API Cosmos DocumentDB-konto"
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
ms.openlocfilehash: 9b54236ce3446fe1c6a2a30b31f6d91ad43a92d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-create-a-documentdb-api-account-using-powershell"></a><span data-ttu-id="a9097-103">Azure Cosmos DB: Skapa ett DocumentDB-API-konto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="a9097-103">Azure Cosmos DB: Create a DocumentDB API account using PowerShell</span></span>

<span data-ttu-id="a9097-104">Detta exempel PowerShell-skript skapar ett Azure Cosmos DB API-konto.</span><span class="sxs-lookup"><span data-stu-id="a9097-104">This sample PowerShell script creates an Azure Cosmos DB API account.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="a9097-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="a9097-105">Sample script</span></span>

<span data-ttu-id="a9097-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/cosmosdb/create-and-configure-database/create-and-configure-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "skapa ett Azure DB som Cosmos-konto")]</span><span class="sxs-lookup"><span data-stu-id="a9097-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-and-configure-database/create-and-configure-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Create an Azure Cosmos DB account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="a9097-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="a9097-107">Clean up deployment</span></span>

<span data-ttu-id="a9097-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="a9097-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="a9097-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="a9097-109">Script explanation</span></span>

<span data-ttu-id="a9097-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="a9097-110">This script uses the following commands.</span></span> <span data-ttu-id="a9097-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a9097-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="a9097-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="a9097-112">Command</span></span> | <span data-ttu-id="a9097-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a9097-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a9097-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a9097-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="a9097-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="a9097-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a9097-116">Ny AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="a9097-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="a9097-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="a9097-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="a9097-118">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a9097-118">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="a9097-119">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="a9097-119">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="a9097-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9097-120">Next steps</span></span>

<span data-ttu-id="a9097-121">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="a9097-121">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="a9097-122">Ytterligare Azure Cosmos DB PowerShell skriptexempel finns i den [Azure Cosmos DB PowerShell-skript](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a9097-122">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>