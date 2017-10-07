---
title: aaaAzure Resource Manager Mallfunktioner - numeriska | Microsoft Docs
description: Beskriver hello funktioner toouse i en Azure Resource Manager mallen toowork med siffror.
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
ms.openlocfilehash: 855d5b354d094b9815edc160e3d72efbfd36ba77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a>Numeriska funktioner för Azure Resource Manager-mallar

Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med heltal hello:

* [Lägg till](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [flyttal](#float)
* [int](#int)
* [Min](#min)
* [Max](#max)
* [MOD](#mod)
* [mul](#mul)
* [Sub](#sub)

<a id="add" />

## <a name="add"></a>Lägg till
`add(operand1, operand2)`

Returnerar hello summan av hello två angivna heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- | 
|operand1 |Ja |int |Första nummer tooadd. |
|operand2 |Ja |int |Andra nummer tooadd. |

### <a name="return-value"></a>Returvärde

Ett heltal som innehåller hello summan av hello parametrar.

### <a name="example"></a>Exempel

hello följande exempel lägger till två parametrar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer tooadd"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer tooadd"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| addResult | int | 8 |

<a id="copyindex" />

## <a name="copyindex"></a>copyIndex
`copyIndex(loopName, offset)`

Returnerar hello index för en upprepning loop. 

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| loopName | Nej | Sträng | hello namnet på hello-slinga för att hämta hello iteration. |
| förskjutning |Nej |int |Hej tooadd toohello nollbaserade iteration siffra. |

### <a name="remarks"></a>Kommentarer

Den här funktionen används alltid med en **kopiera** objekt. Om inget värde har angetts för **offset**, hello aktuella iteration värdet returneras. hello iteration värdet börjar på noll.

Hej **loopName** egenskapen gör att du toospecify om copyIndex hänvisar tooa resurs iteration eller egenskapen iteration. Om inget värde har angetts för **loopName**, hello aktuella resurs typen iteration används. Ange ett värde för **loopName** när iteration av en egenskap. 
 
En fullständig beskrivning av hur du använder **copyIndex**, se [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).

### <a name="example"></a>Exempel

hello visas följande exempel en kopia loop och hello indexvärde ingå i hello namn. 

```json
"resources": [ 
  { 
    "name": "[concat('examplecopy-', copyIndex())]", 
    "type": "Microsoft.Web/sites", 
    "copy": { 
      "name": "websitescopy", 
      "count": "[parameters('count')]" 
    }, 
    ...
  }
]
```

### <a name="return-value"></a>Returvärde

Ett heltal som representerar hello aktuella indexet för hello iteration.

<a id="div" />

## <a name="div"></a>div
`div(operand1, operand2)`

Returnerar hello Heltalsdivision med hello två angivna heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |hello nummer delas. |
| operand2 |Ja |int |hello-nummer som används toodivide. Det går inte att vara 0. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar hello division.

### <a name="example"></a>Exempel

följande exempel hello delar upp en parameter av en annan parametern.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used toodivide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| divResult | int | 2 |

<a id="float" />

## <a name="float"></a>flyttal
`float(arg1)`

Konverterar hello värdet tooa flyttal. Du kan bara använda den här funktionen vid sändning av anpassade parametrar tooan program, till exempel en Logikapp.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |sträng eller ett heltal |hello värdet tooconvert tooa flyttal. |

### <a name="return-value"></a>Returvärde
En flytande peka nummer.

### <a name="example"></a>Exempel

hello som följande exempel visar hur toouse float toopass parametrar tooa Logikapp:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
        "custom1": {
            "value": "[float('3.0')]"
        },
        "custom2": {
            "value": "[float(3)]"
        },
```

<a id="int" />

## <a name="int"></a>int
`int(valueToConvert)`

Konverterar hello angivet värde tooan heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja |sträng eller ett heltal |hello värdet tooconvert tooan heltal. |

### <a name="return-value"></a>Returvärde

Ett heltal hello konverteras värdet.

### <a name="example"></a>Exempel

hello konverterar följande exempel hello som användaren tillhandahållit parametern värdet toointeger.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": { 
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| intResult | int | 4 |


<a id="min" />

## <a name="min"></a>min.
`min (arg1)`

Returnerar hello minimivärdet från en matris av heltal eller en kommaavgränsad lista med heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris med heltal eller en kommaavgränsad lista med heltal |hello samling tooget hello minimivärde. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar minimivärdet från hello samling.

### <a name="example"></a>Exempel

följande exempel visar hur hello toouse min med en matris och en lista med heltal:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| arrayOutput | int | 0 |
| intOutput | int | 0 |

<a id="max" />

## <a name="max"></a>Max
`max (arg1)`

Returnerar hello högsta värde från en heltalsmatris eller en kommaavgränsad lista med heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris med heltal eller en kommaavgränsad lista med heltal |hello samling tooget hello maximivärde. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar maximivärdet för hello från hello samling.

### <a name="example"></a>Exempel

följande exempel visar hur hello toouse max med en matris och en lista med heltal:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| arrayOutput | int | 5 |
| intOutput | int | 5 |

<a id="mod" />

## <a name="mod"></a>MOD
`mod(operand1, operand2)`

Returnerar hello resten av hello Heltalsdivision med hello två angivna heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |hello nummer delas. |
| operand2 |Ja |int |hello-värde som är används toodivide får inte vara 0. |

### <a name="return-value"></a>Returvärde
Ett heltal som representerar hello resten.

### <a name="example"></a>Exempel

hello returneras följande exempel hello återstoden av att dividera en parameter av en annan parametern.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used toodivide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| modResult | int | 1 |

<a id="mul" />

## <a name="mul"></a>mul
`mul(operand1, operand2)`

Returnerar hello multiplicering av hello två angivna heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Första nummer toomultiply. |
| operand2 |Ja |int |Andra nummer toomultiply. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar hello multiplikation.

### <a name="example"></a>Exempel

följande exempel hello multiplicerar en parameter av en annan parametern.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer toomultiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer toomultiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| mulResult | int | 15 |

<a id="sub" />

## <a name="sub"></a>Sub
`sub(operand1, operand2)`

Returnerar hello subtraktion av hello två angivna heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |hello-nummer som dras från. |
| operand2 |Ja |int |hello-nummer som subtraheras. |

### <a name="return-value"></a>Returvärde
Ett heltal som representerar hello subtraktion.

### <a name="example"></a>Exempel

följande exempel hello subtraherar en parameter från en annan parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer toosubtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| subResult | int | 4 |

## <a name="next-steps"></a>Nästa steg
* En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).
* toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

