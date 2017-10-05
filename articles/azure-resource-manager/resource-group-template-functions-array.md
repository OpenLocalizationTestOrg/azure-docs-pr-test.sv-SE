---
title: Azure Resource Manager-mall fungerar - matriser och -objekt | Microsoft Docs
description: "Beskriver funktionerna som ska användas i en Azure Resource Manager-mall för att arbeta med matriser och -objekt."
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
ms.openlocfilehash: 0bd9ec41761c9ce575f3bcf4d1f8e8578b83e01c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="e7e86-103">Array- och funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="e7e86-103">Array and object functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="e7e86-104">Resource Manager innehåller flera funktioner för att arbeta med matriser och -objekt.</span><span class="sxs-lookup"><span data-stu-id="e7e86-104">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="e7e86-105">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-105">array</span></span>](#array)
* [<span data-ttu-id="e7e86-106">Slå samman</span><span class="sxs-lookup"><span data-stu-id="e7e86-106">coalesce</span></span>](#coalesce)
* [<span data-ttu-id="e7e86-107">concat</span><span class="sxs-lookup"><span data-stu-id="e7e86-107">concat</span></span>](#concat)
* [<span data-ttu-id="e7e86-108">innehåller</span><span class="sxs-lookup"><span data-stu-id="e7e86-108">contains</span></span>](#contains)
* [<span data-ttu-id="e7e86-109">createArray</span><span class="sxs-lookup"><span data-stu-id="e7e86-109">createArray</span></span>](#createarray)
* [<span data-ttu-id="e7e86-110">tom</span><span class="sxs-lookup"><span data-stu-id="e7e86-110">empty</span></span>](#empty)
* [<span data-ttu-id="e7e86-111">första</span><span class="sxs-lookup"><span data-stu-id="e7e86-111">first</span></span>](#first)
* [<span data-ttu-id="e7e86-112">skärningspunkten</span><span class="sxs-lookup"><span data-stu-id="e7e86-112">intersection</span></span>](#intersection)
* [<span data-ttu-id="e7e86-113">JSON</span><span class="sxs-lookup"><span data-stu-id="e7e86-113">json</span></span>](#json)
* [<span data-ttu-id="e7e86-114">senaste</span><span class="sxs-lookup"><span data-stu-id="e7e86-114">last</span></span>](#last)
* [<span data-ttu-id="e7e86-115">längd</span><span class="sxs-lookup"><span data-stu-id="e7e86-115">length</span></span>](#length)
* [<span data-ttu-id="e7e86-116">Min</span><span class="sxs-lookup"><span data-stu-id="e7e86-116">min</span></span>](#min)
* [<span data-ttu-id="e7e86-117">Max</span><span class="sxs-lookup"><span data-stu-id="e7e86-117">max</span></span>](#max)
* [<span data-ttu-id="e7e86-118">intervallet</span><span class="sxs-lookup"><span data-stu-id="e7e86-118">range</span></span>](#range)
* [<span data-ttu-id="e7e86-119">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="e7e86-119">skip</span></span>](#skip)
* [<span data-ttu-id="e7e86-120">ta</span><span class="sxs-lookup"><span data-stu-id="e7e86-120">take</span></span>](#take)
* [<span data-ttu-id="e7e86-121">Union</span><span class="sxs-lookup"><span data-stu-id="e7e86-121">union</span></span>](#union)

<span data-ttu-id="e7e86-122">En matris med strängvärden som avgränsas med ett värde finns [dela](resource-group-template-functions-string.md#split).</span><span class="sxs-lookup"><span data-stu-id="e7e86-122">To get an array of string values delimited by a value, see [split](resource-group-template-functions-string.md#split).</span></span>

<a id="array" />

## <a name="array"></a><span data-ttu-id="e7e86-123">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-123">array</span></span>
`array(convertToArray)`

<span data-ttu-id="e7e86-124">Konverterar värdet till en matris.</span><span class="sxs-lookup"><span data-stu-id="e7e86-124">Converts the value to an array.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-125">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-125">Parameters</span></span>

| <span data-ttu-id="e7e86-126">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-126">Parameter</span></span> | <span data-ttu-id="e7e86-127">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-127">Required</span></span> | <span data-ttu-id="e7e86-128">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-128">Type</span></span> | <span data-ttu-id="e7e86-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-129">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-130">convertToArray</span><span class="sxs-lookup"><span data-stu-id="e7e86-130">convertToArray</span></span> |<span data-ttu-id="e7e86-131">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-131">Yes</span></span> |<span data-ttu-id="e7e86-132">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-132">int, string, array, or object</span></span> |<span data-ttu-id="e7e86-133">Värdet som ska konverteras till en matris.</span><span class="sxs-lookup"><span data-stu-id="e7e86-133">The value to convert to an array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-134">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-134">Return value</span></span>

<span data-ttu-id="e7e86-135">En matris.</span><span class="sxs-lookup"><span data-stu-id="e7e86-135">An array.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-136">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-136">Example</span></span>

<span data-ttu-id="e7e86-137">I följande exempel visas hur du använder funktionen matris med olika typer.</span><span class="sxs-lookup"><span data-stu-id="e7e86-137">The following example shows how to use the array function with different types.</span></span>

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

<span data-ttu-id="e7e86-138">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-138">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-139">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-139">Name</span></span> | <span data-ttu-id="e7e86-140">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-140">Type</span></span> | <span data-ttu-id="e7e86-141">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-141">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-142">intOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-142">intOutput</span></span> | <span data-ttu-id="e7e86-143">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-143">Array</span></span> | <span data-ttu-id="e7e86-144">[1]</span><span class="sxs-lookup"><span data-stu-id="e7e86-144">[1]</span></span> |
| <span data-ttu-id="e7e86-145">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-145">stringOutput</span></span> | <span data-ttu-id="e7e86-146">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-146">Array</span></span> | <span data-ttu-id="e7e86-147">[”a”]</span><span class="sxs-lookup"><span data-stu-id="e7e86-147">["a"]</span></span> |
| <span data-ttu-id="e7e86-148">objectOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-148">objectOutput</span></span> | <span data-ttu-id="e7e86-149">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-149">Array</span></span> | <span data-ttu-id="e7e86-150">[{”a”: ”b”, ”c”: ”d”}]</span><span class="sxs-lookup"><span data-stu-id="e7e86-150">[{"a": "b", "c": "d"}]</span></span> |

<a id="coalesce" />

## <a name="coalesce"></a><span data-ttu-id="e7e86-151">Slå samman</span><span class="sxs-lookup"><span data-stu-id="e7e86-151">coalesce</span></span>
`coalesce(arg1, arg2, arg3, ...)`

<span data-ttu-id="e7e86-152">Returnerar första icke-null-värde från parametrarna.</span><span class="sxs-lookup"><span data-stu-id="e7e86-152">Returns first non-null value from the parameters.</span></span> <span data-ttu-id="e7e86-153">Tomma strängar, tomma matriser och tomt-objekt är inte null.</span><span class="sxs-lookup"><span data-stu-id="e7e86-153">Empty strings, empty arrays, and empty objects are not null.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-154">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-154">Parameters</span></span>

| <span data-ttu-id="e7e86-155">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-155">Parameter</span></span> | <span data-ttu-id="e7e86-156">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-156">Required</span></span> | <span data-ttu-id="e7e86-157">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-157">Type</span></span> | <span data-ttu-id="e7e86-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-158">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-159">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-159">arg1</span></span> |<span data-ttu-id="e7e86-160">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-160">Yes</span></span> |<span data-ttu-id="e7e86-161">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-161">int, string, array, or object</span></span> |<span data-ttu-id="e7e86-162">Det första värdet för null.</span><span class="sxs-lookup"><span data-stu-id="e7e86-162">The first value to test for null.</span></span> |
| <span data-ttu-id="e7e86-163">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="e7e86-163">additional args</span></span> |<span data-ttu-id="e7e86-164">Nej</span><span class="sxs-lookup"><span data-stu-id="e7e86-164">No</span></span> |<span data-ttu-id="e7e86-165">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-165">int, string, array, or object</span></span> |<span data-ttu-id="e7e86-166">Ytterligare värden att testa till null.</span><span class="sxs-lookup"><span data-stu-id="e7e86-166">Additional values to test for null.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-167">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-167">Return value</span></span>

<span data-ttu-id="e7e86-168">Värdet för de första icke-null-parametrar, som kan vara en sträng, int, matris eller ett objekt.</span><span class="sxs-lookup"><span data-stu-id="e7e86-168">The value of the first non-null parameters, which can be a string, int, array, or object.</span></span> <span data-ttu-id="e7e86-169">Null om alla parametrar är null.</span><span class="sxs-lookup"><span data-stu-id="e7e86-169">Null if all parameters are null.</span></span> 

### <a name="example"></a><span data-ttu-id="e7e86-170">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-170">Example</span></span>

<span data-ttu-id="e7e86-171">I följande exempel visas utdata från olika användningsområden för coalesce.</span><span class="sxs-lookup"><span data-stu-id="e7e86-171">The following example shows the output from different uses of coalesce.</span></span>

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

<span data-ttu-id="e7e86-172">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-172">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-173">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-173">Name</span></span> | <span data-ttu-id="e7e86-174">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-174">Type</span></span> | <span data-ttu-id="e7e86-175">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-175">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-176">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-176">stringOutput</span></span> | <span data-ttu-id="e7e86-177">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-177">String</span></span> | <span data-ttu-id="e7e86-178">Standard</span><span class="sxs-lookup"><span data-stu-id="e7e86-178">default</span></span> |
| <span data-ttu-id="e7e86-179">intOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-179">intOutput</span></span> | <span data-ttu-id="e7e86-180">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-180">Int</span></span> | <span data-ttu-id="e7e86-181">1</span><span class="sxs-lookup"><span data-stu-id="e7e86-181">1</span></span> |
| <span data-ttu-id="e7e86-182">objectOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-182">objectOutput</span></span> | <span data-ttu-id="e7e86-183">Objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-183">Object</span></span> | <span data-ttu-id="e7e86-184">{”första”: ”default”}</span><span class="sxs-lookup"><span data-stu-id="e7e86-184">{"first": "default"}</span></span> |
| <span data-ttu-id="e7e86-185">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-185">arrayOutput</span></span> | <span data-ttu-id="e7e86-186">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-186">Array</span></span> | <span data-ttu-id="e7e86-187">[1]</span><span class="sxs-lookup"><span data-stu-id="e7e86-187">[1]</span></span> |
| <span data-ttu-id="e7e86-188">emptyOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-188">emptyOutput</span></span> | <span data-ttu-id="e7e86-189">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-189">Bool</span></span> | <span data-ttu-id="e7e86-190">True</span><span class="sxs-lookup"><span data-stu-id="e7e86-190">True</span></span> |

<a id="concat" />

## <a name="concat"></a><span data-ttu-id="e7e86-191">concat</span><span class="sxs-lookup"><span data-stu-id="e7e86-191">concat</span></span>
`concat(arg1, arg2, arg3, ...)`

<span data-ttu-id="e7e86-192">Kombinerar flera matriser och returnerar sammanfogad matris eller kombinerar flera strängvärden och returnerar en sammanfogad sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-192">Combines multiple arrays and returns the concatenated array, or combines multiple string values and returns the concatenated string.</span></span> 

### <a name="parameters"></a><span data-ttu-id="e7e86-193">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-193">Parameters</span></span>

| <span data-ttu-id="e7e86-194">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-194">Parameter</span></span> | <span data-ttu-id="e7e86-195">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-195">Required</span></span> | <span data-ttu-id="e7e86-196">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-196">Type</span></span> | <span data-ttu-id="e7e86-197">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-197">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-198">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-198">arg1</span></span> |<span data-ttu-id="e7e86-199">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-199">Yes</span></span> |<span data-ttu-id="e7e86-200">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-200">array or string</span></span> |<span data-ttu-id="e7e86-201">Den första matrisen eller sträng för sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="e7e86-201">The first array or string for concatenation.</span></span> |
| <span data-ttu-id="e7e86-202">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="e7e86-202">additional arguments</span></span> |<span data-ttu-id="e7e86-203">Nej</span><span class="sxs-lookup"><span data-stu-id="e7e86-203">No</span></span> |<span data-ttu-id="e7e86-204">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-204">array or string</span></span> |<span data-ttu-id="e7e86-205">Ytterligare matriser eller strängar i tur och ordning för sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="e7e86-205">Additional arrays or strings in sequential order for concatenation.</span></span> |

<span data-ttu-id="e7e86-206">Den här funktionen kan ta valfritt antal argument och kan acceptera strängar eller matriser för parametrarna.</span><span class="sxs-lookup"><span data-stu-id="e7e86-206">This function can take any number of arguments, and can accept either strings or arrays for the parameters.</span></span>

### <a name="return-value"></a><span data-ttu-id="e7e86-207">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-207">Return value</span></span>
<span data-ttu-id="e7e86-208">En sträng eller en matris med sammanfogade värdena.</span><span class="sxs-lookup"><span data-stu-id="e7e86-208">A string or array of concatenated values.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-209">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-209">Example</span></span>

<span data-ttu-id="e7e86-210">I följande exempel visas hur du kombinerar två matriser.</span><span class="sxs-lookup"><span data-stu-id="e7e86-210">The following example shows how to combine two arrays.</span></span>

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

<span data-ttu-id="e7e86-211">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-211">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-212">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-212">Name</span></span> | <span data-ttu-id="e7e86-213">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-213">Type</span></span> | <span data-ttu-id="e7e86-214">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-214">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-215">Returnera</span><span class="sxs-lookup"><span data-stu-id="e7e86-215">return</span></span> | <span data-ttu-id="e7e86-216">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-216">Array</span></span> | <span data-ttu-id="e7e86-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="e7e86-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<span data-ttu-id="e7e86-218">I följande exempel visar hur du kombinerar två strängvärden och returnerar en sammanfogad sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-218">The following example shows how to combine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="e7e86-219">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-219">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-220">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-220">Name</span></span> | <span data-ttu-id="e7e86-221">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-221">Type</span></span> | <span data-ttu-id="e7e86-222">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-222">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-223">concatOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-223">concatOutput</span></span> | <span data-ttu-id="e7e86-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-224">String</span></span> | <span data-ttu-id="e7e86-225">prefixet 5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="e7e86-225">prefix-5yj4yjf5mbg72</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="e7e86-226">Innehåller</span><span class="sxs-lookup"><span data-stu-id="e7e86-226">contains</span></span>
`contains(container, itemToFind)`

<span data-ttu-id="e7e86-227">Kontrollerar om en matris som innehåller ett värde, ett objekt som innehåller en nyckel eller en sträng som innehåller understrängen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-227">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-228">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-228">Parameters</span></span>

| <span data-ttu-id="e7e86-229">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-229">Parameter</span></span> | <span data-ttu-id="e7e86-230">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-230">Required</span></span> | <span data-ttu-id="e7e86-231">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-231">Type</span></span> | <span data-ttu-id="e7e86-232">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-232">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-233">Behållaren</span><span class="sxs-lookup"><span data-stu-id="e7e86-233">container</span></span> |<span data-ttu-id="e7e86-234">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-234">Yes</span></span> |<span data-ttu-id="e7e86-235">matris, objekt eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-235">array, object, or string</span></span> |<span data-ttu-id="e7e86-236">Det värde som innehåller ett värde att söka efter.</span><span class="sxs-lookup"><span data-stu-id="e7e86-236">The value that contains the value to find.</span></span> |
| <span data-ttu-id="e7e86-237">itemToFind</span><span class="sxs-lookup"><span data-stu-id="e7e86-237">itemToFind</span></span> |<span data-ttu-id="e7e86-238">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-238">Yes</span></span> |<span data-ttu-id="e7e86-239">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="e7e86-239">string or int</span></span> |<span data-ttu-id="e7e86-240">Värde att söka efter.</span><span class="sxs-lookup"><span data-stu-id="e7e86-240">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-241">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-241">Return value</span></span>

<span data-ttu-id="e7e86-242">**SANT** om objektet är hittas, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="e7e86-242">**True** if the item is found; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-243">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-243">Example</span></span>

<span data-ttu-id="e7e86-244">I följande exempel visas hur du använder innehåller med olika typer:</span><span class="sxs-lookup"><span data-stu-id="e7e86-244">The following example shows how to use contains with different types:</span></span>

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

<span data-ttu-id="e7e86-245">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-245">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-246">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-246">Name</span></span> | <span data-ttu-id="e7e86-247">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-247">Type</span></span> | <span data-ttu-id="e7e86-248">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-249">stringTrue</span><span class="sxs-lookup"><span data-stu-id="e7e86-249">stringTrue</span></span> | <span data-ttu-id="e7e86-250">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-250">Bool</span></span> | <span data-ttu-id="e7e86-251">True</span><span class="sxs-lookup"><span data-stu-id="e7e86-251">True</span></span> |
| <span data-ttu-id="e7e86-252">stringFalse</span><span class="sxs-lookup"><span data-stu-id="e7e86-252">stringFalse</span></span> | <span data-ttu-id="e7e86-253">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-253">Bool</span></span> | <span data-ttu-id="e7e86-254">False</span><span class="sxs-lookup"><span data-stu-id="e7e86-254">False</span></span> |
| <span data-ttu-id="e7e86-255">objectTrue</span><span class="sxs-lookup"><span data-stu-id="e7e86-255">objectTrue</span></span> | <span data-ttu-id="e7e86-256">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-256">Bool</span></span> | <span data-ttu-id="e7e86-257">True</span><span class="sxs-lookup"><span data-stu-id="e7e86-257">True</span></span> |
| <span data-ttu-id="e7e86-258">objectFalse</span><span class="sxs-lookup"><span data-stu-id="e7e86-258">objectFalse</span></span> | <span data-ttu-id="e7e86-259">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-259">Bool</span></span> | <span data-ttu-id="e7e86-260">False</span><span class="sxs-lookup"><span data-stu-id="e7e86-260">False</span></span> |
| <span data-ttu-id="e7e86-261">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="e7e86-261">arrayTrue</span></span> | <span data-ttu-id="e7e86-262">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-262">Bool</span></span> | <span data-ttu-id="e7e86-263">True</span><span class="sxs-lookup"><span data-stu-id="e7e86-263">True</span></span> |
| <span data-ttu-id="e7e86-264">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="e7e86-264">arrayFalse</span></span> | <span data-ttu-id="e7e86-265">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-265">Bool</span></span> | <span data-ttu-id="e7e86-266">False</span><span class="sxs-lookup"><span data-stu-id="e7e86-266">False</span></span> |

<a id="createarray" />

## <a name="createarray"></a><span data-ttu-id="e7e86-267">createarray</span><span class="sxs-lookup"><span data-stu-id="e7e86-267">createarray</span></span>
`createArray (arg1, arg2, arg3, ...)`

<span data-ttu-id="e7e86-268">Skapar en matris av parametrarna.</span><span class="sxs-lookup"><span data-stu-id="e7e86-268">Creates an array from the parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-269">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-269">Parameters</span></span>

| <span data-ttu-id="e7e86-270">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-270">Parameter</span></span> | <span data-ttu-id="e7e86-271">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-271">Required</span></span> | <span data-ttu-id="e7e86-272">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-272">Type</span></span> | <span data-ttu-id="e7e86-273">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-273">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-274">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-274">arg1</span></span> |<span data-ttu-id="e7e86-275">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-275">Yes</span></span> |<span data-ttu-id="e7e86-276">Sträng, heltal, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-276">String, Integer, Array, or Object</span></span> |<span data-ttu-id="e7e86-277">Det första värdet i matrisen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-277">The first value in the array.</span></span> |
| <span data-ttu-id="e7e86-278">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="e7e86-278">additional arguments</span></span> |<span data-ttu-id="e7e86-279">Nej</span><span class="sxs-lookup"><span data-stu-id="e7e86-279">No</span></span> |<span data-ttu-id="e7e86-280">Sträng, heltal, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-280">String, Integer, Array, or Object</span></span> |<span data-ttu-id="e7e86-281">Ytterligare värdena i matrisen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-281">Additional values in the array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-282">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-282">Return value</span></span>

<span data-ttu-id="e7e86-283">En matris.</span><span class="sxs-lookup"><span data-stu-id="e7e86-283">An array.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-284">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-284">Example</span></span>

<span data-ttu-id="e7e86-285">I följande exempel visas hur du använder createArray med olika typer:</span><span class="sxs-lookup"><span data-stu-id="e7e86-285">The following example shows how to use createArray with different types:</span></span>

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

<span data-ttu-id="e7e86-286">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-286">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-287">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-287">Name</span></span> | <span data-ttu-id="e7e86-288">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-288">Type</span></span> | <span data-ttu-id="e7e86-289">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-289">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-290">stringArray</span><span class="sxs-lookup"><span data-stu-id="e7e86-290">stringArray</span></span> | <span data-ttu-id="e7e86-291">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-291">Array</span></span> | <span data-ttu-id="e7e86-292">[”a”, ”b”, ”c”]</span><span class="sxs-lookup"><span data-stu-id="e7e86-292">["a", "b", "c"]</span></span> |
| <span data-ttu-id="e7e86-293">intArray</span><span class="sxs-lookup"><span data-stu-id="e7e86-293">intArray</span></span> | <span data-ttu-id="e7e86-294">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-294">Array</span></span> | <span data-ttu-id="e7e86-295">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="e7e86-295">[1, 2, 3]</span></span> |
| <span data-ttu-id="e7e86-296">objectArray</span><span class="sxs-lookup"><span data-stu-id="e7e86-296">objectArray</span></span> | <span data-ttu-id="e7e86-297">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-297">Array</span></span> | <span data-ttu-id="e7e86-298">[{”1”: ”a”, ”två”: ”b”, ”tre”: ”c”}]</span><span class="sxs-lookup"><span data-stu-id="e7e86-298">[{"one": "a", "two": "b", "three": "c"}]</span></span> |
| <span data-ttu-id="e7e86-299">arrayArray</span><span class="sxs-lookup"><span data-stu-id="e7e86-299">arrayArray</span></span> | <span data-ttu-id="e7e86-300">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-300">Array</span></span> | <span data-ttu-id="e7e86-301">[[”1”, ”två”, ”tre”]]</span><span class="sxs-lookup"><span data-stu-id="e7e86-301">[["one", "two", "three"]]</span></span> |

<a id="empty" />

## <a name="empty"></a><span data-ttu-id="e7e86-302">tom</span><span class="sxs-lookup"><span data-stu-id="e7e86-302">empty</span></span>

`empty(itemToTest)`

<span data-ttu-id="e7e86-303">Anger om en matris, objekt eller sträng är tom.</span><span class="sxs-lookup"><span data-stu-id="e7e86-303">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-304">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-304">Parameters</span></span>

| <span data-ttu-id="e7e86-305">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-305">Parameter</span></span> | <span data-ttu-id="e7e86-306">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-306">Required</span></span> | <span data-ttu-id="e7e86-307">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-307">Type</span></span> | <span data-ttu-id="e7e86-308">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-308">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-309">itemToTest</span><span class="sxs-lookup"><span data-stu-id="e7e86-309">itemToTest</span></span> |<span data-ttu-id="e7e86-310">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-310">Yes</span></span> |<span data-ttu-id="e7e86-311">matris, objekt eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-311">array, object, or string</span></span> |<span data-ttu-id="e7e86-312">Värdet för att kontrollera om den är tom.</span><span class="sxs-lookup"><span data-stu-id="e7e86-312">The value to check if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-313">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-313">Return value</span></span>

<span data-ttu-id="e7e86-314">Returnerar **SANT** om värdet är tomt, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="e7e86-314">Returns **True** if the value is empty; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-315">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-315">Example</span></span>

<span data-ttu-id="e7e86-316">I följande exempel kontrollerar om en matris och objektet sträng är tom.</span><span class="sxs-lookup"><span data-stu-id="e7e86-316">The following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="e7e86-317">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-317">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-318">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-318">Name</span></span> | <span data-ttu-id="e7e86-319">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-319">Type</span></span> | <span data-ttu-id="e7e86-320">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-320">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-321">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="e7e86-321">arrayEmpty</span></span> | <span data-ttu-id="e7e86-322">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-322">Bool</span></span> | <span data-ttu-id="e7e86-323">True</span><span class="sxs-lookup"><span data-stu-id="e7e86-323">True</span></span> |
| <span data-ttu-id="e7e86-324">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="e7e86-324">objectEmpty</span></span> | <span data-ttu-id="e7e86-325">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-325">Bool</span></span> | <span data-ttu-id="e7e86-326">True</span><span class="sxs-lookup"><span data-stu-id="e7e86-326">True</span></span> |
| <span data-ttu-id="e7e86-327">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="e7e86-327">stringEmpty</span></span> | <span data-ttu-id="e7e86-328">bool</span><span class="sxs-lookup"><span data-stu-id="e7e86-328">Bool</span></span> | <span data-ttu-id="e7e86-329">True</span><span class="sxs-lookup"><span data-stu-id="e7e86-329">True</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="e7e86-330">första</span><span class="sxs-lookup"><span data-stu-id="e7e86-330">first</span></span>
`first(arg1)`

<span data-ttu-id="e7e86-331">Returnerar det första elementet i matrisen eller första tecknet i strängen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-331">Returns the first element of the array, or first character of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-332">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-332">Parameters</span></span>

| <span data-ttu-id="e7e86-333">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-333">Parameter</span></span> | <span data-ttu-id="e7e86-334">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-334">Required</span></span> | <span data-ttu-id="e7e86-335">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-335">Type</span></span> | <span data-ttu-id="e7e86-336">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-336">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-337">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-337">arg1</span></span> |<span data-ttu-id="e7e86-338">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-338">Yes</span></span> |<span data-ttu-id="e7e86-339">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-339">array or string</span></span> |<span data-ttu-id="e7e86-340">Värdet för att hämta det första elementet eller tecken.</span><span class="sxs-lookup"><span data-stu-id="e7e86-340">The value to retrieve the first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-341">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-341">Return value</span></span>

<span data-ttu-id="e7e86-342">Typen (sträng, int, matris eller objekt) för det första elementet i en matris eller det första tecknet i en sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-342">The type (string, int, array, or object) of the first element in an array, or the first character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-343">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-343">Example</span></span>

<span data-ttu-id="e7e86-344">I följande exempel visas hur du använder den första funktionen med en matris och sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-344">The following example shows how to use the first function with an array and string.</span></span>

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

<span data-ttu-id="e7e86-345">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-345">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-346">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-346">Name</span></span> | <span data-ttu-id="e7e86-347">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-347">Type</span></span> | <span data-ttu-id="e7e86-348">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-348">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-349">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-349">arrayOutput</span></span> | <span data-ttu-id="e7e86-350">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-350">String</span></span> | <span data-ttu-id="e7e86-351">en</span><span class="sxs-lookup"><span data-stu-id="e7e86-351">one</span></span> |
| <span data-ttu-id="e7e86-352">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-352">stringOutput</span></span> | <span data-ttu-id="e7e86-353">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-353">String</span></span> | <span data-ttu-id="e7e86-354">O</span><span class="sxs-lookup"><span data-stu-id="e7e86-354">O</span></span> |

<a id="intersection" />

## <a name="intersection"></a><span data-ttu-id="e7e86-355">skärningspunkten</span><span class="sxs-lookup"><span data-stu-id="e7e86-355">intersection</span></span>
`intersection(arg1, arg2, arg3, ...)`

<span data-ttu-id="e7e86-356">Returnerar en enda matris eller ett objekt med vanliga element från parametrarna.</span><span class="sxs-lookup"><span data-stu-id="e7e86-356">Returns a single array or object with the common elements from the parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-357">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-357">Parameters</span></span>

| <span data-ttu-id="e7e86-358">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-358">Parameter</span></span> | <span data-ttu-id="e7e86-359">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-359">Required</span></span> | <span data-ttu-id="e7e86-360">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-360">Type</span></span> | <span data-ttu-id="e7e86-361">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-361">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-362">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-362">arg1</span></span> |<span data-ttu-id="e7e86-363">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-363">Yes</span></span> |<span data-ttu-id="e7e86-364">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-364">array or object</span></span> |<span data-ttu-id="e7e86-365">Det första värdet som ska användas för att söka efter vanliga element.</span><span class="sxs-lookup"><span data-stu-id="e7e86-365">The first value to use for finding common elements.</span></span> |
| <span data-ttu-id="e7e86-366">arg2</span><span class="sxs-lookup"><span data-stu-id="e7e86-366">arg2</span></span> |<span data-ttu-id="e7e86-367">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-367">Yes</span></span> |<span data-ttu-id="e7e86-368">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-368">array or object</span></span> |<span data-ttu-id="e7e86-369">Det andra värdet som ska användas för att söka efter vanliga element.</span><span class="sxs-lookup"><span data-stu-id="e7e86-369">The second value to use for finding common elements.</span></span> |
| <span data-ttu-id="e7e86-370">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="e7e86-370">additional arguments</span></span> |<span data-ttu-id="e7e86-371">Nej</span><span class="sxs-lookup"><span data-stu-id="e7e86-371">No</span></span> |<span data-ttu-id="e7e86-372">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-372">array or object</span></span> |<span data-ttu-id="e7e86-373">Ytterligare värden som ska användas för att söka efter vanliga element.</span><span class="sxs-lookup"><span data-stu-id="e7e86-373">Additional values to use for finding common elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-374">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-374">Return value</span></span>

<span data-ttu-id="e7e86-375">En matris eller ett objekt med vanliga element.</span><span class="sxs-lookup"><span data-stu-id="e7e86-375">An array or object with the common elements.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-376">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-376">Example</span></span>

<span data-ttu-id="e7e86-377">I följande exempel visas hur du använder skärningspunkten med matriser och -objekt:</span><span class="sxs-lookup"><span data-stu-id="e7e86-377">The following example shows how to use intersection with arrays and objects:</span></span>

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

<span data-ttu-id="e7e86-378">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-378">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-379">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-379">Name</span></span> | <span data-ttu-id="e7e86-380">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-380">Type</span></span> | <span data-ttu-id="e7e86-381">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-381">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-382">objectOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-382">objectOutput</span></span> | <span data-ttu-id="e7e86-383">Objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-383">Object</span></span> | <span data-ttu-id="e7e86-384">{”1”: ”a”, ”tre”: ”c”}</span><span class="sxs-lookup"><span data-stu-id="e7e86-384">{"one": "a", "three": "c"}</span></span> |
| <span data-ttu-id="e7e86-385">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-385">arrayOutput</span></span> | <span data-ttu-id="e7e86-386">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-386">Array</span></span> | <span data-ttu-id="e7e86-387">[”två”, ”tre”]</span><span class="sxs-lookup"><span data-stu-id="e7e86-387">["two", "three"]</span></span> |


## <a name="json"></a><span data-ttu-id="e7e86-388">JSON</span><span class="sxs-lookup"><span data-stu-id="e7e86-388">json</span></span>
`json(arg1)`

<span data-ttu-id="e7e86-389">Returnerar ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="e7e86-389">Returns a JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-390">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-390">Parameters</span></span>

| <span data-ttu-id="e7e86-391">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-391">Parameter</span></span> | <span data-ttu-id="e7e86-392">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-392">Required</span></span> | <span data-ttu-id="e7e86-393">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-393">Type</span></span> | <span data-ttu-id="e7e86-394">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-394">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-395">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-395">arg1</span></span> |<span data-ttu-id="e7e86-396">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-396">Yes</span></span> |<span data-ttu-id="e7e86-397">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-397">string</span></span> |<span data-ttu-id="e7e86-398">Värdet som ska konverteras till JSON.</span><span class="sxs-lookup"><span data-stu-id="e7e86-398">The value to convert to JSON.</span></span> |


### <a name="return-value"></a><span data-ttu-id="e7e86-399">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-399">Return value</span></span>

<span data-ttu-id="e7e86-400">JSON-objekt från den angivna strängen eller ett tomt-objekt när **null** har angetts.</span><span class="sxs-lookup"><span data-stu-id="e7e86-400">The JSON object from the specified string, or an empty object when **null** is specified.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-401">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-401">Example</span></span>

<span data-ttu-id="e7e86-402">I följande exempel visas hur du använder skärningspunkten med matriser och -objekt:</span><span class="sxs-lookup"><span data-stu-id="e7e86-402">The following example shows how to use intersection with arrays and objects:</span></span>

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

<span data-ttu-id="e7e86-403">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-403">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-404">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-404">Name</span></span> | <span data-ttu-id="e7e86-405">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-405">Type</span></span> | <span data-ttu-id="e7e86-406">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-406">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-407">jsonOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-407">jsonOutput</span></span> | <span data-ttu-id="e7e86-408">Objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-408">Object</span></span> | <span data-ttu-id="e7e86-409">{”a”: ”b”}</span><span class="sxs-lookup"><span data-stu-id="e7e86-409">{"a": "b"}</span></span> |
| <span data-ttu-id="e7e86-410">nullOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-410">nullOutput</span></span> | <span data-ttu-id="e7e86-411">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-411">Boolean</span></span> | <span data-ttu-id="e7e86-412">True</span><span class="sxs-lookup"><span data-stu-id="e7e86-412">True</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="e7e86-413">senaste</span><span class="sxs-lookup"><span data-stu-id="e7e86-413">last</span></span>
`last (arg1)`

<span data-ttu-id="e7e86-414">Returnerar det sista elementet i matrisen eller sista tecknet i strängen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-414">Returns the last element of the array, or last character of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-415">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-415">Parameters</span></span>

| <span data-ttu-id="e7e86-416">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-416">Parameter</span></span> | <span data-ttu-id="e7e86-417">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-417">Required</span></span> | <span data-ttu-id="e7e86-418">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-418">Type</span></span> | <span data-ttu-id="e7e86-419">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-420">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-420">arg1</span></span> |<span data-ttu-id="e7e86-421">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-421">Yes</span></span> |<span data-ttu-id="e7e86-422">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-422">array or string</span></span> |<span data-ttu-id="e7e86-423">Värdet för att hämta det sista elementet eller tecken.</span><span class="sxs-lookup"><span data-stu-id="e7e86-423">The value to retrieve the last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-424">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-424">Return value</span></span>

<span data-ttu-id="e7e86-425">Typen (sträng, int, matris eller objekt) för det sista elementet i en matris eller det sista tecknet i en sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-425">The type (string, int, array, or object) of the last element in an array, or the last character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-426">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-426">Example</span></span>

<span data-ttu-id="e7e86-427">I följande exempel visas hur du använder funktionen senaste med en matris och en sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-427">The following example shows how to use the last function with an array and string.</span></span>

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

<span data-ttu-id="e7e86-428">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-428">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-429">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-429">Name</span></span> | <span data-ttu-id="e7e86-430">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-430">Type</span></span> | <span data-ttu-id="e7e86-431">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-432">arrayOutput</span></span> | <span data-ttu-id="e7e86-433">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-433">String</span></span> | <span data-ttu-id="e7e86-434">tre</span><span class="sxs-lookup"><span data-stu-id="e7e86-434">three</span></span> |
| <span data-ttu-id="e7e86-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-435">stringOutput</span></span> | <span data-ttu-id="e7e86-436">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-436">String</span></span> | <span data-ttu-id="e7e86-437">E</span><span class="sxs-lookup"><span data-stu-id="e7e86-437">e</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="e7e86-438">Längd</span><span class="sxs-lookup"><span data-stu-id="e7e86-438">length</span></span>
`length(arg1)`

<span data-ttu-id="e7e86-439">Returnerar antalet element i en matris eller tecken i en sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-439">Returns the number of elements in an array, or characters in a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-440">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-440">Parameters</span></span>

| <span data-ttu-id="e7e86-441">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-441">Parameter</span></span> | <span data-ttu-id="e7e86-442">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-442">Required</span></span> | <span data-ttu-id="e7e86-443">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-443">Type</span></span> | <span data-ttu-id="e7e86-444">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-444">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-445">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-445">arg1</span></span> |<span data-ttu-id="e7e86-446">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-446">Yes</span></span> |<span data-ttu-id="e7e86-447">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-447">array or string</span></span> |<span data-ttu-id="e7e86-448">Matrisen ska användas för att hämta antalet element eller strängen som ska användas för att hämta antalet tecken.</span><span class="sxs-lookup"><span data-stu-id="e7e86-448">The array to use for getting the number of elements, or the string to use for getting the number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-449">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-449">Return value</span></span>

<span data-ttu-id="e7e86-450">Int.</span><span class="sxs-lookup"><span data-stu-id="e7e86-450">An int.</span></span> 

### <a name="example"></a><span data-ttu-id="e7e86-451">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-451">Example</span></span>

<span data-ttu-id="e7e86-452">I följande exempel visas hur du använder längd med en matris och sträng:</span><span class="sxs-lookup"><span data-stu-id="e7e86-452">The following example shows how to use length with an array and string:</span></span>

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

<span data-ttu-id="e7e86-453">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-453">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-454">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-454">Name</span></span> | <span data-ttu-id="e7e86-455">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-455">Type</span></span> | <span data-ttu-id="e7e86-456">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-456">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-457">arrayLength</span><span class="sxs-lookup"><span data-stu-id="e7e86-457">arrayLength</span></span> | <span data-ttu-id="e7e86-458">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-458">Int</span></span> | <span data-ttu-id="e7e86-459">3</span><span class="sxs-lookup"><span data-stu-id="e7e86-459">3</span></span> |
| <span data-ttu-id="e7e86-460">stringLength</span><span class="sxs-lookup"><span data-stu-id="e7e86-460">stringLength</span></span> | <span data-ttu-id="e7e86-461">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-461">Int</span></span> | <span data-ttu-id="e7e86-462">13</span><span class="sxs-lookup"><span data-stu-id="e7e86-462">13</span></span> |

<span data-ttu-id="e7e86-463">Du kan ange antal upprepningar när du skapar resurser genom att använda den här funktionen med en matris.</span><span class="sxs-lookup"><span data-stu-id="e7e86-463">You can use this function with an array to specify the number of iterations when creating resources.</span></span> <span data-ttu-id="e7e86-464">I följande exempel parametern **siteNames** referera till en matris med namn som ska använda när du skapar webbplatser.</span><span class="sxs-lookup"><span data-stu-id="e7e86-464">In the following example, the parameter **siteNames** would refer to an array of names to use when creating the web sites.</span></span>

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

<span data-ttu-id="e7e86-465">Mer information om hur du använder den här funktionen med en matris finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="e7e86-465">For more information about using this function with an array, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<a id="min" />

## <a name="min"></a><span data-ttu-id="e7e86-466">min.</span><span class="sxs-lookup"><span data-stu-id="e7e86-466">min</span></span>
`min(arg1)`

<span data-ttu-id="e7e86-467">Returnerar det lägsta värdet från en heltalsmatris eller en kommaavgränsad lista med heltal.</span><span class="sxs-lookup"><span data-stu-id="e7e86-467">Returns the minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-468">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-468">Parameters</span></span>

| <span data-ttu-id="e7e86-469">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-469">Parameter</span></span> | <span data-ttu-id="e7e86-470">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-470">Required</span></span> | <span data-ttu-id="e7e86-471">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-471">Type</span></span> | <span data-ttu-id="e7e86-472">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-472">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-473">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-473">arg1</span></span> |<span data-ttu-id="e7e86-474">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-474">Yes</span></span> |<span data-ttu-id="e7e86-475">matris med heltal eller en kommaavgränsad lista med heltal</span><span class="sxs-lookup"><span data-stu-id="e7e86-475">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="e7e86-476">Samlingen för att hämta det minsta värdet.</span><span class="sxs-lookup"><span data-stu-id="e7e86-476">The collection to get the minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-477">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-477">Return value</span></span>

<span data-ttu-id="e7e86-478">Ett heltal som representerar det minsta värdet.</span><span class="sxs-lookup"><span data-stu-id="e7e86-478">An int representing the minimum value.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-479">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-479">Example</span></span>

<span data-ttu-id="e7e86-480">I följande exempel visas hur du använder min med en matris och en lista med heltal:</span><span class="sxs-lookup"><span data-stu-id="e7e86-480">The following example shows how to use min with an array and a list of integers:</span></span>

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

<span data-ttu-id="e7e86-481">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-481">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-482">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-482">Name</span></span> | <span data-ttu-id="e7e86-483">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-483">Type</span></span> | <span data-ttu-id="e7e86-484">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-484">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-485">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-485">arrayOutput</span></span> | <span data-ttu-id="e7e86-486">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-486">Int</span></span> | <span data-ttu-id="e7e86-487">0</span><span class="sxs-lookup"><span data-stu-id="e7e86-487">0</span></span> |
| <span data-ttu-id="e7e86-488">intOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-488">intOutput</span></span> | <span data-ttu-id="e7e86-489">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-489">Int</span></span> | <span data-ttu-id="e7e86-490">0</span><span class="sxs-lookup"><span data-stu-id="e7e86-490">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="e7e86-491">Max</span><span class="sxs-lookup"><span data-stu-id="e7e86-491">max</span></span>
`max(arg1)`

<span data-ttu-id="e7e86-492">Returnerar det största värdet från en matris av heltal eller en kommaavgränsad lista med heltal.</span><span class="sxs-lookup"><span data-stu-id="e7e86-492">Returns the maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-493">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-493">Parameters</span></span>

| <span data-ttu-id="e7e86-494">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-494">Parameter</span></span> | <span data-ttu-id="e7e86-495">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-495">Required</span></span> | <span data-ttu-id="e7e86-496">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-496">Type</span></span> | <span data-ttu-id="e7e86-497">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-497">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-498">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-498">arg1</span></span> |<span data-ttu-id="e7e86-499">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-499">Yes</span></span> |<span data-ttu-id="e7e86-500">matris med heltal eller en kommaavgränsad lista med heltal</span><span class="sxs-lookup"><span data-stu-id="e7e86-500">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="e7e86-501">Samlingen för att hämta det högsta värdet.</span><span class="sxs-lookup"><span data-stu-id="e7e86-501">The collection to get the maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-502">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-502">Return value</span></span>

<span data-ttu-id="e7e86-503">Ett heltal som representerar det högsta värdet.</span><span class="sxs-lookup"><span data-stu-id="e7e86-503">An int representing the maximum value.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-504">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-504">Example</span></span>

<span data-ttu-id="e7e86-505">I följande exempel visas hur du använder max med en matris och en lista med heltal:</span><span class="sxs-lookup"><span data-stu-id="e7e86-505">The following example shows how to use max with an array and a list of integers:</span></span>

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

<span data-ttu-id="e7e86-506">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-506">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-507">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-507">Name</span></span> | <span data-ttu-id="e7e86-508">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-508">Type</span></span> | <span data-ttu-id="e7e86-509">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-509">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-510">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-510">arrayOutput</span></span> | <span data-ttu-id="e7e86-511">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-511">Int</span></span> | <span data-ttu-id="e7e86-512">5</span><span class="sxs-lookup"><span data-stu-id="e7e86-512">5</span></span> |
| <span data-ttu-id="e7e86-513">intOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-513">intOutput</span></span> | <span data-ttu-id="e7e86-514">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-514">Int</span></span> | <span data-ttu-id="e7e86-515">5</span><span class="sxs-lookup"><span data-stu-id="e7e86-515">5</span></span> |

<a id="range" />

## <a name="range"></a><span data-ttu-id="e7e86-516">intervallet</span><span class="sxs-lookup"><span data-stu-id="e7e86-516">range</span></span>
`range(startingInteger, numberOfElements)`

<span data-ttu-id="e7e86-517">Skapar en heltalsmatris från början heltal och som innehåller ett antal objekt.</span><span class="sxs-lookup"><span data-stu-id="e7e86-517">Creates an array of integers from a starting integer and containing a number of items.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-518">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-518">Parameters</span></span>

| <span data-ttu-id="e7e86-519">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-519">Parameter</span></span> | <span data-ttu-id="e7e86-520">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-520">Required</span></span> | <span data-ttu-id="e7e86-521">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-521">Type</span></span> | <span data-ttu-id="e7e86-522">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-522">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-523">startingInteger</span><span class="sxs-lookup"><span data-stu-id="e7e86-523">startingInteger</span></span> |<span data-ttu-id="e7e86-524">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-524">Yes</span></span> |<span data-ttu-id="e7e86-525">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-525">int</span></span> |<span data-ttu-id="e7e86-526">Det första heltal i matrisen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-526">The first integer in the array.</span></span> |
| <span data-ttu-id="e7e86-527">numberofElements</span><span class="sxs-lookup"><span data-stu-id="e7e86-527">numberofElements</span></span> |<span data-ttu-id="e7e86-528">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-528">Yes</span></span> |<span data-ttu-id="e7e86-529">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-529">int</span></span> |<span data-ttu-id="e7e86-530">Antal heltal i matrisen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-530">The number of integers in the array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-531">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-531">Return value</span></span>

<span data-ttu-id="e7e86-532">En heltalsmatris.</span><span class="sxs-lookup"><span data-stu-id="e7e86-532">An array of integers.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-533">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-533">Example</span></span>

<span data-ttu-id="e7e86-534">I följande exempel visas hur du använder funktionen intervall:</span><span class="sxs-lookup"><span data-stu-id="e7e86-534">The following example shows how to use the range function:</span></span>

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

<span data-ttu-id="e7e86-535">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-535">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-536">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-536">Name</span></span> | <span data-ttu-id="e7e86-537">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-537">Type</span></span> | <span data-ttu-id="e7e86-538">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-538">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-539">rangeOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-539">rangeOutput</span></span> | <span data-ttu-id="e7e86-540">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-540">Array</span></span> | <span data-ttu-id="e7e86-541">[5, 6, 7]</span><span class="sxs-lookup"><span data-stu-id="e7e86-541">[5, 6, 7]</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="e7e86-542">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="e7e86-542">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="e7e86-543">Returnerar en matris med alla element efter det angivna värdet i matrisen eller returnerar en sträng med alla tecken efter det angivna värdet i strängen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-543">Returns an array with all the elements after the specified number in the array, or returns a string with all the characters after the specified number in the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-544">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-544">Parameters</span></span>

| <span data-ttu-id="e7e86-545">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-545">Parameter</span></span> | <span data-ttu-id="e7e86-546">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-546">Required</span></span> | <span data-ttu-id="e7e86-547">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-547">Type</span></span> | <span data-ttu-id="e7e86-548">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-548">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-549">Ursprungligt värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-549">originalValue</span></span> |<span data-ttu-id="e7e86-550">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-550">Yes</span></span> |<span data-ttu-id="e7e86-551">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-551">array or string</span></span> |<span data-ttu-id="e7e86-552">Matris eller sträng som ska användas för att hoppa över.</span><span class="sxs-lookup"><span data-stu-id="e7e86-552">The array or string to use for skipping.</span></span> |
| <span data-ttu-id="e7e86-553">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="e7e86-553">numberToSkip</span></span> |<span data-ttu-id="e7e86-554">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-554">Yes</span></span> |<span data-ttu-id="e7e86-555">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-555">int</span></span> |<span data-ttu-id="e7e86-556">Antalet element eller tecken som ska hoppas över.</span><span class="sxs-lookup"><span data-stu-id="e7e86-556">The number of elements or characters to skip.</span></span> <span data-ttu-id="e7e86-557">Om det här värdet är 0 eller mindre, returneras alla element eller tecken i-värdet.</span><span class="sxs-lookup"><span data-stu-id="e7e86-557">If this value is 0 or less, all the elements or characters in the value are returned.</span></span> <span data-ttu-id="e7e86-558">Om den är större än längden på matrisen eller sträng returneras en tom matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-558">If it is larger than the length of the array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-559">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-559">Return value</span></span>

<span data-ttu-id="e7e86-560">En matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-560">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-561">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-561">Example</span></span>

<span data-ttu-id="e7e86-562">I följande exempel hoppar över det angivna antalet element i matrisen och det angivna antalet tecken i en sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-562">The following example skips the specified number of elements in the array, and the specified number of characters in a string.</span></span>

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

<span data-ttu-id="e7e86-563">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-563">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-564">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-564">Name</span></span> | <span data-ttu-id="e7e86-565">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-565">Type</span></span> | <span data-ttu-id="e7e86-566">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-566">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-567">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-567">arrayOutput</span></span> | <span data-ttu-id="e7e86-568">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-568">Array</span></span> | <span data-ttu-id="e7e86-569">[”tre”]</span><span class="sxs-lookup"><span data-stu-id="e7e86-569">["three"]</span></span> |
| <span data-ttu-id="e7e86-570">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-570">stringOutput</span></span> | <span data-ttu-id="e7e86-571">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-571">String</span></span> | <span data-ttu-id="e7e86-572">två tre</span><span class="sxs-lookup"><span data-stu-id="e7e86-572">two three</span></span> |

<a id="take" />

## <a name="take"></a><span data-ttu-id="e7e86-573">ta</span><span class="sxs-lookup"><span data-stu-id="e7e86-573">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="e7e86-574">Returnerar en matris med det angivna antalet element från början av matrisen eller en sträng med det angivna antalet tecken från början av strängen.</span><span class="sxs-lookup"><span data-stu-id="e7e86-574">Returns an array with the specified number of elements from the start of the array, or a string with the specified number of characters from the start of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-575">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-575">Parameters</span></span>

| <span data-ttu-id="e7e86-576">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-576">Parameter</span></span> | <span data-ttu-id="e7e86-577">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-577">Required</span></span> | <span data-ttu-id="e7e86-578">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-578">Type</span></span> | <span data-ttu-id="e7e86-579">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-579">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-580">Ursprungligt värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-580">originalValue</span></span> |<span data-ttu-id="e7e86-581">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-581">Yes</span></span> |<span data-ttu-id="e7e86-582">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-582">array or string</span></span> |<span data-ttu-id="e7e86-583">Matris eller sträng för att ta elementen från.</span><span class="sxs-lookup"><span data-stu-id="e7e86-583">The array or string to take the elements from.</span></span> |
| <span data-ttu-id="e7e86-584">numberToTake</span><span class="sxs-lookup"><span data-stu-id="e7e86-584">numberToTake</span></span> |<span data-ttu-id="e7e86-585">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-585">Yes</span></span> |<span data-ttu-id="e7e86-586">int</span><span class="sxs-lookup"><span data-stu-id="e7e86-586">int</span></span> |<span data-ttu-id="e7e86-587">Antalet element eller tecken som ska ta.</span><span class="sxs-lookup"><span data-stu-id="e7e86-587">The number of elements or characters to take.</span></span> <span data-ttu-id="e7e86-588">Om det här värdet är 0 eller mindre, returneras en tom matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-588">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="e7e86-589">Om den är större än längden på den angivna matrisen eller sträng returneras alla element i en matris eller en sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-589">If it is larger than the length of the given array or string, all the elements in the array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-590">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-590">Return value</span></span>

<span data-ttu-id="e7e86-591">En matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-591">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-592">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-592">Example</span></span>

<span data-ttu-id="e7e86-593">I följande exempel tar det angivna antalet element från matrisen och tecken från en sträng.</span><span class="sxs-lookup"><span data-stu-id="e7e86-593">The following example takes the specified number of elements from the array, and characters from a string.</span></span>

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

<span data-ttu-id="e7e86-594">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-594">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-595">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-595">Name</span></span> | <span data-ttu-id="e7e86-596">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-596">Type</span></span> | <span data-ttu-id="e7e86-597">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-597">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-598">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-598">arrayOutput</span></span> | <span data-ttu-id="e7e86-599">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-599">Array</span></span> | <span data-ttu-id="e7e86-600">[””, ”två”]</span><span class="sxs-lookup"><span data-stu-id="e7e86-600">["one", "two"]</span></span> |
| <span data-ttu-id="e7e86-601">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-601">stringOutput</span></span> | <span data-ttu-id="e7e86-602">Sträng</span><span class="sxs-lookup"><span data-stu-id="e7e86-602">String</span></span> | <span data-ttu-id="e7e86-603">på</span><span class="sxs-lookup"><span data-stu-id="e7e86-603">on</span></span> |

<a id="union" />

## <a name="union"></a><span data-ttu-id="e7e86-604">Union</span><span class="sxs-lookup"><span data-stu-id="e7e86-604">union</span></span>
`union(arg1, arg2, arg3, ...)`

<span data-ttu-id="e7e86-605">Returnerar en enda matris eller ett objekt med alla element från parametrarna.</span><span class="sxs-lookup"><span data-stu-id="e7e86-605">Returns a single array or object with all elements from the parameters.</span></span> <span data-ttu-id="e7e86-606">Duplicerade värden eller nycklar är bara ingår en gång.</span><span class="sxs-lookup"><span data-stu-id="e7e86-606">Duplicate values or keys are only included once.</span></span>

### <a name="parameters"></a><span data-ttu-id="e7e86-607">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e7e86-607">Parameters</span></span>

| <span data-ttu-id="e7e86-608">Parameter</span><span class="sxs-lookup"><span data-stu-id="e7e86-608">Parameter</span></span> | <span data-ttu-id="e7e86-609">Krävs</span><span class="sxs-lookup"><span data-stu-id="e7e86-609">Required</span></span> | <span data-ttu-id="e7e86-610">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-610">Type</span></span> | <span data-ttu-id="e7e86-611">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7e86-611">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e7e86-612">arg1</span><span class="sxs-lookup"><span data-stu-id="e7e86-612">arg1</span></span> |<span data-ttu-id="e7e86-613">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-613">Yes</span></span> |<span data-ttu-id="e7e86-614">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-614">array or object</span></span> |<span data-ttu-id="e7e86-615">Det första värdet ska användas för anslutning element.</span><span class="sxs-lookup"><span data-stu-id="e7e86-615">The first value to use for joining elements.</span></span> |
| <span data-ttu-id="e7e86-616">arg2</span><span class="sxs-lookup"><span data-stu-id="e7e86-616">arg2</span></span> |<span data-ttu-id="e7e86-617">Ja</span><span class="sxs-lookup"><span data-stu-id="e7e86-617">Yes</span></span> |<span data-ttu-id="e7e86-618">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-618">array or object</span></span> |<span data-ttu-id="e7e86-619">Det andra värdet som ska användas för anslutning element.</span><span class="sxs-lookup"><span data-stu-id="e7e86-619">The second value to use for joining elements.</span></span> |
| <span data-ttu-id="e7e86-620">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="e7e86-620">additional arguments</span></span> |<span data-ttu-id="e7e86-621">Nej</span><span class="sxs-lookup"><span data-stu-id="e7e86-621">No</span></span> |<span data-ttu-id="e7e86-622">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-622">array or object</span></span> |<span data-ttu-id="e7e86-623">Ytterligare värden som ska användas för att ansluta till element.</span><span class="sxs-lookup"><span data-stu-id="e7e86-623">Additional values to use for joining elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e7e86-624">Returvärde</span><span class="sxs-lookup"><span data-stu-id="e7e86-624">Return value</span></span>

<span data-ttu-id="e7e86-625">En matris eller ett objekt.</span><span class="sxs-lookup"><span data-stu-id="e7e86-625">An array or object.</span></span>

### <a name="example"></a><span data-ttu-id="e7e86-626">Exempel</span><span class="sxs-lookup"><span data-stu-id="e7e86-626">Example</span></span>

<span data-ttu-id="e7e86-627">I följande exempel visas hur du använder union med matriser och -objekt:</span><span class="sxs-lookup"><span data-stu-id="e7e86-627">The following example shows how to use union with arrays and objects:</span></span>

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

<span data-ttu-id="e7e86-628">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="e7e86-628">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e7e86-629">Namn</span><span class="sxs-lookup"><span data-stu-id="e7e86-629">Name</span></span> | <span data-ttu-id="e7e86-630">Typ</span><span class="sxs-lookup"><span data-stu-id="e7e86-630">Type</span></span> | <span data-ttu-id="e7e86-631">Värde</span><span class="sxs-lookup"><span data-stu-id="e7e86-631">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e7e86-632">objectOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-632">objectOutput</span></span> | <span data-ttu-id="e7e86-633">Objekt</span><span class="sxs-lookup"><span data-stu-id="e7e86-633">Object</span></span> | <span data-ttu-id="e7e86-634">{”1”: ”a”, ”två”: ”b”, ”tre”: ”c”, ”fyra”: ”d”, ”fem”: ”e”}</span><span class="sxs-lookup"><span data-stu-id="e7e86-634">{"one": "a", "two": "b", "three": "c", "four": "d", "five": "e"}</span></span> |
| <span data-ttu-id="e7e86-635">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e7e86-635">arrayOutput</span></span> | <span data-ttu-id="e7e86-636">matris</span><span class="sxs-lookup"><span data-stu-id="e7e86-636">Array</span></span> | <span data-ttu-id="e7e86-637">[”1”, ”två”, ”tre”, ”fyra”]</span><span class="sxs-lookup"><span data-stu-id="e7e86-637">["one", "two", "three", "four"]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e7e86-638">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7e86-638">Next steps</span></span>
* <span data-ttu-id="e7e86-639">En beskrivning av avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e7e86-639">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="e7e86-640">Om du vill slå samman flera mallar, se [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e7e86-640">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="e7e86-641">Iterera ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="e7e86-641">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="e7e86-642">Information om hur du distribuerar mallen som du har skapat finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="e7e86-642">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

