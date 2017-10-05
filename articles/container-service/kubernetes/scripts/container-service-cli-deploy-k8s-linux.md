---
title: Azure CLI Script exempel - skapa ACS Linux Kubernetes kluster | Microsoft Docs
description: Azure CLI Script exempel - skapa ACS Linux Kubernetes kluster
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
ms.openlocfilehash: c6a392217f84f549f2cae3c68fed85b9f888db77
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="create-an-azure-container-service-kubernetes-linux-cluster"></a><span data-ttu-id="eb438-104">Skapa ett Azure Container Service Kubernetes Linux-kluster</span><span class="sxs-lookup"><span data-stu-id="eb438-104">Create an Azure Container Service Kubernetes Linux Cluster</span></span>

<span data-ttu-id="eb438-105">Det här exemplet skapar ett Azure Container Service-kluster som kör Kubernetes för Linux-baserade behållare.</span><span class="sxs-lookup"><span data-stu-id="eb438-105">This sample creates an Azure Container Service cluster running Kubernetes for Linux based containers.</span></span>

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="eb438-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="eb438-106">Sample script</span></span>

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type kubernetes \
  --resource-group myResourceGroup \
  --name myK8SCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a><span data-ttu-id="eb438-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="eb438-107">Clean up deployment</span></span> 

<span data-ttu-id="eb438-108">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="eb438-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="eb438-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="eb438-109">Script explanation</span></span>

<span data-ttu-id="eb438-110">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="eb438-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="eb438-111">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="eb438-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="eb438-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="eb438-112">Command</span></span> | <span data-ttu-id="eb438-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="eb438-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="eb438-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="eb438-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="eb438-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="eb438-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="eb438-116">AZ acs skapa</span><span class="sxs-lookup"><span data-stu-id="eb438-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="eb438-117">Skapar och ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="eb438-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="eb438-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eb438-118">Next steps</span></span>

<span data-ttu-id="eb438-119">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="eb438-119">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="eb438-120">Ytterligare Azure Container Service CLI skriptexempel finns i den [Azure Container Service-dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="eb438-120">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>

