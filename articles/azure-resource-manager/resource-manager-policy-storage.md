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
# <a name="apply-resource-policies-toostorage-accounts"></a><span data-ttu-id="b8187-103">Tillämpa principer toostorage för resurskonton</span><span class="sxs-lookup"><span data-stu-id="b8187-103">Apply resource policies toostorage accounts</span></span>
<span data-ttu-id="b8187-104">Det här avsnittet beskrivs flera [resursprinciper](resource-manager-policy.md) kan du använda tooAzure storage-konton.</span><span class="sxs-lookup"><span data-stu-id="b8187-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply tooAzure storage accounts.</span></span> <span data-ttu-id="b8187-105">Dessa principer säkerställa konsekvens för hello storage-konton distribueras i organisationen.</span><span class="sxs-lookup"><span data-stu-id="b8187-105">These policies ensure consistency for hello storage accounts deployed in your organization.</span></span> 

## <a name="define-permitted-storage-account-types"></a><span data-ttu-id="b8187-106">Definiera tillåtna lagringskontotyper</span><span class="sxs-lookup"><span data-stu-id="b8187-106">Define permitted storage account types</span></span>

<span data-ttu-id="b8187-107">hello följande princip begränsar som [lagringskontotyper](../storage/common/storage-redundancy.md) kan distribueras:</span><span class="sxs-lookup"><span data-stu-id="b8187-107">hello following policy restricts which [storage account types](../storage/common/storage-redundancy.md) can be deployed:</span></span>

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

<span data-ttu-id="b8187-108">En liknande regel med en parameter för att acceptera hello tillåtna SKU: er är tillgängliga som en inbyggd principdefinition.</span><span class="sxs-lookup"><span data-stu-id="b8187-108">A similar policy rule with a parameter for accepting hello allowed SKUs is available as a built-in policy definition.</span></span> <span data-ttu-id="b8187-109">hello inbyggda principen har hello resurs-ID för `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span><span class="sxs-lookup"><span data-stu-id="b8187-109">hello built-in policy has hello resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span></span> 

## <a name="define-permitted-access-tier"></a><span data-ttu-id="b8187-110">Definiera tillåten åtkomstnivå</span><span class="sxs-lookup"><span data-stu-id="b8187-110">Define permitted access tier</span></span>

<span data-ttu-id="b8187-111">hello följande princip anger hello [åtkomstnivå](../storage/blobs/storage-blob-storage-tiers.md) som kan anges för storage-konton:</span><span class="sxs-lookup"><span data-stu-id="b8187-111">hello following policy specifies hello type of [access tier](../storage/blobs/storage-blob-storage-tiers.md) that can be specified for storage accounts:</span></span>

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

## <a name="ensure-encryption-is-enabled"></a><span data-ttu-id="b8187-112">Se till att kryptering är aktiverat</span><span class="sxs-lookup"><span data-stu-id="b8187-112">Ensure encryption is enabled</span></span>

<span data-ttu-id="b8187-113">hello följande principen kräver att alla storage-konton tooenable [lagringstjänstens kryptering](../storage/common/storage-service-encryption.md):</span><span class="sxs-lookup"><span data-stu-id="b8187-113">hello following policy requires all storage accounts tooenable [Storage service encryption](../storage/common/storage-service-encryption.md):</span></span>

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

<span data-ttu-id="b8187-114">Den här principregeln är också tillgängliga som en inbyggd principdefinitionen med hello resurs-ID för `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span><span class="sxs-lookup"><span data-stu-id="b8187-114">This policy rule is also available as a built-in policy definition with hello resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8187-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b8187-115">Next steps</span></span>
* <span data-ttu-id="b8187-116">När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång.</span><span class="sxs-lookup"><span data-stu-id="b8187-116">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="b8187-117">hello scope kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="b8187-117">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="b8187-118">tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b8187-118">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="b8187-119">tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="b8187-119">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="b8187-120">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="b8187-120">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

