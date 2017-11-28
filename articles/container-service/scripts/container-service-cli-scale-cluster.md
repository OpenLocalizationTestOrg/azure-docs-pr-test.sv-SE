---
title: Azure CLI skriptexempel - skala en ACS-kluster | Microsoft Docs
description: Azure CLI skriptexempel - skala en ACS-kluster
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
ms.openlocfilehash: 0ea2c73436e650fdb37535538baccc565369fa9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scale-an-azure-container-service-cluster"></a><span data-ttu-id="8e8ea-104">Skala ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="8e8ea-104">Scale an Azure Container Service Cluster</span></span>

<span data-ttu-id="8e8ea-105">Det här exemplet skalor och Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="8e8ea-105">This sample scales and Azure Container Service.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8e8ea-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="8e8ea-106">Sample script</span></span>

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
```

## <a name="clean-up-deployment"></a><span data-ttu-id="8e8ea-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="8e8ea-107">Clean up deployment</span></span> 

<span data-ttu-id="8e8ea-108">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="8e8ea-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8e8ea-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="8e8ea-109">Script explanation</span></span>

<span data-ttu-id="8e8ea-110">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="8e8ea-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="8e8ea-111">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8e8ea-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8e8ea-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="8e8ea-112">Command</span></span> | <span data-ttu-id="8e8ea-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="8e8ea-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8e8ea-114">AZ acs skala</span><span class="sxs-lookup"><span data-stu-id="8e8ea-114">az acs scale</span></span>](/cli/azure/acs#scale) | <span data-ttu-id="8e8ea-115">Skala en ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="8e8ea-115">Scale an ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8e8ea-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e8ea-116">Next steps</span></span>

<span data-ttu-id="8e8ea-117">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8e8ea-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8e8ea-118">Ytterligare Azure Container Service CLI skriptexempel finns i den [Azure Container Service-dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8e8ea-118">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>

