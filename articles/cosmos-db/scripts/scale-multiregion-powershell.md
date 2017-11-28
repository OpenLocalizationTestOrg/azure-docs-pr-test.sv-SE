---
title: "aaaAzure PowerShell-skript-Multiregion replikering för Azure Cosmos DB | Microsoft Docs"
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
ms.openlocfilehash: 8e3e79f086f46fc037d52589eb8bcf4557214566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-powershell"></a><span data-ttu-id="b86ef-103">Replikera en Azure Cosmos DB databaskonto i flera områden och konfigurera redundans prioriteringar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b86ef-103">Replicate an Azure Cosmos DB database account in multiple regions and configure failover priorities using PowerShell</span></span>

<span data-ttu-id="b86ef-104">Det här exemplet replikerar alla slags Azure Cosmos DB databaskonto i flera områden, och konfigurerar redundans prioriteringar med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b86ef-104">This sample replicates any kind of Azure Cosmos DB database account in multiple regions and configures failover priorities using PowerShell.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="b86ef-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="b86ef-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/replicate-database-multiple-regions/replicate-database-multiple-regions.ps1?highlight=37-44,47-48,51-55 "Replicate an Azure Cosmos DB account across multiple regions")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b86ef-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="b86ef-106">Clean up deployment</span></span>

<span data-ttu-id="b86ef-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="b86ef-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="b86ef-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="b86ef-108">Script explanation</span></span>

<span data-ttu-id="b86ef-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="b86ef-109">This script uses hello following commands.</span></span> <span data-ttu-id="b86ef-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="b86ef-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b86ef-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="b86ef-111">Command</span></span> | <span data-ttu-id="b86ef-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b86ef-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b86ef-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b86ef-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="b86ef-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="b86ef-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b86ef-115">Ny AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="b86ef-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="b86ef-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="b86ef-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="b86ef-117">Ange AzureRMResource</span><span class="sxs-lookup"><span data-stu-id="b86ef-117">Set-AzureRMResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="b86ef-118">Ändrar hello konto.</span><span class="sxs-lookup"><span data-stu-id="b86ef-118">Modifies hello database account.</span></span> |
| [<span data-ttu-id="b86ef-119">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b86ef-119">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="b86ef-120">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="b86ef-120">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="b86ef-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b86ef-121">Next steps</span></span>

<span data-ttu-id="b86ef-122">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="b86ef-122">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="b86ef-123">Ytterligare Azure Cosmos DB PowerShell skriptexempel finns i hello [Azure Cosmos DB PowerShell-skript](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b86ef-123">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
