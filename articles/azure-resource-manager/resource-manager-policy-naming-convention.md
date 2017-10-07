---
title: "aaaAzure resursprinciper för namnkonventioner | Microsoft Docs"
description: "Beskriver Azure Resource Manager principer för resource namngivningsregler."
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
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: c8384b231263fb694aed8b936a953d5c0ca31e71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a><span data-ttu-id="73c6e-103">Tillämpa principer för företagsresurser för namn och text</span><span class="sxs-lookup"><span data-stu-id="73c6e-103">Apply resource policies for names and text</span></span>
<span data-ttu-id="73c6e-104">Det här avsnittet beskrivs flera [resursprinciper](resource-manager-policy.md) kan du använda tooestablish konventioner för namngivning och text.</span><span class="sxs-lookup"><span data-stu-id="73c6e-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply tooestablish naming and text conventions.</span></span> <span data-ttu-id="73c6e-105">Dessa principer säkerställa konsekvens för namn och värden.</span><span class="sxs-lookup"><span data-stu-id="73c6e-105">These policies ensure consistency for resource names and tag values.</span></span> 

## <a name="set-naming-convention-with-wildcard"></a><span data-ttu-id="73c6e-106">Ange namngivningskonvention med jokertecken</span><span class="sxs-lookup"><span data-stu-id="73c6e-106">Set naming convention with wildcard</span></span>
<span data-ttu-id="73c6e-107">hello följande exempel visar hello användning av jokertecken, som stöds av hello **som** villkor.</span><span class="sxs-lookup"><span data-stu-id="73c6e-107">hello following example shows hello use of wildcard, which is supported by hello **like** condition.</span></span> <span data-ttu-id="73c6e-108">hello villkoret anger att om hello namn matchar hello nämnda mönstret (namePrefix\*nameSuffix) neka hello begäran:</span><span class="sxs-lookup"><span data-stu-id="73c6e-108">hello condition states that if hello name does match hello mentioned pattern (namePrefix\*nameSuffix) then deny hello request:</span></span>

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a><span data-ttu-id="73c6e-109">Ange namngivningskonvention med mönster</span><span class="sxs-lookup"><span data-stu-id="73c6e-109">Set naming convention with pattern</span></span>

<span data-ttu-id="73c6e-110">toospecify att resursnamn matchar ett mönster, Använd hello matchar villkoret.</span><span class="sxs-lookup"><span data-stu-id="73c6e-110">toospecify that resource names match a pattern, use hello match condition.</span></span> <span data-ttu-id="73c6e-111">hello följande exempel kräver namn toostart med `contoso` och innehåller ytterligare sex bokstäverna:</span><span class="sxs-lookup"><span data-stu-id="73c6e-111">hello following example requires names toostart with `contoso` and contain six additional letters:</span></span>

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a><span data-ttu-id="73c6e-112">Ange datum för Taggvärdet</span><span class="sxs-lookup"><span data-stu-id="73c6e-112">Set date pattern for tag value</span></span>

<span data-ttu-id="73c6e-113">toorequire ett datum användningsmönstret i två siffror, streck, tre bokstäver, bindestreck och fyra siffror:</span><span class="sxs-lookup"><span data-stu-id="73c6e-113">toorequire a date pattern of two digits, dash, three letters, dash, and four digits, use:</span></span>

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="73c6e-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73c6e-114">Next steps</span></span>
* <span data-ttu-id="73c6e-115">När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång.</span><span class="sxs-lookup"><span data-stu-id="73c6e-115">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="73c6e-116">hello scope kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="73c6e-116">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="73c6e-117">tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="73c6e-117">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="73c6e-118">tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="73c6e-118">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="73c6e-119">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="73c6e-119">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

