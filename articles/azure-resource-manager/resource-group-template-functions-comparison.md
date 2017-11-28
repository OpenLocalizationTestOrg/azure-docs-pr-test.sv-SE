---
title: "aaaAzure Resource Manager Mallfunktioner - jämförelse | Microsoft Docs"
description: "Beskriver hello funktioner toouse i en Azure Resource Manager mallen toocompare värden."
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
ms.openlocfilehash: ebcfc9ed6c93f8b540ec4c066e9457c621800b7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="5a522-103">Jämförelse funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="5a522-103">Comparison functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="5a522-104">Resource Manager innehåller flera funktioner för att göra jämförelser i dina mallar.</span><span class="sxs-lookup"><span data-stu-id="5a522-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="5a522-105">är lika med</span><span class="sxs-lookup"><span data-stu-id="5a522-105">equals</span></span>](#equals)
* [<span data-ttu-id="5a522-106">större</span><span class="sxs-lookup"><span data-stu-id="5a522-106">greater</span></span>](#greater)
* [<span data-ttu-id="5a522-107">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="5a522-107">greaterOrEquals</span></span>](#greaterorequals)
* [<span data-ttu-id="5a522-108">mindre</span><span class="sxs-lookup"><span data-stu-id="5a522-108">less</span></span>](#less)
* [<span data-ttu-id="5a522-109">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="5a522-109">lessOrEquals</span></span>](#lessorequals)

## <a name="equals"></a><span data-ttu-id="5a522-110">är lika med</span><span class="sxs-lookup"><span data-stu-id="5a522-110">equals</span></span>
`equals(arg1, arg2)`

<span data-ttu-id="5a522-111">Kontrollerar om två värden är lika med varandra.</span><span class="sxs-lookup"><span data-stu-id="5a522-111">Checks whether two values equal each other.</span></span>

### <a name="parameters"></a><span data-ttu-id="5a522-112">Parametrar</span><span class="sxs-lookup"><span data-stu-id="5a522-112">Parameters</span></span>

| <span data-ttu-id="5a522-113">Parameter</span><span class="sxs-lookup"><span data-stu-id="5a522-113">Parameter</span></span> | <span data-ttu-id="5a522-114">Krävs</span><span class="sxs-lookup"><span data-stu-id="5a522-114">Required</span></span> | <span data-ttu-id="5a522-115">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-115">Type</span></span> | <span data-ttu-id="5a522-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a522-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5a522-117">arg1</span><span class="sxs-lookup"><span data-stu-id="5a522-117">arg1</span></span> |<span data-ttu-id="5a522-118">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-118">Yes</span></span> |<span data-ttu-id="5a522-119">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="5a522-119">int, string, array, or object</span></span> |<span data-ttu-id="5a522-120">hello första värde toocheck sinsemellan.</span><span class="sxs-lookup"><span data-stu-id="5a522-120">hello first value toocheck for equality.</span></span> |
| <span data-ttu-id="5a522-121">arg2</span><span class="sxs-lookup"><span data-stu-id="5a522-121">arg2</span></span> |<span data-ttu-id="5a522-122">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-122">Yes</span></span> |<span data-ttu-id="5a522-123">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="5a522-123">int, string, array, or object</span></span> |<span data-ttu-id="5a522-124">hello andra värdet toocheck sinsemellan.</span><span class="sxs-lookup"><span data-stu-id="5a522-124">hello second value toocheck for equality.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5a522-125">Returvärde</span><span class="sxs-lookup"><span data-stu-id="5a522-125">Return value</span></span>

<span data-ttu-id="5a522-126">Returnerar **SANT** om hello värdena är lika, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="5a522-126">Returns **True** if hello values are equal; otherwise, **False**.</span></span>

### <a name="remarks"></a><span data-ttu-id="5a522-127">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="5a522-127">Remarks</span></span>

<span data-ttu-id="5a522-128">hello är lika med funktionen används ofta med hello `condition` elementet tootest om en resurs har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="5a522-128">hello equals function is often used with hello `condition` element tootest whether a resource is deployed.</span></span>

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### <a name="example"></a><span data-ttu-id="5a522-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="5a522-129">Example</span></span>

<span data-ttu-id="5a522-130">hello exempelmall kontrollerar olika typer av värden sinsemellan.</span><span class="sxs-lookup"><span data-stu-id="5a522-130">hello example template checks different types of values for equality.</span></span> <span data-ttu-id="5a522-131">Alla standardvärden för hello returnera True.</span><span class="sxs-lookup"><span data-stu-id="5a522-131">All hello default values return True.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

<span data-ttu-id="5a522-132">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="5a522-132">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5a522-133">Namn</span><span class="sxs-lookup"><span data-stu-id="5a522-133">Name</span></span> | <span data-ttu-id="5a522-134">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-134">Type</span></span> | <span data-ttu-id="5a522-135">Värde</span><span class="sxs-lookup"><span data-stu-id="5a522-135">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5a522-136">checkInts</span><span class="sxs-lookup"><span data-stu-id="5a522-136">checkInts</span></span> | <span data-ttu-id="5a522-137">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-137">Bool</span></span> | <span data-ttu-id="5a522-138">True</span><span class="sxs-lookup"><span data-stu-id="5a522-138">True</span></span> |
| <span data-ttu-id="5a522-139">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5a522-139">checkStrings</span></span> | <span data-ttu-id="5a522-140">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-140">Bool</span></span> | <span data-ttu-id="5a522-141">True</span><span class="sxs-lookup"><span data-stu-id="5a522-141">True</span></span> |
| <span data-ttu-id="5a522-142">checkArrays</span><span class="sxs-lookup"><span data-stu-id="5a522-142">checkArrays</span></span> | <span data-ttu-id="5a522-143">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-143">Bool</span></span> | <span data-ttu-id="5a522-144">True</span><span class="sxs-lookup"><span data-stu-id="5a522-144">True</span></span> |
| <span data-ttu-id="5a522-145">checkObjects</span><span class="sxs-lookup"><span data-stu-id="5a522-145">checkObjects</span></span> | <span data-ttu-id="5a522-146">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-146">Bool</span></span> | <span data-ttu-id="5a522-147">True</span><span class="sxs-lookup"><span data-stu-id="5a522-147">True</span></span> |


<span data-ttu-id="5a522-148">hello följande exempel används [inte](resource-group-template-functions-logical.md#not) med **är lika med**.</span><span class="sxs-lookup"><span data-stu-id="5a522-148">hello following example uses [not](resource-group-template-functions-logical.md#not) with **equals**.</span></span>

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

<span data-ttu-id="5a522-149">hello utdata från hello föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="5a522-149">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="5a522-150">Namn</span><span class="sxs-lookup"><span data-stu-id="5a522-150">Name</span></span> | <span data-ttu-id="5a522-151">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-151">Type</span></span> | <span data-ttu-id="5a522-152">Värde</span><span class="sxs-lookup"><span data-stu-id="5a522-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5a522-153">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="5a522-153">checkNotEquals</span></span> | <span data-ttu-id="5a522-154">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-154">Bool</span></span> | <span data-ttu-id="5a522-155">True</span><span class="sxs-lookup"><span data-stu-id="5a522-155">True</span></span> |


## <a name="greater"></a><span data-ttu-id="5a522-156">större</span><span class="sxs-lookup"><span data-stu-id="5a522-156">greater</span></span>
`greater(arg1, arg2)`

<span data-ttu-id="5a522-157">Kontrollerar om hello första värde är större än andra hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="5a522-157">Checks whether hello first value is greater than hello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="5a522-158">Parametrar</span><span class="sxs-lookup"><span data-stu-id="5a522-158">Parameters</span></span>

| <span data-ttu-id="5a522-159">Parameter</span><span class="sxs-lookup"><span data-stu-id="5a522-159">Parameter</span></span> | <span data-ttu-id="5a522-160">Krävs</span><span class="sxs-lookup"><span data-stu-id="5a522-160">Required</span></span> | <span data-ttu-id="5a522-161">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-161">Type</span></span> | <span data-ttu-id="5a522-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a522-162">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5a522-163">arg1</span><span class="sxs-lookup"><span data-stu-id="5a522-163">arg1</span></span> |<span data-ttu-id="5a522-164">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-164">Yes</span></span> |<span data-ttu-id="5a522-165">int eller string</span><span class="sxs-lookup"><span data-stu-id="5a522-165">int or string</span></span> |<span data-ttu-id="5a522-166">hello första värde för hello större jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5a522-166">hello first value for hello greater comparison.</span></span> |
| <span data-ttu-id="5a522-167">arg2</span><span class="sxs-lookup"><span data-stu-id="5a522-167">arg2</span></span> |<span data-ttu-id="5a522-168">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-168">Yes</span></span> |<span data-ttu-id="5a522-169">int eller string</span><span class="sxs-lookup"><span data-stu-id="5a522-169">int or string</span></span> |<span data-ttu-id="5a522-170">hello sekundvärdet hello större jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5a522-170">hello second value for hello greater comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5a522-171">Returvärde</span><span class="sxs-lookup"><span data-stu-id="5a522-171">Return value</span></span>

<span data-ttu-id="5a522-172">Returnerar **SANT** om hello första värdet är större än hello sekundvärdet, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="5a522-172">Returns **True** if hello first value is greater than hello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="5a522-173">Exempel</span><span class="sxs-lookup"><span data-stu-id="5a522-173">Example</span></span>

<span data-ttu-id="5a522-174">hello exempelmall kontrollerar om ett värde för hello är större än hello andra.</span><span class="sxs-lookup"><span data-stu-id="5a522-174">hello example template checks whether hello one value is greater than hello other.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

<span data-ttu-id="5a522-175">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="5a522-175">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5a522-176">Namn</span><span class="sxs-lookup"><span data-stu-id="5a522-176">Name</span></span> | <span data-ttu-id="5a522-177">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-177">Type</span></span> | <span data-ttu-id="5a522-178">Värde</span><span class="sxs-lookup"><span data-stu-id="5a522-178">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5a522-179">checkInts</span><span class="sxs-lookup"><span data-stu-id="5a522-179">checkInts</span></span> | <span data-ttu-id="5a522-180">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-180">Bool</span></span> | <span data-ttu-id="5a522-181">False</span><span class="sxs-lookup"><span data-stu-id="5a522-181">False</span></span> |
| <span data-ttu-id="5a522-182">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5a522-182">checkStrings</span></span> | <span data-ttu-id="5a522-183">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-183">Bool</span></span> | <span data-ttu-id="5a522-184">True</span><span class="sxs-lookup"><span data-stu-id="5a522-184">True</span></span> |


## <a name="greaterorequals"></a><span data-ttu-id="5a522-185">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="5a522-185">greaterOrEquals</span></span>
`greaterOrEquals(arg1, arg2)`

<span data-ttu-id="5a522-186">Kontrollerar om hello första värde är större än eller lika toohello andra värdet.</span><span class="sxs-lookup"><span data-stu-id="5a522-186">Checks whether hello first value is greater than or equal toohello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="5a522-187">Parametrar</span><span class="sxs-lookup"><span data-stu-id="5a522-187">Parameters</span></span>

| <span data-ttu-id="5a522-188">Parameter</span><span class="sxs-lookup"><span data-stu-id="5a522-188">Parameter</span></span> | <span data-ttu-id="5a522-189">Krävs</span><span class="sxs-lookup"><span data-stu-id="5a522-189">Required</span></span> | <span data-ttu-id="5a522-190">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-190">Type</span></span> | <span data-ttu-id="5a522-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a522-191">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5a522-192">arg1</span><span class="sxs-lookup"><span data-stu-id="5a522-192">arg1</span></span> |<span data-ttu-id="5a522-193">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-193">Yes</span></span> |<span data-ttu-id="5a522-194">int eller string</span><span class="sxs-lookup"><span data-stu-id="5a522-194">int or string</span></span> |<span data-ttu-id="5a522-195">hello första värde för hello större eller lika jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5a522-195">hello first value for hello greater or equal comparison.</span></span> |
| <span data-ttu-id="5a522-196">arg2</span><span class="sxs-lookup"><span data-stu-id="5a522-196">arg2</span></span> |<span data-ttu-id="5a522-197">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-197">Yes</span></span> |<span data-ttu-id="5a522-198">int eller string</span><span class="sxs-lookup"><span data-stu-id="5a522-198">int or string</span></span> |<span data-ttu-id="5a522-199">hello sekundvärdet hello större eller lika jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5a522-199">hello second value for hello greater or equal comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5a522-200">Returvärde</span><span class="sxs-lookup"><span data-stu-id="5a522-200">Return value</span></span>

<span data-ttu-id="5a522-201">Returnerar **SANT** om hello första värdet är större än eller lika toohello sekundvärdet; annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="5a522-201">Returns **True** if hello first value is greater than or equal toohello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="5a522-202">Exempel</span><span class="sxs-lookup"><span data-stu-id="5a522-202">Example</span></span>

<span data-ttu-id="5a522-203">hello exempelmall kontrollerar om ett värde för hello är större än eller lika med toohello andra.</span><span class="sxs-lookup"><span data-stu-id="5a522-203">hello example template checks whether hello one value is greater than or equal toohello other.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

<span data-ttu-id="5a522-204">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="5a522-204">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5a522-205">Namn</span><span class="sxs-lookup"><span data-stu-id="5a522-205">Name</span></span> | <span data-ttu-id="5a522-206">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-206">Type</span></span> | <span data-ttu-id="5a522-207">Värde</span><span class="sxs-lookup"><span data-stu-id="5a522-207">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5a522-208">checkInts</span><span class="sxs-lookup"><span data-stu-id="5a522-208">checkInts</span></span> | <span data-ttu-id="5a522-209">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-209">Bool</span></span> | <span data-ttu-id="5a522-210">False</span><span class="sxs-lookup"><span data-stu-id="5a522-210">False</span></span> |
| <span data-ttu-id="5a522-211">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5a522-211">checkStrings</span></span> | <span data-ttu-id="5a522-212">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-212">Bool</span></span> | <span data-ttu-id="5a522-213">True</span><span class="sxs-lookup"><span data-stu-id="5a522-213">True</span></span> |



## <a name="less"></a><span data-ttu-id="5a522-214">mindre</span><span class="sxs-lookup"><span data-stu-id="5a522-214">less</span></span>
`less(arg1, arg2)`

<span data-ttu-id="5a522-215">Kontrollerar om hello första värdet är mindre än hello andra värdet.</span><span class="sxs-lookup"><span data-stu-id="5a522-215">Checks whether hello first value is less than hello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="5a522-216">Parametrar</span><span class="sxs-lookup"><span data-stu-id="5a522-216">Parameters</span></span>

| <span data-ttu-id="5a522-217">Parameter</span><span class="sxs-lookup"><span data-stu-id="5a522-217">Parameter</span></span> | <span data-ttu-id="5a522-218">Krävs</span><span class="sxs-lookup"><span data-stu-id="5a522-218">Required</span></span> | <span data-ttu-id="5a522-219">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-219">Type</span></span> | <span data-ttu-id="5a522-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a522-220">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5a522-221">arg1</span><span class="sxs-lookup"><span data-stu-id="5a522-221">arg1</span></span> |<span data-ttu-id="5a522-222">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-222">Yes</span></span> |<span data-ttu-id="5a522-223">int eller string</span><span class="sxs-lookup"><span data-stu-id="5a522-223">int or string</span></span> |<span data-ttu-id="5a522-224">hello första värde för hello mindre jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5a522-224">hello first value for hello less comparison.</span></span> |
| <span data-ttu-id="5a522-225">arg2</span><span class="sxs-lookup"><span data-stu-id="5a522-225">arg2</span></span> |<span data-ttu-id="5a522-226">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-226">Yes</span></span> |<span data-ttu-id="5a522-227">int eller string</span><span class="sxs-lookup"><span data-stu-id="5a522-227">int or string</span></span> |<span data-ttu-id="5a522-228">hello sekundvärdet för hello mindre jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5a522-228">hello second value for hello less comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5a522-229">Returvärde</span><span class="sxs-lookup"><span data-stu-id="5a522-229">Return value</span></span>

<span data-ttu-id="5a522-230">Returnerar **SANT** om hello första värdet är mindre än hello value andra; annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="5a522-230">Returns **True** if hello first value is less than hello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="5a522-231">Exempel</span><span class="sxs-lookup"><span data-stu-id="5a522-231">Example</span></span>

<span data-ttu-id="5a522-232">hello exempelmall kontrollerar om ett värde för hello är mindre än hello andra.</span><span class="sxs-lookup"><span data-stu-id="5a522-232">hello example template checks whether hello one value is less than hello other.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

<span data-ttu-id="5a522-233">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="5a522-233">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5a522-234">Namn</span><span class="sxs-lookup"><span data-stu-id="5a522-234">Name</span></span> | <span data-ttu-id="5a522-235">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-235">Type</span></span> | <span data-ttu-id="5a522-236">Värde</span><span class="sxs-lookup"><span data-stu-id="5a522-236">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5a522-237">checkInts</span><span class="sxs-lookup"><span data-stu-id="5a522-237">checkInts</span></span> | <span data-ttu-id="5a522-238">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-238">Bool</span></span> | <span data-ttu-id="5a522-239">True</span><span class="sxs-lookup"><span data-stu-id="5a522-239">True</span></span> |
| <span data-ttu-id="5a522-240">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5a522-240">checkStrings</span></span> | <span data-ttu-id="5a522-241">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-241">Bool</span></span> | <span data-ttu-id="5a522-242">False</span><span class="sxs-lookup"><span data-stu-id="5a522-242">False</span></span> |


## <a name="lessorequals"></a><span data-ttu-id="5a522-243">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="5a522-243">lessOrEquals</span></span>
`lessOrEquals(arg1, arg2)`

<span data-ttu-id="5a522-244">Kontrollerar om hello första värde är mindre än eller lika med andra toohello-värdet.</span><span class="sxs-lookup"><span data-stu-id="5a522-244">Checks whether hello first value is less than or equal toohello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="5a522-245">Parametrar</span><span class="sxs-lookup"><span data-stu-id="5a522-245">Parameters</span></span>

| <span data-ttu-id="5a522-246">Parameter</span><span class="sxs-lookup"><span data-stu-id="5a522-246">Parameter</span></span> | <span data-ttu-id="5a522-247">Krävs</span><span class="sxs-lookup"><span data-stu-id="5a522-247">Required</span></span> | <span data-ttu-id="5a522-248">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-248">Type</span></span> | <span data-ttu-id="5a522-249">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a522-249">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5a522-250">arg1</span><span class="sxs-lookup"><span data-stu-id="5a522-250">arg1</span></span> |<span data-ttu-id="5a522-251">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-251">Yes</span></span> |<span data-ttu-id="5a522-252">int eller string</span><span class="sxs-lookup"><span data-stu-id="5a522-252">int or string</span></span> |<span data-ttu-id="5a522-253">Hej första värde för hello mindre eller lika med jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5a522-253">hello first value for hello less or equals comparison.</span></span> |
| <span data-ttu-id="5a522-254">arg2</span><span class="sxs-lookup"><span data-stu-id="5a522-254">arg2</span></span> |<span data-ttu-id="5a522-255">Ja</span><span class="sxs-lookup"><span data-stu-id="5a522-255">Yes</span></span> |<span data-ttu-id="5a522-256">int eller string</span><span class="sxs-lookup"><span data-stu-id="5a522-256">int or string</span></span> |<span data-ttu-id="5a522-257">Hej sekundvärdet för hello mindre eller lika med jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5a522-257">hello second value for hello less or equals comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5a522-258">Returvärde</span><span class="sxs-lookup"><span data-stu-id="5a522-258">Return value</span></span>

<span data-ttu-id="5a522-259">Returnerar **SANT** om hello första värdet är mindre än eller lika toohello sekundvärdet, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="5a522-259">Returns **True** if hello first value is less than or equal toohello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="5a522-260">Exempel</span><span class="sxs-lookup"><span data-stu-id="5a522-260">Example</span></span>

<span data-ttu-id="5a522-261">hello exempelmall kontrollerar om ett värde för hello är mindre än eller lika toohello andra.</span><span class="sxs-lookup"><span data-stu-id="5a522-261">hello example template checks whether hello one value is less than or equal toohello other.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

<span data-ttu-id="5a522-262">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="5a522-262">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5a522-263">Namn</span><span class="sxs-lookup"><span data-stu-id="5a522-263">Name</span></span> | <span data-ttu-id="5a522-264">Typ</span><span class="sxs-lookup"><span data-stu-id="5a522-264">Type</span></span> | <span data-ttu-id="5a522-265">Värde</span><span class="sxs-lookup"><span data-stu-id="5a522-265">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5a522-266">checkInts</span><span class="sxs-lookup"><span data-stu-id="5a522-266">checkInts</span></span> | <span data-ttu-id="5a522-267">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-267">Bool</span></span> | <span data-ttu-id="5a522-268">True</span><span class="sxs-lookup"><span data-stu-id="5a522-268">True</span></span> |
| <span data-ttu-id="5a522-269">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5a522-269">checkStrings</span></span> | <span data-ttu-id="5a522-270">bool</span><span class="sxs-lookup"><span data-stu-id="5a522-270">Bool</span></span> | <span data-ttu-id="5a522-271">False</span><span class="sxs-lookup"><span data-stu-id="5a522-271">False</span></span> |



## <a name="next-steps"></a><span data-ttu-id="5a522-272">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a522-272">Next steps</span></span>
* <span data-ttu-id="5a522-273">En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5a522-273">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="5a522-274">flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5a522-274">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="5a522-275">tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="5a522-275">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="5a522-276">toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="5a522-276">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

