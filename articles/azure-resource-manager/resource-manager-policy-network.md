---
title: "aaaAzure resursprinciper för nätverksresurser | Microsoft Docs"
description: "Beskriver principer för Azure Resource Manager för att hantera hello distribution av nätverksresurser."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: tomfitz
ms.openlocfilehash: a6072c1c30db0a4e4a1cae04efc7828d14069709
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-toonetwork-resources"></a><span data-ttu-id="66386-103">Tillämpa resurs principer toonetwork resurser</span><span class="sxs-lookup"><span data-stu-id="66386-103">Apply resource policies toonetwork resources</span></span>
<span data-ttu-id="66386-104">Den här artikeln visar ett exempel [resursprincip](resource-manager-policy.md) kan du använda tooAzure virtuella nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="66386-104">This article shows an example [resource policy](resource-manager-policy.md) you can apply tooAzure virtual network gateways.</span></span> <span data-ttu-id="66386-105">Den här principen garanterar konsekvens för gateways som distribuerats i organisationen.</span><span class="sxs-lookup"><span data-stu-id="66386-105">This policy ensures consistency for gateways deployed in your organization.</span></span> 

## <a name="define-permitted-virtual-network-gateway-sku"></a><span data-ttu-id="66386-106">Definiera tillåtna virtuell nätverksgateway SKU</span><span class="sxs-lookup"><span data-stu-id="66386-106">Define permitted virtual network gateway SKU</span></span>

<span data-ttu-id="66386-107">hello följa principen begränsar vilka SKU: er kan distribueras för virtuella nätverksgatewayer:</span><span class="sxs-lookup"><span data-stu-id="66386-107">hello following policy restricts which SKUs can be deployed for virtual network gateways:</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Network/virtualNetworkGateways"
      },
      {
        "not": {
          "field": "Microsoft.Network/virtualNetworkGateways/sku.name",
          "in": [
            "Basic",
            "VpnGw1"
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="66386-108">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66386-108">Next steps</span></span>
* <span data-ttu-id="66386-109">När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång.</span><span class="sxs-lookup"><span data-stu-id="66386-109">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="66386-110">hello scope kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="66386-110">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="66386-111">tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="66386-111">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="66386-112">tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="66386-112">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="66386-113">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="66386-113">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

