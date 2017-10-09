---
title: "aaaAzure Mallfunktioner för Resource Manager - sträng | Microsoft Docs"
description: "Beskriver hello funktioner toouse i en Azure Resource Manager-mallen toowork med strängar."
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
ms.date: 08/01/2017
ms.author: tomfitz
ms.openlocfilehash: 27f7f6a52cbe4e9915718184433e92ca92999346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="string-functions-for-azure-resource-manager-templates"></a>Strängfunktioner för Azure Resource Manager-mallar

Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med strängar hello:

* [Base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [concat](#concat)
* [innehåller](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [tom](#empty)
* [endsWith](#endswith)
* [första](#first)
* [indexOf](#indexof)
* [senaste](#last)
* [lastIndexOf](#lastindexof)
* [längd](#length)
* [padLeft](#padleft)
* [Ersätt](#replace)
* [Hoppa över](#skip)
* [split](#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [sträng](#string)
* [delsträngen](#substring)
* [ta](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [trim](#trim)
* [uniqueString](#uniquestring)
* [URI: n](#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a>Base64
`base64(inputString)`

Returnerar hello base64-representation av hello Indatasträngen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| inputString |Ja |Sträng |hello värdet tooreturn som en base64-representation. |

### <a name="return-value"></a>Returvärde

En sträng som innehåller hello base64-representation.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur toouse hello base64-funktionen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| base64Output | Sträng | b25lLCB0d28sIHRocmVl |
| toStringOutput | Sträng | Ett två tre |
| toJsonOutput | Objekt | {”1”: ”a”, ”två”: ”b”} |

<a id="base64tojson" />

## <a name="base64tojson"></a>base64ToJson
`base64tojson`

Konverterar ett base64-representation tooa JSON-objekt.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| base64Value |Ja |Sträng |hello base64-representation tooconvert tooa JSON-objekt. |

### <a name="return-value"></a>Returvärde

Ett JSON-objekt.

### <a name="examples"></a>Exempel

hello används följande exempel hello base64ToJson funktionen tooconvert base64-värde:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| base64Output | Sträng | b25lLCB0d28sIHRocmVl |
| toStringOutput | Sträng | Ett två tre |
| toJsonOutput | Objekt | {”1”: ”a”, ”två”: ”b”} |

<a id="base64tostring" />

## <a name="base64tostring"></a>base64ToString
`base64ToString(base64Value)`

Konverterar en base64-representation tooa sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| base64Value |Ja |Sträng |hello base64-representation tooconvert tooa sträng. |

### <a name="return-value"></a>Returvärde

En sträng med hello konverteras base64-värde.

### <a name="examples"></a>Exempel

hello används följande exempel hello base64ToString funktionen tooconvert base64-värde:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| base64Output | Sträng | b25lLCB0d28sIHRocmVl |
| toStringOutput | Sträng | Ett två tre |
| toJsonOutput | Objekt | {”1”: ”a”, ”två”: ”b”} |



<a id="concat" />

## <a name="concat"></a>concat
`concat (arg1, arg2, arg3, ...)`

Kombinerar flera strängvärden och returnerar hello sammanfogas sträng eller kombinerar flera matriser och returnerar hello sammanfogas matris.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |sträng eller matris |hello första värde för sammanslagning. |
| ytterligare argument |Nej |Sträng |Ytterligare värden i tur och ordning för sammanslagning. |

### <a name="return-value"></a>Returvärde
En sträng eller en matris med sammanfogade värdena.

### <a name="examples"></a>Exempel

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

<a id="contains" />

## <a name="contains"></a>Innehåller
`contains (container, itemToFind)`

Kontrollerar om en matris som innehåller ett värde, ett objekt som innehåller en nyckel eller en sträng som innehåller understrängen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Behållaren |Ja |matris, objekt eller sträng |hello-värde som innehåller hello värdet toofind. |
| itemToFind |Ja |sträng eller ett heltal |hello värdet toofind. |

### <a name="return-value"></a>Returvärde

**SANT** om hello objekt hittades, annars **FALSKT**.

### <a name="examples"></a>Exempel

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

<a id="datauri" />

## <a name="datauri"></a>dataUri
`dataUri(stringToConvert)`

Konverterar en tooa värdedata URI.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToConvert |Ja |Sträng |Hej tooconvert tooa värdedata URI. |

### <a name="return-value"></a>Returvärde

En sträng formaterad som en data-URI.

### <a name="examples"></a>Exempel

hello följande exempel konverterar en tooa värdedata URI och konverterar data URI tooa sträng:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| dataUriOutput | Sträng | data: text / oformaterad; charset = utf8; base64 SGVsbG8 = |
| toStringOutput | Sträng | Hej världen! |

<a id="datauritostring" />

## <a name="datauritostring"></a>dataUriToString
`dataUriToString(dataUriToConvert)`

Konverterar ett data-URI formaterade värdet tooa sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Ja |Sträng |hello data tooconvert för URI-värdet. |

### <a name="return-value"></a>Returvärde

En sträng som innehåller hello konverteras värdet.

### <a name="examples"></a>Exempel

hello följande exempel konverterar en tooa värdedata URI och konverterar data URI tooa sträng:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| dataUriOutput | Sträng | data: text / oformaterad; charset = utf8; base64 SGVsbG8 = |
| toStringOutput | Sträng | Hej världen! |

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

### <a name="examples"></a>Exempel

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

<a id="endswith" />

## <a name="endswith"></a>endsWith
`endsWith(stringToSearch, stringToFind)`

Anger om en sträng som slutar med ett värde. hello jämförelse är skiftlägeskänsliga.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |Sträng |hello-värde som innehåller hello objektet toofind. |
| stringToFind |Ja |Sträng |hello värdet toofind. |

### <a name="return-value"></a>Returvärde

**SANT** om hello sista tecknet eller tecknen i hello strängen matchar hello värdet; annars **FALSKT**.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur toouse hello startsWith och endsWith-funktioner:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| startsTrue | bool | True |
| startsCapTrue | bool | True |
| startsFalse | bool | False |
| endsTrue | bool | True |
| endsCapTrue | bool | True |
| endsFalse | bool | False |

<a id="first" />

## <a name="first"></a>första
`first(arg1)`

Returnerar hello första tecknet i hello sträng eller första elementet i matrisen hello.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller sträng |hello värdet tooretrieve hello första elementet eller tecken. |

### <a name="return-value"></a>Returvärde

En sträng med hello första tecken eller hello typ (sträng, int, matris eller objekt) för hello första element i en matris.

### <a name="examples"></a>Exempel

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

<a id="indexof" />

## <a name="indexof"></a>indexOf
`indexOf(stringToSearch, stringToFind)`

Returnerar hello första positionen för ett värde inom en sträng. hello jämförelse är skiftlägeskänsliga.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |Sträng |hello-värde som innehåller hello objektet toofind. |
| stringToFind |Ja |Sträng |hello värdet toofind. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar hello position i hello objektet toofind. hello-värdet är nollbaserade. Om hello objektet inte hittades, returneras -1.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur toouse hello indexOf och lastIndexOf funktioner:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

<a id="last" />

## <a name="last"></a>senaste
`last (arg1)`

Returnerar det sista tecknet i hello strängen eller hello sista elementet i matrisen hello.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller sträng |hello värdet tooretrieve hello sista elementet eller tecken. |

### <a name="return-value"></a>Returvärde

En sträng med hello sista tecknet eller hello typ (sträng, int, matris eller objekt) av hello sista elementet i en matris.

### <a name="examples"></a>Exempel

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

<a id="lastindexof" />

## <a name="lastindexof"></a>lastIndexOf
`lastIndexOf(stringToSearch, stringToFind)`

Returnerar hello sista positionen för ett värde inom en sträng. hello jämförelse är skiftlägeskänsliga.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |Sträng |hello-värde som innehåller hello objektet toofind. |
| stringToFind |Ja |Sträng |hello värdet toofind. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar hello senaste position i hello objektet toofind. hello-värdet är nollbaserade. Om hello objektet inte hittades, returneras -1.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur toouse hello indexOf och lastIndexOf funktioner:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

<a id="length" />

## <a name="length"></a>Längd
`length(string)`

Returnerar hello antalet tecken i en sträng eller element i en matris.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris eller sträng |Hej matris toouse för att hämta hello antalet element eller hello sträng toouse för att hämta hello antalet tecken. |

### <a name="return-value"></a>Returvärde

Int. 

### <a name="examples"></a>Exempel

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

<a id="padleft" />

## <a name="padleft"></a>padLeft
`padLeft(valueToPad, totalLength, paddingCharacter)`

Returnerar en högerjusterad sträng genom att lägga till tecken toohello vänster tills hello totala angiven längd.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| valueToPad |Ja |sträng eller ett heltal |Hej värdet tooright-justera. |
| totalLength |Ja |int |hello totala antalet tecken i hello returnerade sträng. |
| paddingCharacter |Nej |enskilt tecken |hello tecken toouse för vänster-utfyllnad tills hello totala längden har uppnåtts. hello standardvärdet är ett blanksteg. |

Om ursprungliga hello-strängen är längre än hello antal tecken toopad läggs inga tecken.

### <a name="return-value"></a>Returvärde

En sträng med hello minst antal angivna tecken.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur toopad hello som användaren tillhandahållit parametervärdet genom att lägga till hello noll tecken tills hello Totalt antal tecken. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| stringOutput | Sträng | 0000000123 |

<a id="replace" />

## <a name="replace"></a>Ersätt
`replace(originalString, oldString, newString)`

Returnerar en ny sträng med alla instanser av en sträng som har ersatts av en annan sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| originalString |Ja |Sträng |hello-värde som har alla instanser av en sträng som har ersatts av en annan sträng. |
| oldString |Ja |Sträng |hello sträng toobe tas bort från hello ursprungliga strängen. |
| newString |Ja |Sträng |hello sträng tooadd i stället för hello bort sträng. |

### <a name="return-value"></a>Returvärde

En sträng med hello ersättas tecken.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur tooremove alla bindestreck från hello som användaren tillhandahållit sträng, och hur tooreplace tillhör hello sträng med en annan sträng.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secodeOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| firstOutput | Sträng | 1231231234 |
| secodeOutput | Sträng | 123-123-xxxx |

<a id="skip" />

## <a name="skip"></a>Hoppa över
`skip(originalValue, numberToSkip)`

Returnerar en sträng med tecken för alla hello efter hello angivet antal tecken, eller en matris med alla hello element när hello angivet antal element.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Ursprungligt värde |Ja |matris eller sträng |Hej array eller string toouse för att hoppa över. |
| numberToSkip |Ja |int |hello antal element eller tecken tooskip. Om det här värdet är 0 eller mindre, alla hello element eller tecken i hello-värde som ska returneras. Om den är större än hello längd hello matris eller sträng returneras en tom matris eller sträng. |

### <a name="return-value"></a>Returvärde

En matris eller sträng.

### <a name="examples"></a>Exempel

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

<a id="split" />

## <a name="split"></a>split
`split(inputString, delimiter)`

Returnerar en matris med strängar som innehåller hello delsträngar i hello Indatasträngen som avgränsas med hello angiven avgränsare.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| inputString |Ja |Sträng |hello sträng toosplit. |
| Avgränsare |Ja |sträng eller strängmatris |hello avgränsare toouse för att dela hello-sträng. |

### <a name="return-value"></a>Returvärde

En matris med strängar.

### <a name="examples"></a>Exempel

hello delar följande exempel hello Indatasträngen med kommatecken och med kommatecken eller semikolon.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| firstOutput | Matris | [”1”, ”två”, ”tre”] |
| secondOutput | Matris | [”1”, ”två”, ”tre”] |

<a id="startswith" />

## <a name="startswith"></a>StartsWith
`startsWith(stringToSearch, stringToFind)`

Anger om en sträng som börjar med ett värde. hello jämförelse är skiftlägeskänsliga.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |Sträng |hello-värde som innehåller hello objektet toofind. |
| stringToFind |Ja |Sträng |hello värdet toofind. |

### <a name="return-value"></a>Returvärde

**SANT** om hello första tecknet eller tecknen i hello strängen matchar hello värdet; annars **FALSKT**.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur toouse hello startsWith och endsWith-funktioner:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| startsTrue | bool | True |
| startsCapTrue | bool | True |
| startsFalse | bool | False |
| endsTrue | bool | True |
| endsCapTrue | bool | True |
| endsFalse | bool | False |

<a id="string" />

## <a name="string"></a>Sträng
`string(valueToConvert)`

Konverterar hello anges värdet tooa sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja | Alla |hello värdet tooconvert toostring. Någon typ av värde kan konverteras, inklusive objekt och matriser. |

### <a name="return-value"></a>Returvärde

En sträng med hello konverteras värdet.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur tooconvert olika typer av värden toostrings:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| objectOutput | Sträng | {”valueA”: 10, ”valueB”: ”exempeltext”} |
| arrayOutput | Sträng | [”a”, ”b”, ”c”] |
| intOutput | Sträng | 5 |

<a id="substring" />

## <a name="substring"></a>delsträngen
`substring(stringToParse, startIndex, length)`

Returnerar en understräng som börjar på hello angetts teckenposition och innehåller hello angivet antal tecken.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToParse |Ja |Sträng |hello ursprungliga sträng från vilken hello understräng ska extraheras. |
| startIndex |Nej |int |hello nollbaserade tecken startposition hello delsträngen. |
| Längd |Nej |int |hello antal tecken för hello delsträngen. Måste referera tooa plats inom strängen hello. |

### <a name="return-value"></a>Returvärde

hello delsträngen.

### <a name="remarks"></a>Kommentarer

hello funktionen misslyckas när hello delsträngen sträcker sig utanför hello slutet av hello-sträng. följande exempel hello misslyckas med hello fel ”hello index och längd måste referera tooa plats inom strängen hello. Hej indexparametern: '0' hello längdparameter: 11, hello längden på strängparametern hello: ”10” ”..

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Exempel

följande exempel hello extraherar en understräng från en parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| substringOutput | Sträng | två |


<a id="take" />

## <a name="take"></a>ta
`take(originalValue, numberToTake)`

Returnerar en sträng med hello angivet antal tecken från början hello hello sträng eller en matris med hello angivna antalet element från hello start av hello matris.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Ursprungligt värde |Ja |matris eller sträng |Hej array eller string tootake hello element från. |
| numberToTake |Ja |int |hello antal element eller tecken tootake. Om det här värdet är 0 eller mindre, returneras en tom matris eller sträng. Om den är större än hello hello matris eller sträng returneras alla hello-element i hello array eller string. |

### <a name="return-value"></a>Returvärde

En matris eller sträng.

### <a name="examples"></a>Exempel

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

<a id="tolower" />

## <a name="tolower"></a>toLower
`toLower(stringToChange)`

Konverterar hello angetts sträng toolower fallet.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |Sträng |hello värdet tooconvert toolower case. |

### <a name="return-value"></a>Returvärde

toolower fall konvertera hello-strängen.

### <a name="examples"></a>Exempel

hello följande exempel konverterar en parameter värdet toolower fallet och tooupper fallet.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| toLowerOutput | Sträng | Ett två tre |
| toUpperOutput | Sträng | ETT TVÅ TRE |

<a id="toupper" />

## <a name="toupper"></a>toUpper
`toUpper(stringToChange)`

Konverterar hello angetts sträng tooupper fallet.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |Sträng |hello värdet tooconvert tooupper case. |

### <a name="return-value"></a>Returvärde

tooupper fall konvertera hello-strängen.

### <a name="examples"></a>Exempel

hello följande exempel konverterar en parameter värdet toolower fallet och tooupper fallet.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| toLowerOutput | Sträng | Ett två tre |
| toUpperOutput | Sträng | ETT TVÅ TRE |

<a id="trim" />

## <a name="trim"></a>Rensa
`trim (stringToTrim)`

Tar bort alla inledande och avslutande blanksteg från hello angivna strängen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToTrim |Ja |Sträng |hello värdet tootrim. |

### <a name="return-value"></a>Returvärde

hello strängen utan inledande och avslutande blanksteg.

### <a name="examples"></a>Exempel

hello följande exempel tar bort hello blanksteg från hello-parametern.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| Returnera | Sträng | Ett två tre |

<a id="uniquestring" />

## <a name="uniquestring"></a>uniqueString
`uniqueString (baseString, ...)`

Skapar en deterministisk hash-sträng baserat på hello värden som parametrar. 

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| baseString |Ja |Sträng |hello-värde som används i hello hash-funktionen toocreate en unik sträng. |
| ytterligare parametrar som krävs |Nej |Sträng |Du kan lägga till så många strängar som behövs toocreate hello-värde som anger hello unikhet. |

### <a name="remarks"></a>Kommentarer

Den här funktionen är användbart när du behöver toocreate ett unikt namn för en resurs. Du kan ange parametervärden som begränsar hello omfång unikhet för hello resultat. Du kan ange om hello namn är unikt ned toosubscription, resursgrupp eller distribution. 

hello returnerade värdet är inte en slumpmässig sträng, men i stället hello resultatet av en hashfunktionen. hello returnerade värdet är 13 tecken. Det är inte globalt unikt. Du kanske vill toocombine hello-värde med ett prefix från din naming convention toocreate ett beskrivande namn. hello visar följande exempel hello format hello returnerade värde. hello faktiska värdet varierar beroende på hello angivna parametrar.

    tcvhiyu5h2o5o

hello som följande exempel visar hur toouse uniqueString toocreate ett unikt värde för ofta använda nivåer.

Unikt begränsade toosubscription

```json
"[uniqueString(subscription().subscriptionId)]"
```

Unikt begränsade tooresource grupp

```json
"[uniqueString(resourceGroup().id)]"
```

Unikt begränsade toodeployment för en resursgrupp

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

hello som följande exempel visar hur toocreate ett unikt namn för ett lagringskonto baserat på din resursgrupp. Inuti hello resursgruppen hello namn är inte unikt om konstruerat hello samma sätt.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a>Returvärde

En sträng som innehåller 13 tecken.

### <a name="examples"></a>Exempel

hello följande exempel returnerar resultat från uniquestring:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

<a id="uri" />

## <a name="uri"></a>URI: N
`uri (baseUri, relativeUri)`

Skapar en absolut URI genom att kombinera hello baseUri och hello relativeUri sträng.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| baseUri |Ja |Sträng |hello bas-uri-sträng. |
| relativeUri |Ja |Sträng |hello relativ uri sträng tooadd toohello bas-uri-strängen. |

Hej värde för hello **baseUri** parameter kan innehålla en viss fil, men endast hello bassökväg används när man skapar hello URI. Till exempel skicka `http://contoso.com/resources/azuredeploy.json` som hello baseUri parametern resultat i en bas-URI för `http://contoso.com/resources/`.

### <a name="return-value"></a>Returvärde

En sträng som representerar hello absolut URI för hello grundläggande och relativa värden.

### <a name="examples"></a>Exempel

hello som följande exempel visar hur tooconstruct en länk tooa kapslad mall baserat på hello värdet för hello överordnade mallen.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

följande exempel visar hur hello toouse uri, uriComponent, och uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| uriOutput | Sträng | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Sträng | HTTP%3a%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Sträng | http://contoso.com/resources/Nested/azuredeploy.JSON |

<a id="uricomponent" />

## <a name="uricomponent"></a>uriComponent
`uricomponent(stringToEncode)`

Kodar en URI.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| stringToEncode |Ja |Sträng |hello värdet tooencode. |

### <a name="return-value"></a>Returvärde

En sträng med hello URI kodad värde.

### <a name="examples"></a>Exempel

följande exempel visar hur hello toouse uri, uriComponent, och uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| uriOutput | Sträng | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Sträng | HTTP%3a%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Sträng | http://contoso.com/resources/Nested/azuredeploy.JSON |


<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a>uriComponentToString
`uriComponentToString(uriEncodedString)`

Returnerar en sträng med en URI-kodade värde.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Ja |Sträng |hello URI-kodad värdet tooconvert tooa sträng. |

### <a name="return-value"></a>Returvärde

Den avkodade strängen URI kodad värde.

### <a name="examples"></a>Exempel

följande exempel visar hur hello toouse uri, uriComponent, och uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| uriOutput | Sträng | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Sträng | HTTP%3a%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Sträng | http://contoso.com/resources/Nested/azuredeploy.JSON |


## <a name="next-steps"></a>Nästa steg
* En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).
* toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

