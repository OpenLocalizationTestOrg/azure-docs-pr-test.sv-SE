---
title: "Azure PowerShell Script Get Azure Cosmos DB-anslutningssträngen för MongoDB-appar | Microsoft Docs"
description: "Azure PowerShell skriptexempel - hämta Azure Cosmos DB-anslutningssträngen för MongoDB-appar"
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
ms.openlocfilehash: e44e35cc7d11db48cd82e470ce8226b3c8cc220a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-powershell"></a><span data-ttu-id="cd0ca-103">Hämta en Azure Cosmos DB-anslutningssträng för MongoDB-appar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd0ca-103">Get an Azure Cosmos DB connection string for MongoDB apps using PowerShell</span></span>

<span data-ttu-id="cd0ca-104">Det här exemplet hämtar en Azure Cosmos DB-anslutningssträng för MongoDB-appar som använder PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cd0ca-104">This sample gets an Azure Cosmos DB connection string for MongoDB apps using PowerShell.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="cd0ca-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="cd0ca-105">Sample script</span></span>

<span data-ttu-id="cd0ca-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/cosmosdb/get-mongodb-connection-string/get-mongodb-connection-string.ps1?highlight=37-41 "hämta anslutningssträngen för MongoDB från ett Azure DB som Cosmos-konto")]</span><span class="sxs-lookup"><span data-stu-id="cd0ca-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/get-mongodb-connection-string/get-mongodb-connection-string.ps1?highlight=37-41 "Get the MongoDB connection string from an Azure Cosmos DB account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="cd0ca-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="cd0ca-107">Clean up deployment</span></span>

<span data-ttu-id="cd0ca-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="cd0ca-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="cd0ca-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="cd0ca-109">Script explanation</span></span>

<span data-ttu-id="cd0ca-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="cd0ca-110">This script uses the following commands.</span></span> <span data-ttu-id="cd0ca-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="cd0ca-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="cd0ca-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="cd0ca-112">Command</span></span> | <span data-ttu-id="cd0ca-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="cd0ca-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cd0ca-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cd0ca-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="cd0ca-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="cd0ca-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cd0ca-116">Ny AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="cd0ca-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="cd0ca-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="cd0ca-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="cd0ca-118">Anropa AzureRmResourceAction</span><span class="sxs-lookup"><span data-stu-id="cd0ca-118">Invoke-AzureRmResourceAction</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | <span data-ttu-id="cd0ca-119">Anropar en åtgärd på CosmosDB Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="cd0ca-119">Invokes an action on the Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="cd0ca-120">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cd0ca-120">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="cd0ca-121">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="cd0ca-121">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="cd0ca-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cd0ca-122">Next steps</span></span>

<span data-ttu-id="cd0ca-123">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="cd0ca-123">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="cd0ca-124">Ytterligare Azure Cosmos DB PowerShell skriptexempel finns i den [Azure Cosmos DB PowerShell-skript](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cd0ca-124">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>