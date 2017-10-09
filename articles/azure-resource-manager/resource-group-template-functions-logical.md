---
title: aaaAzure Resource Manager Mallfunktioner - logiska | Microsoft Docs
description: "Beskriver hello funktioner toouse i en Azure Resource Manager mallen toodetermine logiska värden."
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
ms.openlocfilehash: aec6341fbde00b4eba3b4539ff9a9aec774333fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Logiska funktioner för Azure Resource Manager-mallar

Resource Manager innehåller flera funktioner för att göra jämförelser i dina mallar.

* [och](#and)
* [bool](#bool)
* [Om](#if)
* [inte](#not)
* [eller](#or)

## <a name="and"></a>och
`and(arg1, arg2)`

Kontrollerar om båda parametervärden är true.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Booleskt värde |Hej första värde toocheck om är true. |
| arg2 |Ja |Booleskt värde |Hej andra värdet toocheck om är true. |

### <a name="return-value"></a>Returvärde

Returnerar **SANT** om båda värdena är true, annars **FALSKT**.

### <a name="examples"></a>Exempel

följande exempel visar hur hello toouse logiska funktioner.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

hello utdata från hello föregående exempel är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| andExampleOutput | bool | False |
| orExampleOutput | bool | True |
| notExampleOutput | bool | False |


## <a name="bool"></a>bool
`bool(arg1)`

Konverterar hello parametern tooa boolean.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |sträng eller ett heltal |Hej värdet tooconvert tooa boolean. |

### <a name="return-value"></a>Returvärde
Ett booleskt värde för hello konverteras värdet.

### <a name="examples"></a>Exempel

följande exempel visar hur hello toouse bool med en sträng eller ett heltal.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| trueString | bool | True |
| falseString | bool | False |
| trueInt | bool | True |
| falseInt | bool | False |

## <a name="if"></a>Om
`if(condition, trueValue, falseValue)`

Returnerar ett värde baserat på om ett villkor är SANT eller FALSKT.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Villkor |Ja |Booleskt värde |Hej värdet toocheck om det inte stämmer. |
| trueValue |Ja | sträng, int, objekt eller matris |hello värdet tooreturn när hello villkoret är sant. |
| falseValue |Ja | sträng, int, objekt eller matris |hello värdet tooreturn när hello villkoret är FALSKT. |

### <a name="return-value"></a>Returvärde

Returnerar andra parameter när den första parametern är **SANT**, annars returnerar tredje parametern.

### <a name="remarks"></a>Kommentarer

Du kan använda den här funktionen tooconditionally en resursegenskap. hello följande exempel är inte en fullständig mall, men den visar hello relevanta delar för att ange villkorligt hello tillgänglighetsuppsättning.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "availabilitySet": {
            "type": "string",
            "allowedValues": [
                "yes",
                "no"
            ]
        }
    },
    "variables": {
        ...
        "availabilitySetName": "availabilitySet1",
        "availabilitySet": {
            "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
        }
    },
    "resources": [
        {
            "condition": "[equals(parameters('availabilitySet'),'yes')]",
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            ...
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "properties": {
                "availabilitySet": "[if(equals(parameters('availabilitySet'),'yes'), variables('availabilitySet'), json('null'))]",
                ...
            }
        },
        ...
    ],
    ...
}
```

### <a name="examples"></a>Exempel

följande exempel visar hur hello toouse hello `if` funktion.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        }
    }
}
```

hello utdata från hello föregående exempel är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| yesOutput | Sträng | Ja |
| noOutput | Sträng | Nej |


## <a name="not"></a>inte
`not(arg1)`

Konverterar booleskt värde tooits motsatt värde.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Booleskt värde |hello värdet tooconvert. |


### <a name="return-value"></a>Returvärde

Returnerar **SANT** när parametern är **FALSKT**. Returnerar **FALSKT** när parametern är **SANT**.

### <a name="examples"></a>Exempel

följande exempel visar hur hello toouse logiska funktioner.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

hello utdata från hello föregående exempel är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| andExampleOutput | bool | False |
| orExampleOutput | bool | True |
| notExampleOutput | bool | False |

hello följande exempel används **inte** med [är lika med](resource-group-template-functions-comparison.md#equals).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

hello utdata från hello föregående exempel är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| checkNotEquals | bool | True |


## <a name="or"></a>eller
`or(arg1, arg2)`

Kontrollerar om antingen parametervärdet är true.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Booleskt värde |Hej första värde toocheck om är true. |
| arg2 |Ja |Booleskt värde |Hej andra värdet toocheck om är true. |

### <a name="return-value"></a>Returvärde

Returnerar **SANT** om antingen värdet är true, annars **FALSKT**.

### <a name="examples"></a>Exempel

följande exempel visar hur hello toouse logiska funktioner.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

hello utdata från hello föregående exempel är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| andExampleOutput | bool | False |
| orExampleOutput | bool | True |
| notExampleOutput | bool | False |


## <a name="next-steps"></a>Nästa steg
* En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).
* toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

