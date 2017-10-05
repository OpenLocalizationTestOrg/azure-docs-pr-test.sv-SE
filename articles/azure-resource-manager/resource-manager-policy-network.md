---
title: "Azure resursprinciper för nätverksresurser | Microsoft Docs"
description: "Beskriver principer för Azure Resource Manager för att hantera distributionen av nätverksresurser."
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
ms.openlocfilehash: bca66bbdd9da9b3e4099d0d961f42c9368a17f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="apply-resource-policies-to-network-resources"></a><span data-ttu-id="25842-103">Använda resursprinciper för nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="25842-103">Apply resource policies to network resources</span></span>
<span data-ttu-id="25842-104">Den här artikeln visar ett exempel [resursprincip](resource-manager-policy.md) du kan använda Azure virtuella nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="25842-104">This article shows an example [resource policy](resource-manager-policy.md) you can apply to Azure virtual network gateways.</span></span> <span data-ttu-id="25842-105">Den här principen garanterar konsekvens för gateways som distribuerats i organisationen.</span><span class="sxs-lookup"><span data-stu-id="25842-105">This policy ensures consistency for gateways deployed in your organization.</span></span> 

## <a name="define-permitted-virtual-network-gateway-sku"></a><span data-ttu-id="25842-106">Definiera tillåtna virtuell nätverksgateway SKU</span><span class="sxs-lookup"><span data-stu-id="25842-106">Define permitted virtual network gateway SKU</span></span>

<span data-ttu-id="25842-107">Följande princip begränsar vilka SKU: er kan distribueras för virtuella nätverksgatewayer:</span><span class="sxs-lookup"><span data-stu-id="25842-107">The following policy restricts which SKUs can be deployed for virtual network gateways:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="25842-108">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25842-108">Next steps</span></span>
* <span data-ttu-id="25842-109">När du definierar en regel (som visas i föregående exempel) behöver du skapar principdefinitionen och kopplar den till ett omfång.</span><span class="sxs-lookup"><span data-stu-id="25842-109">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="25842-110">Omfattningen kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="25842-110">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="25842-111">Om du vill tilldela principer via portalen finns [Använd Azure-portalen för att tilldela och hantera resursprinciper](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="25842-111">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="25842-112">Om du vill tilldela principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="25842-112">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="25842-113">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="25842-113">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

