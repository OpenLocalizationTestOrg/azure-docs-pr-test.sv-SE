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
# <a name="apply-resource-policies-toonetwork-resources"></a>Tillämpa resurs principer toonetwork resurser
Den här artikeln visar ett exempel [resursprincip](resource-manager-policy.md) kan du använda tooAzure virtuella nätverksgatewayerna. Den här principen garanterar konsekvens för gateways som distribuerats i organisationen. 

## <a name="define-permitted-virtual-network-gateway-sku"></a>Definiera tillåtna virtuell nätverksgateway SKU

hello följa principen begränsar vilka SKU: er kan distribueras för virtuella nätverksgatewayer:

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

## <a name="next-steps"></a>Nästa steg
* När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång. hello scope kan vara en prenumeration, resursgrupp eller resurs. tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md). tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md). 
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

