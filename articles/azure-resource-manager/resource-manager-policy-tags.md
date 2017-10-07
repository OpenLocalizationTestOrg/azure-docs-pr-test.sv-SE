---
title: "aaaAzure resursprinciper för taggar | Microsoft Docs"
description: "Ger exempel på resursprinciper för hantering av taggar för resurser"
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
ms.date: 08/24/2017
ms.author: tomfitz
ms.openlocfilehash: 5a5b3d5ed52b47544b397694b9da0070f61b1faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-for-tags"></a><span data-ttu-id="2e7df-103">Tillämpa principer för företagsresurser för taggar</span><span class="sxs-lookup"><span data-stu-id="2e7df-103">Apply resource policies for tags</span></span>

<span data-ttu-id="2e7df-104">Det här avsnittet innehåller gemensamma regler som du kan använda tooensure konsekvent användning av taggar på resurser.</span><span class="sxs-lookup"><span data-stu-id="2e7df-104">This topic provides common policy rules you can apply tooensure consistent use of tags on resources.</span></span>

<span data-ttu-id="2e7df-105">Tillämpa en tagg princip tooa resursgrupp eller prenumeration med befintliga resurser inte applicera hello princip toothose resurser.</span><span class="sxs-lookup"><span data-stu-id="2e7df-105">Applying a tag policy tooa resource group or subscription with existing resources does not retroactively apply hello policy toothose resources.</span></span> <span data-ttu-id="2e7df-106">tooenforce hello principer på dessa resurser, utlöser en uppdatering toohello befintliga resurser.</span><span class="sxs-lookup"><span data-stu-id="2e7df-106">tooenforce hello policies on those resources, trigger an update toohello existing resources.</span></span> <span data-ttu-id="2e7df-107">Den här artikeln innehåller ett PowerShell-exempel för att utlösa en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="2e7df-107">This article includes a PowerShell example for triggering an update.</span></span>

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a><span data-ttu-id="2e7df-108">Kontrollera att alla resurser i en resursgrupp har ett Taggvärde</span><span class="sxs-lookup"><span data-stu-id="2e7df-108">Ensure all resources in a resource group have a tag/value</span></span>

<span data-ttu-id="2e7df-109">Ett vanligt krav är att alla resurser i en resursgrupp har en viss tagg och värde.</span><span class="sxs-lookup"><span data-stu-id="2e7df-109">A common requirement is that all resources in a resource group have a particular tag and value.</span></span> <span data-ttu-id="2e7df-110">Det här kravet är ofta nödvändiga tootrack kostnader per avdelning.</span><span class="sxs-lookup"><span data-stu-id="2e7df-110">This requirement is often needed tootrack costs by department.</span></span> <span data-ttu-id="2e7df-111">hello följande villkor uppfyllas:</span><span class="sxs-lookup"><span data-stu-id="2e7df-111">hello following conditions must be met:</span></span>

* <span data-ttu-id="2e7df-112">hello krävs taggen och värdet är tillagda toonew och uppdatera resurser som inte har hello-tagg.</span><span class="sxs-lookup"><span data-stu-id="2e7df-112">hello required tag and value are appended toonew and updated resources that do not have hello tag.</span></span>
* <span data-ttu-id="2e7df-113">hello krävs taggen och värde kan inte tas bort från alla befintliga resurser.</span><span class="sxs-lookup"><span data-stu-id="2e7df-113">hello required tag and value cannot be removed from any existing resources.</span></span>

<span data-ttu-id="2e7df-114">Du kan göra det här kravet genom att använda två inbyggda principer tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2e7df-114">You accomplish this requirement by applying two built-in policies tooa resource group.</span></span>

| <span data-ttu-id="2e7df-115">ID</span><span class="sxs-lookup"><span data-stu-id="2e7df-115">ID</span></span> | <span data-ttu-id="2e7df-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2e7df-116">Description</span></span> |
| ---- | ---- |
| <span data-ttu-id="2e7df-117">2a0e14a6-b0a6-4fab-991a-187a4f81c498</span><span class="sxs-lookup"><span data-stu-id="2e7df-117">2a0e14a6-b0a6-4fab-991a-187a4f81c498</span></span> | <span data-ttu-id="2e7df-118">Gäller obligatoriska taggen med sitt standardvärde när det inte anges av hello användare.</span><span class="sxs-lookup"><span data-stu-id="2e7df-118">Applies a required tag and its default value when it is not specified by hello user.</span></span> |
| <span data-ttu-id="2e7df-119">1e30110a-5ceb-460c-a204-c1c3969c6d62</span><span class="sxs-lookup"><span data-stu-id="2e7df-119">1e30110a-5ceb-460c-a204-c1c3969c6d62</span></span> | <span data-ttu-id="2e7df-120">Tillämpar en nödvändig tagg och dess värde.</span><span class="sxs-lookup"><span data-stu-id="2e7df-120">Enforces a required tag and its value.</span></span> |

### <a name="powershell"></a><span data-ttu-id="2e7df-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e7df-121">PowerShell</span></span>

<span data-ttu-id="2e7df-122">hello följande PowerShell-skript tilldelar hello två inbyggda princip definitioner tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2e7df-122">hello following PowerShell script assigns hello two built-in policy definitions tooa resource group.</span></span> <span data-ttu-id="2e7df-123">Innan du kör skriptet hello tilldela alla taggar som krävs toohello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2e7df-123">Before running hello script, assign all required tags toohello resource group.</span></span> <span data-ttu-id="2e7df-124">Varje tagg på hello resursgruppens namn måste anges för hello resurser i hello grupp.</span><span class="sxs-lookup"><span data-stu-id="2e7df-124">Each tag on hello resource group is required for hello resources in hello group.</span></span> <span data-ttu-id="2e7df-125">tooassign tooall resursgrupper i din prenumeration, ger inte hello `-Name` parameter när du hämtar hello resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="2e7df-125">tooassign tooall resource groups in your subscription, do not provide hello `-Name` parameter when getting hello resource groups.</span></span>

```powershell
$appendpolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '2a0e14a6-b0a6-4fab-991a-187a4f81c498'}
$denypolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '1e30110a-5ceb-460c-a204-c1c3969c6d62'}

$rgs = Get-AzureRMResourceGroup -Name ExampleGroup

foreach($rg in $rgs)
{
    $tags = $rg.Tags
    foreach($key in $tags.Keys){
        $key 
        $tags[$key]
        New-AzureRmPolicyAssignment -Name ("append"+$key+"tag") -PolicyDefinition $appendpolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
        New-AzureRmPolicyAssignment -Name ("denywithout"+$key+"tag") -PolicyDefinition $denypolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
    }
}
```

<span data-ttu-id="2e7df-126">Du kan utlösa en uppdatering tooall befintliga resurser tooenforce hello taggen principer som du har lagt till efter att hello principer.</span><span class="sxs-lookup"><span data-stu-id="2e7df-126">After assigning hello policies, you can trigger an update tooall existing resources tooenforce hello tag policies you have added.</span></span> <span data-ttu-id="2e7df-127">hello behåller följande skript andra taggar som fanns på hello resurser:</span><span class="sxs-lookup"><span data-stu-id="2e7df-127">hello following script retains any other tags that existed on hello resources:</span></span>

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```

## <a name="require-tags-for-a-resource-type"></a><span data-ttu-id="2e7df-128">Kräv taggar för en resurstyp</span><span class="sxs-lookup"><span data-stu-id="2e7df-128">Require tags for a resource type</span></span>
<span data-ttu-id="2e7df-129">hello som följande exempel visar hur toonest logiska operatorer toorequire ett program tagga för endast en viss resurstyp (i det här fallet, storage-konton).</span><span class="sxs-lookup"><span data-stu-id="2e7df-129">hello following example shows how toonest logical operators toorequire an application tag for only a specified resource type (in this case, storage accounts).</span></span>

```json
{
    "if": {
        "allOf": [
          {
            "not": {
              "field": "tags",
              "containsKey": "application"
            }
          },
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          }
        ]
    },
    "then": {
        "effect": "audit"
    }
}
```

## <a name="require-tag"></a><span data-ttu-id="2e7df-130">Kräv tagg</span><span class="sxs-lookup"><span data-stu-id="2e7df-130">Require tag</span></span>
<span data-ttu-id="2e7df-131">hello nekar följande princip förfrågningar som inte har en tagg som innehåller ”costCenter” nyckel (valfritt värde kan tillämpas):</span><span class="sxs-lookup"><span data-stu-id="2e7df-131">hello following policy denies requests that don't have a tag containing "costCenter" key (any value can be applied):</span></span>

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="2e7df-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e7df-132">Next steps</span></span>
* <span data-ttu-id="2e7df-133">När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång.</span><span class="sxs-lookup"><span data-stu-id="2e7df-133">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="2e7df-134">hello scope kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="2e7df-134">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="2e7df-135">tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2e7df-135">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="2e7df-136">tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="2e7df-136">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="2e7df-137">En introduktion tooresource principer finns i [resurs uppgifter](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="2e7df-137">For an introduction tooresource policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="2e7df-138">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="2e7df-138">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

