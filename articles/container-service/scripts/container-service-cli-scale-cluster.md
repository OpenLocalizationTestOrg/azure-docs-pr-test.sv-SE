---
title: aaaAzure CLI skriptexempel - skala en ACS-kluster | Microsoft Docs
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
ms.openlocfilehash: b1c214d7cca615257ec8cd6e9993cd15f694289b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-an-azure-container-service-cluster"></a><span data-ttu-id="73352-104">Skala ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="73352-104">Scale an Azure Container Service Cluster</span></span>

<span data-ttu-id="73352-105">Det här exemplet skalor och Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="73352-105">This sample scales and Azure Container Service.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="73352-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="73352-106">Sample script</span></span>

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
```

## <a name="clean-up-deployment"></a><span data-ttu-id="73352-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="73352-107">Clean up deployment</span></span> 

<span data-ttu-id="73352-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="73352-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="73352-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="73352-109">Script explanation</span></span>

<span data-ttu-id="73352-110">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="73352-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="73352-111">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="73352-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="73352-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="73352-112">Command</span></span> | <span data-ttu-id="73352-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="73352-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="73352-114">AZ acs skala</span><span class="sxs-lookup"><span data-stu-id="73352-114">az acs scale</span></span>](/cli/azure/acs#scale) | <span data-ttu-id="73352-115">Skala en ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="73352-115">Scale an ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="73352-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73352-116">Next steps</span></span>

<span data-ttu-id="73352-117">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="73352-117">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="73352-118">Ytterligare Azure Container Service CLI skriptexempel finns i hello [Azure Container Service-dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="73352-118">Additional Azure Container Service CLI script samples can be found in hello [Azure Container Service documentation](../cli-samples.md).</span></span>

