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
# <a name="assign-and-manage-resource-policies"></a>Tilldela och hantera principer för företagsresurser

tooimplement en princip, måste du utföra de här stegen:

1. Kontrollera princip definitioner (inklusive inbyggda principer som tillhandahålls av Azure) toosee om det redan finns i din prenumeration som uppfyller dina krav.
2. Hämta namnet om det finns ett.
3. Om det inte finns, definiera hello principregeln med JSON och Lägg till den som en principdefinition i din prenumeration. Det här steget gör hello princip som är tillgängliga för tilldelning, men gäller inte hello regler tooyour prenumeration.
4. För båda fallen kan du tilldela hello princip tooa omfattning (t.ex en grupp för prenumerationen eller resursen). hello regler för hello principen tillämpas nu.

Den här artikeln fokuserar på hello steg toocreate en principdefinition och tilldela detta definition tooa omfång via REST-API, PowerShell eller Azure CLI. Om du föredrar toouse hello portal tooassign principer finns [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md). Den här artikeln inte fokusera på hello syntax för att skapa hello principdefinitionen. Information om principen syntax finns [resurs uppgifter](resource-manager-policy.md).

## <a name="rest-api"></a>REST API

### <a name="create-policy-definition"></a>Skapa en principdefinition för

Du kan skapa en princip med hello [REST API för Principdefinitioner](/rest/api/resources/policydefinitions). hello REST-API kan du toocreate och ta bort principdefinitioner och få information om befintliga definitioner.

toocreate en principdefinition kör:

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Är en begäran brödtext liknande toohello följande exempel:

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

### <a name="assign-policy"></a>Tilldela principen

Du kan använda hello principdefinitionen definitionsområdet hello önskad via hello [REST API för principtilldelningar](/rest/api/resources/policyassignments). hello REST-API kan du toocreate och ta bort principtilldelningar och få information om befintliga tilldelningar.

toocreate en principtilldelning kör:

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

hello {principtilldelning} är hello principtilldelning hello namn.

Är en begäran brödtext liknande toohello följande exempel:

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

### <a name="view-policy"></a>Visa policy
tooget en princip, använda hello [hämta principdefinitionen](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) igen.

### <a name="get-aliases"></a>Hämta alias
Du kan hämta alias via hello REST-API:

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

hello som följande exempel visar en definition av ett alias. Som du ser definierar ett alias sökvägar i olika API-versioner, även om det finns en namnändring för egenskapen. 

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

## <a name="powershell"></a>PowerShell

Innan du fortsätter med hello PowerShell-exemplen, kontrollera att du har [installerat hello senaste versionen](/powershell/azure/install-azurerm-ps) av Azure PowerShell. Principparametrar har lagts till i version 3.6.0. Om du har en tidigare version returnera hello exempel ett fel som anger hello-parameter inte kan hittas.

### <a name="view-policy-definitions"></a>Visa principdefinitioner
toosee alla principdefinitioner i din prenumeration, Använd hello följande kommando:

```powershell
Get-AzureRmPolicyDefinition
```

Den returnerar alla tillgängliga principdefinitioner, inklusive inbyggda principer. Varje princip returneras i hello följande format:

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

Granska hello inbyggda principer innan du fortsätter toocreate en principdefinition. Om du hittar en inbyggd princip som gäller hello gränser som du behöver kan du hoppa över att skapa en principdefinition. I stället tilldela hello inbyggda toohello önskad principområdet.

### <a name="create-policy-definition"></a>Skapa en principdefinition för
Du kan skapa en principdefinition med hello `New-AzureRmPolicyDefinition` cmdlet.

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

hello utdata lagras i en `$definition` -objekt som ska användas vid tilldelning av principer. 

Du kan ange hello sökvägen tooa JSON-fil som innehåller hello principregeln i stället för att ange hello JSON som en parameter.

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy "c:\policies\coolAccessTier.json"
```

hello skapas följande exempel en principdefinition som innehåller parametrar:

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

### <a name="assign-policy"></a>Tilldela principen

Du använder hello princip definitionsområdet hello önskad med hello `New-AzureRmPolicyAssignment` cmdlet. följande exempel hello tilldelar hello princip tooa resursgruppen.

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

tooassign en princip som kräver parametrar, skapa och objekt med dessa värden. hello följande exempel hämtar en inbyggd princip och skickar in värden för parametrar:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a>Visa tilldelning av principer

tooget en specifik principtilldelning använder:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

tooview hello principregel för en principdefinition, Använd:

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a>Ta bort tilldelning av principer 

tooremove en principtilldelning använder:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a>Azure CLI

### <a name="view-policy-definitions"></a>Visa principdefinitioner
toosee alla principdefinitioner i din prenumeration, Använd hello följande kommando:

```azurecli
az policy definition list
```

Den returnerar alla tillgängliga principdefinitioner, inklusive inbyggda principer. Varje princip returneras i hello följande format:

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

Granska hello inbyggda principer innan du fortsätter toocreate en principdefinition. Om du hittar en inbyggd princip som gäller hello gränser som du behöver kan du hoppa över att skapa en principdefinition. I stället tilldela hello inbyggda toohello önskad principområdet.

### <a name="create-policy-definition"></a>Skapa en principdefinition för

Du kan skapa en principdefinition med Azure CLI med hello princip definition kommando.

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

### <a name="assign-policy"></a>Tilldela principen

Du kan använda hello toohello önskad principområdet med kommandot hello princip tilldelningen. följande exempel hello tilldelar en princip tooa resursgrupp.

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>Visa tilldelning av principer

tooview en principtilldelning innehåller hello principtilldelningsnamnet och hello omfång:

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a>Ta bort tilldelning av principer 

tooremove en principtilldelning använder:

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Nästa steg
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

