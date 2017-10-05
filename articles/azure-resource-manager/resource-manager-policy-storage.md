---
title: "Principer för Azure-resurs för lagringskonton | Microsoft Docs"
description: "Beskriver principer för Azure Resource Manager för att hantera distributionen av storage-konton."
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
ms.openlocfilehash: 6612ee61f5c50e743241b92030660cea7ae7094d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="apply-resource-policies-to-storage-accounts"></a><span data-ttu-id="252e8-103">Använda resursprinciper för storage-konton</span><span class="sxs-lookup"><span data-stu-id="252e8-103">Apply resource policies to storage accounts</span></span>
<span data-ttu-id="252e8-104">Det här avsnittet beskrivs flera [resursprinciper](resource-manager-policy.md) du kan använda Azure storage-konton.</span><span class="sxs-lookup"><span data-stu-id="252e8-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply to Azure storage accounts.</span></span> <span data-ttu-id="252e8-105">Dessa principer säkerställa konsekvens för storage-konton som har distribuerats i organisationen.</span><span class="sxs-lookup"><span data-stu-id="252e8-105">These policies ensure consistency for the storage accounts deployed in your organization.</span></span> 

## <a name="define-permitted-storage-account-types"></a><span data-ttu-id="252e8-106">Definiera tillåtna lagringskontotyper</span><span class="sxs-lookup"><span data-stu-id="252e8-106">Define permitted storage account types</span></span>

<span data-ttu-id="252e8-107">Följande princip begränsar som [lagringskontotyper](../storage/common/storage-redundancy.md) kan distribueras:</span><span class="sxs-lookup"><span data-stu-id="252e8-107">The following policy restricts which [storage account types](../storage/common/storage-redundancy.md) can be deployed:</span></span>

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

<span data-ttu-id="252e8-108">En liknande regel med en parameter för att acceptera tillåtna SKU: er är tillgängliga som en inbyggd principdefinition.</span><span class="sxs-lookup"><span data-stu-id="252e8-108">A similar policy rule with a parameter for accepting the allowed SKUs is available as a built-in policy definition.</span></span> <span data-ttu-id="252e8-109">Den inbyggda principen har resurs-ID för `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span><span class="sxs-lookup"><span data-stu-id="252e8-109">The built-in policy has the resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span></span> 

## <a name="define-permitted-access-tier"></a><span data-ttu-id="252e8-110">Definiera tillåten åtkomstnivå</span><span class="sxs-lookup"><span data-stu-id="252e8-110">Define permitted access tier</span></span>

<span data-ttu-id="252e8-111">Följande princip anger vilken typ av [åtkomstnivå](../storage/blobs/storage-blob-storage-tiers.md) som kan anges för storage-konton:</span><span class="sxs-lookup"><span data-stu-id="252e8-111">The following policy specifies the type of [access tier](../storage/blobs/storage-blob-storage-tiers.md) that can be specified for storage accounts:</span></span>

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

## <a name="ensure-encryption-is-enabled"></a><span data-ttu-id="252e8-112">Se till att kryptering är aktiverat</span><span class="sxs-lookup"><span data-stu-id="252e8-112">Ensure encryption is enabled</span></span>

<span data-ttu-id="252e8-113">Följande principen kräver att alla lagringskonton för att aktivera [lagringstjänstens kryptering](../storage/common/storage-service-encryption.md):</span><span class="sxs-lookup"><span data-stu-id="252e8-113">The following policy requires all storage accounts to enable [Storage service encryption](../storage/common/storage-service-encryption.md):</span></span>

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

<span data-ttu-id="252e8-114">Den här principregeln är också tillgängliga som en inbyggd principdefinitionen med resurs-ID för `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span><span class="sxs-lookup"><span data-stu-id="252e8-114">This policy rule is also available as a built-in policy definition with the resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="252e8-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="252e8-115">Next steps</span></span>
* <span data-ttu-id="252e8-116">När du definierar en regel (som visas i föregående exempel) behöver du skapar principdefinitionen och kopplar den till ett omfång.</span><span class="sxs-lookup"><span data-stu-id="252e8-116">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="252e8-117">Omfattningen kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="252e8-117">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="252e8-118">Om du vill tilldela principer via portalen finns [Använd Azure-portalen för att tilldela och hantera resursprinciper](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="252e8-118">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="252e8-119">Om du vill tilldela principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="252e8-119">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="252e8-120">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="252e8-120">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

