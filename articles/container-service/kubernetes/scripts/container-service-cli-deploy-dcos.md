---
title: Azure CLI Script exempel - skapa ACS DC/OS-kluster | Microsoft Docs
description: Azure CLI Script exempel - skapa ACS DC/OS-klustret
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: nepeters
ms.openlocfilehash: ff90aee308a993ae0d36288191d1496affacce2a
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="create-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="b12d2-104">Skapa ett Azure Container Service DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="b12d2-104">Create an Azure Container Service DC/OS Cluster</span></span>

<span data-ttu-id="b12d2-105">Det här exemplet skapar ett Azure Container Service-kluster som kör DC/OS.</span><span class="sxs-lookup"><span data-stu-id="b12d2-105">This sample creates an Azure Container Service cluster running DCOS.</span></span>

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b12d2-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="b12d2-106">Sample script</span></span>

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a><span data-ttu-id="b12d2-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="b12d2-107">Clean up deployment</span></span> 

<span data-ttu-id="b12d2-108">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="b12d2-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="b12d2-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="b12d2-109">Script explanation</span></span>

<span data-ttu-id="b12d2-110">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="b12d2-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="b12d2-111">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="b12d2-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="b12d2-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="b12d2-112">Command</span></span> | <span data-ttu-id="b12d2-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b12d2-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b12d2-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="b12d2-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="b12d2-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="b12d2-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b12d2-116">AZ acs skapa</span><span class="sxs-lookup"><span data-stu-id="b12d2-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="b12d2-117">Skapar och ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="b12d2-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b12d2-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b12d2-118">Next steps</span></span>

<span data-ttu-id="b12d2-119">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b12d2-119">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b12d2-120">Ytterligare Azure Container Service CLI skriptexempel finns i den [Azure Container Service-dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b12d2-120">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>