---
title: aaaAzure CLI skriptexempel - skapa ACS DC/OS-kluster | Microsoft Docs
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
ms.openlocfilehash: 35213fd615b2145642add908dfc48516c33ff332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="8b058-104">Skapa ett Azure Container Service DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="8b058-104">Create an Azure Container Service DC/OS Cluster</span></span>

<span data-ttu-id="8b058-105">Det här exemplet skapar ett Azure Container Service-kluster som kör DC/OS.</span><span class="sxs-lookup"><span data-stu-id="8b058-105">This sample creates an Azure Container Service cluster running DCOS.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8b058-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="8b058-106">Sample script</span></span>

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a><span data-ttu-id="8b058-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="8b058-107">Clean up deployment</span></span> 

<span data-ttu-id="8b058-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="8b058-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8b058-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="8b058-109">Script explanation</span></span>

<span data-ttu-id="8b058-110">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="8b058-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="8b058-111">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8b058-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8b058-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="8b058-112">Command</span></span> | <span data-ttu-id="8b058-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="8b058-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8b058-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="8b058-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8b058-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="8b058-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8b058-116">AZ acs skapa</span><span class="sxs-lookup"><span data-stu-id="8b058-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="8b058-117">Skapar och ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="8b058-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8b058-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b058-118">Next steps</span></span>

<span data-ttu-id="8b058-119">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8b058-119">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8b058-120">Ytterligare Azure Container Service CLI skriptexempel finns i hello [Azure Container Service-dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8b058-120">Additional Azure Container Service CLI script samples can be found in hello [Azure Container Service documentation](../cli-samples.md).</span></span>
