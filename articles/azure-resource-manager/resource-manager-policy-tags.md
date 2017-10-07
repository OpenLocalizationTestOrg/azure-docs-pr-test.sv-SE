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
# <a name="apply-resource-policies-for-tags"></a>Tillämpa principer för företagsresurser för taggar

Det här avsnittet innehåller gemensamma regler som du kan använda tooensure konsekvent användning av taggar på resurser.

Tillämpa en tagg princip tooa resursgrupp eller prenumeration med befintliga resurser inte applicera hello princip toothose resurser. tooenforce hello principer på dessa resurser, utlöser en uppdatering toohello befintliga resurser. Den här artikeln innehåller ett PowerShell-exempel för att utlösa en uppdatering.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Kontrollera att alla resurser i en resursgrupp har ett Taggvärde

Ett vanligt krav är att alla resurser i en resursgrupp har en viss tagg och värde. Det här kravet är ofta nödvändiga tootrack kostnader per avdelning. hello följande villkor uppfyllas:

* hello krävs taggen och värdet är tillagda toonew och uppdatera resurser som inte har hello-tagg.
* hello krävs taggen och värde kan inte tas bort från alla befintliga resurser.

Du kan göra det här kravet genom att använda två inbyggda principer tooa resursgruppen.

| ID | Beskrivning |
| ---- | ---- |
| 2a0e14a6-b0a6-4fab-991a-187a4f81c498 | Gäller obligatoriska taggen med sitt standardvärde när det inte anges av hello användare. |
| 1e30110a-5ceb-460c-a204-c1c3969c6d62 | Tillämpar en nödvändig tagg och dess värde. |

### <a name="powershell"></a>PowerShell

hello följande PowerShell-skript tilldelar hello två inbyggda princip definitioner tooa resursgruppen. Innan du kör skriptet hello tilldela alla taggar som krävs toohello resursgruppen. Varje tagg på hello resursgruppens namn måste anges för hello resurser i hello grupp. tooassign tooall resursgrupper i din prenumeration, ger inte hello `-Name` parameter när du hämtar hello resursgrupper.

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

Du kan utlösa en uppdatering tooall befintliga resurser tooenforce hello taggen principer som du har lagt till efter att hello principer. hello behåller följande skript andra taggar som fanns på hello resurser:

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

## <a name="require-tags-for-a-resource-type"></a>Kräv taggar för en resurstyp
hello som följande exempel visar hur toonest logiska operatorer toorequire ett program tagga för endast en viss resurstyp (i det här fallet, storage-konton).

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

## <a name="require-tag"></a>Kräv tagg
hello nekar följande princip förfrågningar som inte har en tagg som innehåller ”costCenter” nyckel (valfritt värde kan tillämpas):

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

## <a name="next-steps"></a>Nästa steg
* När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång. hello scope kan vara en prenumeration, resursgrupp eller resurs. tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md). tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).
* En introduktion tooresource principer finns i [resurs uppgifter](resource-manager-policy.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

