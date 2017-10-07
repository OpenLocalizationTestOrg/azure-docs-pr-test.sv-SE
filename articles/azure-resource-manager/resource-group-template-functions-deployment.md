---
title: "Mallfunktioner för aaaAzure Resource Manager - distribution | Microsoft Docs"
description: Beskriver hello funktioner toouse i en Azure Resource Manager mallen tooretrieve distributionsinformation.
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
ms.date: 06/13/2017
ms.author: tomfitz
ms.openlocfilehash: 458c3f740504fdd6799ed24cc386219726737636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Distribution av funktioner för Azure Resource Manager-mallar 

Resource Manager tillhandahåller hello följande funktioner för att hämta värden från delar av hello mall och värden relaterade toohello distribution:

* [distribution](#deployment)
* [parametrar](#parameters)
* [variabler](#variables)

tooget värden från resurser, resursgrupper eller prenumerationer, finns [resursfunktioner](resource-group-template-functions-resource.md).

<a id="deployment" />

## <a name="deployment"></a>distribution
`deployment()`

Returnerar information om hello aktuella distributionsåtgärder.

### <a name="return-value"></a>Returvärde

Den här funktionen returnerar hello-objektet som överförs under distributionen. hello egenskaper i hello returnerade objekt variera beroende på om hello distributionsobjektet skickas som en länk eller som ett objekt i rad. När hello distributionsobjektet skickas i rad, som när du använder hello **- TemplateFile** parameter i Azure PowerShell toopoint tooa lokal fil, hello returnerade objekt har hello följande format:

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

När hello objektet skickas som en länk till exempel när du använder hello **- TemplateUri** parametern toopoint tooa remote objekt hello objekt returneras i hello följande format: 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

### <a name="remarks"></a>Kommentarer

Du kan använda deployment() toolink tooanother mall baserat på hello URI för hello överordnade mallen.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a>Exempel

hello returnerar följande exempel hello distributionsobjektet:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

hello returnerar föregående exempel hello följande objekt:

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

<a id="parameters" />

## <a name="parameters"></a>parameters
`parameters(parameterName)`

Returnerar ett parametervärde. hello angivna parameternamnet måste definieras under hello parametrar i hello mallen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| parameterName |Ja |Sträng |hello namnet på hello parametern tooreturn. |

### <a name="return-value"></a>Returvärde

hello-värdet för hello angetts parametern.

### <a name="remarks"></a>Kommentarer

Normalt använder du parametrarna tooset resurs-värden. hello anger följande exempel hello namnet på webbplatsen toohello parametervärdet skickades under distributionen.

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a>Exempel

hello visas följande exempel en förenklad användning av funktionen för hello-parametrar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| stringOutput | Sträng | Alternativ 1 |
| intOutput | int | 1 |
| objectOutput | Objekt | {”1”: ”a”, ”två”: ”b”} |
| arrayOutput | Matris | [1, 2, 3] |
| crossOutput | Sträng | Alternativ 1 |

<a id="variables" />

## <a name="variables"></a>variabler
`variables(variableName)`

Returnerar hello värdet för variabeln. hello angivna variabelnamnet måste definieras under hello variabler i hello mallen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Variabelnamn |Ja |Sträng |hello namnet på variabeln hello-tooreturn. |

### <a name="return-value"></a>Returvärde

hello-värdet för angivna hello-variabeln.

### <a name="remarks"></a>Kommentarer

Normalt använder du variabler toosimplify mallen genom att skapa komplexa värden bara en gång. hello skapas följande exempel ett unikt namn för ett lagringskonto.

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a>Exempel

hello exempelmall returnerar olika variabelvärden.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| exampleOutput1 | Sträng | myVariable |
| exampleOutput2 | Matris | [1, 2, 3, 4] |
| exampleOutput3 | Sträng | myVariable |
| exampleOutput4 |  Objekt | {”egenskap1”: ”värde1”, ”egenskap2”: ”value2”} |

## <a name="next-steps"></a>Nästa steg
* En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).
* toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

