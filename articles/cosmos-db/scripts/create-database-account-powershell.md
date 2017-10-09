---
title: aaaAzure PowerShell Script-skapa ett Azure DB-API Cosmos DocumentDB-konto | Microsoft Docs
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
ms.openlocfilehash: f0751faeca3c1de5906b675e736c58f3d5beaea9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-documentdb-api-account-using-powershell"></a><span data-ttu-id="609c0-103">Azure Cosmos DB: Skapa ett DocumentDB-API-konto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="609c0-103">Azure Cosmos DB: Create a DocumentDB API account using PowerShell</span></span>

<span data-ttu-id="609c0-104">Detta exempel PowerShell-skript skapar ett Azure Cosmos DB API-konto.</span><span class="sxs-lookup"><span data-stu-id="609c0-104">This sample PowerShell script creates an Azure Cosmos DB API account.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="609c0-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="609c0-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-and-configure-database/create-and-configure-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Create an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="609c0-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="609c0-106">Clean up deployment</span></span>

<span data-ttu-id="609c0-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="609c0-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="609c0-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="609c0-108">Script explanation</span></span>

<span data-ttu-id="609c0-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="609c0-109">This script uses hello following commands.</span></span> <span data-ttu-id="609c0-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="609c0-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="609c0-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="609c0-111">Command</span></span> | <span data-ttu-id="609c0-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="609c0-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="609c0-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="609c0-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="609c0-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="609c0-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="609c0-115">Ny AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="609c0-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="609c0-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="609c0-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="609c0-117">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="609c0-117">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="609c0-118">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="609c0-118">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="609c0-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="609c0-119">Next steps</span></span>

<span data-ttu-id="609c0-120">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="609c0-120">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="609c0-121">Ytterligare Azure Cosmos DB PowerShell skriptexempel finns i hello [Azure Cosmos DB PowerShell-skript](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="609c0-121">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
