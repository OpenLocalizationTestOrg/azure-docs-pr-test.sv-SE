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
# <a name="apply-resource-policies-for-names-and-text"></a>Tillämpa principer för företagsresurser för namn och text
Det här avsnittet beskrivs flera [resursprinciper](resource-manager-policy.md) kan du använda tooestablish konventioner för namngivning och text. Dessa principer säkerställa konsekvens för namn och värden. 

## <a name="set-naming-convention-with-wildcard"></a>Ange namngivningskonvention med jokertecken
hello följande exempel visar hello användning av jokertecken, som stöds av hello **som** villkor. hello villkoret anger att om hello namn matchar hello nämnda mönstret (namePrefix\*nameSuffix) neka hello begäran:

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

## <a name="set-naming-convention-with-pattern"></a>Ange namngivningskonvention med mönster

toospecify att resursnamn matchar ett mönster, Använd hello matchar villkoret. hello följande exempel kräver namn toostart med `contoso` och innehåller ytterligare sex bokstäverna:

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

## <a name="set-date-pattern-for-tag-value"></a>Ange datum för Taggvärdet

toorequire ett datum användningsmönstret i två siffror, streck, tre bokstäver, bindestreck och fyra siffror:

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

## <a name="next-steps"></a>Nästa steg
* När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång. hello scope kan vara en prenumeration, resursgrupp eller resurs. tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md). tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md). 
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

