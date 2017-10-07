---
title: "aaaAzure resursprinciper för lagringskonton | Microsoft Docs"
description: "Beskriver principer för Azure Resource Manager för att hantera hello distribution av storage-konton."
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
ms.openlocfilehash: d37fc4bcf7cdec71b0e14f6231fc138bfb6a7893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-toostorage-accounts"></a>Tillämpa principer toostorage för resurskonton
Det här avsnittet beskrivs flera [resursprinciper](resource-manager-policy.md) kan du använda tooAzure storage-konton. Dessa principer säkerställa konsekvens för hello storage-konton distribueras i organisationen. 

## <a name="define-permitted-storage-account-types"></a>Definiera tillåtna lagringskontotyper

hello följande princip begränsar som [lagringskontotyper](../storage/common/storage-redundancy.md) kan distribueras:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/sku.name",
          "in": [
            "Standard_LRS",
            "Standard_GRS"
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

En liknande regel med en parameter för att acceptera hello tillåtna SKU: er är tillgängliga som en inbyggd principdefinition. hello inbyggda principen har hello resurs-ID för `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`. 

## <a name="define-permitted-access-tier"></a>Definiera tillåten åtkomstnivå

hello följande princip anger hello [åtkomstnivå](../storage/blobs/storage-blob-storage-tiers.md) som kan anges för storage-konton:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="ensure-encryption-is-enabled"></a>Se till att kryptering är aktiverat

hello följande principen kräver att alla storage-konton tooenable [lagringstjänstens kryptering](../storage/common/storage-service-encryption.md):

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/enableBlobEncryption",
          "equals": "true"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Den här principregeln är också tillgängliga som en inbyggd principdefinitionen med hello resurs-ID för `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.

## <a name="next-steps"></a>Nästa steg
* När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång. hello scope kan vara en prenumeration, resursgrupp eller resurs. tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md). tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md). 
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

