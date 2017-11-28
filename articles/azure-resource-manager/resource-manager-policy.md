---
title: aaaAzure resursprinciper | Microsoft Docs
description: "Beskriver hur toouse Azure Resource Manager konsekvent resursegenskaper för principer tooensure anges under distributionen. Principer kan tillämpas på hello prenumerationen eller resursen grupper."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: tomfitz
ms.openlocfilehash: f1b0bbb5f838f6bb70721e1040ad3eac2d881cea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-policy-overview"></a><span data-ttu-id="c5c3c-104">Översikt över princip för resurs</span><span class="sxs-lookup"><span data-stu-id="c5c3c-104">Resource policy overview</span></span>
<span data-ttu-id="c5c3c-105">Resursprinciper aktivera tooestablish konventioner för resurser i din organisation.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-105">Resource policies enable you tooestablish conventions for resources in your organization.</span></span> <span data-ttu-id="c5c3c-106">Du kan styra kostnader genom att definiera konventioner och mer hantera enkelt dina resurser.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-106">By defining conventions, you can control costs and more easily manage your resources.</span></span> <span data-ttu-id="c5c3c-107">Du kan till exempel ange att endast vissa typer av virtuella datorer är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-107">For example, you can specify that only certain types of virtual machines are allowed.</span></span> <span data-ttu-id="c5c3c-108">Eller, du kan kräva att alla resurser som har en viss tagg.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-108">Or, you can require that all resources have a particular tag.</span></span> <span data-ttu-id="c5c3c-109">Principer ärvs av alla underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-109">Policies are inherited by all child resources.</span></span> <span data-ttu-id="c5c3c-110">Så om en princip är tillämpade tooa resursgrupp, är det tillämpliga tooall hello resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-110">So, if a policy is applied tooa resource group, it is applicable tooall hello resources in that resource group.</span></span>

<span data-ttu-id="c5c3c-111">Det finns två begrepp toounderstand om principer:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-111">There are two concepts toounderstand about policies:</span></span>

* <span data-ttu-id="c5c3c-112">principdefinitionen - beskrivs när hello tillämpas och vilken åtgärd tootake</span><span class="sxs-lookup"><span data-stu-id="c5c3c-112">policy definition - you describe when hello policy is enforced and what action tootake</span></span>
* <span data-ttu-id="c5c3c-113">tilldelning av principer - installation av hello definition tooa principområdet (prenumeration eller resursgrupp)</span><span class="sxs-lookup"><span data-stu-id="c5c3c-113">policy assignment - you apply hello policy definition tooa scope (subscription or resource group)</span></span>

<span data-ttu-id="c5c3c-114">Det här avsnittet fokuserar på principdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-114">This topic focuses on policy definition.</span></span> <span data-ttu-id="c5c3c-115">Information om tilldelning av principer finns [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md) eller [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-115">For information about policy assignment, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md) or [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>

<span data-ttu-id="c5c3c-116">Principer utvärderas när du skapar och uppdaterar resurser (PLACERA och korrigering operations).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-116">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

> [!NOTE]
> <span data-ttu-id="c5c3c-117">Principen utvärderas för närvarande inte resurstyper som inte stöder taggar, typ och plats, till exempel hello Microsoft.Resources/deployments resurstypen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-117">Currently, policy does not evaluate resource types that do not support tags, kind, and location, such as hello Microsoft.Resources/deployments resource type.</span></span> <span data-ttu-id="c5c3c-118">Det här stödet läggs vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-118">This support will be added at a future time.</span></span> <span data-ttu-id="c5c3c-119">tooavoid bakåtkompatibilitet problem bör du uttryckligen ange typen vid redigering av principer.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-119">tooavoid backward compatibility issues, you should explicitly specify type when authoring policies.</span></span> <span data-ttu-id="c5c3c-120">Till exempel används taggen princip som inte anger typer för alla typer.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-120">For example, a tag policy that does not specify types is applied for all types.</span></span> <span data-ttu-id="c5c3c-121">I så fall kan en för malldistribution kan misslyckas om det finns en kapslad resurs som inte stöder taggar och hello distributionstypen resursen har lagts toopolicy utvärdering.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-121">In that case, a template deployment may fail if there is a nested resource that doesn't support tags, and hello deployment resource type has been added toopolicy evaluation.</span></span> 
> 
> 

## <a name="how-is-it-different-from-rbac"></a><span data-ttu-id="c5c3c-122">Hur skiljer den från RBAC?</span><span class="sxs-lookup"><span data-stu-id="c5c3c-122">How is it different from RBAC?</span></span>
<span data-ttu-id="c5c3c-123">Det finns några viktiga skillnader mellan principen och rollbaserad åtkomstkontroll (RBAC).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-123">There are a few key differences between policy and role-based access control (RBAC).</span></span> <span data-ttu-id="c5c3c-124">RBAC fokuserar på **användaren** åtgärder på olika omfång.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-124">RBAC focuses on **user** actions at different scopes.</span></span> <span data-ttu-id="c5c3c-125">Till exempel läggs du toohello deltagarrollen för en resursgrupp hello önskad definitionsområdet, så att du kan göra ändringar toothat resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-125">For example, you are added toohello contributor role for a resource group at hello desired scope, so you can make changes toothat resource group.</span></span> <span data-ttu-id="c5c3c-126">Principen fokuserar på **resurs** egenskaper under distributionen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-126">Policy focuses on **resource** properties during deployment.</span></span> <span data-ttu-id="c5c3c-127">Du kan till exempel styra hello typer av resurser som kan etableras via principer.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-127">For example, through policies, you can control hello types of resources that can be provisioned.</span></span> <span data-ttu-id="c5c3c-128">Eller så kan du begränsa hello platser där hello resurser kan etableras.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-128">Or, you can restrict hello locations in which hello resources can be provisioned.</span></span> <span data-ttu-id="c5c3c-129">Till skillnad från RBAC, principen är en standard Tillåt och explicit neka system.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-129">Unlike RBAC, policy is a default allow and explicit deny system.</span></span> 

<span data-ttu-id="c5c3c-130">toouse principer kan autentiseras via RBAC.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-130">toouse policies, you must be authenticated through RBAC.</span></span> <span data-ttu-id="c5c3c-131">Mer specifikt måste ditt konto den:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-131">Specifically, your account needs the:</span></span>

* <span data-ttu-id="c5c3c-132">`Microsoft.Authorization/policydefinitions/write`behörighet toodefine en princip</span><span class="sxs-lookup"><span data-stu-id="c5c3c-132">`Microsoft.Authorization/policydefinitions/write` permission toodefine a policy</span></span>
* <span data-ttu-id="c5c3c-133">`Microsoft.Authorization/policyassignments/write`behörighet tooassign en princip</span><span class="sxs-lookup"><span data-stu-id="c5c3c-133">`Microsoft.Authorization/policyassignments/write` permission tooassign a policy</span></span> 

<span data-ttu-id="c5c3c-134">Dessa behörigheter ingår inte i hello **deltagare** roll.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-134">These permissions are not included in hello **Contributor** role.</span></span>

## <a name="built-in-policies"></a><span data-ttu-id="c5c3c-135">Inbyggda principer</span><span class="sxs-lookup"><span data-stu-id="c5c3c-135">Built-in policies</span></span>

<span data-ttu-id="c5c3c-136">Azure tillhandahåller vissa inbyggda principdefinitioner som kan minska antalet hello av principer som du har toodefine.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-136">Azure provides some built-in policy definitions that may reduce hello number of policies you have toodefine.</span></span> <span data-ttu-id="c5c3c-137">Innan du fortsätter med principdefinitioner, bör du överväga om hello paketdefinition innehåller redan en inbyggd princip.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-137">Before proceeding with policy definitions, you should consider whether a built-in policy already provides hello definition you need.</span></span> <span data-ttu-id="c5c3c-138">hello inbyggda principdefinitioner är:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-138">hello built-in policy definitions are:</span></span>

* <span data-ttu-id="c5c3c-139">Tillåtna platser</span><span class="sxs-lookup"><span data-stu-id="c5c3c-139">Allowed locations</span></span>
* <span data-ttu-id="c5c3c-140">Tillåtna resurstyper</span><span class="sxs-lookup"><span data-stu-id="c5c3c-140">Allowed resource types</span></span>
* <span data-ttu-id="c5c3c-141">Tillåtna lagringskonto SKU: er</span><span class="sxs-lookup"><span data-stu-id="c5c3c-141">Allowed storage account SKUs</span></span>
* <span data-ttu-id="c5c3c-142">Tillåtna virtuella SKU: er</span><span class="sxs-lookup"><span data-stu-id="c5c3c-142">Allowed virtual machine SKUs</span></span>
* <span data-ttu-id="c5c3c-143">Värdet för taggen och standard</span><span class="sxs-lookup"><span data-stu-id="c5c3c-143">Apply tag and default value</span></span>
* <span data-ttu-id="c5c3c-144">Framtvinga taggen och värdet</span><span class="sxs-lookup"><span data-stu-id="c5c3c-144">Enforce tag and value</span></span>
* <span data-ttu-id="c5c3c-145">Inte tillåtet resurstyper</span><span class="sxs-lookup"><span data-stu-id="c5c3c-145">Not allowed resource types</span></span>
* <span data-ttu-id="c5c3c-146">Kräv SQL Server version 12.0</span><span class="sxs-lookup"><span data-stu-id="c5c3c-146">Require SQL Server version 12.0</span></span>
* <span data-ttu-id="c5c3c-147">Kräv kryptering för storage-konto</span><span class="sxs-lookup"><span data-stu-id="c5c3c-147">Require storage account encryption</span></span>

<span data-ttu-id="c5c3c-148">Du kan tilldela någon av dessa policys via hello [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), eller [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-148">You can assign any of these policies through hello [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), or [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).</span></span>

## <a name="policy-definition-structure"></a><span data-ttu-id="c5c3c-149">Definition av principstruktur</span><span class="sxs-lookup"><span data-stu-id="c5c3c-149">Policy definition structure</span></span>
<span data-ttu-id="c5c3c-150">Du kan använda JSON toocreate en principdefinition.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-150">You use JSON toocreate a policy definition.</span></span> <span data-ttu-id="c5c3c-151">hello principdefinitionen innehåller element för:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-151">hello policy definition contains elements for:</span></span>

* <span data-ttu-id="c5c3c-152">parameters</span><span class="sxs-lookup"><span data-stu-id="c5c3c-152">parameters</span></span>
* <span data-ttu-id="c5c3c-153">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="c5c3c-153">display name</span></span>
* <span data-ttu-id="c5c3c-154">description</span><span class="sxs-lookup"><span data-stu-id="c5c3c-154">description</span></span>
* <span data-ttu-id="c5c3c-155">Principregel</span><span class="sxs-lookup"><span data-stu-id="c5c3c-155">policy rule</span></span>
  * <span data-ttu-id="c5c3c-156">logiska utvärdering</span><span class="sxs-lookup"><span data-stu-id="c5c3c-156">logical evaluation</span></span>
  * <span data-ttu-id="c5c3c-157">effekt</span><span class="sxs-lookup"><span data-stu-id="c5c3c-157">effect</span></span>

<span data-ttu-id="c5c3c-158">hello som följande exempel visar en princip som begränsar där resurser har distribuerats:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-158">hello following example shows a policy that limits where resources are deployed:</span></span>

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="parameters"></a><span data-ttu-id="c5c3c-159">Parametrar</span><span class="sxs-lookup"><span data-stu-id="c5c3c-159">Parameters</span></span>
<span data-ttu-id="c5c3c-160">Med parametrar som förenklar din för principhantering genom att minska hello antal principdefinitioner.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-160">Using parameters helps simplify your policy management by reducing hello number of policy definitions.</span></span> <span data-ttu-id="c5c3c-161">Definiera en princip för en resursegenskap (till exempel begränsa hello platser där resurser kan distribueras) och inkludera parametrar i hello definition.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-161">You define a policy for a resource property (such as limiting hello locations where resources can be deployed), and include parameters in hello definition.</span></span> <span data-ttu-id="c5c3c-162">Sedan kan du återanvända den principdefinitionen för olika scenarier genom att passera i olika värden (t.ex att ange en uppsättning platser för en prenumeration) när tilldelar hello principer.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-162">Then, you reuse that policy definition for different scenarios by passing in different values (such as specifying one set of locations for a subscription) when assigning hello policy.</span></span>

<span data-ttu-id="c5c3c-163">Du kan ange parametrar när du skapar principdefinitioner.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-163">You declare parameters when you create policy definitions.</span></span>

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "hello list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

<span data-ttu-id="c5c3c-164">hello-typ för en parameter kan vara antingen sträng eller matris.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-164">hello type of a parameter can be either string or array.</span></span> <span data-ttu-id="c5c3c-165">Hej metadataegenskapen används för verktyg som Azure portal toodisplay användarvänliga information.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-165">hello metadata property is used for tools like Azure portal toodisplay user-friendly information.</span></span> 

<span data-ttu-id="c5c3c-166">I hello principregeln referera parametrar med hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-166">In hello policy rule, you reference parameters with hello following syntax:</span></span> 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a><span data-ttu-id="c5c3c-167">Namn och beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-167">Display name and description</span></span>

<span data-ttu-id="c5c3c-168">Du använder hello **displayName** och **beskrivning** tooidentify hello principdefinitionen och ge kontext för när den används.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-168">You use hello **displayName** and **description** tooidentify hello policy definition, and provide context for when it is used.</span></span>

## <a name="policy-rule"></a><span data-ttu-id="c5c3c-169">Principregel</span><span class="sxs-lookup"><span data-stu-id="c5c3c-169">Policy rule</span></span>

<span data-ttu-id="c5c3c-170">hello principregeln består av **om** och **sedan** block.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-170">hello policy rule consists of **If** and **Then** blocks.</span></span> <span data-ttu-id="c5c3c-171">I hello **om** block du definierar en eller flera villkor som anger när hello tillämpas.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-171">In hello **If** block, you define one or more conditions that specify when hello policy is enforced.</span></span> <span data-ttu-id="c5c3c-172">Du kan använda logiska operatorer toothese villkor tooprecisely definiera hello scenario för en princip.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-172">You can apply logical operators toothese conditions tooprecisely define hello scenario for a policy.</span></span> <span data-ttu-id="c5c3c-173">I hello **sedan** block, definierar du hello effekt som händer när hello **om** villkor är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-173">In hello **Then** block, you define hello effect that happens when hello **If** conditions are fulfilled.</span></span>

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a><span data-ttu-id="c5c3c-174">Logiska operatorer</span><span class="sxs-lookup"><span data-stu-id="c5c3c-174">Logical operators</span></span>
<span data-ttu-id="c5c3c-175">hello stöds logiska operatorer är:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-175">hello supported logical operators are:</span></span>

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

<span data-ttu-id="c5c3c-176">Hej **inte** syntax inverterar hello resultatet av hello villkor.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-176">hello **not** syntax inverts hello result of hello condition.</span></span> <span data-ttu-id="c5c3c-177">Hej **allOf** syntax (liknande toohello logiska **och** åtgärden) kräver att alla villkor toobe är true.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-177">hello **allOf** syntax (similar toohello logical **And** operation) requires all conditions toobe true.</span></span> <span data-ttu-id="c5c3c-178">Hej **anyOf** syntax (liknande toohello logiska **eller** åtgärden) kräver en eller flera villkor toobe true.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-178">hello **anyOf** syntax (similar toohello logical **Or** operation) requires one or more conditions toobe true.</span></span>

<span data-ttu-id="c5c3c-179">Du kan kapsla logiska operatorer.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-179">You can nest logical operators.</span></span> <span data-ttu-id="c5c3c-180">följande exempel visar hello en **inte** åtgärden som är kapslad i en **allOf** igen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-180">hello following example shows a **not** operation that is nested within an **allOf** operation.</span></span> 

```json
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
```

### <a name="conditions"></a><span data-ttu-id="c5c3c-181">Villkor</span><span class="sxs-lookup"><span data-stu-id="c5c3c-181">Conditions</span></span>
<span data-ttu-id="c5c3c-182">hello villkoret utvärderas om en **fältet** uppfyller vissa villkor.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-182">hello condition evaluates whether a **field** meets certain criteria.</span></span> <span data-ttu-id="c5c3c-183">villkor för hello som stöds är:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-183">hello supported conditions are:</span></span>

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

<span data-ttu-id="c5c3c-184">När du använder hello **som** tillstånd, kan du ange ett jokertecken (*) i hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-184">When using hello **like** condition, you can provide a wildcard (*) in hello value.</span></span>

<span data-ttu-id="c5c3c-185">När du använder hello **matchar** villkor, ange `#` toorepresent en siffra `?` för en bokstav och andra tecken toorepresent faktiska tecknet.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-185">When using hello **match** condition, provide `#` toorepresent a digit, `?` for a letter, and any other character toorepresent that actual character.</span></span> <span data-ttu-id="c5c3c-186">Exempel finns i [gäller resursprinciper för namn och text](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-186">For examples, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>

### <a name="fields"></a><span data-ttu-id="c5c3c-187">Fält</span><span class="sxs-lookup"><span data-stu-id="c5c3c-187">Fields</span></span>
<span data-ttu-id="c5c3c-188">Villkor bildas genom att använda fält.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-188">Conditions are formed by using fields.</span></span> <span data-ttu-id="c5c3c-189">Ett fält representerar egenskaper i nyttolasten för hello resursen begäran som används toodescribe hello tillstånd hello resurs.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-189">A field represents properties in hello resource request payload that is used toodescribe hello state of hello resource.</span></span>  

<span data-ttu-id="c5c3c-190">följande fält hello stöds:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-190">hello following fields are supported:</span></span>

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* <span data-ttu-id="c5c3c-191">Egenskapen alias - lista, se [alias](#aliases).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-191">property aliases - for a list, see [Aliases](#aliases).</span></span>

### <a name="effect"></a><span data-ttu-id="c5c3c-192">Verkan</span><span class="sxs-lookup"><span data-stu-id="c5c3c-192">Effect</span></span>
<span data-ttu-id="c5c3c-193">Stöder tre typer av effekt - `deny`, `audit`, och `append`.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-193">Policy supports three types of effect - `deny`, `audit`, and `append`.</span></span> 

* <span data-ttu-id="c5c3c-194">**Neka** genererar en händelse i hello granskningsloggen och misslyckas hello begäran</span><span class="sxs-lookup"><span data-stu-id="c5c3c-194">**Deny** generates an event in hello audit log and fails hello request</span></span>
* <span data-ttu-id="c5c3c-195">**Granska** genererar en varning-händelse i granskningsloggen men inte misslyckas hello begäran</span><span class="sxs-lookup"><span data-stu-id="c5c3c-195">**Audit** generates a warning event in audit log but does not fail hello request</span></span>
* <span data-ttu-id="c5c3c-196">**Lägg till** lägger till hello definierad uppsättning fält toohello begäran</span><span class="sxs-lookup"><span data-stu-id="c5c3c-196">**Append** adds hello defined set of fields toohello request</span></span> 

<span data-ttu-id="c5c3c-197">För **bifoga**, måste du ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-197">For **append**, you must provide hello following details:</span></span>

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of hello field"
  }
]
```

<span data-ttu-id="c5c3c-198">hello-värdet kan vara en sträng eller ett JSON-format-objekt.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-198">hello value can be either a string or a JSON format object.</span></span> 

## <a name="aliases"></a><span data-ttu-id="c5c3c-199">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-199">Aliases</span></span>

<span data-ttu-id="c5c3c-200">Du kan använda egenskapen alias tooaccess specifika egenskaper för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-200">You use property aliases tooaccess specific properties for a resource type.</span></span> <span data-ttu-id="c5c3c-201">Alias kan du toorestrict vilka värden eller villkor tillåts för en egenskap för en resurs.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-201">Aliases enable you toorestrict what values or conditions are permitted for a property on a resource.</span></span> <span data-ttu-id="c5c3c-202">Varje alias mappar toopaths i olika API-versioner för en viss resurstyp.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-202">Each alias maps toopaths in different API versions for a given resource type.</span></span> <span data-ttu-id="c5c3c-203">Under principutvärdering av hämtar hello principmodulen hello egenskapssökvägen för den API-versionen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-203">During policy evaluation, hello policy engine gets hello property path for that API version.</span></span>

<span data-ttu-id="c5c3c-204">**Microsoft.Cache/Redis**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-204">**Microsoft.Cache/Redis**</span></span>

| <span data-ttu-id="c5c3c-205">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-205">Alias</span></span> | <span data-ttu-id="c5c3c-206">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-206">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-207">Microsoft.Cache/Redis/enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="c5c3c-207">Microsoft.Cache/Redis/enableNonSslPort</span></span> | <span data-ttu-id="c5c3c-208">Ange är om hello ssl-Redis-servern port (6379) aktiverat.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-208">Set whether hello non-ssl Redis server port (6379) is enabled.</span></span> |
| <span data-ttu-id="c5c3c-209">Microsoft.Cache/Redis/shardCount</span><span class="sxs-lookup"><span data-stu-id="c5c3c-209">Microsoft.Cache/Redis/shardCount</span></span> | <span data-ttu-id="c5c3c-210">Ange hello antalet shards toobe skapas på en Premium klustret Cache.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-210">Set hello number of shards toobe created on a Premium Cluster Cache.</span></span>  |
| <span data-ttu-id="c5c3c-211">Microsoft.Cache/Redis/sku.capacity</span><span class="sxs-lookup"><span data-stu-id="c5c3c-211">Microsoft.Cache/Redis/sku.capacity</span></span> | <span data-ttu-id="c5c3c-212">Ange hello storleken på hello Redis-cache toodeploy.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-212">Set hello size of hello Redis cache toodeploy.</span></span>  |
| <span data-ttu-id="c5c3c-213">Microsoft.Cache/Redis/sku.family</span><span class="sxs-lookup"><span data-stu-id="c5c3c-213">Microsoft.Cache/Redis/sku.family</span></span> | <span data-ttu-id="c5c3c-214">Ange hello SKU-familjen toouse.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-214">Set hello SKU family toouse.</span></span> |
| <span data-ttu-id="c5c3c-215">Microsoft.Cache/Redis/sku.name</span><span class="sxs-lookup"><span data-stu-id="c5c3c-215">Microsoft.Cache/Redis/sku.name</span></span> | <span data-ttu-id="c5c3c-216">Ange hello typ av toodeploy Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-216">Set hello type of Redis Cache toodeploy.</span></span> |

<span data-ttu-id="c5c3c-217">**Microsoft.Cdn/profiles**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-217">**Microsoft.Cdn/profiles**</span></span>

| <span data-ttu-id="c5c3c-218">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-218">Alias</span></span> | <span data-ttu-id="c5c3c-219">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-219">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-220">Microsoft.CDN/profiles/sku.name</span><span class="sxs-lookup"><span data-stu-id="c5c3c-220">Microsoft.CDN/profiles/sku.name</span></span> | <span data-ttu-id="c5c3c-221">Ange hello namn för hello prisnivån.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-221">Set hello name of hello pricing tier.</span></span> |

<span data-ttu-id="c5c3c-222">**Microsoft.Compute/disks**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-222">**Microsoft.Compute/disks**</span></span>

| <span data-ttu-id="c5c3c-223">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-223">Alias</span></span> | <span data-ttu-id="c5c3c-224">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-224">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-225">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="c5c3c-225">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="c5c3c-226">Ange hello erbjudande hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-226">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-227">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="c5c3c-227">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="c5c3c-228">Ange hello utgivaren av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-228">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-229">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="c5c3c-229">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="c5c3c-230">Ange hello SKU hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-230">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-231">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="c5c3c-231">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="c5c3c-232">Ange hello version av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-232">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |


<span data-ttu-id="c5c3c-233">**Microsoft.Compute/virtualMachines**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-233">**Microsoft.Compute/virtualMachines**</span></span>

| <span data-ttu-id="c5c3c-234">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-234">Alias</span></span> | <span data-ttu-id="c5c3c-235">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-235">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-236">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="c5c3c-236">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="c5c3c-237">Ange hello identifierare för hello avbildningen toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-237">Set hello identifier of hello image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-238">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="c5c3c-238">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="c5c3c-239">Ange hello erbjudande hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-239">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-240">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="c5c3c-240">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="c5c3c-241">Ange hello utgivaren av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-241">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-242">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="c5c3c-242">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="c5c3c-243">Ange hello SKU hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-243">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-244">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="c5c3c-244">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="c5c3c-245">Ange hello version av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-245">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-246">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="c5c3c-246">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="c5c3c-247">Ange hello avbildningen eller disken är licensierad lokalt.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-247">Set that hello image or disk is licensed on-premises.</span></span> <span data-ttu-id="c5c3c-248">Det här värdet används bara för bilder som innehåller hello Windows Server-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-248">This value is only used for images that contain hello Windows Server operating system.</span></span>  |
| <span data-ttu-id="c5c3c-249">Microsoft.Compute/virtualMachines/imageOffer</span><span class="sxs-lookup"><span data-stu-id="c5c3c-249">Microsoft.Compute/virtualMachines/imageOffer</span></span> | <span data-ttu-id="c5c3c-250">Ange hello erbjudande hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-250">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-251">Microsoft.Compute/virtualMachines/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="c5c3c-251">Microsoft.Compute/virtualMachines/imagePublisher</span></span> | <span data-ttu-id="c5c3c-252">Ange hello utgivaren av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-252">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-253">Microsoft.Compute/virtualMachines/imageSku</span><span class="sxs-lookup"><span data-stu-id="c5c3c-253">Microsoft.Compute/virtualMachines/imageSku</span></span> | <span data-ttu-id="c5c3c-254">Ange hello SKU hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-254">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-255">Microsoft.Compute/virtualMachines/imageVersion</span><span class="sxs-lookup"><span data-stu-id="c5c3c-255">Microsoft.Compute/virtualMachines/imageVersion</span></span> | <span data-ttu-id="c5c3c-256">Ange hello version av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-256">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span><span class="sxs-lookup"><span data-stu-id="c5c3c-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span></span> | <span data-ttu-id="c5c3c-258">Ange hello vhd URI.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-258">Set hello vhd URI.</span></span> |
| <span data-ttu-id="c5c3c-259">Microsoft.Compute/virtualMachines/sku.name</span><span class="sxs-lookup"><span data-stu-id="c5c3c-259">Microsoft.Compute/virtualMachines/sku.name</span></span> | <span data-ttu-id="c5c3c-260">Ange hello storleken på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-260">Set hello size of hello virtual machine.</span></span> |

<span data-ttu-id="c5c3c-261">**Microsoft.Compute/virtualMachines/extensions**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-261">**Microsoft.Compute/virtualMachines/extensions**</span></span>

| <span data-ttu-id="c5c3c-262">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-262">Alias</span></span> | <span data-ttu-id="c5c3c-263">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-263">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-264">Microsoft.Compute/virtualMachines/extensions/publisher</span><span class="sxs-lookup"><span data-stu-id="c5c3c-264">Microsoft.Compute/virtualMachines/extensions/publisher</span></span> | <span data-ttu-id="c5c3c-265">Ange hello namnet på hello tillägget utgivare.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-265">Set hello name of hello extension’s publisher.</span></span> |
| <span data-ttu-id="c5c3c-266">Microsoft.Compute/virtualMachines/extensions/type</span><span class="sxs-lookup"><span data-stu-id="c5c3c-266">Microsoft.Compute/virtualMachines/extensions/type</span></span> | <span data-ttu-id="c5c3c-267">Ange hello typ av tillägget.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-267">Set hello type of extension.</span></span> |
| <span data-ttu-id="c5c3c-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="c5c3c-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span></span> | <span data-ttu-id="c5c3c-269">Ange hello version av hello extension.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-269">Set hello version of hello extension.</span></span> |

<span data-ttu-id="c5c3c-270">**Microsoft.Compute/virtualMachineScaleSets**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-270">**Microsoft.Compute/virtualMachineScaleSets**</span></span>

| <span data-ttu-id="c5c3c-271">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-271">Alias</span></span> | <span data-ttu-id="c5c3c-272">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-272">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-273">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="c5c3c-273">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="c5c3c-274">Ange hello identifierare för hello avbildningen toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-274">Set hello identifier of hello image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-275">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="c5c3c-275">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="c5c3c-276">Ange hello erbjudande hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-276">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-277">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="c5c3c-277">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="c5c3c-278">Ange hello utgivaren av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-278">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-279">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="c5c3c-279">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="c5c3c-280">Ange hello SKU hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-280">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-281">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="c5c3c-281">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="c5c3c-282">Ange hello version av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-282">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="c5c3c-283">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="c5c3c-283">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="c5c3c-284">Ange hello avbildningen eller disken är licensierad lokalt.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-284">Set that hello image or disk is licensed on-premises.</span></span> <span data-ttu-id="c5c3c-285">Det här värdet används bara för bilder som innehåller hello Windows Server-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-285">This value is only used for images that contain hello Windows Server operating system.</span></span> |
| <span data-ttu-id="c5c3c-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="c5c3c-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span></span> | <span data-ttu-id="c5c3c-287">Ange hello prefixet för alla hello virtuella datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-287">Set hello computer name prefix for all  hello virtual machines in hello scale set.</span></span> |
| <span data-ttu-id="c5c3c-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span><span class="sxs-lookup"><span data-stu-id="c5c3c-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span></span> | <span data-ttu-id="c5c3c-289">Ange hello blob-URI för användaravbildning.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-289">Set hello blob URI for user image.</span></span> |
| <span data-ttu-id="c5c3c-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span><span class="sxs-lookup"><span data-stu-id="c5c3c-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span></span> | <span data-ttu-id="c5c3c-291">Ange hello behållaren URL-adresser som används toostore operativsystemet diskar för hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-291">Set hello container URLs that are used toostore operating system disks for hello scale set.</span></span> |
| <span data-ttu-id="c5c3c-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span><span class="sxs-lookup"><span data-stu-id="c5c3c-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span></span> | <span data-ttu-id="c5c3c-293">Ange hello storleken på virtuella datorer i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-293">Set hello size of virtual machines in a scale set.</span></span> |
| <span data-ttu-id="c5c3c-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span><span class="sxs-lookup"><span data-stu-id="c5c3c-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span></span> | <span data-ttu-id="c5c3c-295">Ange hello nivå med virtuella datorer i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-295">Set hello tier of virtual machines in a scale set.</span></span> |
  
<span data-ttu-id="c5c3c-296">**Microsoft.Network/applicationGateways**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-296">**Microsoft.Network/applicationGateways**</span></span>

| <span data-ttu-id="c5c3c-297">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-297">Alias</span></span> | <span data-ttu-id="c5c3c-298">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-298">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-299">Microsoft.Network/applicationGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="c5c3c-299">Microsoft.Network/applicationGateways/sku.name</span></span> | <span data-ttu-id="c5c3c-300">Ange hello storleken på hello gateway.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-300">Set hello size of hello gateway.</span></span> |

<span data-ttu-id="c5c3c-301">**Microsoft.Network/virtualNetworkGateways**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-301">**Microsoft.Network/virtualNetworkGateways**</span></span>

| <span data-ttu-id="c5c3c-302">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-302">Alias</span></span> | <span data-ttu-id="c5c3c-303">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-303">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span><span class="sxs-lookup"><span data-stu-id="c5c3c-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span></span> | <span data-ttu-id="c5c3c-305">Ange hello typ av den här virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-305">Set hello type of this virtual network gateway.</span></span> |
| <span data-ttu-id="c5c3c-306">Microsoft.Network/virtualNetworkGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="c5c3c-306">Microsoft.Network/virtualNetworkGateways/sku.name</span></span> | <span data-ttu-id="c5c3c-307">Ange hello gateway SKU namn.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-307">Set hello gateway SKU name.</span></span> |

<span data-ttu-id="c5c3c-308">**Microsoft.Sql/servers**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-308">**Microsoft.Sql/servers**</span></span>

| <span data-ttu-id="c5c3c-309">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-309">Alias</span></span> | <span data-ttu-id="c5c3c-310">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-310">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-311">Microsoft.Sql/servers/version</span><span class="sxs-lookup"><span data-stu-id="c5c3c-311">Microsoft.Sql/servers/version</span></span> | <span data-ttu-id="c5c3c-312">Ange hello hello server-versionen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-312">Set hello version of hello server.</span></span> |

<span data-ttu-id="c5c3c-313">**Microsoft.Sql/databases**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-313">**Microsoft.Sql/databases**</span></span>

| <span data-ttu-id="c5c3c-314">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-314">Alias</span></span> | <span data-ttu-id="c5c3c-315">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-315">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-316">Microsoft.Sql/servers/databases/edition</span><span class="sxs-lookup"><span data-stu-id="c5c3c-316">Microsoft.Sql/servers/databases/edition</span></span> | <span data-ttu-id="c5c3c-317">Ange hello version av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-317">Set hello edition of hello database.</span></span> |
| <span data-ttu-id="c5c3c-318">Microsoft.Sql/servers/databases/elasticPoolName</span><span class="sxs-lookup"><span data-stu-id="c5c3c-318">Microsoft.Sql/servers/databases/elasticPoolName</span></span> | <span data-ttu-id="c5c3c-319">Ange hello namnet på hello elastisk pool hello databasen är i.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-319">Set hello name of hello elastic pool hello database is in.</span></span> |
| <span data-ttu-id="c5c3c-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span><span class="sxs-lookup"><span data-stu-id="c5c3c-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span></span> | <span data-ttu-id="c5c3c-321">Ange hello konfigurerats service nivå mål-ID för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-321">Set hello configured service level objective ID of hello database.</span></span> |
| <span data-ttu-id="c5c3c-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span><span class="sxs-lookup"><span data-stu-id="c5c3c-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span></span> | <span data-ttu-id="c5c3c-323">Ange namnet på hello av hello konfigurerad servicenivåmål av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-323">Set hello name of hello configured service level objective of hello database.</span></span>  |

<span data-ttu-id="c5c3c-324">**Microsoft.Sql/elasticpools**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-324">**Microsoft.Sql/elasticpools**</span></span>

| <span data-ttu-id="c5c3c-325">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-325">Alias</span></span> | <span data-ttu-id="c5c3c-326">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-326">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-327">servrar/elasticpools</span><span class="sxs-lookup"><span data-stu-id="c5c3c-327">servers/elasticpools</span></span> | <span data-ttu-id="c5c3c-328">Microsoft.Sql/servers/elasticPools/dtu</span><span class="sxs-lookup"><span data-stu-id="c5c3c-328">Microsoft.Sql/servers/elasticPools/dtu</span></span> | <span data-ttu-id="c5c3c-329">Ange hello totalt delade DTU för hello database-elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-329">Set hello total shared DTU for hello database elastic pool.</span></span> |
| <span data-ttu-id="c5c3c-330">servrar/elasticpools</span><span class="sxs-lookup"><span data-stu-id="c5c3c-330">servers/elasticpools</span></span> | <span data-ttu-id="c5c3c-331">Microsoft.Sql/servers/elasticPools/edition</span><span class="sxs-lookup"><span data-stu-id="c5c3c-331">Microsoft.Sql/servers/elasticPools/edition</span></span> | <span data-ttu-id="c5c3c-332">Ange hello utgåva av hello elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-332">Set hello edition of hello elastic pool.</span></span> |

<span data-ttu-id="c5c3c-333">**Microsoft.Storage/storageAccounts**</span><span class="sxs-lookup"><span data-stu-id="c5c3c-333">**Microsoft.Storage/storageAccounts**</span></span>

| <span data-ttu-id="c5c3c-334">Alias</span><span class="sxs-lookup"><span data-stu-id="c5c3c-334">Alias</span></span> | <span data-ttu-id="c5c3c-335">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5c3c-335">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="c5c3c-336">Microsoft.Storage/storageAccounts/accessTier</span><span class="sxs-lookup"><span data-stu-id="c5c3c-336">Microsoft.Storage/storageAccounts/accessTier</span></span> | <span data-ttu-id="c5c3c-337">Ange hello åtkomstnivå används för fakturering.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-337">Set hello access tier used for billing.</span></span> |
| <span data-ttu-id="c5c3c-338">Microsoft.Storage/storageAccounts/accountType</span><span class="sxs-lookup"><span data-stu-id="c5c3c-338">Microsoft.Storage/storageAccounts/accountType</span></span> | <span data-ttu-id="c5c3c-339">Ange hello SKU namn.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-339">Set hello SKU name.</span></span> |
| <span data-ttu-id="c5c3c-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span><span class="sxs-lookup"><span data-stu-id="c5c3c-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span></span> | <span data-ttu-id="c5c3c-341">Ange om hello service krypterar hello data som lagras i hello blob storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-341">Set whether hello service encrypts hello data as it is stored in hello blob storage service.</span></span> |
| <span data-ttu-id="c5c3c-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span><span class="sxs-lookup"><span data-stu-id="c5c3c-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span></span> | <span data-ttu-id="c5c3c-343">Ange om hello service krypterar hello data som lagras i hello file storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-343">Set whether hello service encrypts hello data as it is stored in hello file storage service.</span></span> |
| <span data-ttu-id="c5c3c-344">Microsoft.Storage/storageAccounts/sku.name</span><span class="sxs-lookup"><span data-stu-id="c5c3c-344">Microsoft.Storage/storageAccounts/sku.name</span></span> | <span data-ttu-id="c5c3c-345">Ange hello SKU namn.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-345">Set hello SKU name.</span></span> |
| <span data-ttu-id="c5c3c-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span><span class="sxs-lookup"><span data-stu-id="c5c3c-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span></span> | <span data-ttu-id="c5c3c-347">Ange tooallow endast https-trafik toostorage service.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-347">Set tooallow only https traffic toostorage service.</span></span> |


## <a name="policy-examples"></a><span data-ttu-id="c5c3c-348">Exempel på</span><span class="sxs-lookup"><span data-stu-id="c5c3c-348">Policy examples</span></span>

<span data-ttu-id="c5c3c-349">hello följande avsnitt innehåller exempel på:</span><span class="sxs-lookup"><span data-stu-id="c5c3c-349">hello following topics contain policy examples:</span></span>

* <span data-ttu-id="c5c3c-350">Exempel på taggen principerna finns [gäller resursprinciper för taggar](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-350">For examples of tag polices, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>
* <span data-ttu-id="c5c3c-351">Exempel på namngivning och text mönster finns [gäller resursprinciper för namn och text](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-351">For examples of naming and text patterns, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>
* <span data-ttu-id="c5c3c-352">Exempel på principer för lagring finns [gäller resurskonton principer toostorage](resource-manager-policy-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-352">For examples of storage policies, see [Apply resource policies toostorage accounts](resource-manager-policy-storage.md).</span></span>
* <span data-ttu-id="c5c3c-353">Exempel på principer för virtuell dator finns [gäller resurs principer tooLinux VMs](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) och [gäller resurs principer tooWindows virtuella datorer](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c5c3c-353">For examples of virtual machine policies, see [Apply resource policies tooLinux VMs](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) and [Apply resource policies tooWindows VMs](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span></span>


## <a name="next-steps"></a><span data-ttu-id="c5c3c-354">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5c3c-354">Next steps</span></span>
* <span data-ttu-id="c5c3c-355">När du har definierat en regel, tilldela den tooa omfång.</span><span class="sxs-lookup"><span data-stu-id="c5c3c-355">After defining a policy rule, assign it tooa scope.</span></span> <span data-ttu-id="c5c3c-356">tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-356">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="c5c3c-357">tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-357">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="c5c3c-358">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-358">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="c5c3c-359">hello princip schema har publicerats på [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="c5c3c-359">hello policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 

