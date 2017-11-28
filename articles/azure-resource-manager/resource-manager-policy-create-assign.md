---
title: aaaAssign och hantera Azure resursprinciper | Microsoft Docs
description: Beskriver hur tooapply Azure principer toosubscriptions och resurs-resursgrupper, och hur tooview resursprinciper.
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
ms.date: 07/26/2017
ms.author: tomfitz
ms.openlocfilehash: b6999b43bbcc80d2fde9911352fd4352fa453443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-and-manage-resource-policies"></a><span data-ttu-id="75bd3-103">Tilldela och hantera principer för företagsresurser</span><span class="sxs-lookup"><span data-stu-id="75bd3-103">Assign and manage resource policies</span></span>

<span data-ttu-id="75bd3-104">tooimplement en princip, måste du utföra de här stegen:</span><span class="sxs-lookup"><span data-stu-id="75bd3-104">tooimplement a policy, you must perform these steps:</span></span>

1. <span data-ttu-id="75bd3-105">Kontrollera princip definitioner (inklusive inbyggda principer som tillhandahålls av Azure) toosee om det redan finns i din prenumeration som uppfyller dina krav.</span><span class="sxs-lookup"><span data-stu-id="75bd3-105">Check policy definitions (including built-in policies provided by Azure) toosee if one already exists in your subscription that fulfills your requirements.</span></span>
2. <span data-ttu-id="75bd3-106">Hämta namnet om det finns ett.</span><span class="sxs-lookup"><span data-stu-id="75bd3-106">If one exists, get its name.</span></span>
3. <span data-ttu-id="75bd3-107">Om det inte finns, definiera hello principregeln med JSON och Lägg till den som en principdefinition i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="75bd3-107">If one does not exist, define hello policy rule with JSON, and add it as a policy definition in your subscription.</span></span> <span data-ttu-id="75bd3-108">Det här steget gör hello princip som är tillgängliga för tilldelning, men gäller inte hello regler tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="75bd3-108">This step makes hello policy available for assignment but does not apply hello rules tooyour subscription.</span></span>
4. <span data-ttu-id="75bd3-109">För båda fallen kan du tilldela hello princip tooa omfattning (t.ex en grupp för prenumerationen eller resursen).</span><span class="sxs-lookup"><span data-stu-id="75bd3-109">For either case, assign hello policy tooa scope (such as a subscription or resource group).</span></span> <span data-ttu-id="75bd3-110">hello regler för hello principen tillämpas nu.</span><span class="sxs-lookup"><span data-stu-id="75bd3-110">hello rules of hello policy are now enforced.</span></span>

<span data-ttu-id="75bd3-111">Den här artikeln fokuserar på hello steg toocreate en principdefinition och tilldela detta definition tooa omfång via REST-API, PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="75bd3-111">This article focuses on hello steps toocreate a policy definition and assign that definition tooa scope through REST API, PowerShell, or Azure CLI.</span></span> <span data-ttu-id="75bd3-112">Om du föredrar toouse hello portal tooassign principer finns [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="75bd3-112">If you prefer toouse hello portal tooassign policies, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="75bd3-113">Den här artikeln inte fokusera på hello syntax för att skapa hello principdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="75bd3-113">This article does not focus on hello syntax for creating hello policy definition.</span></span> <span data-ttu-id="75bd3-114">Information om principen syntax finns [resurs uppgifter](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="75bd3-114">For information about policy syntax, see [Resource policy overview](resource-manager-policy.md).</span></span>

## <a name="rest-api"></a><span data-ttu-id="75bd3-115">REST API</span><span class="sxs-lookup"><span data-stu-id="75bd3-115">REST API</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="75bd3-116">Skapa en principdefinition för</span><span class="sxs-lookup"><span data-stu-id="75bd3-116">Create policy definition</span></span>

<span data-ttu-id="75bd3-117">Du kan skapa en princip med hello [REST API för Principdefinitioner](/rest/api/resources/policydefinitions).</span><span class="sxs-lookup"><span data-stu-id="75bd3-117">You can create a policy with hello [REST API for Policy Definitions](/rest/api/resources/policydefinitions).</span></span> <span data-ttu-id="75bd3-118">hello REST-API kan du toocreate och ta bort principdefinitioner och få information om befintliga definitioner.</span><span class="sxs-lookup"><span data-stu-id="75bd3-118">hello REST API enables you toocreate and delete policy definitions, and get information about existing definitions.</span></span>

<span data-ttu-id="75bd3-119">toocreate en principdefinition kör:</span><span class="sxs-lookup"><span data-stu-id="75bd3-119">toocreate a policy definition, run:</span></span>

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

<span data-ttu-id="75bd3-120">Är en begäran brödtext liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="75bd3-120">Include a request body similar toohello following example:</span></span>

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

### <a name="assign-policy"></a><span data-ttu-id="75bd3-121">Tilldela principen</span><span class="sxs-lookup"><span data-stu-id="75bd3-121">Assign policy</span></span>

<span data-ttu-id="75bd3-122">Du kan använda hello principdefinitionen definitionsområdet hello önskad via hello [REST API för principtilldelningar](/rest/api/resources/policyassignments).</span><span class="sxs-lookup"><span data-stu-id="75bd3-122">You can apply hello policy definition at hello desired scope through hello [REST API for policy assignments](/rest/api/resources/policyassignments).</span></span> <span data-ttu-id="75bd3-123">hello REST-API kan du toocreate och ta bort principtilldelningar och få information om befintliga tilldelningar.</span><span class="sxs-lookup"><span data-stu-id="75bd3-123">hello REST API enables you toocreate and delete policy assignments, and get information about existing assignments.</span></span>

<span data-ttu-id="75bd3-124">toocreate en principtilldelning kör:</span><span class="sxs-lookup"><span data-stu-id="75bd3-124">toocreate a policy assignment, run:</span></span>

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

<span data-ttu-id="75bd3-125">hello {principtilldelning} är hello principtilldelning hello namn.</span><span class="sxs-lookup"><span data-stu-id="75bd3-125">hello {policy-assignment} is hello name of hello policy assignment.</span></span>

<span data-ttu-id="75bd3-126">Är en begäran brödtext liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="75bd3-126">Include a request body similar toohello following example:</span></span>

```json
{
  "properties":{
    "displayName":"West US only policy assignment on hello subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a><span data-ttu-id="75bd3-127">Visa policy</span><span class="sxs-lookup"><span data-stu-id="75bd3-127">View policy</span></span>
<span data-ttu-id="75bd3-128">tooget en princip, använda hello [hämta principdefinitionen](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) igen.</span><span class="sxs-lookup"><span data-stu-id="75bd3-128">tooget a policy, use hello [Get policy definition](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operation.</span></span>

### <a name="get-aliases"></a><span data-ttu-id="75bd3-129">Hämta alias</span><span class="sxs-lookup"><span data-stu-id="75bd3-129">Get aliases</span></span>
<span data-ttu-id="75bd3-130">Du kan hämta alias via hello REST-API:</span><span class="sxs-lookup"><span data-stu-id="75bd3-130">You can retrieve aliases through hello REST API:</span></span>

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

<span data-ttu-id="75bd3-131">hello som följande exempel visar en definition av ett alias.</span><span class="sxs-lookup"><span data-stu-id="75bd3-131">hello following example shows a definition of an alias.</span></span> <span data-ttu-id="75bd3-132">Som du ser definierar ett alias sökvägar i olika API-versioner, även om det finns en namnändring för egenskapen.</span><span class="sxs-lookup"><span data-stu-id="75bd3-132">As you can see, an alias defines paths in different API versions, even when there is a property name change.</span></span> 

```json
"aliases": [
    {
      "name": "Microsoft.Storage/storageAccounts/sku.name",
      "paths": [
        {
          "path": "properties.accountType",
          "apiVersions": [
            "2015-06-15",
            "2015-05-01-preview"
          ]
        },
        {
          "path": "sku.name",
          "apiVersions": [
            "2016-01-01"
          ]
        }
      ]
    }
]
```

## <a name="powershell"></a><span data-ttu-id="75bd3-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="75bd3-133">PowerShell</span></span>

<span data-ttu-id="75bd3-134">Innan du fortsätter med hello PowerShell-exemplen, kontrollera att du har [installerat hello senaste versionen](/powershell/azure/install-azurerm-ps) av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75bd3-134">Before proceeding with hello PowerShell examples, make sure you have [installed hello latest version](/powershell/azure/install-azurerm-ps) of Azure PowerShell.</span></span> <span data-ttu-id="75bd3-135">Principparametrar har lagts till i version 3.6.0.</span><span class="sxs-lookup"><span data-stu-id="75bd3-135">Policy parameters were added in version 3.6.0.</span></span> <span data-ttu-id="75bd3-136">Om du har en tidigare version returnera hello exempel ett fel som anger hello-parameter inte kan hittas.</span><span class="sxs-lookup"><span data-stu-id="75bd3-136">If you have an earlier version, hello examples return an error indicating hello parameter cannot be found.</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="75bd3-137">Visa principdefinitioner</span><span class="sxs-lookup"><span data-stu-id="75bd3-137">View policy definitions</span></span>
<span data-ttu-id="75bd3-138">toosee alla principdefinitioner i din prenumeration, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75bd3-138">toosee all policy definitions in your subscription, use hello following command:</span></span>

```powershell
Get-AzureRmPolicyDefinition
```

<span data-ttu-id="75bd3-139">Den returnerar alla tillgängliga principdefinitioner, inklusive inbyggda principer.</span><span class="sxs-lookup"><span data-stu-id="75bd3-139">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="75bd3-140">Varje princip returneras i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="75bd3-140">Each policy is returned in hello following format:</span></span>

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict hello locations your organization can specify when deploying resources. Use tooenforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

<span data-ttu-id="75bd3-141">Granska hello inbyggda principer innan du fortsätter toocreate en principdefinition.</span><span class="sxs-lookup"><span data-stu-id="75bd3-141">Before proceeding toocreate a policy definition, look at hello built-in policies.</span></span> <span data-ttu-id="75bd3-142">Om du hittar en inbyggd princip som gäller hello gränser som du behöver kan du hoppa över att skapa en principdefinition.</span><span class="sxs-lookup"><span data-stu-id="75bd3-142">If you find a built-in policy that applies hello limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="75bd3-143">I stället tilldela hello inbyggda toohello önskad principområdet.</span><span class="sxs-lookup"><span data-stu-id="75bd3-143">Instead, assign hello built-in policy toohello desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="75bd3-144">Skapa en principdefinition för</span><span class="sxs-lookup"><span data-stu-id="75bd3-144">Create policy definition</span></span>
<span data-ttu-id="75bd3-145">Du kan skapa en principdefinition med hello `New-AzureRmPolicyDefinition` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75bd3-145">You can create a policy definition using hello `New-AzureRmPolicyDefinition` cmdlet.</span></span>

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy '{
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
}'
```            

<span data-ttu-id="75bd3-146">hello utdata lagras i en `$definition` -objekt som ska användas vid tilldelning av principer.</span><span class="sxs-lookup"><span data-stu-id="75bd3-146">hello output is stored in a `$definition` object, which is used during policy assignment.</span></span> 

<span data-ttu-id="75bd3-147">Du kan ange hello sökvägen tooa JSON-fil som innehåller hello principregeln i stället för att ange hello JSON som en parameter.</span><span class="sxs-lookup"><span data-stu-id="75bd3-147">Rather than specifying hello JSON as a parameter, you can provide hello path tooa .json file containing hello policy rule.</span></span>

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy "c:\policies\coolAccessTier.json"
```

<span data-ttu-id="75bd3-148">hello skapas följande exempel en principdefinition som innehåller parametrar:</span><span class="sxs-lookup"><span data-stu-id="75bd3-148">hello following example creates a policy definition that includes parameters:</span></span>

```powershell
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy toospecify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a><span data-ttu-id="75bd3-149">Tilldela principen</span><span class="sxs-lookup"><span data-stu-id="75bd3-149">Assign policy</span></span>

<span data-ttu-id="75bd3-150">Du använder hello princip definitionsområdet hello önskad med hello `New-AzureRmPolicyAssignment` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75bd3-150">You apply hello policy at hello desired scope by using hello `New-AzureRmPolicyAssignment` cmdlet.</span></span> <span data-ttu-id="75bd3-151">följande exempel hello tilldelar hello princip tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="75bd3-151">hello following example assigns hello policy tooa resource group.</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

<span data-ttu-id="75bd3-152">tooassign en princip som kräver parametrar, skapa och objekt med dessa värden.</span><span class="sxs-lookup"><span data-stu-id="75bd3-152">tooassign a policy that requires parameters, create and object with those values.</span></span> <span data-ttu-id="75bd3-153">hello följande exempel hämtar en inbyggd princip och skickar in värden för parametrar:</span><span class="sxs-lookup"><span data-stu-id="75bd3-153">hello following example retrieves a built-in policy and passes in parameters values:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a><span data-ttu-id="75bd3-154">Visa tilldelning av principer</span><span class="sxs-lookup"><span data-stu-id="75bd3-154">View policy assignment</span></span>

<span data-ttu-id="75bd3-155">tooget en specifik principtilldelning använder:</span><span class="sxs-lookup"><span data-stu-id="75bd3-155">tooget a specific policy assignment, use:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

<span data-ttu-id="75bd3-156">tooview hello principregel för en principdefinition, Använd:</span><span class="sxs-lookup"><span data-stu-id="75bd3-156">tooview hello policy rule for a policy definition, use:</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="75bd3-157">Ta bort tilldelning av principer</span><span class="sxs-lookup"><span data-stu-id="75bd3-157">Remove policy assignment</span></span> 

<span data-ttu-id="75bd3-158">tooremove en principtilldelning använder:</span><span class="sxs-lookup"><span data-stu-id="75bd3-158">tooremove a policy assignment, use:</span></span>

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a><span data-ttu-id="75bd3-159">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="75bd3-159">Azure CLI</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="75bd3-160">Visa principdefinitioner</span><span class="sxs-lookup"><span data-stu-id="75bd3-160">View policy definitions</span></span>
<span data-ttu-id="75bd3-161">toosee alla principdefinitioner i din prenumeration, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75bd3-161">toosee all policy definitions in your subscription, use hello following command:</span></span>

```azurecli
az policy definition list
```

<span data-ttu-id="75bd3-162">Den returnerar alla tillgängliga principdefinitioner, inklusive inbyggda principer.</span><span class="sxs-lookup"><span data-stu-id="75bd3-162">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="75bd3-163">Varje princip returneras i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="75bd3-163">Each policy is returned in hello following format:</span></span>

```azurecli
{                                                            
  "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources. Use tooenforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

<span data-ttu-id="75bd3-164">Granska hello inbyggda principer innan du fortsätter toocreate en principdefinition.</span><span class="sxs-lookup"><span data-stu-id="75bd3-164">Before proceeding toocreate a policy definition, look at hello built-in policies.</span></span> <span data-ttu-id="75bd3-165">Om du hittar en inbyggd princip som gäller hello gränser som du behöver kan du hoppa över att skapa en principdefinition.</span><span class="sxs-lookup"><span data-stu-id="75bd3-165">If you find a built-in policy that applies hello limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="75bd3-166">I stället tilldela hello inbyggda toohello önskad principområdet.</span><span class="sxs-lookup"><span data-stu-id="75bd3-166">Instead, assign hello built-in policy toohello desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="75bd3-167">Skapa en principdefinition för</span><span class="sxs-lookup"><span data-stu-id="75bd3-167">Create policy definition</span></span>

<span data-ttu-id="75bd3-168">Du kan skapa en principdefinition med Azure CLI med hello princip definition kommando.</span><span class="sxs-lookup"><span data-stu-id="75bd3-168">You can create a policy definition using Azure CLI with hello policy definition command.</span></span>

```azurecli
az policy definition create --name coolAccessTier --description "Policy toospecify access tier." --rules '{
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
}'    
```

### <a name="assign-policy"></a><span data-ttu-id="75bd3-169">Tilldela principen</span><span class="sxs-lookup"><span data-stu-id="75bd3-169">Assign policy</span></span>

<span data-ttu-id="75bd3-170">Du kan använda hello toohello önskad principområdet med kommandot hello princip tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="75bd3-170">You can apply hello policy toohello desired scope by using hello policy assignment command.</span></span> <span data-ttu-id="75bd3-171">följande exempel hello tilldelar en princip tooa resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="75bd3-171">hello following example assigns a policy tooa resource group.</span></span>

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a><span data-ttu-id="75bd3-172">Visa tilldelning av principer</span><span class="sxs-lookup"><span data-stu-id="75bd3-172">View policy assignment</span></span>

<span data-ttu-id="75bd3-173">tooview en principtilldelning innehåller hello principtilldelningsnamnet och hello omfång:</span><span class="sxs-lookup"><span data-stu-id="75bd3-173">tooview a policy assignment, provide hello policy assignment name and hello scope:</span></span>

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="75bd3-174">Ta bort tilldelning av principer</span><span class="sxs-lookup"><span data-stu-id="75bd3-174">Remove policy assignment</span></span> 

<span data-ttu-id="75bd3-175">tooremove en principtilldelning använder:</span><span class="sxs-lookup"><span data-stu-id="75bd3-175">tooremove a policy assignment, use:</span></span>

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a><span data-ttu-id="75bd3-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75bd3-176">Next steps</span></span>
* <span data-ttu-id="75bd3-177">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="75bd3-177">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

