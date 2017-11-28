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
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="85f41-103">Array- och funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="85f41-103">Array and object functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="85f41-104">Resource Manager innehåller flera funktioner för att arbeta med matriser och -objekt.</span><span class="sxs-lookup"><span data-stu-id="85f41-104">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="85f41-105">matris</span><span class="sxs-lookup"><span data-stu-id="85f41-105">array</span></span>](#array)
* [<span data-ttu-id="85f41-106">Slå samman</span><span class="sxs-lookup"><span data-stu-id="85f41-106">coalesce</span></span>](#coalesce)
* [<span data-ttu-id="85f41-107">concat</span><span class="sxs-lookup"><span data-stu-id="85f41-107">concat</span></span>](#concat)
* [<span data-ttu-id="85f41-108">innehåller</span><span class="sxs-lookup"><span data-stu-id="85f41-108">contains</span></span>](#contains)
* [<span data-ttu-id="85f41-109">createArray</span><span class="sxs-lookup"><span data-stu-id="85f41-109">createArray</span></span>](#createarray)
* [<span data-ttu-id="85f41-110">tom</span><span class="sxs-lookup"><span data-stu-id="85f41-110">empty</span></span>](#empty)
* [<span data-ttu-id="85f41-111">första</span><span class="sxs-lookup"><span data-stu-id="85f41-111">first</span></span>](#first)
* [<span data-ttu-id="85f41-112">skärningspunkten</span><span class="sxs-lookup"><span data-stu-id="85f41-112">intersection</span></span>](#intersection)
* [<span data-ttu-id="85f41-113">JSON</span><span class="sxs-lookup"><span data-stu-id="85f41-113">json</span></span>](#json)
* [<span data-ttu-id="85f41-114">senaste</span><span class="sxs-lookup"><span data-stu-id="85f41-114">last</span></span>](#last)
* [<span data-ttu-id="85f41-115">längd</span><span class="sxs-lookup"><span data-stu-id="85f41-115">length</span></span>](#length)
* [<span data-ttu-id="85f41-116">Min</span><span class="sxs-lookup"><span data-stu-id="85f41-116">min</span></span>](#min)
* [<span data-ttu-id="85f41-117">Max</span><span class="sxs-lookup"><span data-stu-id="85f41-117">max</span></span>](#max)
* [<span data-ttu-id="85f41-118">intervallet</span><span class="sxs-lookup"><span data-stu-id="85f41-118">range</span></span>](#range)
* [<span data-ttu-id="85f41-119">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="85f41-119">skip</span></span>](#skip)
* [<span data-ttu-id="85f41-120">ta</span><span class="sxs-lookup"><span data-stu-id="85f41-120">take</span></span>](#take)
* [<span data-ttu-id="85f41-121">Union</span><span class="sxs-lookup"><span data-stu-id="85f41-121">union</span></span>](#union)

<span data-ttu-id="85f41-122">tooget en matris med strängvärden som avgränsas med ett värde, se [dela](resource-group-template-functions-string.md#split).</span><span class="sxs-lookup"><span data-stu-id="85f41-122">tooget an array of string values delimited by a value, see [split](resource-group-template-functions-string.md#split).</span></span>

<a id="array" />

## <a name="array"></a><span data-ttu-id="85f41-123">matris</span><span class="sxs-lookup"><span data-stu-id="85f41-123">array</span></span>
`array(convertToArray)`

<span data-ttu-id="85f41-124">Konverterar hello tooan värdematrisen.</span><span class="sxs-lookup"><span data-stu-id="85f41-124">Converts hello value tooan array.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-125">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-125">Parameters</span></span>

| <span data-ttu-id="85f41-126">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-126">Parameter</span></span> | <span data-ttu-id="85f41-127">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-127">Required</span></span> | <span data-ttu-id="85f41-128">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-128">Type</span></span> | <span data-ttu-id="85f41-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-129">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-130">convertToArray</span><span class="sxs-lookup"><span data-stu-id="85f41-130">convertToArray</span></span> |<span data-ttu-id="85f41-131">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-131">Yes</span></span> |<span data-ttu-id="85f41-132">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-132">int, string, array, or object</span></span> |<span data-ttu-id="85f41-133">Hej tooconvert tooan värdematrisen.</span><span class="sxs-lookup"><span data-stu-id="85f41-133">hello value tooconvert tooan array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-134">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-134">Return value</span></span>

<span data-ttu-id="85f41-135">En matris.</span><span class="sxs-lookup"><span data-stu-id="85f41-135">An array.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-136">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-136">Example</span></span>

<span data-ttu-id="85f41-137">hello följande exempel visas hur toouse hello matris-funktionen med olika typer.</span><span class="sxs-lookup"><span data-stu-id="85f41-137">hello following example shows how toouse hello array function with different types.</span></span>

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

<span data-ttu-id="85f41-138">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-138">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-139">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-139">Name</span></span> | <span data-ttu-id="85f41-140">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-140">Type</span></span> | <span data-ttu-id="85f41-141">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-141">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-142">intOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-142">intOutput</span></span> | <span data-ttu-id="85f41-143">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-143">Array</span></span> | <span data-ttu-id="85f41-144">[1]</span><span class="sxs-lookup"><span data-stu-id="85f41-144">[1]</span></span> |
| <span data-ttu-id="85f41-145">stringOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-145">stringOutput</span></span> | <span data-ttu-id="85f41-146">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-146">Array</span></span> | <span data-ttu-id="85f41-147">[”a”]</span><span class="sxs-lookup"><span data-stu-id="85f41-147">["a"]</span></span> |
| <span data-ttu-id="85f41-148">objectOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-148">objectOutput</span></span> | <span data-ttu-id="85f41-149">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-149">Array</span></span> | <span data-ttu-id="85f41-150">[{”a”: ”b”, ”c”: ”d”}]</span><span class="sxs-lookup"><span data-stu-id="85f41-150">[{"a": "b", "c": "d"}]</span></span> |

<a id="coalesce" />

## <a name="coalesce"></a><span data-ttu-id="85f41-151">Slå samman</span><span class="sxs-lookup"><span data-stu-id="85f41-151">coalesce</span></span>
`coalesce(arg1, arg2, arg3, ...)`

<span data-ttu-id="85f41-152">Returnerar första icke-null-värde från hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="85f41-152">Returns first non-null value from hello parameters.</span></span> <span data-ttu-id="85f41-153">Tomma strängar, tomma matriser och tomt-objekt är inte null.</span><span class="sxs-lookup"><span data-stu-id="85f41-153">Empty strings, empty arrays, and empty objects are not null.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-154">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-154">Parameters</span></span>

| <span data-ttu-id="85f41-155">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-155">Parameter</span></span> | <span data-ttu-id="85f41-156">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-156">Required</span></span> | <span data-ttu-id="85f41-157">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-157">Type</span></span> | <span data-ttu-id="85f41-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-158">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-159">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-159">arg1</span></span> |<span data-ttu-id="85f41-160">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-160">Yes</span></span> |<span data-ttu-id="85f41-161">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-161">int, string, array, or object</span></span> |<span data-ttu-id="85f41-162">hello första värde tootest för null.</span><span class="sxs-lookup"><span data-stu-id="85f41-162">hello first value tootest for null.</span></span> |
| <span data-ttu-id="85f41-163">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="85f41-163">additional args</span></span> |<span data-ttu-id="85f41-164">Nej</span><span class="sxs-lookup"><span data-stu-id="85f41-164">No</span></span> |<span data-ttu-id="85f41-165">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-165">int, string, array, or object</span></span> |<span data-ttu-id="85f41-166">Ytterligare värden tootest för null.</span><span class="sxs-lookup"><span data-stu-id="85f41-166">Additional values tootest for null.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-167">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-167">Return value</span></span>

<span data-ttu-id="85f41-168">hello värde av hello första icke-null-parametrar, vilket kan vara en sträng, int, matris eller ett objekt.</span><span class="sxs-lookup"><span data-stu-id="85f41-168">hello value of hello first non-null parameters, which can be a string, int, array, or object.</span></span> <span data-ttu-id="85f41-169">Null om alla parametrar är null.</span><span class="sxs-lookup"><span data-stu-id="85f41-169">Null if all parameters are null.</span></span> 

### <a name="example"></a><span data-ttu-id="85f41-170">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-170">Example</span></span>

<span data-ttu-id="85f41-171">hello visar följande exempel hello utdata från olika användningsområden för coalesce.</span><span class="sxs-lookup"><span data-stu-id="85f41-171">hello following example shows hello output from different uses of coalesce.</span></span>

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

<span data-ttu-id="85f41-172">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-172">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-173">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-173">Name</span></span> | <span data-ttu-id="85f41-174">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-174">Type</span></span> | <span data-ttu-id="85f41-175">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-175">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-176">stringOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-176">stringOutput</span></span> | <span data-ttu-id="85f41-177">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-177">String</span></span> | <span data-ttu-id="85f41-178">Standard</span><span class="sxs-lookup"><span data-stu-id="85f41-178">default</span></span> |
| <span data-ttu-id="85f41-179">intOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-179">intOutput</span></span> | <span data-ttu-id="85f41-180">int</span><span class="sxs-lookup"><span data-stu-id="85f41-180">Int</span></span> | <span data-ttu-id="85f41-181">1</span><span class="sxs-lookup"><span data-stu-id="85f41-181">1</span></span> |
| <span data-ttu-id="85f41-182">objectOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-182">objectOutput</span></span> | <span data-ttu-id="85f41-183">Objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-183">Object</span></span> | <span data-ttu-id="85f41-184">{”första”: ”default”}</span><span class="sxs-lookup"><span data-stu-id="85f41-184">{"first": "default"}</span></span> |
| <span data-ttu-id="85f41-185">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-185">arrayOutput</span></span> | <span data-ttu-id="85f41-186">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-186">Array</span></span> | <span data-ttu-id="85f41-187">[1]</span><span class="sxs-lookup"><span data-stu-id="85f41-187">[1]</span></span> |
| <span data-ttu-id="85f41-188">emptyOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-188">emptyOutput</span></span> | <span data-ttu-id="85f41-189">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-189">Bool</span></span> | <span data-ttu-id="85f41-190">True</span><span class="sxs-lookup"><span data-stu-id="85f41-190">True</span></span> |

<a id="concat" />

## <a name="concat"></a><span data-ttu-id="85f41-191">concat</span><span class="sxs-lookup"><span data-stu-id="85f41-191">concat</span></span>
`concat(arg1, arg2, arg3, ...)`

<span data-ttu-id="85f41-192">Kombinerar flera matriser och returnerar hello sammanfogas matrisen eller kombinerar flera strängvärden och returnerar hello sammanfogas sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-192">Combines multiple arrays and returns hello concatenated array, or combines multiple string values and returns hello concatenated string.</span></span> 

### <a name="parameters"></a><span data-ttu-id="85f41-193">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-193">Parameters</span></span>

| <span data-ttu-id="85f41-194">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-194">Parameter</span></span> | <span data-ttu-id="85f41-195">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-195">Required</span></span> | <span data-ttu-id="85f41-196">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-196">Type</span></span> | <span data-ttu-id="85f41-197">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-197">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-198">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-198">arg1</span></span> |<span data-ttu-id="85f41-199">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-199">Yes</span></span> |<span data-ttu-id="85f41-200">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-200">array or string</span></span> |<span data-ttu-id="85f41-201">Hej första matrisen eller sträng för sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="85f41-201">hello first array or string for concatenation.</span></span> |
| <span data-ttu-id="85f41-202">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="85f41-202">additional arguments</span></span> |<span data-ttu-id="85f41-203">Nej</span><span class="sxs-lookup"><span data-stu-id="85f41-203">No</span></span> |<span data-ttu-id="85f41-204">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-204">array or string</span></span> |<span data-ttu-id="85f41-205">Ytterligare matriser eller strängar i tur och ordning för sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="85f41-205">Additional arrays or strings in sequential order for concatenation.</span></span> |

<span data-ttu-id="85f41-206">Den här funktionen kan ta valfritt antal argument och kan acceptera strängar eller matriser för hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="85f41-206">This function can take any number of arguments, and can accept either strings or arrays for hello parameters.</span></span>

### <a name="return-value"></a><span data-ttu-id="85f41-207">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-207">Return value</span></span>
<span data-ttu-id="85f41-208">En sträng eller en matris med sammanfogade värdena.</span><span class="sxs-lookup"><span data-stu-id="85f41-208">A string or array of concatenated values.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-209">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-209">Example</span></span>

<span data-ttu-id="85f41-210">hello som följande exempel visar hur toocombine två matriser.</span><span class="sxs-lookup"><span data-stu-id="85f41-210">hello following example shows how toocombine two arrays.</span></span>

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

<span data-ttu-id="85f41-211">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-211">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-212">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-212">Name</span></span> | <span data-ttu-id="85f41-213">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-213">Type</span></span> | <span data-ttu-id="85f41-214">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-214">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-215">Returnera</span><span class="sxs-lookup"><span data-stu-id="85f41-215">return</span></span> | <span data-ttu-id="85f41-216">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-216">Array</span></span> | <span data-ttu-id="85f41-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="85f41-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<span data-ttu-id="85f41-218">hello som följande exempel visar hur toocombine två sträng värden och returnerar en sammanfogad sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-218">hello following example shows how toocombine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="85f41-219">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-219">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-220">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-220">Name</span></span> | <span data-ttu-id="85f41-221">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-221">Type</span></span> | <span data-ttu-id="85f41-222">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-222">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-223">concatOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-223">concatOutput</span></span> | <span data-ttu-id="85f41-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-224">String</span></span> | <span data-ttu-id="85f41-225">prefixet 5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="85f41-225">prefix-5yj4yjf5mbg72</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="85f41-226">Innehåller</span><span class="sxs-lookup"><span data-stu-id="85f41-226">contains</span></span>
`contains(container, itemToFind)`

<span data-ttu-id="85f41-227">Kontrollerar om en matris som innehåller ett värde, ett objekt som innehåller en nyckel eller en sträng som innehåller understrängen.</span><span class="sxs-lookup"><span data-stu-id="85f41-227">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-228">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-228">Parameters</span></span>

| <span data-ttu-id="85f41-229">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-229">Parameter</span></span> | <span data-ttu-id="85f41-230">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-230">Required</span></span> | <span data-ttu-id="85f41-231">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-231">Type</span></span> | <span data-ttu-id="85f41-232">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-232">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-233">Behållaren</span><span class="sxs-lookup"><span data-stu-id="85f41-233">container</span></span> |<span data-ttu-id="85f41-234">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-234">Yes</span></span> |<span data-ttu-id="85f41-235">matris, objekt eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-235">array, object, or string</span></span> |<span data-ttu-id="85f41-236">hello-värde som innehåller hello värdet toofind.</span><span class="sxs-lookup"><span data-stu-id="85f41-236">hello value that contains hello value toofind.</span></span> |
| <span data-ttu-id="85f41-237">itemToFind</span><span class="sxs-lookup"><span data-stu-id="85f41-237">itemToFind</span></span> |<span data-ttu-id="85f41-238">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-238">Yes</span></span> |<span data-ttu-id="85f41-239">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="85f41-239">string or int</span></span> |<span data-ttu-id="85f41-240">hello värdet toofind.</span><span class="sxs-lookup"><span data-stu-id="85f41-240">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-241">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-241">Return value</span></span>

<span data-ttu-id="85f41-242">**SANT** om hello objekt hittades, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="85f41-242">**True** if hello item is found; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-243">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-243">Example</span></span>

<span data-ttu-id="85f41-244">hello följande exempel visas hur toouse innehåller med olika typer:</span><span class="sxs-lookup"><span data-stu-id="85f41-244">hello following example shows how toouse contains with different types:</span></span>

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

<span data-ttu-id="85f41-245">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-245">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-246">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-246">Name</span></span> | <span data-ttu-id="85f41-247">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-247">Type</span></span> | <span data-ttu-id="85f41-248">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-249">stringTrue</span><span class="sxs-lookup"><span data-stu-id="85f41-249">stringTrue</span></span> | <span data-ttu-id="85f41-250">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-250">Bool</span></span> | <span data-ttu-id="85f41-251">True</span><span class="sxs-lookup"><span data-stu-id="85f41-251">True</span></span> |
| <span data-ttu-id="85f41-252">stringFalse</span><span class="sxs-lookup"><span data-stu-id="85f41-252">stringFalse</span></span> | <span data-ttu-id="85f41-253">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-253">Bool</span></span> | <span data-ttu-id="85f41-254">False</span><span class="sxs-lookup"><span data-stu-id="85f41-254">False</span></span> |
| <span data-ttu-id="85f41-255">objectTrue</span><span class="sxs-lookup"><span data-stu-id="85f41-255">objectTrue</span></span> | <span data-ttu-id="85f41-256">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-256">Bool</span></span> | <span data-ttu-id="85f41-257">True</span><span class="sxs-lookup"><span data-stu-id="85f41-257">True</span></span> |
| <span data-ttu-id="85f41-258">objectFalse</span><span class="sxs-lookup"><span data-stu-id="85f41-258">objectFalse</span></span> | <span data-ttu-id="85f41-259">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-259">Bool</span></span> | <span data-ttu-id="85f41-260">False</span><span class="sxs-lookup"><span data-stu-id="85f41-260">False</span></span> |
| <span data-ttu-id="85f41-261">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="85f41-261">arrayTrue</span></span> | <span data-ttu-id="85f41-262">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-262">Bool</span></span> | <span data-ttu-id="85f41-263">True</span><span class="sxs-lookup"><span data-stu-id="85f41-263">True</span></span> |
| <span data-ttu-id="85f41-264">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="85f41-264">arrayFalse</span></span> | <span data-ttu-id="85f41-265">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-265">Bool</span></span> | <span data-ttu-id="85f41-266">False</span><span class="sxs-lookup"><span data-stu-id="85f41-266">False</span></span> |

<a id="createarray" />

## <a name="createarray"></a><span data-ttu-id="85f41-267">createarray</span><span class="sxs-lookup"><span data-stu-id="85f41-267">createarray</span></span>
`createArray (arg1, arg2, arg3, ...)`

<span data-ttu-id="85f41-268">Skapar en matris av hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="85f41-268">Creates an array from hello parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-269">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-269">Parameters</span></span>

| <span data-ttu-id="85f41-270">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-270">Parameter</span></span> | <span data-ttu-id="85f41-271">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-271">Required</span></span> | <span data-ttu-id="85f41-272">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-272">Type</span></span> | <span data-ttu-id="85f41-273">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-273">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-274">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-274">arg1</span></span> |<span data-ttu-id="85f41-275">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-275">Yes</span></span> |<span data-ttu-id="85f41-276">Sträng, heltal, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-276">String, Integer, Array, or Object</span></span> |<span data-ttu-id="85f41-277">hello första värdet i hello matris.</span><span class="sxs-lookup"><span data-stu-id="85f41-277">hello first value in hello array.</span></span> |
| <span data-ttu-id="85f41-278">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="85f41-278">additional arguments</span></span> |<span data-ttu-id="85f41-279">Nej</span><span class="sxs-lookup"><span data-stu-id="85f41-279">No</span></span> |<span data-ttu-id="85f41-280">Sträng, heltal, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-280">String, Integer, Array, or Object</span></span> |<span data-ttu-id="85f41-281">Ytterligare värden i hello matris.</span><span class="sxs-lookup"><span data-stu-id="85f41-281">Additional values in hello array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-282">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-282">Return value</span></span>

<span data-ttu-id="85f41-283">En matris.</span><span class="sxs-lookup"><span data-stu-id="85f41-283">An array.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-284">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-284">Example</span></span>

<span data-ttu-id="85f41-285">följande exempel visar hur hello toouse createArray med olika typer:</span><span class="sxs-lookup"><span data-stu-id="85f41-285">hello following example shows how toouse createArray with different types:</span></span>

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

<span data-ttu-id="85f41-286">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-286">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-287">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-287">Name</span></span> | <span data-ttu-id="85f41-288">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-288">Type</span></span> | <span data-ttu-id="85f41-289">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-289">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-290">stringArray</span><span class="sxs-lookup"><span data-stu-id="85f41-290">stringArray</span></span> | <span data-ttu-id="85f41-291">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-291">Array</span></span> | <span data-ttu-id="85f41-292">[”a”, ”b”, ”c”]</span><span class="sxs-lookup"><span data-stu-id="85f41-292">["a", "b", "c"]</span></span> |
| <span data-ttu-id="85f41-293">intArray</span><span class="sxs-lookup"><span data-stu-id="85f41-293">intArray</span></span> | <span data-ttu-id="85f41-294">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-294">Array</span></span> | <span data-ttu-id="85f41-295">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="85f41-295">[1, 2, 3]</span></span> |
| <span data-ttu-id="85f41-296">objectArray</span><span class="sxs-lookup"><span data-stu-id="85f41-296">objectArray</span></span> | <span data-ttu-id="85f41-297">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-297">Array</span></span> | <span data-ttu-id="85f41-298">[{”1”: ”a”, ”två”: ”b”, ”tre”: ”c”}]</span><span class="sxs-lookup"><span data-stu-id="85f41-298">[{"one": "a", "two": "b", "three": "c"}]</span></span> |
| <span data-ttu-id="85f41-299">arrayArray</span><span class="sxs-lookup"><span data-stu-id="85f41-299">arrayArray</span></span> | <span data-ttu-id="85f41-300">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-300">Array</span></span> | <span data-ttu-id="85f41-301">[[”1”, ”två”, ”tre”]]</span><span class="sxs-lookup"><span data-stu-id="85f41-301">[["one", "two", "three"]]</span></span> |

<a id="empty" />

## <a name="empty"></a><span data-ttu-id="85f41-302">tom</span><span class="sxs-lookup"><span data-stu-id="85f41-302">empty</span></span>

`empty(itemToTest)`

<span data-ttu-id="85f41-303">Anger om en matris, objekt eller sträng är tom.</span><span class="sxs-lookup"><span data-stu-id="85f41-303">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-304">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-304">Parameters</span></span>

| <span data-ttu-id="85f41-305">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-305">Parameter</span></span> | <span data-ttu-id="85f41-306">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-306">Required</span></span> | <span data-ttu-id="85f41-307">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-307">Type</span></span> | <span data-ttu-id="85f41-308">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-308">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-309">itemToTest</span><span class="sxs-lookup"><span data-stu-id="85f41-309">itemToTest</span></span> |<span data-ttu-id="85f41-310">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-310">Yes</span></span> |<span data-ttu-id="85f41-311">matris, objekt eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-311">array, object, or string</span></span> |<span data-ttu-id="85f41-312">Hej värdet toocheck om den är tom.</span><span class="sxs-lookup"><span data-stu-id="85f41-312">hello value toocheck if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-313">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-313">Return value</span></span>

<span data-ttu-id="85f41-314">Returnerar **SANT** om hello-värdet är tomt, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="85f41-314">Returns **True** if hello value is empty; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-315">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-315">Example</span></span>

<span data-ttu-id="85f41-316">följande exempel hello kontrollerar om en matris och objektet sträng är tom.</span><span class="sxs-lookup"><span data-stu-id="85f41-316">hello following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="85f41-317">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-317">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-318">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-318">Name</span></span> | <span data-ttu-id="85f41-319">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-319">Type</span></span> | <span data-ttu-id="85f41-320">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-320">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-321">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="85f41-321">arrayEmpty</span></span> | <span data-ttu-id="85f41-322">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-322">Bool</span></span> | <span data-ttu-id="85f41-323">True</span><span class="sxs-lookup"><span data-stu-id="85f41-323">True</span></span> |
| <span data-ttu-id="85f41-324">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="85f41-324">objectEmpty</span></span> | <span data-ttu-id="85f41-325">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-325">Bool</span></span> | <span data-ttu-id="85f41-326">True</span><span class="sxs-lookup"><span data-stu-id="85f41-326">True</span></span> |
| <span data-ttu-id="85f41-327">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="85f41-327">stringEmpty</span></span> | <span data-ttu-id="85f41-328">bool</span><span class="sxs-lookup"><span data-stu-id="85f41-328">Bool</span></span> | <span data-ttu-id="85f41-329">True</span><span class="sxs-lookup"><span data-stu-id="85f41-329">True</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="85f41-330">första</span><span class="sxs-lookup"><span data-stu-id="85f41-330">first</span></span>
`first(arg1)`

<span data-ttu-id="85f41-331">Returnerar hello första elementet i matrisen hello eller första tecknet i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-331">Returns hello first element of hello array, or first character of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-332">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-332">Parameters</span></span>

| <span data-ttu-id="85f41-333">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-333">Parameter</span></span> | <span data-ttu-id="85f41-334">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-334">Required</span></span> | <span data-ttu-id="85f41-335">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-335">Type</span></span> | <span data-ttu-id="85f41-336">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-336">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-337">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-337">arg1</span></span> |<span data-ttu-id="85f41-338">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-338">Yes</span></span> |<span data-ttu-id="85f41-339">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-339">array or string</span></span> |<span data-ttu-id="85f41-340">hello värdet tooretrieve hello första elementet eller tecken.</span><span class="sxs-lookup"><span data-stu-id="85f41-340">hello value tooretrieve hello first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-341">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-341">Return value</span></span>

<span data-ttu-id="85f41-342">hello (sträng, int, matris eller objekt) typ av hello första elementet i en matris eller hello första tecknet i en sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-342">hello type (string, int, array, or object) of hello first element in an array, or hello first character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-343">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-343">Example</span></span>

<span data-ttu-id="85f41-344">hello följande exempel visas hur toouse hello första funktion med en matris och en sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-344">hello following example shows how toouse hello first function with an array and string.</span></span>

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

<span data-ttu-id="85f41-345">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-345">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-346">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-346">Name</span></span> | <span data-ttu-id="85f41-347">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-347">Type</span></span> | <span data-ttu-id="85f41-348">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-348">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-349">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-349">arrayOutput</span></span> | <span data-ttu-id="85f41-350">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-350">String</span></span> | <span data-ttu-id="85f41-351">en</span><span class="sxs-lookup"><span data-stu-id="85f41-351">one</span></span> |
| <span data-ttu-id="85f41-352">stringOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-352">stringOutput</span></span> | <span data-ttu-id="85f41-353">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-353">String</span></span> | <span data-ttu-id="85f41-354">O</span><span class="sxs-lookup"><span data-stu-id="85f41-354">O</span></span> |

<a id="intersection" />

## <a name="intersection"></a><span data-ttu-id="85f41-355">skärningspunkten</span><span class="sxs-lookup"><span data-stu-id="85f41-355">intersection</span></span>
`intersection(arg1, arg2, arg3, ...)`

<span data-ttu-id="85f41-356">Returnerar en enda matris eller ett objekt med hello gemensamma element från hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="85f41-356">Returns a single array or object with hello common elements from hello parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-357">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-357">Parameters</span></span>

| <span data-ttu-id="85f41-358">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-358">Parameter</span></span> | <span data-ttu-id="85f41-359">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-359">Required</span></span> | <span data-ttu-id="85f41-360">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-360">Type</span></span> | <span data-ttu-id="85f41-361">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-361">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-362">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-362">arg1</span></span> |<span data-ttu-id="85f41-363">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-363">Yes</span></span> |<span data-ttu-id="85f41-364">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-364">array or object</span></span> |<span data-ttu-id="85f41-365">hello första värde toouse för att söka efter vanliga element.</span><span class="sxs-lookup"><span data-stu-id="85f41-365">hello first value toouse for finding common elements.</span></span> |
| <span data-ttu-id="85f41-366">arg2</span><span class="sxs-lookup"><span data-stu-id="85f41-366">arg2</span></span> |<span data-ttu-id="85f41-367">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-367">Yes</span></span> |<span data-ttu-id="85f41-368">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-368">array or object</span></span> |<span data-ttu-id="85f41-369">hello andra värdet toouse för att söka efter vanliga element.</span><span class="sxs-lookup"><span data-stu-id="85f41-369">hello second value toouse for finding common elements.</span></span> |
| <span data-ttu-id="85f41-370">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="85f41-370">additional arguments</span></span> |<span data-ttu-id="85f41-371">Nej</span><span class="sxs-lookup"><span data-stu-id="85f41-371">No</span></span> |<span data-ttu-id="85f41-372">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-372">array or object</span></span> |<span data-ttu-id="85f41-373">Ytterligare värden toouse för att söka efter vanliga element.</span><span class="sxs-lookup"><span data-stu-id="85f41-373">Additional values toouse for finding common elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-374">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-374">Return value</span></span>

<span data-ttu-id="85f41-375">En matris eller ett objekt med hello gemensamma element.</span><span class="sxs-lookup"><span data-stu-id="85f41-375">An array or object with hello common elements.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-376">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-376">Example</span></span>

<span data-ttu-id="85f41-377">Hej följande exempel visar hur toouse snitt med matriser och -objekt:</span><span class="sxs-lookup"><span data-stu-id="85f41-377">hello following example shows how toouse intersection with arrays and objects:</span></span>

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

<span data-ttu-id="85f41-378">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-378">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-379">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-379">Name</span></span> | <span data-ttu-id="85f41-380">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-380">Type</span></span> | <span data-ttu-id="85f41-381">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-381">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-382">objectOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-382">objectOutput</span></span> | <span data-ttu-id="85f41-383">Objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-383">Object</span></span> | <span data-ttu-id="85f41-384">{”1”: ”a”, ”tre”: ”c”}</span><span class="sxs-lookup"><span data-stu-id="85f41-384">{"one": "a", "three": "c"}</span></span> |
| <span data-ttu-id="85f41-385">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-385">arrayOutput</span></span> | <span data-ttu-id="85f41-386">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-386">Array</span></span> | <span data-ttu-id="85f41-387">[”två”, ”tre”]</span><span class="sxs-lookup"><span data-stu-id="85f41-387">["two", "three"]</span></span> |


## <a name="json"></a><span data-ttu-id="85f41-388">JSON</span><span class="sxs-lookup"><span data-stu-id="85f41-388">json</span></span>
`json(arg1)`

<span data-ttu-id="85f41-389">Returnerar ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="85f41-389">Returns a JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-390">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-390">Parameters</span></span>

| <span data-ttu-id="85f41-391">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-391">Parameter</span></span> | <span data-ttu-id="85f41-392">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-392">Required</span></span> | <span data-ttu-id="85f41-393">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-393">Type</span></span> | <span data-ttu-id="85f41-394">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-394">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-395">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-395">arg1</span></span> |<span data-ttu-id="85f41-396">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-396">Yes</span></span> |<span data-ttu-id="85f41-397">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-397">string</span></span> |<span data-ttu-id="85f41-398">hello värdet tooconvert tooJSON.</span><span class="sxs-lookup"><span data-stu-id="85f41-398">hello value tooconvert tooJSON.</span></span> |


### <a name="return-value"></a><span data-ttu-id="85f41-399">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-399">Return value</span></span>

<span data-ttu-id="85f41-400">hello JSON-objekt från hello angiven sträng eller ett tomt-objekt när **null** har angetts.</span><span class="sxs-lookup"><span data-stu-id="85f41-400">hello JSON object from hello specified string, or an empty object when **null** is specified.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-401">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-401">Example</span></span>

<span data-ttu-id="85f41-402">Hej följande exempel visar hur toouse snitt med matriser och -objekt:</span><span class="sxs-lookup"><span data-stu-id="85f41-402">hello following example shows how toouse intersection with arrays and objects:</span></span>

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

<span data-ttu-id="85f41-403">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-403">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-404">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-404">Name</span></span> | <span data-ttu-id="85f41-405">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-405">Type</span></span> | <span data-ttu-id="85f41-406">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-406">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-407">jsonOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-407">jsonOutput</span></span> | <span data-ttu-id="85f41-408">Objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-408">Object</span></span> | <span data-ttu-id="85f41-409">{”a”: ”b”}</span><span class="sxs-lookup"><span data-stu-id="85f41-409">{"a": "b"}</span></span> |
| <span data-ttu-id="85f41-410">nullOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-410">nullOutput</span></span> | <span data-ttu-id="85f41-411">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="85f41-411">Boolean</span></span> | <span data-ttu-id="85f41-412">True</span><span class="sxs-lookup"><span data-stu-id="85f41-412">True</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="85f41-413">senaste</span><span class="sxs-lookup"><span data-stu-id="85f41-413">last</span></span>
`last (arg1)`

<span data-ttu-id="85f41-414">Returnerar hello sista elementet i matrisen hello eller sista tecknet i hello strängen.</span><span class="sxs-lookup"><span data-stu-id="85f41-414">Returns hello last element of hello array, or last character of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-415">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-415">Parameters</span></span>

| <span data-ttu-id="85f41-416">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-416">Parameter</span></span> | <span data-ttu-id="85f41-417">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-417">Required</span></span> | <span data-ttu-id="85f41-418">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-418">Type</span></span> | <span data-ttu-id="85f41-419">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-420">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-420">arg1</span></span> |<span data-ttu-id="85f41-421">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-421">Yes</span></span> |<span data-ttu-id="85f41-422">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-422">array or string</span></span> |<span data-ttu-id="85f41-423">hello värdet tooretrieve hello sista elementet eller tecken.</span><span class="sxs-lookup"><span data-stu-id="85f41-423">hello value tooretrieve hello last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-424">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-424">Return value</span></span>

<span data-ttu-id="85f41-425">hello (sträng, int, matris eller objekt) typ av hello sista elementet i en matris eller hello sista tecknet i en sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-425">hello type (string, int, array, or object) of hello last element in an array, or hello last character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-426">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-426">Example</span></span>

<span data-ttu-id="85f41-427">hello följande exempel visas hur toouse hello senaste funktion med en matris och en sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-427">hello following example shows how toouse hello last function with an array and string.</span></span>

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

<span data-ttu-id="85f41-428">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-428">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-429">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-429">Name</span></span> | <span data-ttu-id="85f41-430">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-430">Type</span></span> | <span data-ttu-id="85f41-431">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-432">arrayOutput</span></span> | <span data-ttu-id="85f41-433">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-433">String</span></span> | <span data-ttu-id="85f41-434">tre</span><span class="sxs-lookup"><span data-stu-id="85f41-434">three</span></span> |
| <span data-ttu-id="85f41-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-435">stringOutput</span></span> | <span data-ttu-id="85f41-436">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-436">String</span></span> | <span data-ttu-id="85f41-437">E</span><span class="sxs-lookup"><span data-stu-id="85f41-437">e</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="85f41-438">Längd</span><span class="sxs-lookup"><span data-stu-id="85f41-438">length</span></span>
`length(arg1)`

<span data-ttu-id="85f41-439">Returnerar hello antalet element i en matris eller tecken i en sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-439">Returns hello number of elements in an array, or characters in a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-440">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-440">Parameters</span></span>

| <span data-ttu-id="85f41-441">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-441">Parameter</span></span> | <span data-ttu-id="85f41-442">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-442">Required</span></span> | <span data-ttu-id="85f41-443">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-443">Type</span></span> | <span data-ttu-id="85f41-444">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-444">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-445">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-445">arg1</span></span> |<span data-ttu-id="85f41-446">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-446">Yes</span></span> |<span data-ttu-id="85f41-447">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-447">array or string</span></span> |<span data-ttu-id="85f41-448">Hej matris toouse för att hämta hello antalet element eller hello sträng toouse för att hämta hello antalet tecken.</span><span class="sxs-lookup"><span data-stu-id="85f41-448">hello array toouse for getting hello number of elements, or hello string toouse for getting hello number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-449">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-449">Return value</span></span>

<span data-ttu-id="85f41-450">Int.</span><span class="sxs-lookup"><span data-stu-id="85f41-450">An int.</span></span> 

### <a name="example"></a><span data-ttu-id="85f41-451">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-451">Example</span></span>

<span data-ttu-id="85f41-452">följande exempel visar hur hello toouse längd med en matris och sträng:</span><span class="sxs-lookup"><span data-stu-id="85f41-452">hello following example shows how toouse length with an array and string:</span></span>

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

<span data-ttu-id="85f41-453">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-453">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-454">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-454">Name</span></span> | <span data-ttu-id="85f41-455">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-455">Type</span></span> | <span data-ttu-id="85f41-456">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-456">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-457">arrayLength</span><span class="sxs-lookup"><span data-stu-id="85f41-457">arrayLength</span></span> | <span data-ttu-id="85f41-458">int</span><span class="sxs-lookup"><span data-stu-id="85f41-458">Int</span></span> | <span data-ttu-id="85f41-459">3</span><span class="sxs-lookup"><span data-stu-id="85f41-459">3</span></span> |
| <span data-ttu-id="85f41-460">stringLength</span><span class="sxs-lookup"><span data-stu-id="85f41-460">stringLength</span></span> | <span data-ttu-id="85f41-461">int</span><span class="sxs-lookup"><span data-stu-id="85f41-461">Int</span></span> | <span data-ttu-id="85f41-462">13</span><span class="sxs-lookup"><span data-stu-id="85f41-462">13</span></span> |

<span data-ttu-id="85f41-463">Du kan använda den här funktionen med en matris toospecify hello antal upprepningar när du skapar resurser.</span><span class="sxs-lookup"><span data-stu-id="85f41-463">You can use this function with an array toospecify hello number of iterations when creating resources.</span></span> <span data-ttu-id="85f41-464">I följande exempel hello, hello parametern **siteNames** referera tooan matris med namnen toouse när du skapar hello-webbplatser.</span><span class="sxs-lookup"><span data-stu-id="85f41-464">In hello following example, hello parameter **siteNames** would refer tooan array of names toouse when creating hello web sites.</span></span>

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

<span data-ttu-id="85f41-465">Mer information om hur du använder den här funktionen med en matris finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="85f41-465">For more information about using this function with an array, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<a id="min" />

## <a name="min"></a><span data-ttu-id="85f41-466">min.</span><span class="sxs-lookup"><span data-stu-id="85f41-466">min</span></span>
`min(arg1)`

<span data-ttu-id="85f41-467">Returnerar hello minimivärdet från en matris av heltal eller en kommaavgränsad lista med heltal.</span><span class="sxs-lookup"><span data-stu-id="85f41-467">Returns hello minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-468">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-468">Parameters</span></span>

| <span data-ttu-id="85f41-469">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-469">Parameter</span></span> | <span data-ttu-id="85f41-470">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-470">Required</span></span> | <span data-ttu-id="85f41-471">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-471">Type</span></span> | <span data-ttu-id="85f41-472">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-472">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-473">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-473">arg1</span></span> |<span data-ttu-id="85f41-474">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-474">Yes</span></span> |<span data-ttu-id="85f41-475">matris med heltal eller en kommaavgränsad lista med heltal</span><span class="sxs-lookup"><span data-stu-id="85f41-475">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="85f41-476">hello samling tooget hello minimivärde.</span><span class="sxs-lookup"><span data-stu-id="85f41-476">hello collection tooget hello minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-477">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-477">Return value</span></span>

<span data-ttu-id="85f41-478">Ett heltal som representerar hello minimivärdet.</span><span class="sxs-lookup"><span data-stu-id="85f41-478">An int representing hello minimum value.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-479">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-479">Example</span></span>

<span data-ttu-id="85f41-480">följande exempel visar hur hello toouse min med en matris och en lista med heltal:</span><span class="sxs-lookup"><span data-stu-id="85f41-480">hello following example shows how toouse min with an array and a list of integers:</span></span>

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

<span data-ttu-id="85f41-481">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-481">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-482">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-482">Name</span></span> | <span data-ttu-id="85f41-483">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-483">Type</span></span> | <span data-ttu-id="85f41-484">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-484">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-485">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-485">arrayOutput</span></span> | <span data-ttu-id="85f41-486">int</span><span class="sxs-lookup"><span data-stu-id="85f41-486">Int</span></span> | <span data-ttu-id="85f41-487">0</span><span class="sxs-lookup"><span data-stu-id="85f41-487">0</span></span> |
| <span data-ttu-id="85f41-488">intOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-488">intOutput</span></span> | <span data-ttu-id="85f41-489">int</span><span class="sxs-lookup"><span data-stu-id="85f41-489">Int</span></span> | <span data-ttu-id="85f41-490">0</span><span class="sxs-lookup"><span data-stu-id="85f41-490">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="85f41-491">Max</span><span class="sxs-lookup"><span data-stu-id="85f41-491">max</span></span>
`max(arg1)`

<span data-ttu-id="85f41-492">Returnerar hello högsta värde från en heltalsmatris eller en kommaavgränsad lista med heltal.</span><span class="sxs-lookup"><span data-stu-id="85f41-492">Returns hello maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-493">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-493">Parameters</span></span>

| <span data-ttu-id="85f41-494">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-494">Parameter</span></span> | <span data-ttu-id="85f41-495">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-495">Required</span></span> | <span data-ttu-id="85f41-496">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-496">Type</span></span> | <span data-ttu-id="85f41-497">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-497">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-498">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-498">arg1</span></span> |<span data-ttu-id="85f41-499">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-499">Yes</span></span> |<span data-ttu-id="85f41-500">matris med heltal eller en kommaavgränsad lista med heltal</span><span class="sxs-lookup"><span data-stu-id="85f41-500">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="85f41-501">hello samling tooget hello maximivärde.</span><span class="sxs-lookup"><span data-stu-id="85f41-501">hello collection tooget hello maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-502">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-502">Return value</span></span>

<span data-ttu-id="85f41-503">Ett heltal som representerar hello maximivärdet.</span><span class="sxs-lookup"><span data-stu-id="85f41-503">An int representing hello maximum value.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-504">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-504">Example</span></span>

<span data-ttu-id="85f41-505">följande exempel visar hur hello toouse max med en matris och en lista med heltal:</span><span class="sxs-lookup"><span data-stu-id="85f41-505">hello following example shows how toouse max with an array and a list of integers:</span></span>

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

<span data-ttu-id="85f41-506">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-506">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-507">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-507">Name</span></span> | <span data-ttu-id="85f41-508">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-508">Type</span></span> | <span data-ttu-id="85f41-509">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-509">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-510">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-510">arrayOutput</span></span> | <span data-ttu-id="85f41-511">int</span><span class="sxs-lookup"><span data-stu-id="85f41-511">Int</span></span> | <span data-ttu-id="85f41-512">5</span><span class="sxs-lookup"><span data-stu-id="85f41-512">5</span></span> |
| <span data-ttu-id="85f41-513">intOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-513">intOutput</span></span> | <span data-ttu-id="85f41-514">int</span><span class="sxs-lookup"><span data-stu-id="85f41-514">Int</span></span> | <span data-ttu-id="85f41-515">5</span><span class="sxs-lookup"><span data-stu-id="85f41-515">5</span></span> |

<a id="range" />

## <a name="range"></a><span data-ttu-id="85f41-516">intervallet</span><span class="sxs-lookup"><span data-stu-id="85f41-516">range</span></span>
`range(startingInteger, numberOfElements)`

<span data-ttu-id="85f41-517">Skapar en heltalsmatris från början heltal och som innehåller ett antal objekt.</span><span class="sxs-lookup"><span data-stu-id="85f41-517">Creates an array of integers from a starting integer and containing a number of items.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-518">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-518">Parameters</span></span>

| <span data-ttu-id="85f41-519">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-519">Parameter</span></span> | <span data-ttu-id="85f41-520">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-520">Required</span></span> | <span data-ttu-id="85f41-521">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-521">Type</span></span> | <span data-ttu-id="85f41-522">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-522">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-523">startingInteger</span><span class="sxs-lookup"><span data-stu-id="85f41-523">startingInteger</span></span> |<span data-ttu-id="85f41-524">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-524">Yes</span></span> |<span data-ttu-id="85f41-525">int</span><span class="sxs-lookup"><span data-stu-id="85f41-525">int</span></span> |<span data-ttu-id="85f41-526">hello första heltal i hello matris.</span><span class="sxs-lookup"><span data-stu-id="85f41-526">hello first integer in hello array.</span></span> |
| <span data-ttu-id="85f41-527">numberofElements</span><span class="sxs-lookup"><span data-stu-id="85f41-527">numberofElements</span></span> |<span data-ttu-id="85f41-528">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-528">Yes</span></span> |<span data-ttu-id="85f41-529">int</span><span class="sxs-lookup"><span data-stu-id="85f41-529">int</span></span> |<span data-ttu-id="85f41-530">hello antal heltal i hello matris.</span><span class="sxs-lookup"><span data-stu-id="85f41-530">hello number of integers in hello array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-531">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-531">Return value</span></span>

<span data-ttu-id="85f41-532">En heltalsmatris.</span><span class="sxs-lookup"><span data-stu-id="85f41-532">An array of integers.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-533">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-533">Example</span></span>

<span data-ttu-id="85f41-534">hello som följande exempel visar hur toouse hello intervallet funktionen:</span><span class="sxs-lookup"><span data-stu-id="85f41-534">hello following example shows how toouse hello range function:</span></span>

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

<span data-ttu-id="85f41-535">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-535">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-536">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-536">Name</span></span> | <span data-ttu-id="85f41-537">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-537">Type</span></span> | <span data-ttu-id="85f41-538">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-538">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-539">rangeOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-539">rangeOutput</span></span> | <span data-ttu-id="85f41-540">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-540">Array</span></span> | <span data-ttu-id="85f41-541">[5, 6, 7]</span><span class="sxs-lookup"><span data-stu-id="85f41-541">[5, 6, 7]</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="85f41-542">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="85f41-542">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="85f41-543">Returnerar en matris med alla hello-element efter hello nummer som angetts i hello matrisen eller returnerar en sträng med tecken för alla hello när hello angett numret i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-543">Returns an array with all hello elements after hello specified number in hello array, or returns a string with all hello characters after hello specified number in hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-544">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-544">Parameters</span></span>

| <span data-ttu-id="85f41-545">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-545">Parameter</span></span> | <span data-ttu-id="85f41-546">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-546">Required</span></span> | <span data-ttu-id="85f41-547">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-547">Type</span></span> | <span data-ttu-id="85f41-548">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-548">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-549">Ursprungligt värde</span><span class="sxs-lookup"><span data-stu-id="85f41-549">originalValue</span></span> |<span data-ttu-id="85f41-550">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-550">Yes</span></span> |<span data-ttu-id="85f41-551">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-551">array or string</span></span> |<span data-ttu-id="85f41-552">Hej array eller string toouse för att hoppa över.</span><span class="sxs-lookup"><span data-stu-id="85f41-552">hello array or string toouse for skipping.</span></span> |
| <span data-ttu-id="85f41-553">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="85f41-553">numberToSkip</span></span> |<span data-ttu-id="85f41-554">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-554">Yes</span></span> |<span data-ttu-id="85f41-555">int</span><span class="sxs-lookup"><span data-stu-id="85f41-555">int</span></span> |<span data-ttu-id="85f41-556">hello antal element eller tecken tooskip.</span><span class="sxs-lookup"><span data-stu-id="85f41-556">hello number of elements or characters tooskip.</span></span> <span data-ttu-id="85f41-557">Om det här värdet är 0 eller mindre, alla hello element eller tecken i hello-värde som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="85f41-557">If this value is 0 or less, all hello elements or characters in hello value are returned.</span></span> <span data-ttu-id="85f41-558">Om den är större än hello längd hello matris eller sträng returneras en tom matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-558">If it is larger than hello length of hello array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-559">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-559">Return value</span></span>

<span data-ttu-id="85f41-560">En matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-560">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-561">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-561">Example</span></span>

<span data-ttu-id="85f41-562">hello följande exempel hoppar över hello angivna antalet element i matrisen hello och hello angivet antal tecken i en sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-562">hello following example skips hello specified number of elements in hello array, and hello specified number of characters in a string.</span></span>

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

<span data-ttu-id="85f41-563">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-563">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-564">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-564">Name</span></span> | <span data-ttu-id="85f41-565">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-565">Type</span></span> | <span data-ttu-id="85f41-566">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-566">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-567">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-567">arrayOutput</span></span> | <span data-ttu-id="85f41-568">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-568">Array</span></span> | <span data-ttu-id="85f41-569">[”tre”]</span><span class="sxs-lookup"><span data-stu-id="85f41-569">["three"]</span></span> |
| <span data-ttu-id="85f41-570">stringOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-570">stringOutput</span></span> | <span data-ttu-id="85f41-571">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-571">String</span></span> | <span data-ttu-id="85f41-572">två tre</span><span class="sxs-lookup"><span data-stu-id="85f41-572">two three</span></span> |

<a id="take" />

## <a name="take"></a><span data-ttu-id="85f41-573">ta</span><span class="sxs-lookup"><span data-stu-id="85f41-573">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="85f41-574">Returnerar en matris med hello angivna antalet element från hello start av hello matris eller en sträng med hello angivet antal tecken från hello början av hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-574">Returns an array with hello specified number of elements from hello start of hello array, or a string with hello specified number of characters from hello start of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-575">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-575">Parameters</span></span>

| <span data-ttu-id="85f41-576">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-576">Parameter</span></span> | <span data-ttu-id="85f41-577">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-577">Required</span></span> | <span data-ttu-id="85f41-578">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-578">Type</span></span> | <span data-ttu-id="85f41-579">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-579">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-580">Ursprungligt värde</span><span class="sxs-lookup"><span data-stu-id="85f41-580">originalValue</span></span> |<span data-ttu-id="85f41-581">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-581">Yes</span></span> |<span data-ttu-id="85f41-582">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-582">array or string</span></span> |<span data-ttu-id="85f41-583">Hej array eller string tootake hello element från.</span><span class="sxs-lookup"><span data-stu-id="85f41-583">hello array or string tootake hello elements from.</span></span> |
| <span data-ttu-id="85f41-584">numberToTake</span><span class="sxs-lookup"><span data-stu-id="85f41-584">numberToTake</span></span> |<span data-ttu-id="85f41-585">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-585">Yes</span></span> |<span data-ttu-id="85f41-586">int</span><span class="sxs-lookup"><span data-stu-id="85f41-586">int</span></span> |<span data-ttu-id="85f41-587">hello antal element eller tecken tootake.</span><span class="sxs-lookup"><span data-stu-id="85f41-587">hello number of elements or characters tootake.</span></span> <span data-ttu-id="85f41-588">Om det här värdet är 0 eller mindre, returneras en tom matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-588">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="85f41-589">Om den är större än hello hello matris eller sträng returneras alla hello-element i hello array eller string.</span><span class="sxs-lookup"><span data-stu-id="85f41-589">If it is larger than hello length of hello given array or string, all hello elements in hello array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-590">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-590">Return value</span></span>

<span data-ttu-id="85f41-591">En matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-591">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-592">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-592">Example</span></span>

<span data-ttu-id="85f41-593">följande exempel tar hello hello angivet antal element från hello matris och tecken från en sträng.</span><span class="sxs-lookup"><span data-stu-id="85f41-593">hello following example takes hello specified number of elements from hello array, and characters from a string.</span></span>

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

<span data-ttu-id="85f41-594">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-594">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-595">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-595">Name</span></span> | <span data-ttu-id="85f41-596">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-596">Type</span></span> | <span data-ttu-id="85f41-597">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-597">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-598">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-598">arrayOutput</span></span> | <span data-ttu-id="85f41-599">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-599">Array</span></span> | <span data-ttu-id="85f41-600">[””, ”två”]</span><span class="sxs-lookup"><span data-stu-id="85f41-600">["one", "two"]</span></span> |
| <span data-ttu-id="85f41-601">stringOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-601">stringOutput</span></span> | <span data-ttu-id="85f41-602">Sträng</span><span class="sxs-lookup"><span data-stu-id="85f41-602">String</span></span> | <span data-ttu-id="85f41-603">på</span><span class="sxs-lookup"><span data-stu-id="85f41-603">on</span></span> |

<a id="union" />

## <a name="union"></a><span data-ttu-id="85f41-604">Union</span><span class="sxs-lookup"><span data-stu-id="85f41-604">union</span></span>
`union(arg1, arg2, arg3, ...)`

<span data-ttu-id="85f41-605">Returnerar en enda matris eller ett objekt med alla element från hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="85f41-605">Returns a single array or object with all elements from hello parameters.</span></span> <span data-ttu-id="85f41-606">Duplicerade värden eller nycklar är bara ingår en gång.</span><span class="sxs-lookup"><span data-stu-id="85f41-606">Duplicate values or keys are only included once.</span></span>

### <a name="parameters"></a><span data-ttu-id="85f41-607">Parametrar</span><span class="sxs-lookup"><span data-stu-id="85f41-607">Parameters</span></span>

| <span data-ttu-id="85f41-608">Parameter</span><span class="sxs-lookup"><span data-stu-id="85f41-608">Parameter</span></span> | <span data-ttu-id="85f41-609">Krävs</span><span class="sxs-lookup"><span data-stu-id="85f41-609">Required</span></span> | <span data-ttu-id="85f41-610">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-610">Type</span></span> | <span data-ttu-id="85f41-611">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="85f41-611">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="85f41-612">arg1</span><span class="sxs-lookup"><span data-stu-id="85f41-612">arg1</span></span> |<span data-ttu-id="85f41-613">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-613">Yes</span></span> |<span data-ttu-id="85f41-614">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-614">array or object</span></span> |<span data-ttu-id="85f41-615">hello första värde toouse för att ansluta till element.</span><span class="sxs-lookup"><span data-stu-id="85f41-615">hello first value toouse for joining elements.</span></span> |
| <span data-ttu-id="85f41-616">arg2</span><span class="sxs-lookup"><span data-stu-id="85f41-616">arg2</span></span> |<span data-ttu-id="85f41-617">Ja</span><span class="sxs-lookup"><span data-stu-id="85f41-617">Yes</span></span> |<span data-ttu-id="85f41-618">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-618">array or object</span></span> |<span data-ttu-id="85f41-619">hello andra värdet toouse för att ansluta till element.</span><span class="sxs-lookup"><span data-stu-id="85f41-619">hello second value toouse for joining elements.</span></span> |
| <span data-ttu-id="85f41-620">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="85f41-620">additional arguments</span></span> |<span data-ttu-id="85f41-621">Nej</span><span class="sxs-lookup"><span data-stu-id="85f41-621">No</span></span> |<span data-ttu-id="85f41-622">matris eller ett objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-622">array or object</span></span> |<span data-ttu-id="85f41-623">Ytterligare värden toouse för att ansluta till element.</span><span class="sxs-lookup"><span data-stu-id="85f41-623">Additional values toouse for joining elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="85f41-624">Returvärde</span><span class="sxs-lookup"><span data-stu-id="85f41-624">Return value</span></span>

<span data-ttu-id="85f41-625">En matris eller ett objekt.</span><span class="sxs-lookup"><span data-stu-id="85f41-625">An array or object.</span></span>

### <a name="example"></a><span data-ttu-id="85f41-626">Exempel</span><span class="sxs-lookup"><span data-stu-id="85f41-626">Example</span></span>

<span data-ttu-id="85f41-627">Hej följande exempel visar hur toouse union med matriser och -objekt:</span><span class="sxs-lookup"><span data-stu-id="85f41-627">hello following example shows how toouse union with arrays and objects:</span></span>

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

<span data-ttu-id="85f41-628">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="85f41-628">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="85f41-629">Namn</span><span class="sxs-lookup"><span data-stu-id="85f41-629">Name</span></span> | <span data-ttu-id="85f41-630">Typ</span><span class="sxs-lookup"><span data-stu-id="85f41-630">Type</span></span> | <span data-ttu-id="85f41-631">Värde</span><span class="sxs-lookup"><span data-stu-id="85f41-631">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="85f41-632">objectOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-632">objectOutput</span></span> | <span data-ttu-id="85f41-633">Objekt</span><span class="sxs-lookup"><span data-stu-id="85f41-633">Object</span></span> | <span data-ttu-id="85f41-634">{”1”: ”a”, ”två”: ”b”, ”tre”: ”c”, ”fyra”: ”d”, ”fem”: ”e”}</span><span class="sxs-lookup"><span data-stu-id="85f41-634">{"one": "a", "two": "b", "three": "c", "four": "d", "five": "e"}</span></span> |
| <span data-ttu-id="85f41-635">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="85f41-635">arrayOutput</span></span> | <span data-ttu-id="85f41-636">Matris</span><span class="sxs-lookup"><span data-stu-id="85f41-636">Array</span></span> | <span data-ttu-id="85f41-637">[”1”, ”två”, ”tre”, ”fyra”]</span><span class="sxs-lookup"><span data-stu-id="85f41-637">["one", "two", "three", "four"]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="85f41-638">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85f41-638">Next steps</span></span>
* <span data-ttu-id="85f41-639">En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="85f41-639">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="85f41-640">flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="85f41-640">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="85f41-641">tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="85f41-641">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="85f41-642">toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="85f41-642">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

