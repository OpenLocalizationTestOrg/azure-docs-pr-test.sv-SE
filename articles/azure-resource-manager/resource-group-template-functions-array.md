---
title: aaaAzure Resource Manager-mall fungerar - matriser och -objekt | Microsoft Docs
description: "Beskriver hello funktioner toouse i en Azure Resource Manager-mall för att arbeta med matriser och -objekt."
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
ms.date: 06/12/2017
ms.author: tomfitz
ms.openlocfilehash: e5f1a9b2a71039562eae7e48c2474a1fa59a7bea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a>Array- och funktioner för Azure Resource Manager-mallar 

Resource Manager innehåller flera funktioner för att arbeta med matriser och -objekt.

* [matris](#array)
* [Slå samman](#coalesce)
* [concat](#concat)
* [innehåller](#contains)
* [createArray](#createarray)
* [tom](#empty)
* [första](#first)
* [skärningspunkten](#intersection)
* [JSON](#json)
* [senaste](#last)
* [längd](#length)
* [Min](#min)
* [Max](#max)
* [intervallet](#range)
* [Hoppa över](#skip)
* [ta](#take)
* [Union](#union)

tooget en matris med strängvärden som avgränsas med ett värde, se [dela](resource-group-template-functions-string.md#split).

<a id="array" />

## <a name="array"></a>matris
`array(convertToArray)`

Konverterar hello tooan värdematrisen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| convertToArray |Ja |int, string, matris eller objekt |Hej tooconvert tooan värdematrisen. |

### <a name="return-value"></a>Returvärde

En matris.

### <a name="example"></a>Exempel

hello följande exempel visas hur toouse hello matris-funktionen med olika typer.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "intToConvert": {
            "type": "int",
            "defaultValue": 1
        },
        "stringToConvert": {
            "type": "string",
            "defaultValue": "a"
        },
        "objectToConvert": {
            "type": "object",
            "defaultValue": {"a": "b", "c": "d"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "intOutput": {
            "type": "array",
            "value": "[array(parameters('intToConvert'))]"
        },
        "stringOutput": {
            "type": "array",
            "value": "[array(parameters('stringToConvert'))]"
        },
        "objectOutput": {
            "type": "array",
            "value": "[array(parameters('objectToConvert'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| intOutput | Matris | [1] |
| stringOutput | Matris | [”a”] |
| objectOutput | Matris | [{”a”: ”b”, ”c”: ”d”}] |

<a id="coalesce" />

## <a name="coalesce"></a>Slå samman
`coalesce(arg1, arg2, arg3, ...)`

Returnerar första icke-null-värde från hello parametrar. Tomma strängar, tomma matriser och tomt-objekt är inte null.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |int, string, matris eller objekt |hello första värde tootest för null. |
| ytterligare argument |Nej |int, string, matris eller objekt |Ytterligare värden tootest för null. |

### <a name="return-value"></a>Returvärde

hello värde av hello första icke-null-parametrar, vilket kan vara en sträng, int, matris eller ett objekt. Null om alla parametrar är null. 

### <a name="example"></a>Exempel

hello visar följande exempel hello utdata från olika användningsområden för coalesce.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "null1": null, 
                "null2": null,
                "string": "default",
                "int": 1,
                "object": {"first": "default"},
                "array": [1]
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').string)]"
        },
        "intOutput": {
            "type": "int",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').int)]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').object)]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').array)]"
        },
        "emptyOutput": {
            "type": "bool",
            "value": "[empty(coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| stringOutput | Sträng | Standard |
| intOutput | int | 1 |
| objectOutput | Objekt | {”första”: ”default”} |
| arrayOutput | Matris | [1] |
| emptyOutput | bool | True |

<a id="concat" />

## <a name="concat"></a>concat
`concat(arg1, arg2, arg3, ...)`

Kombinerar flera matriser och returnerar hello sammanfogas matrisen eller kombinerar flera strängvärden och returnerar hello sammanfogas sträng. 

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller sträng |Hej första matrisen eller sträng för sammanslagning. |
| ytterligare argument |Nej |matris eller sträng |Ytterligare matriser eller strängar i tur och ordning för sammanslagning. |

Den här funktionen kan ta valfritt antal argument och kan acceptera strängar eller matriser för hello parametrar.

### <a name="return-value"></a>Returvärde
En sträng eller en matris med sammanfogade värdena.

### <a name="example"></a>Exempel

hello som följande exempel visar hur toocombine två matriser.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| Returnera | Matris | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

hello som följande exempel visar hur toocombine två sträng värden och returnerar en sammanfogad sträng.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| concatOutput | Sträng | prefixet 5yj4yjf5mbg72 |

<a id="contains" />

## <a name="contains"></a>Innehåller
`contains(container, itemToFind)`

Kontrollerar om en matris som innehåller ett värde, ett objekt som innehåller en nyckel eller en sträng som innehåller understrängen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Behållaren |Ja |matris, objekt eller sträng |hello-värde som innehåller hello värdet toofind. |
| itemToFind |Ja |sträng eller ett heltal |hello värdet toofind. |

### <a name="return-value"></a>Returvärde

**SANT** om hello objekt hittades, annars **FALSKT**.

### <a name="example"></a>Exempel

hello följande exempel visas hur toouse innehåller med olika typer:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| stringTrue | bool | True |
| stringFalse | bool | False |
| objectTrue | bool | True |
| objectFalse | bool | False |
| arrayTrue | bool | True |
| arrayFalse | bool | False |

<a id="createarray" />

## <a name="createarray"></a>createarray
`createArray (arg1, arg2, arg3, ...)`

Skapar en matris av hello parametrar.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Sträng, heltal, matris eller objekt |hello första värdet i hello matris. |
| ytterligare argument |Nej |Sträng, heltal, matris eller objekt |Ytterligare värden i hello matris. |

### <a name="return-value"></a>Returvärde

En matris.

### <a name="example"></a>Exempel

följande exempel visar hur hello toouse createArray med olika typer:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringArray": {
            "type": "array",
            "value": "[createArray('a', 'b', 'c')]"
        },
        "intArray": {
            "type": "array",
            "value": "[createArray(1, 2, 3)]"
        },
        "objectArray": {
            "type": "array",
            "value": "[createArray(parameters('objectToTest'))]"
        },
        "arrayArray": {
            "type": "array",
            "value": "[createArray(parameters('arrayToTest'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| stringArray | Matris | [”a”, ”b”, ”c”] |
| intArray | Matris | [1, 2, 3] |
| objectArray | Matris | [{”1”: ”a”, ”två”: ”b”, ”tre”: ”c”}] |
| arrayArray | Matris | [[”1”, ”två”, ”tre”]] |

<a id="empty" />

## <a name="empty"></a>tom

`empty(itemToTest)`

Anger om en matris, objekt eller sträng är tom.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| itemToTest |Ja |matris, objekt eller sträng |Hej värdet toocheck om den är tom. |

### <a name="return-value"></a>Returvärde

Returnerar **SANT** om hello-värdet är tomt, annars **FALSKT**.

### <a name="example"></a>Exempel

följande exempel hello kontrollerar om en matris och objektet sträng är tom.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| arrayEmpty | bool | True |
| objectEmpty | bool | True |
| stringEmpty | bool | True |

<a id="first" />

## <a name="first"></a>första
`first(arg1)`

Returnerar hello första elementet i matrisen hello eller första tecknet i hello-sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller sträng |hello värdet tooretrieve hello första elementet eller tecken. |

### <a name="return-value"></a>Returvärde

hello (sträng, int, matris eller objekt) typ av hello första elementet i en matris eller hello första tecknet i en sträng.

### <a name="example"></a>Exempel

hello följande exempel visas hur toouse hello första funktion med en matris och en sträng.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| arrayOutput | Sträng | en |
| stringOutput | Sträng | O |

<a id="intersection" />

## <a name="intersection"></a>skärningspunkten
`intersection(arg1, arg2, arg3, ...)`

Returnerar en enda matris eller ett objekt med hello gemensamma element från hello parametrar.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller ett objekt |hello första värde toouse för att söka efter vanliga element. |
| arg2 |Ja |matris eller ett objekt |hello andra värdet toouse för att söka efter vanliga element. |
| ytterligare argument |Nej |matris eller ett objekt |Ytterligare värden toouse för att söka efter vanliga element. |

### <a name="return-value"></a>Returvärde

En matris eller ett objekt med hello gemensamma element.

### <a name="example"></a>Exempel

Hej följande exempel visar hur toouse snitt med matriser och -objekt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "z", "three": "c"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[intersection(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[intersection(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| objectOutput | Objekt | {”1”: ”a”, ”tre”: ”c”} |
| arrayOutput | Matris | [”två”, ”tre”] |


## <a name="json"></a>JSON
`json(arg1)`

Returnerar ett JSON-objekt.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Sträng |hello värdet tooconvert tooJSON. |


### <a name="return-value"></a>Returvärde

hello JSON-objekt från hello angiven sträng eller ett tomt-objekt när **null** har angetts.

### <a name="example"></a>Exempel

Hej följande exempel visar hur toouse snitt med matriser och -objekt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "jsonOutput": {
            "type": "object",
            "value": "[json('{\"a\": \"b\"}')]"
        },
        "nullOutput": {
            "type": "bool",
            "value": "[empty(json('null'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| jsonOutput | Objekt | {”a”: ”b”} |
| nullOutput | Booleskt värde | True |

<a id="last" />

## <a name="last"></a>senaste
`last (arg1)`

Returnerar hello sista elementet i matrisen hello eller sista tecknet i hello strängen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller sträng |hello värdet tooretrieve hello sista elementet eller tecken. |

### <a name="return-value"></a>Returvärde

hello (sträng, int, matris eller objekt) typ av hello sista elementet i en matris eller hello sista tecknet i en sträng.

### <a name="example"></a>Exempel

hello följande exempel visas hur toouse hello senaste funktion med en matris och en sträng.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| arrayOutput | Sträng | tre |
| stringOutput | Sträng | E |

<a id="length" />

## <a name="length"></a>Längd
`length(arg1)`

Returnerar hello antalet element i en matris eller tecken i en sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller sträng |Hej matris toouse för att hämta hello antalet element eller hello sträng toouse för att hämta hello antalet tecken. |

### <a name="return-value"></a>Returvärde

Int. 

### <a name="example"></a>Exempel

följande exempel visar hur hello toouse längd med en matris och sträng:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| arrayLength | int | 3 |
| stringLength | int | 13 |

Du kan använda den här funktionen med en matris toospecify hello antal upprepningar när du skapar resurser. I följande exempel hello, hello parametern **siteNames** referera tooan matris med namnen toouse när du skapar hello-webbplatser.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Mer information om hur du använder den här funktionen med en matris finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).

<a id="min" />

## <a name="min"></a>min.
`min(arg1)`

Returnerar hello minimivärdet från en matris av heltal eller en kommaavgränsad lista med heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris med heltal eller en kommaavgränsad lista med heltal |hello samling tooget hello minimivärde. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar hello minimivärdet.

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
`max(arg1)`

Returnerar hello högsta värde från en heltalsmatris eller en kommaavgränsad lista med heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris med heltal eller en kommaavgränsad lista med heltal |hello samling tooget hello maximivärde. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar hello maximivärdet.

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

<a id="range" />

## <a name="range"></a>intervallet
`range(startingInteger, numberOfElements)`

Skapar en heltalsmatris från början heltal och som innehåller ett antal objekt.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| startingInteger |Ja |int |hello första heltal i hello matris. |
| numberofElements |Ja |int |hello antal heltal i hello matris. |

### <a name="return-value"></a>Returvärde

En heltalsmatris.

### <a name="example"></a>Exempel

hello som följande exempel visar hur toouse hello intervallet funktionen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "startingInt": {
            "type": "int",
            "defaultValue": 5
        },
        "numberOfElements": {
            "type": "int",
            "defaultValue": 3
        }
    },
    "resources": [],
    "outputs": {
        "rangeOutput": {
            "type": "array",
            "value": "[range(parameters('startingInt'),parameters('numberOfElements'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| rangeOutput | Matris | [5, 6, 7] |

<a id="skip" />

## <a name="skip"></a>Hoppa över
`skip(originalValue, numberToSkip)`

Returnerar en matris med alla hello-element efter hello nummer som angetts i hello matrisen eller returnerar en sträng med tecken för alla hello när hello angett numret i hello-sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Ursprungligt värde |Ja |matris eller sträng |Hej array eller string toouse för att hoppa över. |
| numberToSkip |Ja |int |hello antal element eller tecken tooskip. Om det här värdet är 0 eller mindre, alla hello element eller tecken i hello-värde som ska returneras. Om den är större än hello längd hello matris eller sträng returneras en tom matris eller sträng. |

### <a name="return-value"></a>Returvärde

En matris eller sträng.

### <a name="example"></a>Exempel

hello följande exempel hoppar över hello angivna antalet element i matrisen hello och hello angivet antal tecken i en sträng.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| arrayOutput | Matris | [”tre”] |
| stringOutput | Sträng | två tre |

<a id="take" />

## <a name="take"></a>ta
`take(originalValue, numberToTake)`

Returnerar en matris med hello angivna antalet element från hello start av hello matris eller en sträng med hello angivet antal tecken från hello början av hello-sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Ursprungligt värde |Ja |matris eller sträng |Hej array eller string tootake hello element från. |
| numberToTake |Ja |int |hello antal element eller tecken tootake. Om det här värdet är 0 eller mindre, returneras en tom matris eller sträng. Om den är större än hello hello matris eller sträng returneras alla hello-element i hello array eller string. |

### <a name="return-value"></a>Returvärde

En matris eller sträng.

### <a name="example"></a>Exempel

följande exempel tar hello hello angivet antal element från hello matris och tecken från en sträng.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| arrayOutput | Matris | [””, ”två”] |
| stringOutput | Sträng | på |

<a id="union" />

## <a name="union"></a>Union
`union(arg1, arg2, arg3, ...)`

Returnerar en enda matris eller ett objekt med alla element från hello parametrar. Duplicerade värden eller nycklar är bara ingår en gång.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller ett objekt |hello första värde toouse för att ansluta till element. |
| arg2 |Ja |matris eller ett objekt |hello andra värdet toouse för att ansluta till element. |
| ytterligare argument |Nej |matris eller ett objekt |Ytterligare värden toouse för att ansluta till element. |

### <a name="return-value"></a>Returvärde

En matris eller ett objekt.

### <a name="example"></a>Exempel

Hej följande exempel visar hur toouse union med matriser och -objekt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"three": "c", "four": "d", "five": "e"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["three", "four"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[union(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[union(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| objectOutput | Objekt | {”1”: ”a”, ”två”: ”b”, ”tre”: ”c”, ”fyra”: ”d”, ”fem”: ”e”} |
| arrayOutput | Matris | [”1”, ”två”, ”tre”, ”fyra”] |

## <a name="next-steps"></a>Nästa steg
* En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).
* toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

