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
# <a name="string-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="73a75-103">Strängfunktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="73a75-103">String functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="73a75-104">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med strängar hello:</span><span class="sxs-lookup"><span data-stu-id="73a75-104">Resource Manager provides hello following functions for working with strings:</span></span>

* [<span data-ttu-id="73a75-105">Base64</span><span class="sxs-lookup"><span data-stu-id="73a75-105">base64</span></span>](#base64)
* [<span data-ttu-id="73a75-106">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="73a75-106">base64ToJson</span></span>](#base64tojson)
* [<span data-ttu-id="73a75-107">base64ToString</span><span class="sxs-lookup"><span data-stu-id="73a75-107">base64ToString</span></span>](#base64tostring)
* [<span data-ttu-id="73a75-108">concat</span><span class="sxs-lookup"><span data-stu-id="73a75-108">concat</span></span>](#concat)
* [<span data-ttu-id="73a75-109">innehåller</span><span class="sxs-lookup"><span data-stu-id="73a75-109">contains</span></span>](#contains)
* [<span data-ttu-id="73a75-110">dataUri</span><span class="sxs-lookup"><span data-stu-id="73a75-110">dataUri</span></span>](#datauri)
* [<span data-ttu-id="73a75-111">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="73a75-111">dataUriToString</span></span>](#datauritostring)
* [<span data-ttu-id="73a75-112">tom</span><span class="sxs-lookup"><span data-stu-id="73a75-112">empty</span></span>](#empty)
* [<span data-ttu-id="73a75-113">endsWith</span><span class="sxs-lookup"><span data-stu-id="73a75-113">endsWith</span></span>](#endswith)
* [<span data-ttu-id="73a75-114">första</span><span class="sxs-lookup"><span data-stu-id="73a75-114">first</span></span>](#first)
* [<span data-ttu-id="73a75-115">indexOf</span><span class="sxs-lookup"><span data-stu-id="73a75-115">indexOf</span></span>](#indexof)
* [<span data-ttu-id="73a75-116">senaste</span><span class="sxs-lookup"><span data-stu-id="73a75-116">last</span></span>](#last)
* [<span data-ttu-id="73a75-117">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="73a75-117">lastIndexOf</span></span>](#lastindexof)
* [<span data-ttu-id="73a75-118">längd</span><span class="sxs-lookup"><span data-stu-id="73a75-118">length</span></span>](#length)
* [<span data-ttu-id="73a75-119">padLeft</span><span class="sxs-lookup"><span data-stu-id="73a75-119">padLeft</span></span>](#padleft)
* [<span data-ttu-id="73a75-120">Ersätt</span><span class="sxs-lookup"><span data-stu-id="73a75-120">replace</span></span>](#replace)
* [<span data-ttu-id="73a75-121">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="73a75-121">skip</span></span>](#skip)
* [<span data-ttu-id="73a75-122">split</span><span class="sxs-lookup"><span data-stu-id="73a75-122">split</span></span>](#split)
* [<span data-ttu-id="73a75-123">startsWith</span><span class="sxs-lookup"><span data-stu-id="73a75-123">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="73a75-124">sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-124">string</span></span>](#string)
* [<span data-ttu-id="73a75-125">delsträngen</span><span class="sxs-lookup"><span data-stu-id="73a75-125">substring</span></span>](#substring)
* [<span data-ttu-id="73a75-126">ta</span><span class="sxs-lookup"><span data-stu-id="73a75-126">take</span></span>](#take)
* [<span data-ttu-id="73a75-127">toLower</span><span class="sxs-lookup"><span data-stu-id="73a75-127">toLower</span></span>](#tolower)
* [<span data-ttu-id="73a75-128">toUpper</span><span class="sxs-lookup"><span data-stu-id="73a75-128">toUpper</span></span>](#toupper)
* [<span data-ttu-id="73a75-129">trim</span><span class="sxs-lookup"><span data-stu-id="73a75-129">trim</span></span>](#trim)
* [<span data-ttu-id="73a75-130">uniqueString</span><span class="sxs-lookup"><span data-stu-id="73a75-130">uniqueString</span></span>](#uniquestring)
* [<span data-ttu-id="73a75-131">URI: n</span><span class="sxs-lookup"><span data-stu-id="73a75-131">uri</span></span>](#uri)
* [<span data-ttu-id="73a75-132">uriComponent</span><span class="sxs-lookup"><span data-stu-id="73a75-132">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="73a75-133">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="73a75-133">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a><span data-ttu-id="73a75-134">Base64</span><span class="sxs-lookup"><span data-stu-id="73a75-134">base64</span></span>
`base64(inputString)`

<span data-ttu-id="73a75-135">Returnerar hello base64-representation av hello Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="73a75-135">Returns hello base64 representation of hello input string.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-136">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-136">Parameters</span></span>

| <span data-ttu-id="73a75-137">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-137">Parameter</span></span> | <span data-ttu-id="73a75-138">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-138">Required</span></span> | <span data-ttu-id="73a75-139">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-139">Type</span></span> | <span data-ttu-id="73a75-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-140">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-141">inputString</span><span class="sxs-lookup"><span data-stu-id="73a75-141">inputString</span></span> |<span data-ttu-id="73a75-142">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-142">Yes</span></span> |<span data-ttu-id="73a75-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-143">string</span></span> |<span data-ttu-id="73a75-144">hello värdet tooreturn som en base64-representation.</span><span class="sxs-lookup"><span data-stu-id="73a75-144">hello value tooreturn as a base64 representation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-145">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-145">Return value</span></span>

<span data-ttu-id="73a75-146">En sträng som innehåller hello base64-representation.</span><span class="sxs-lookup"><span data-stu-id="73a75-146">A string containing hello base64 representation.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-147">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-147">Examples</span></span>

<span data-ttu-id="73a75-148">hello som följande exempel visar hur toouse hello base64-funktionen.</span><span class="sxs-lookup"><span data-stu-id="73a75-148">hello following example shows how toouse hello base64 function.</span></span>

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

<span data-ttu-id="73a75-149">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-149">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-150">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-150">Name</span></span> | <span data-ttu-id="73a75-151">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-151">Type</span></span> | <span data-ttu-id="73a75-152">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-153">base64Output</span><span class="sxs-lookup"><span data-stu-id="73a75-153">base64Output</span></span> | <span data-ttu-id="73a75-154">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-154">String</span></span> | <span data-ttu-id="73a75-155">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="73a75-155">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="73a75-156">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-156">toStringOutput</span></span> | <span data-ttu-id="73a75-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-157">String</span></span> | <span data-ttu-id="73a75-158">Ett två tre</span><span class="sxs-lookup"><span data-stu-id="73a75-158">one, two, three</span></span> |
| <span data-ttu-id="73a75-159">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-159">toJsonOutput</span></span> | <span data-ttu-id="73a75-160">Objekt</span><span class="sxs-lookup"><span data-stu-id="73a75-160">Object</span></span> | <span data-ttu-id="73a75-161">{”1”: ”a”, ”två”: ”b”}</span><span class="sxs-lookup"><span data-stu-id="73a75-161">{"one": "a", "two": "b"}</span></span> |

<a id="base64tojson" />

## <a name="base64tojson"></a><span data-ttu-id="73a75-162">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="73a75-162">base64ToJson</span></span>
`base64tojson`

<span data-ttu-id="73a75-163">Konverterar ett base64-representation tooa JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="73a75-163">Converts a base64 representation tooa JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-164">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-164">Parameters</span></span>

| <span data-ttu-id="73a75-165">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-165">Parameter</span></span> | <span data-ttu-id="73a75-166">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-166">Required</span></span> | <span data-ttu-id="73a75-167">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-167">Type</span></span> | <span data-ttu-id="73a75-168">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-168">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-169">base64Value</span><span class="sxs-lookup"><span data-stu-id="73a75-169">base64Value</span></span> |<span data-ttu-id="73a75-170">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-170">Yes</span></span> |<span data-ttu-id="73a75-171">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-171">string</span></span> |<span data-ttu-id="73a75-172">hello base64-representation tooconvert tooa JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="73a75-172">hello base64 representation tooconvert tooa JSON object.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-173">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-173">Return value</span></span>

<span data-ttu-id="73a75-174">Ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="73a75-174">A JSON object.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-175">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-175">Examples</span></span>

<span data-ttu-id="73a75-176">hello används följande exempel hello base64ToJson funktionen tooconvert base64-värde:</span><span class="sxs-lookup"><span data-stu-id="73a75-176">hello following example uses hello base64ToJson function tooconvert a base64 value:</span></span>

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

<span data-ttu-id="73a75-177">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-177">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-178">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-178">Name</span></span> | <span data-ttu-id="73a75-179">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-179">Type</span></span> | <span data-ttu-id="73a75-180">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-180">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-181">base64Output</span><span class="sxs-lookup"><span data-stu-id="73a75-181">base64Output</span></span> | <span data-ttu-id="73a75-182">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-182">String</span></span> | <span data-ttu-id="73a75-183">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="73a75-183">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="73a75-184">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-184">toStringOutput</span></span> | <span data-ttu-id="73a75-185">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-185">String</span></span> | <span data-ttu-id="73a75-186">Ett två tre</span><span class="sxs-lookup"><span data-stu-id="73a75-186">one, two, three</span></span> |
| <span data-ttu-id="73a75-187">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-187">toJsonOutput</span></span> | <span data-ttu-id="73a75-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="73a75-188">Object</span></span> | <span data-ttu-id="73a75-189">{”1”: ”a”, ”två”: ”b”}</span><span class="sxs-lookup"><span data-stu-id="73a75-189">{"one": "a", "two": "b"}</span></span> |

<a id="base64tostring" />

## <a name="base64tostring"></a><span data-ttu-id="73a75-190">base64ToString</span><span class="sxs-lookup"><span data-stu-id="73a75-190">base64ToString</span></span>
`base64ToString(base64Value)`

<span data-ttu-id="73a75-191">Konverterar en base64-representation tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-191">Converts a base64 representation tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-192">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-192">Parameters</span></span>

| <span data-ttu-id="73a75-193">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-193">Parameter</span></span> | <span data-ttu-id="73a75-194">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-194">Required</span></span> | <span data-ttu-id="73a75-195">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-195">Type</span></span> | <span data-ttu-id="73a75-196">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-196">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-197">base64Value</span><span class="sxs-lookup"><span data-stu-id="73a75-197">base64Value</span></span> |<span data-ttu-id="73a75-198">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-198">Yes</span></span> |<span data-ttu-id="73a75-199">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-199">string</span></span> |<span data-ttu-id="73a75-200">hello base64-representation tooconvert tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-200">hello base64 representation tooconvert tooa string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-201">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-201">Return value</span></span>

<span data-ttu-id="73a75-202">En sträng med hello konverteras base64-värde.</span><span class="sxs-lookup"><span data-stu-id="73a75-202">A string of hello converted base64 value.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-203">Examples</span></span>

<span data-ttu-id="73a75-204">hello används följande exempel hello base64ToString funktionen tooconvert base64-värde:</span><span class="sxs-lookup"><span data-stu-id="73a75-204">hello following example uses hello base64ToString function tooconvert a base64 value:</span></span>

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

<span data-ttu-id="73a75-205">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-205">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-206">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-206">Name</span></span> | <span data-ttu-id="73a75-207">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-207">Type</span></span> | <span data-ttu-id="73a75-208">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-208">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-209">base64Output</span><span class="sxs-lookup"><span data-stu-id="73a75-209">base64Output</span></span> | <span data-ttu-id="73a75-210">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-210">String</span></span> | <span data-ttu-id="73a75-211">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="73a75-211">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="73a75-212">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-212">toStringOutput</span></span> | <span data-ttu-id="73a75-213">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-213">String</span></span> | <span data-ttu-id="73a75-214">Ett två tre</span><span class="sxs-lookup"><span data-stu-id="73a75-214">one, two, three</span></span> |
| <span data-ttu-id="73a75-215">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-215">toJsonOutput</span></span> | <span data-ttu-id="73a75-216">Objekt</span><span class="sxs-lookup"><span data-stu-id="73a75-216">Object</span></span> | <span data-ttu-id="73a75-217">{”1”: ”a”, ”två”: ”b”}</span><span class="sxs-lookup"><span data-stu-id="73a75-217">{"one": "a", "two": "b"}</span></span> |



<a id="concat" />

## <a name="concat"></a><span data-ttu-id="73a75-218">concat</span><span class="sxs-lookup"><span data-stu-id="73a75-218">concat</span></span>
`concat (arg1, arg2, arg3, ...)`

<span data-ttu-id="73a75-219">Kombinerar flera strängvärden och returnerar hello sammanfogas sträng eller kombinerar flera matriser och returnerar hello sammanfogas matris.</span><span class="sxs-lookup"><span data-stu-id="73a75-219">Combines multiple string values and returns hello concatenated string, or combines multiple arrays and returns hello concatenated array.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-220">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-220">Parameters</span></span>

| <span data-ttu-id="73a75-221">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-221">Parameter</span></span> | <span data-ttu-id="73a75-222">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-222">Required</span></span> | <span data-ttu-id="73a75-223">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-223">Type</span></span> | <span data-ttu-id="73a75-224">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-224">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-225">arg1</span><span class="sxs-lookup"><span data-stu-id="73a75-225">arg1</span></span> |<span data-ttu-id="73a75-226">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-226">Yes</span></span> |<span data-ttu-id="73a75-227">sträng eller matris</span><span class="sxs-lookup"><span data-stu-id="73a75-227">string or array</span></span> |<span data-ttu-id="73a75-228">hello första värde för sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="73a75-228">hello first value for concatenation.</span></span> |
| <span data-ttu-id="73a75-229">ytterligare argument</span><span class="sxs-lookup"><span data-stu-id="73a75-229">additional arguments</span></span> |<span data-ttu-id="73a75-230">Nej</span><span class="sxs-lookup"><span data-stu-id="73a75-230">No</span></span> |<span data-ttu-id="73a75-231">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-231">string</span></span> |<span data-ttu-id="73a75-232">Ytterligare värden i tur och ordning för sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="73a75-232">Additional values in sequential order for concatenation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-233">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-233">Return value</span></span>
<span data-ttu-id="73a75-234">En sträng eller en matris med sammanfogade värdena.</span><span class="sxs-lookup"><span data-stu-id="73a75-234">A string or array of concatenated values.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-235">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-235">Examples</span></span>

<span data-ttu-id="73a75-236">hello som följande exempel visar hur toocombine två sträng värden och returnerar en sammanfogad sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-236">hello following example shows how toocombine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="73a75-237">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-237">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-238">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-238">Name</span></span> | <span data-ttu-id="73a75-239">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-239">Type</span></span> | <span data-ttu-id="73a75-240">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-240">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-241">concatOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-241">concatOutput</span></span> | <span data-ttu-id="73a75-242">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-242">String</span></span> | <span data-ttu-id="73a75-243">prefixet 5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="73a75-243">prefix-5yj4yjf5mbg72</span></span> |

<span data-ttu-id="73a75-244">hello som följande exempel visar hur toocombine två matriser.</span><span class="sxs-lookup"><span data-stu-id="73a75-244">hello following example shows how toocombine two arrays.</span></span>

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

<span data-ttu-id="73a75-245">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-245">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-246">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-246">Name</span></span> | <span data-ttu-id="73a75-247">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-247">Type</span></span> | <span data-ttu-id="73a75-248">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-249">Returnera</span><span class="sxs-lookup"><span data-stu-id="73a75-249">return</span></span> | <span data-ttu-id="73a75-250">Matris</span><span class="sxs-lookup"><span data-stu-id="73a75-250">Array</span></span> | <span data-ttu-id="73a75-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="73a75-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="73a75-252">Innehåller</span><span class="sxs-lookup"><span data-stu-id="73a75-252">contains</span></span>
`contains (container, itemToFind)`

<span data-ttu-id="73a75-253">Kontrollerar om en matris som innehåller ett värde, ett objekt som innehåller en nyckel eller en sträng som innehåller understrängen.</span><span class="sxs-lookup"><span data-stu-id="73a75-253">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-254">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-254">Parameters</span></span>

| <span data-ttu-id="73a75-255">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-255">Parameter</span></span> | <span data-ttu-id="73a75-256">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-256">Required</span></span> | <span data-ttu-id="73a75-257">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-257">Type</span></span> | <span data-ttu-id="73a75-258">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-258">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-259">Behållaren</span><span class="sxs-lookup"><span data-stu-id="73a75-259">container</span></span> |<span data-ttu-id="73a75-260">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-260">Yes</span></span> |<span data-ttu-id="73a75-261">matris, objekt eller sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-261">array, object, or string</span></span> |<span data-ttu-id="73a75-262">hello-värde som innehåller hello värdet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-262">hello value that contains hello value toofind.</span></span> |
| <span data-ttu-id="73a75-263">itemToFind</span><span class="sxs-lookup"><span data-stu-id="73a75-263">itemToFind</span></span> |<span data-ttu-id="73a75-264">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-264">Yes</span></span> |<span data-ttu-id="73a75-265">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="73a75-265">string or int</span></span> |<span data-ttu-id="73a75-266">hello värdet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-266">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-267">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-267">Return value</span></span>

<span data-ttu-id="73a75-268">**SANT** om hello objekt hittades, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="73a75-268">**True** if hello item is found; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-269">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-269">Examples</span></span>

<span data-ttu-id="73a75-270">hello följande exempel visas hur toouse innehåller med olika typer:</span><span class="sxs-lookup"><span data-stu-id="73a75-270">hello following example shows how toouse contains with different types:</span></span>

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

<span data-ttu-id="73a75-271">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-271">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-272">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-272">Name</span></span> | <span data-ttu-id="73a75-273">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-273">Type</span></span> | <span data-ttu-id="73a75-274">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-274">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-275">stringTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-275">stringTrue</span></span> | <span data-ttu-id="73a75-276">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-276">Bool</span></span> | <span data-ttu-id="73a75-277">True</span><span class="sxs-lookup"><span data-stu-id="73a75-277">True</span></span> |
| <span data-ttu-id="73a75-278">stringFalse</span><span class="sxs-lookup"><span data-stu-id="73a75-278">stringFalse</span></span> | <span data-ttu-id="73a75-279">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-279">Bool</span></span> | <span data-ttu-id="73a75-280">False</span><span class="sxs-lookup"><span data-stu-id="73a75-280">False</span></span> |
| <span data-ttu-id="73a75-281">objectTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-281">objectTrue</span></span> | <span data-ttu-id="73a75-282">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-282">Bool</span></span> | <span data-ttu-id="73a75-283">True</span><span class="sxs-lookup"><span data-stu-id="73a75-283">True</span></span> |
| <span data-ttu-id="73a75-284">objectFalse</span><span class="sxs-lookup"><span data-stu-id="73a75-284">objectFalse</span></span> | <span data-ttu-id="73a75-285">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-285">Bool</span></span> | <span data-ttu-id="73a75-286">False</span><span class="sxs-lookup"><span data-stu-id="73a75-286">False</span></span> |
| <span data-ttu-id="73a75-287">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-287">arrayTrue</span></span> | <span data-ttu-id="73a75-288">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-288">Bool</span></span> | <span data-ttu-id="73a75-289">True</span><span class="sxs-lookup"><span data-stu-id="73a75-289">True</span></span> |
| <span data-ttu-id="73a75-290">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="73a75-290">arrayFalse</span></span> | <span data-ttu-id="73a75-291">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-291">Bool</span></span> | <span data-ttu-id="73a75-292">False</span><span class="sxs-lookup"><span data-stu-id="73a75-292">False</span></span> |

<a id="datauri" />

## <a name="datauri"></a><span data-ttu-id="73a75-293">dataUri</span><span class="sxs-lookup"><span data-stu-id="73a75-293">dataUri</span></span>
`dataUri(stringToConvert)`

<span data-ttu-id="73a75-294">Konverterar en tooa värdedata URI.</span><span class="sxs-lookup"><span data-stu-id="73a75-294">Converts a value tooa data URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-295">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-295">Parameters</span></span>

| <span data-ttu-id="73a75-296">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-296">Parameter</span></span> | <span data-ttu-id="73a75-297">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-297">Required</span></span> | <span data-ttu-id="73a75-298">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-298">Type</span></span> | <span data-ttu-id="73a75-299">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-299">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-300">stringToConvert</span><span class="sxs-lookup"><span data-stu-id="73a75-300">stringToConvert</span></span> |<span data-ttu-id="73a75-301">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-301">Yes</span></span> |<span data-ttu-id="73a75-302">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-302">string</span></span> |<span data-ttu-id="73a75-303">Hej tooconvert tooa värdedata URI.</span><span class="sxs-lookup"><span data-stu-id="73a75-303">hello value tooconvert tooa data URI.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-304">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-304">Return value</span></span>

<span data-ttu-id="73a75-305">En sträng formaterad som en data-URI.</span><span class="sxs-lookup"><span data-stu-id="73a75-305">A string formatted as a data URI.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-306">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-306">Examples</span></span>

<span data-ttu-id="73a75-307">hello följande exempel konverterar en tooa värdedata URI och konverterar data URI tooa sträng:</span><span class="sxs-lookup"><span data-stu-id="73a75-307">hello following example converts a value tooa data URI, and converts a data URI tooa string:</span></span>

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

<span data-ttu-id="73a75-308">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-308">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-309">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-309">Name</span></span> | <span data-ttu-id="73a75-310">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-310">Type</span></span> | <span data-ttu-id="73a75-311">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-311">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-312">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-312">dataUriOutput</span></span> | <span data-ttu-id="73a75-313">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-313">String</span></span> | <span data-ttu-id="73a75-314">data: text / oformaterad; charset = utf8; base64 SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="73a75-314">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="73a75-315">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-315">toStringOutput</span></span> | <span data-ttu-id="73a75-316">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-316">String</span></span> | <span data-ttu-id="73a75-317">Hej världen!</span><span class="sxs-lookup"><span data-stu-id="73a75-317">Hello, World!</span></span> |

<a id="datauritostring" />

## <a name="datauritostring"></a><span data-ttu-id="73a75-318">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="73a75-318">dataUriToString</span></span>
`dataUriToString(dataUriToConvert)`

<span data-ttu-id="73a75-319">Konverterar ett data-URI formaterade värdet tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-319">Converts a data URI formatted value tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-320">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-320">Parameters</span></span>

| <span data-ttu-id="73a75-321">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-321">Parameter</span></span> | <span data-ttu-id="73a75-322">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-322">Required</span></span> | <span data-ttu-id="73a75-323">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-323">Type</span></span> | <span data-ttu-id="73a75-324">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-324">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-325">dataUriToConvert</span><span class="sxs-lookup"><span data-stu-id="73a75-325">dataUriToConvert</span></span> |<span data-ttu-id="73a75-326">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-326">Yes</span></span> |<span data-ttu-id="73a75-327">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-327">string</span></span> |<span data-ttu-id="73a75-328">hello data tooconvert för URI-värdet.</span><span class="sxs-lookup"><span data-stu-id="73a75-328">hello data URI value tooconvert.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-329">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-329">Return value</span></span>

<span data-ttu-id="73a75-330">En sträng som innehåller hello konverteras värdet.</span><span class="sxs-lookup"><span data-stu-id="73a75-330">A string containing hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-331">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-331">Examples</span></span>

<span data-ttu-id="73a75-332">hello följande exempel konverterar en tooa värdedata URI och konverterar data URI tooa sträng:</span><span class="sxs-lookup"><span data-stu-id="73a75-332">hello following example converts a value tooa data URI, and converts a data URI tooa string:</span></span>

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

<span data-ttu-id="73a75-333">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-333">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-334">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-334">Name</span></span> | <span data-ttu-id="73a75-335">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-335">Type</span></span> | <span data-ttu-id="73a75-336">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-336">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-337">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-337">dataUriOutput</span></span> | <span data-ttu-id="73a75-338">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-338">String</span></span> | <span data-ttu-id="73a75-339">data: text / oformaterad; charset = utf8; base64 SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="73a75-339">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="73a75-340">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-340">toStringOutput</span></span> | <span data-ttu-id="73a75-341">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-341">String</span></span> | <span data-ttu-id="73a75-342">Hej världen!</span><span class="sxs-lookup"><span data-stu-id="73a75-342">Hello, World!</span></span> |

<a id="empty" /> 

## <a name="empty"></a><span data-ttu-id="73a75-343">tom</span><span class="sxs-lookup"><span data-stu-id="73a75-343">empty</span></span>
`empty(itemToTest)`

<span data-ttu-id="73a75-344">Anger om en matris, objekt eller sträng är tom.</span><span class="sxs-lookup"><span data-stu-id="73a75-344">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-345">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-345">Parameters</span></span>

| <span data-ttu-id="73a75-346">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-346">Parameter</span></span> | <span data-ttu-id="73a75-347">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-347">Required</span></span> | <span data-ttu-id="73a75-348">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-348">Type</span></span> | <span data-ttu-id="73a75-349">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-349">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-350">itemToTest</span><span class="sxs-lookup"><span data-stu-id="73a75-350">itemToTest</span></span> |<span data-ttu-id="73a75-351">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-351">Yes</span></span> |<span data-ttu-id="73a75-352">matris, objekt eller sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-352">array, object, or string</span></span> |<span data-ttu-id="73a75-353">Hej värdet toocheck om den är tom.</span><span class="sxs-lookup"><span data-stu-id="73a75-353">hello value toocheck if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-354">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-354">Return value</span></span>

<span data-ttu-id="73a75-355">Returnerar **SANT** om hello-värdet är tomt, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="73a75-355">Returns **True** if hello value is empty; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-356">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-356">Examples</span></span>

<span data-ttu-id="73a75-357">följande exempel hello kontrollerar om en matris och objektet sträng är tom.</span><span class="sxs-lookup"><span data-stu-id="73a75-357">hello following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="73a75-358">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-358">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-359">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-359">Name</span></span> | <span data-ttu-id="73a75-360">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-360">Type</span></span> | <span data-ttu-id="73a75-361">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-361">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-362">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="73a75-362">arrayEmpty</span></span> | <span data-ttu-id="73a75-363">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-363">Bool</span></span> | <span data-ttu-id="73a75-364">True</span><span class="sxs-lookup"><span data-stu-id="73a75-364">True</span></span> |
| <span data-ttu-id="73a75-365">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="73a75-365">objectEmpty</span></span> | <span data-ttu-id="73a75-366">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-366">Bool</span></span> | <span data-ttu-id="73a75-367">True</span><span class="sxs-lookup"><span data-stu-id="73a75-367">True</span></span> |
| <span data-ttu-id="73a75-368">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="73a75-368">stringEmpty</span></span> | <span data-ttu-id="73a75-369">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-369">Bool</span></span> | <span data-ttu-id="73a75-370">True</span><span class="sxs-lookup"><span data-stu-id="73a75-370">True</span></span> |

<a id="endswith" />

## <a name="endswith"></a><span data-ttu-id="73a75-371">endsWith</span><span class="sxs-lookup"><span data-stu-id="73a75-371">endsWith</span></span>
`endsWith(stringToSearch, stringToFind)`

<span data-ttu-id="73a75-372">Anger om en sträng som slutar med ett värde.</span><span class="sxs-lookup"><span data-stu-id="73a75-372">Determines whether a string ends with a value.</span></span> <span data-ttu-id="73a75-373">hello jämförelse är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="73a75-373">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-374">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-374">Parameters</span></span>

| <span data-ttu-id="73a75-375">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-375">Parameter</span></span> | <span data-ttu-id="73a75-376">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-376">Required</span></span> | <span data-ttu-id="73a75-377">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-377">Type</span></span> | <span data-ttu-id="73a75-378">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-378">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-379">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="73a75-379">stringToSearch</span></span> |<span data-ttu-id="73a75-380">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-380">Yes</span></span> |<span data-ttu-id="73a75-381">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-381">string</span></span> |<span data-ttu-id="73a75-382">hello-värde som innehåller hello objektet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-382">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="73a75-383">stringToFind</span><span class="sxs-lookup"><span data-stu-id="73a75-383">stringToFind</span></span> |<span data-ttu-id="73a75-384">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-384">Yes</span></span> |<span data-ttu-id="73a75-385">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-385">string</span></span> |<span data-ttu-id="73a75-386">hello värdet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-386">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-387">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-387">Return value</span></span>

<span data-ttu-id="73a75-388">**SANT** om hello sista tecknet eller tecknen i hello strängen matchar hello värdet; annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="73a75-388">**True** if hello last character or characters of hello string match hello value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-389">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-389">Examples</span></span>

<span data-ttu-id="73a75-390">hello som följande exempel visar hur toouse hello startsWith och endsWith-funktioner:</span><span class="sxs-lookup"><span data-stu-id="73a75-390">hello following example shows how toouse hello startsWith and endsWith functions:</span></span>

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

<span data-ttu-id="73a75-391">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-391">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-392">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-392">Name</span></span> | <span data-ttu-id="73a75-393">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-393">Type</span></span> | <span data-ttu-id="73a75-394">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-394">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-395">startsTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-395">startsTrue</span></span> | <span data-ttu-id="73a75-396">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-396">Bool</span></span> | <span data-ttu-id="73a75-397">True</span><span class="sxs-lookup"><span data-stu-id="73a75-397">True</span></span> |
| <span data-ttu-id="73a75-398">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-398">startsCapTrue</span></span> | <span data-ttu-id="73a75-399">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-399">Bool</span></span> | <span data-ttu-id="73a75-400">True</span><span class="sxs-lookup"><span data-stu-id="73a75-400">True</span></span> |
| <span data-ttu-id="73a75-401">startsFalse</span><span class="sxs-lookup"><span data-stu-id="73a75-401">startsFalse</span></span> | <span data-ttu-id="73a75-402">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-402">Bool</span></span> | <span data-ttu-id="73a75-403">False</span><span class="sxs-lookup"><span data-stu-id="73a75-403">False</span></span> |
| <span data-ttu-id="73a75-404">endsTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-404">endsTrue</span></span> | <span data-ttu-id="73a75-405">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-405">Bool</span></span> | <span data-ttu-id="73a75-406">True</span><span class="sxs-lookup"><span data-stu-id="73a75-406">True</span></span> |
| <span data-ttu-id="73a75-407">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-407">endsCapTrue</span></span> | <span data-ttu-id="73a75-408">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-408">Bool</span></span> | <span data-ttu-id="73a75-409">True</span><span class="sxs-lookup"><span data-stu-id="73a75-409">True</span></span> |
| <span data-ttu-id="73a75-410">endsFalse</span><span class="sxs-lookup"><span data-stu-id="73a75-410">endsFalse</span></span> | <span data-ttu-id="73a75-411">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-411">Bool</span></span> | <span data-ttu-id="73a75-412">False</span><span class="sxs-lookup"><span data-stu-id="73a75-412">False</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="73a75-413">första</span><span class="sxs-lookup"><span data-stu-id="73a75-413">first</span></span>
`first(arg1)`

<span data-ttu-id="73a75-414">Returnerar hello första tecknet i hello sträng eller första elementet i matrisen hello.</span><span class="sxs-lookup"><span data-stu-id="73a75-414">Returns hello first character of hello string, or first element of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-415">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-415">Parameters</span></span>

| <span data-ttu-id="73a75-416">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-416">Parameter</span></span> | <span data-ttu-id="73a75-417">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-417">Required</span></span> | <span data-ttu-id="73a75-418">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-418">Type</span></span> | <span data-ttu-id="73a75-419">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-420">arg1</span><span class="sxs-lookup"><span data-stu-id="73a75-420">arg1</span></span> |<span data-ttu-id="73a75-421">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-421">Yes</span></span> |<span data-ttu-id="73a75-422">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-422">array or string</span></span> |<span data-ttu-id="73a75-423">hello värdet tooretrieve hello första elementet eller tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-423">hello value tooretrieve hello first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-424">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-424">Return value</span></span>

<span data-ttu-id="73a75-425">En sträng med hello första tecken eller hello typ (sträng, int, matris eller objekt) för hello första element i en matris.</span><span class="sxs-lookup"><span data-stu-id="73a75-425">A string of hello first character, or hello type (string, int, array, or object) of hello first element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-426">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-426">Examples</span></span>

<span data-ttu-id="73a75-427">hello följande exempel visas hur toouse hello första funktion med en matris och en sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-427">hello following example shows how toouse hello first function with an array and string.</span></span>

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

<span data-ttu-id="73a75-428">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-428">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-429">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-429">Name</span></span> | <span data-ttu-id="73a75-430">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-430">Type</span></span> | <span data-ttu-id="73a75-431">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-432">arrayOutput</span></span> | <span data-ttu-id="73a75-433">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-433">String</span></span> | <span data-ttu-id="73a75-434">en</span><span class="sxs-lookup"><span data-stu-id="73a75-434">one</span></span> |
| <span data-ttu-id="73a75-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-435">stringOutput</span></span> | <span data-ttu-id="73a75-436">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-436">String</span></span> | <span data-ttu-id="73a75-437">O</span><span class="sxs-lookup"><span data-stu-id="73a75-437">O</span></span> |

<a id="indexof" />

## <a name="indexof"></a><span data-ttu-id="73a75-438">indexOf</span><span class="sxs-lookup"><span data-stu-id="73a75-438">indexOf</span></span>
`indexOf(stringToSearch, stringToFind)`

<span data-ttu-id="73a75-439">Returnerar hello första positionen för ett värde inom en sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-439">Returns hello first position of a value within a string.</span></span> <span data-ttu-id="73a75-440">hello jämförelse är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="73a75-440">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-441">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-441">Parameters</span></span>

| <span data-ttu-id="73a75-442">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-442">Parameter</span></span> | <span data-ttu-id="73a75-443">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-443">Required</span></span> | <span data-ttu-id="73a75-444">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-444">Type</span></span> | <span data-ttu-id="73a75-445">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-445">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-446">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="73a75-446">stringToSearch</span></span> |<span data-ttu-id="73a75-447">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-447">Yes</span></span> |<span data-ttu-id="73a75-448">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-448">string</span></span> |<span data-ttu-id="73a75-449">hello-värde som innehåller hello objektet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-449">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="73a75-450">stringToFind</span><span class="sxs-lookup"><span data-stu-id="73a75-450">stringToFind</span></span> |<span data-ttu-id="73a75-451">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-451">Yes</span></span> |<span data-ttu-id="73a75-452">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-452">string</span></span> |<span data-ttu-id="73a75-453">hello värdet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-453">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-454">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-454">Return value</span></span>

<span data-ttu-id="73a75-455">Ett heltal som representerar hello position i hello objektet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-455">An integer that represents hello position of hello item toofind.</span></span> <span data-ttu-id="73a75-456">hello-värdet är nollbaserade.</span><span class="sxs-lookup"><span data-stu-id="73a75-456">hello value is zero-based.</span></span> <span data-ttu-id="73a75-457">Om hello objektet inte hittades, returneras -1.</span><span class="sxs-lookup"><span data-stu-id="73a75-457">If hello item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-458">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-458">Examples</span></span>

<span data-ttu-id="73a75-459">hello som följande exempel visar hur toouse hello indexOf och lastIndexOf funktioner:</span><span class="sxs-lookup"><span data-stu-id="73a75-459">hello following example shows how toouse hello indexOf and lastIndexOf functions:</span></span>

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

<span data-ttu-id="73a75-460">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-460">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-461">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-461">Name</span></span> | <span data-ttu-id="73a75-462">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-462">Type</span></span> | <span data-ttu-id="73a75-463">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-463">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-464">firstT</span><span class="sxs-lookup"><span data-stu-id="73a75-464">firstT</span></span> | <span data-ttu-id="73a75-465">int</span><span class="sxs-lookup"><span data-stu-id="73a75-465">Int</span></span> | <span data-ttu-id="73a75-466">0</span><span class="sxs-lookup"><span data-stu-id="73a75-466">0</span></span> |
| <span data-ttu-id="73a75-467">lastT</span><span class="sxs-lookup"><span data-stu-id="73a75-467">lastT</span></span> | <span data-ttu-id="73a75-468">int</span><span class="sxs-lookup"><span data-stu-id="73a75-468">Int</span></span> | <span data-ttu-id="73a75-469">3</span><span class="sxs-lookup"><span data-stu-id="73a75-469">3</span></span> |
| <span data-ttu-id="73a75-470">firstString</span><span class="sxs-lookup"><span data-stu-id="73a75-470">firstString</span></span> | <span data-ttu-id="73a75-471">int</span><span class="sxs-lookup"><span data-stu-id="73a75-471">Int</span></span> | <span data-ttu-id="73a75-472">2</span><span class="sxs-lookup"><span data-stu-id="73a75-472">2</span></span> |
| <span data-ttu-id="73a75-473">lastString</span><span class="sxs-lookup"><span data-stu-id="73a75-473">lastString</span></span> | <span data-ttu-id="73a75-474">int</span><span class="sxs-lookup"><span data-stu-id="73a75-474">Int</span></span> | <span data-ttu-id="73a75-475">0</span><span class="sxs-lookup"><span data-stu-id="73a75-475">0</span></span> |
| <span data-ttu-id="73a75-476">notFound</span><span class="sxs-lookup"><span data-stu-id="73a75-476">notFound</span></span> | <span data-ttu-id="73a75-477">int</span><span class="sxs-lookup"><span data-stu-id="73a75-477">Int</span></span> | <span data-ttu-id="73a75-478">-1</span><span class="sxs-lookup"><span data-stu-id="73a75-478">-1</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="73a75-479">senaste</span><span class="sxs-lookup"><span data-stu-id="73a75-479">last</span></span>
`last (arg1)`

<span data-ttu-id="73a75-480">Returnerar det sista tecknet i hello strängen eller hello sista elementet i matrisen hello.</span><span class="sxs-lookup"><span data-stu-id="73a75-480">Returns last character of hello string, or hello last element of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-481">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-481">Parameters</span></span>

| <span data-ttu-id="73a75-482">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-482">Parameter</span></span> | <span data-ttu-id="73a75-483">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-483">Required</span></span> | <span data-ttu-id="73a75-484">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-484">Type</span></span> | <span data-ttu-id="73a75-485">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-485">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-486">arg1</span><span class="sxs-lookup"><span data-stu-id="73a75-486">arg1</span></span> |<span data-ttu-id="73a75-487">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-487">Yes</span></span> |<span data-ttu-id="73a75-488">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-488">array or string</span></span> |<span data-ttu-id="73a75-489">hello värdet tooretrieve hello sista elementet eller tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-489">hello value tooretrieve hello last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-490">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-490">Return value</span></span>

<span data-ttu-id="73a75-491">En sträng med hello sista tecknet eller hello typ (sträng, int, matris eller objekt) av hello sista elementet i en matris.</span><span class="sxs-lookup"><span data-stu-id="73a75-491">A string of hello last character, or hello type (string, int, array, or object) of hello last element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-492">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-492">Examples</span></span>

<span data-ttu-id="73a75-493">hello följande exempel visas hur toouse hello senaste funktion med en matris och en sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-493">hello following example shows how toouse hello last function with an array and string.</span></span>

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

<span data-ttu-id="73a75-494">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-494">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-495">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-495">Name</span></span> | <span data-ttu-id="73a75-496">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-496">Type</span></span> | <span data-ttu-id="73a75-497">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-497">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-498">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-498">arrayOutput</span></span> | <span data-ttu-id="73a75-499">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-499">String</span></span> | <span data-ttu-id="73a75-500">tre</span><span class="sxs-lookup"><span data-stu-id="73a75-500">three</span></span> |
| <span data-ttu-id="73a75-501">stringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-501">stringOutput</span></span> | <span data-ttu-id="73a75-502">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-502">String</span></span> | <span data-ttu-id="73a75-503">E</span><span class="sxs-lookup"><span data-stu-id="73a75-503">e</span></span> |

<a id="lastindexof" />

## <a name="lastindexof"></a><span data-ttu-id="73a75-504">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="73a75-504">lastIndexOf</span></span>
`lastIndexOf(stringToSearch, stringToFind)`

<span data-ttu-id="73a75-505">Returnerar hello sista positionen för ett värde inom en sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-505">Returns hello last position of a value within a string.</span></span> <span data-ttu-id="73a75-506">hello jämförelse är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="73a75-506">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-507">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-507">Parameters</span></span>

| <span data-ttu-id="73a75-508">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-508">Parameter</span></span> | <span data-ttu-id="73a75-509">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-509">Required</span></span> | <span data-ttu-id="73a75-510">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-510">Type</span></span> | <span data-ttu-id="73a75-511">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-511">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-512">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="73a75-512">stringToSearch</span></span> |<span data-ttu-id="73a75-513">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-513">Yes</span></span> |<span data-ttu-id="73a75-514">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-514">string</span></span> |<span data-ttu-id="73a75-515">hello-värde som innehåller hello objektet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-515">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="73a75-516">stringToFind</span><span class="sxs-lookup"><span data-stu-id="73a75-516">stringToFind</span></span> |<span data-ttu-id="73a75-517">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-517">Yes</span></span> |<span data-ttu-id="73a75-518">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-518">string</span></span> |<span data-ttu-id="73a75-519">hello värdet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-519">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-520">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-520">Return value</span></span>

<span data-ttu-id="73a75-521">Ett heltal som representerar hello senaste position i hello objektet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-521">An integer that represents hello last position of hello item toofind.</span></span> <span data-ttu-id="73a75-522">hello-värdet är nollbaserade.</span><span class="sxs-lookup"><span data-stu-id="73a75-522">hello value is zero-based.</span></span> <span data-ttu-id="73a75-523">Om hello objektet inte hittades, returneras -1.</span><span class="sxs-lookup"><span data-stu-id="73a75-523">If hello item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-524">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-524">Examples</span></span>

<span data-ttu-id="73a75-525">hello som följande exempel visar hur toouse hello indexOf och lastIndexOf funktioner:</span><span class="sxs-lookup"><span data-stu-id="73a75-525">hello following example shows how toouse hello indexOf and lastIndexOf functions:</span></span>

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

<span data-ttu-id="73a75-526">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-526">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-527">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-527">Name</span></span> | <span data-ttu-id="73a75-528">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-528">Type</span></span> | <span data-ttu-id="73a75-529">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-529">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-530">firstT</span><span class="sxs-lookup"><span data-stu-id="73a75-530">firstT</span></span> | <span data-ttu-id="73a75-531">int</span><span class="sxs-lookup"><span data-stu-id="73a75-531">Int</span></span> | <span data-ttu-id="73a75-532">0</span><span class="sxs-lookup"><span data-stu-id="73a75-532">0</span></span> |
| <span data-ttu-id="73a75-533">lastT</span><span class="sxs-lookup"><span data-stu-id="73a75-533">lastT</span></span> | <span data-ttu-id="73a75-534">int</span><span class="sxs-lookup"><span data-stu-id="73a75-534">Int</span></span> | <span data-ttu-id="73a75-535">3</span><span class="sxs-lookup"><span data-stu-id="73a75-535">3</span></span> |
| <span data-ttu-id="73a75-536">firstString</span><span class="sxs-lookup"><span data-stu-id="73a75-536">firstString</span></span> | <span data-ttu-id="73a75-537">int</span><span class="sxs-lookup"><span data-stu-id="73a75-537">Int</span></span> | <span data-ttu-id="73a75-538">2</span><span class="sxs-lookup"><span data-stu-id="73a75-538">2</span></span> |
| <span data-ttu-id="73a75-539">lastString</span><span class="sxs-lookup"><span data-stu-id="73a75-539">lastString</span></span> | <span data-ttu-id="73a75-540">int</span><span class="sxs-lookup"><span data-stu-id="73a75-540">Int</span></span> | <span data-ttu-id="73a75-541">0</span><span class="sxs-lookup"><span data-stu-id="73a75-541">0</span></span> |
| <span data-ttu-id="73a75-542">notFound</span><span class="sxs-lookup"><span data-stu-id="73a75-542">notFound</span></span> | <span data-ttu-id="73a75-543">int</span><span class="sxs-lookup"><span data-stu-id="73a75-543">Int</span></span> | <span data-ttu-id="73a75-544">-1</span><span class="sxs-lookup"><span data-stu-id="73a75-544">-1</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="73a75-545">Längd</span><span class="sxs-lookup"><span data-stu-id="73a75-545">length</span></span>
`length(string)`

<span data-ttu-id="73a75-546">Returnerar hello antalet tecken i en sträng eller element i en matris.</span><span class="sxs-lookup"><span data-stu-id="73a75-546">Returns hello number of characters in a string, or elements in an array.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-547">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-547">Parameters</span></span>

| <span data-ttu-id="73a75-548">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-548">Parameter</span></span> | <span data-ttu-id="73a75-549">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-549">Required</span></span> | <span data-ttu-id="73a75-550">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-550">Type</span></span> | <span data-ttu-id="73a75-551">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-551">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-552">arg1</span><span class="sxs-lookup"><span data-stu-id="73a75-552">arg1</span></span> |<span data-ttu-id="73a75-553">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-553">Yes</span></span> |<span data-ttu-id="73a75-554">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-554">array or string</span></span> |<span data-ttu-id="73a75-555">Hej matris toouse för att hämta hello antalet element eller hello sträng toouse för att hämta hello antalet tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-555">hello array toouse for getting hello number of elements, or hello string toouse for getting hello number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-556">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-556">Return value</span></span>

<span data-ttu-id="73a75-557">Int.</span><span class="sxs-lookup"><span data-stu-id="73a75-557">An int.</span></span> 

### <a name="examples"></a><span data-ttu-id="73a75-558">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-558">Examples</span></span>

<span data-ttu-id="73a75-559">följande exempel visar hur hello toouse längd med en matris och sträng:</span><span class="sxs-lookup"><span data-stu-id="73a75-559">hello following example shows how toouse length with an array and string:</span></span>

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

<span data-ttu-id="73a75-560">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-560">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-561">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-561">Name</span></span> | <span data-ttu-id="73a75-562">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-562">Type</span></span> | <span data-ttu-id="73a75-563">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-563">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-564">arrayLength</span><span class="sxs-lookup"><span data-stu-id="73a75-564">arrayLength</span></span> | <span data-ttu-id="73a75-565">int</span><span class="sxs-lookup"><span data-stu-id="73a75-565">Int</span></span> | <span data-ttu-id="73a75-566">3</span><span class="sxs-lookup"><span data-stu-id="73a75-566">3</span></span> |
| <span data-ttu-id="73a75-567">stringLength</span><span class="sxs-lookup"><span data-stu-id="73a75-567">stringLength</span></span> | <span data-ttu-id="73a75-568">int</span><span class="sxs-lookup"><span data-stu-id="73a75-568">Int</span></span> | <span data-ttu-id="73a75-569">13</span><span class="sxs-lookup"><span data-stu-id="73a75-569">13</span></span> |

<a id="padleft" />

## <a name="padleft"></a><span data-ttu-id="73a75-570">padLeft</span><span class="sxs-lookup"><span data-stu-id="73a75-570">padLeft</span></span>
`padLeft(valueToPad, totalLength, paddingCharacter)`

<span data-ttu-id="73a75-571">Returnerar en högerjusterad sträng genom att lägga till tecken toohello vänster tills hello totala angiven längd.</span><span class="sxs-lookup"><span data-stu-id="73a75-571">Returns a right-aligned string by adding characters toohello left until reaching hello total specified length.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-572">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-572">Parameters</span></span>

| <span data-ttu-id="73a75-573">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-573">Parameter</span></span> | <span data-ttu-id="73a75-574">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-574">Required</span></span> | <span data-ttu-id="73a75-575">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-575">Type</span></span> | <span data-ttu-id="73a75-576">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-576">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-577">valueToPad</span><span class="sxs-lookup"><span data-stu-id="73a75-577">valueToPad</span></span> |<span data-ttu-id="73a75-578">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-578">Yes</span></span> |<span data-ttu-id="73a75-579">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="73a75-579">string or int</span></span> |<span data-ttu-id="73a75-580">Hej värdet tooright-justera.</span><span class="sxs-lookup"><span data-stu-id="73a75-580">hello value tooright-align.</span></span> |
| <span data-ttu-id="73a75-581">totalLength</span><span class="sxs-lookup"><span data-stu-id="73a75-581">totalLength</span></span> |<span data-ttu-id="73a75-582">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-582">Yes</span></span> |<span data-ttu-id="73a75-583">int</span><span class="sxs-lookup"><span data-stu-id="73a75-583">int</span></span> |<span data-ttu-id="73a75-584">hello totala antalet tecken i hello returnerade sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-584">hello total number of characters in hello returned string.</span></span> |
| <span data-ttu-id="73a75-585">paddingCharacter</span><span class="sxs-lookup"><span data-stu-id="73a75-585">paddingCharacter</span></span> |<span data-ttu-id="73a75-586">Nej</span><span class="sxs-lookup"><span data-stu-id="73a75-586">No</span></span> |<span data-ttu-id="73a75-587">enskilt tecken</span><span class="sxs-lookup"><span data-stu-id="73a75-587">single character</span></span> |<span data-ttu-id="73a75-588">hello tecken toouse för vänster-utfyllnad tills hello totala längden har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="73a75-588">hello character toouse for left-padding until hello total length is reached.</span></span> <span data-ttu-id="73a75-589">hello standardvärdet är ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="73a75-589">hello default value is a space.</span></span> |

<span data-ttu-id="73a75-590">Om ursprungliga hello-strängen är längre än hello antal tecken toopad läggs inga tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-590">If hello original string is longer than hello number of characters toopad, no characters are added.</span></span>

### <a name="return-value"></a><span data-ttu-id="73a75-591">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-591">Return value</span></span>

<span data-ttu-id="73a75-592">En sträng med hello minst antal angivna tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-592">A string with at least hello number of specified characters.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-593">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-593">Examples</span></span>

<span data-ttu-id="73a75-594">hello som följande exempel visar hur toopad hello som användaren tillhandahållit parametervärdet genom att lägga till hello noll tecken tills hello Totalt antal tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-594">hello following example shows how toopad hello user-provided parameter value by adding hello zero character until it reaches hello total number of characters.</span></span> 

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

<span data-ttu-id="73a75-595">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-595">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-596">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-596">Name</span></span> | <span data-ttu-id="73a75-597">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-597">Type</span></span> | <span data-ttu-id="73a75-598">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-598">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-599">stringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-599">stringOutput</span></span> | <span data-ttu-id="73a75-600">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-600">String</span></span> | <span data-ttu-id="73a75-601">0000000123</span><span class="sxs-lookup"><span data-stu-id="73a75-601">0000000123</span></span> |

<a id="replace" />

## <a name="replace"></a><span data-ttu-id="73a75-602">Ersätt</span><span class="sxs-lookup"><span data-stu-id="73a75-602">replace</span></span>
`replace(originalString, oldString, newString)`

<span data-ttu-id="73a75-603">Returnerar en ny sträng med alla instanser av en sträng som har ersatts av en annan sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-603">Returns a new string with all instances of one string replaced by another string.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-604">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-604">Parameters</span></span>

| <span data-ttu-id="73a75-605">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-605">Parameter</span></span> | <span data-ttu-id="73a75-606">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-606">Required</span></span> | <span data-ttu-id="73a75-607">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-607">Type</span></span> | <span data-ttu-id="73a75-608">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-608">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-609">originalString</span><span class="sxs-lookup"><span data-stu-id="73a75-609">originalString</span></span> |<span data-ttu-id="73a75-610">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-610">Yes</span></span> |<span data-ttu-id="73a75-611">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-611">string</span></span> |<span data-ttu-id="73a75-612">hello-värde som har alla instanser av en sträng som har ersatts av en annan sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-612">hello value that has all instances of one string replaced by another string.</span></span> |
| <span data-ttu-id="73a75-613">oldString</span><span class="sxs-lookup"><span data-stu-id="73a75-613">oldString</span></span> |<span data-ttu-id="73a75-614">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-614">Yes</span></span> |<span data-ttu-id="73a75-615">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-615">string</span></span> |<span data-ttu-id="73a75-616">hello sträng toobe tas bort från hello ursprungliga strängen.</span><span class="sxs-lookup"><span data-stu-id="73a75-616">hello string toobe removed from hello original string.</span></span> |
| <span data-ttu-id="73a75-617">newString</span><span class="sxs-lookup"><span data-stu-id="73a75-617">newString</span></span> |<span data-ttu-id="73a75-618">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-618">Yes</span></span> |<span data-ttu-id="73a75-619">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-619">string</span></span> |<span data-ttu-id="73a75-620">hello sträng tooadd i stället för hello bort sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-620">hello string tooadd in place of hello removed string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-621">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-621">Return value</span></span>

<span data-ttu-id="73a75-622">En sträng med hello ersättas tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-622">A string with hello replaced characters.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-623">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-623">Examples</span></span>

<span data-ttu-id="73a75-624">hello som följande exempel visar hur tooremove alla bindestreck från hello som användaren tillhandahållit sträng, och hur tooreplace tillhör hello sträng med en annan sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-624">hello following example shows how tooremove all dashes from hello user-provided string, and how tooreplace part of hello string with another string.</span></span>

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

<span data-ttu-id="73a75-625">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-625">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-626">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-626">Name</span></span> | <span data-ttu-id="73a75-627">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-627">Type</span></span> | <span data-ttu-id="73a75-628">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-628">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-629">firstOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-629">firstOutput</span></span> | <span data-ttu-id="73a75-630">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-630">String</span></span> | <span data-ttu-id="73a75-631">1231231234</span><span class="sxs-lookup"><span data-stu-id="73a75-631">1231231234</span></span> |
| <span data-ttu-id="73a75-632">secodeOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-632">secodeOutput</span></span> | <span data-ttu-id="73a75-633">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-633">String</span></span> | <span data-ttu-id="73a75-634">123-123-xxxx</span><span class="sxs-lookup"><span data-stu-id="73a75-634">123-123-xxxx</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="73a75-635">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="73a75-635">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="73a75-636">Returnerar en sträng med tecken för alla hello efter hello angivet antal tecken, eller en matris med alla hello element när hello angivet antal element.</span><span class="sxs-lookup"><span data-stu-id="73a75-636">Returns a string with all hello characters after hello specified number of characters, or an array with all hello elements after hello specified number of elements.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-637">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-637">Parameters</span></span>

| <span data-ttu-id="73a75-638">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-638">Parameter</span></span> | <span data-ttu-id="73a75-639">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-639">Required</span></span> | <span data-ttu-id="73a75-640">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-640">Type</span></span> | <span data-ttu-id="73a75-641">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-641">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-642">Ursprungligt värde</span><span class="sxs-lookup"><span data-stu-id="73a75-642">originalValue</span></span> |<span data-ttu-id="73a75-643">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-643">Yes</span></span> |<span data-ttu-id="73a75-644">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-644">array or string</span></span> |<span data-ttu-id="73a75-645">Hej array eller string toouse för att hoppa över.</span><span class="sxs-lookup"><span data-stu-id="73a75-645">hello array or string toouse for skipping.</span></span> |
| <span data-ttu-id="73a75-646">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="73a75-646">numberToSkip</span></span> |<span data-ttu-id="73a75-647">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-647">Yes</span></span> |<span data-ttu-id="73a75-648">int</span><span class="sxs-lookup"><span data-stu-id="73a75-648">int</span></span> |<span data-ttu-id="73a75-649">hello antal element eller tecken tooskip.</span><span class="sxs-lookup"><span data-stu-id="73a75-649">hello number of elements or characters tooskip.</span></span> <span data-ttu-id="73a75-650">Om det här värdet är 0 eller mindre, alla hello element eller tecken i hello-värde som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="73a75-650">If this value is 0 or less, all hello elements or characters in hello value are returned.</span></span> <span data-ttu-id="73a75-651">Om den är större än hello längd hello matris eller sträng returneras en tom matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-651">If it is larger than hello length of hello array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-652">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-652">Return value</span></span>

<span data-ttu-id="73a75-653">En matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-653">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-654">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-654">Examples</span></span>

<span data-ttu-id="73a75-655">hello följande exempel hoppar över hello angivna antalet element i matrisen hello och hello angivet antal tecken i en sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-655">hello following example skips hello specified number of elements in hello array, and hello specified number of characters in a string.</span></span>

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

<span data-ttu-id="73a75-656">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-656">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-657">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-657">Name</span></span> | <span data-ttu-id="73a75-658">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-658">Type</span></span> | <span data-ttu-id="73a75-659">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-659">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-660">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-660">arrayOutput</span></span> | <span data-ttu-id="73a75-661">Matris</span><span class="sxs-lookup"><span data-stu-id="73a75-661">Array</span></span> | <span data-ttu-id="73a75-662">[”tre”]</span><span class="sxs-lookup"><span data-stu-id="73a75-662">["three"]</span></span> |
| <span data-ttu-id="73a75-663">stringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-663">stringOutput</span></span> | <span data-ttu-id="73a75-664">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-664">String</span></span> | <span data-ttu-id="73a75-665">två tre</span><span class="sxs-lookup"><span data-stu-id="73a75-665">two three</span></span> |

<a id="split" />

## <a name="split"></a><span data-ttu-id="73a75-666">split</span><span class="sxs-lookup"><span data-stu-id="73a75-666">split</span></span>
`split(inputString, delimiter)`

<span data-ttu-id="73a75-667">Returnerar en matris med strängar som innehåller hello delsträngar i hello Indatasträngen som avgränsas med hello angiven avgränsare.</span><span class="sxs-lookup"><span data-stu-id="73a75-667">Returns an array of strings that contains hello substrings of hello input string that are delimited by hello specified delimiters.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-668">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-668">Parameters</span></span>

| <span data-ttu-id="73a75-669">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-669">Parameter</span></span> | <span data-ttu-id="73a75-670">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-670">Required</span></span> | <span data-ttu-id="73a75-671">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-671">Type</span></span> | <span data-ttu-id="73a75-672">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-672">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-673">inputString</span><span class="sxs-lookup"><span data-stu-id="73a75-673">inputString</span></span> |<span data-ttu-id="73a75-674">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-674">Yes</span></span> |<span data-ttu-id="73a75-675">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-675">string</span></span> |<span data-ttu-id="73a75-676">hello sträng toosplit.</span><span class="sxs-lookup"><span data-stu-id="73a75-676">hello string toosplit.</span></span> |
| <span data-ttu-id="73a75-677">Avgränsare</span><span class="sxs-lookup"><span data-stu-id="73a75-677">delimiter</span></span> |<span data-ttu-id="73a75-678">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-678">Yes</span></span> |<span data-ttu-id="73a75-679">sträng eller strängmatris</span><span class="sxs-lookup"><span data-stu-id="73a75-679">string or array of strings</span></span> |<span data-ttu-id="73a75-680">hello avgränsare toouse för att dela hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-680">hello delimiter toouse for splitting hello string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-681">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-681">Return value</span></span>

<span data-ttu-id="73a75-682">En matris med strängar.</span><span class="sxs-lookup"><span data-stu-id="73a75-682">An array of strings.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-683">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-683">Examples</span></span>

<span data-ttu-id="73a75-684">hello delar följande exempel hello Indatasträngen med kommatecken och med kommatecken eller semikolon.</span><span class="sxs-lookup"><span data-stu-id="73a75-684">hello following example splits hello input string with a comma, and with either a comma or a semi-colon.</span></span>

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

<span data-ttu-id="73a75-685">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-685">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-686">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-686">Name</span></span> | <span data-ttu-id="73a75-687">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-687">Type</span></span> | <span data-ttu-id="73a75-688">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-688">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-689">firstOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-689">firstOutput</span></span> | <span data-ttu-id="73a75-690">Matris</span><span class="sxs-lookup"><span data-stu-id="73a75-690">Array</span></span> | <span data-ttu-id="73a75-691">[”1”, ”två”, ”tre”]</span><span class="sxs-lookup"><span data-stu-id="73a75-691">["one", "two", "three"]</span></span> |
| <span data-ttu-id="73a75-692">secondOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-692">secondOutput</span></span> | <span data-ttu-id="73a75-693">Matris</span><span class="sxs-lookup"><span data-stu-id="73a75-693">Array</span></span> | <span data-ttu-id="73a75-694">[”1”, ”två”, ”tre”]</span><span class="sxs-lookup"><span data-stu-id="73a75-694">["one", "two", "three"]</span></span> |

<a id="startswith" />

## <a name="startswith"></a><span data-ttu-id="73a75-695">StartsWith</span><span class="sxs-lookup"><span data-stu-id="73a75-695">startsWith</span></span>
`startsWith(stringToSearch, stringToFind)`

<span data-ttu-id="73a75-696">Anger om en sträng som börjar med ett värde.</span><span class="sxs-lookup"><span data-stu-id="73a75-696">Determines whether a string starts with a value.</span></span> <span data-ttu-id="73a75-697">hello jämförelse är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="73a75-697">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-698">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-698">Parameters</span></span>

| <span data-ttu-id="73a75-699">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-699">Parameter</span></span> | <span data-ttu-id="73a75-700">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-700">Required</span></span> | <span data-ttu-id="73a75-701">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-701">Type</span></span> | <span data-ttu-id="73a75-702">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-702">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-703">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="73a75-703">stringToSearch</span></span> |<span data-ttu-id="73a75-704">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-704">Yes</span></span> |<span data-ttu-id="73a75-705">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-705">string</span></span> |<span data-ttu-id="73a75-706">hello-värde som innehåller hello objektet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-706">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="73a75-707">stringToFind</span><span class="sxs-lookup"><span data-stu-id="73a75-707">stringToFind</span></span> |<span data-ttu-id="73a75-708">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-708">Yes</span></span> |<span data-ttu-id="73a75-709">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-709">string</span></span> |<span data-ttu-id="73a75-710">hello värdet toofind.</span><span class="sxs-lookup"><span data-stu-id="73a75-710">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-711">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-711">Return value</span></span>

<span data-ttu-id="73a75-712">**SANT** om hello första tecknet eller tecknen i hello strängen matchar hello värdet; annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="73a75-712">**True** if hello first character or characters of hello string match hello value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-713">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-713">Examples</span></span>

<span data-ttu-id="73a75-714">hello som följande exempel visar hur toouse hello startsWith och endsWith-funktioner:</span><span class="sxs-lookup"><span data-stu-id="73a75-714">hello following example shows how toouse hello startsWith and endsWith functions:</span></span>

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

<span data-ttu-id="73a75-715">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-715">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-716">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-716">Name</span></span> | <span data-ttu-id="73a75-717">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-717">Type</span></span> | <span data-ttu-id="73a75-718">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-718">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-719">startsTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-719">startsTrue</span></span> | <span data-ttu-id="73a75-720">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-720">Bool</span></span> | <span data-ttu-id="73a75-721">True</span><span class="sxs-lookup"><span data-stu-id="73a75-721">True</span></span> |
| <span data-ttu-id="73a75-722">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-722">startsCapTrue</span></span> | <span data-ttu-id="73a75-723">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-723">Bool</span></span> | <span data-ttu-id="73a75-724">True</span><span class="sxs-lookup"><span data-stu-id="73a75-724">True</span></span> |
| <span data-ttu-id="73a75-725">startsFalse</span><span class="sxs-lookup"><span data-stu-id="73a75-725">startsFalse</span></span> | <span data-ttu-id="73a75-726">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-726">Bool</span></span> | <span data-ttu-id="73a75-727">False</span><span class="sxs-lookup"><span data-stu-id="73a75-727">False</span></span> |
| <span data-ttu-id="73a75-728">endsTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-728">endsTrue</span></span> | <span data-ttu-id="73a75-729">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-729">Bool</span></span> | <span data-ttu-id="73a75-730">True</span><span class="sxs-lookup"><span data-stu-id="73a75-730">True</span></span> |
| <span data-ttu-id="73a75-731">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="73a75-731">endsCapTrue</span></span> | <span data-ttu-id="73a75-732">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-732">Bool</span></span> | <span data-ttu-id="73a75-733">True</span><span class="sxs-lookup"><span data-stu-id="73a75-733">True</span></span> |
| <span data-ttu-id="73a75-734">endsFalse</span><span class="sxs-lookup"><span data-stu-id="73a75-734">endsFalse</span></span> | <span data-ttu-id="73a75-735">bool</span><span class="sxs-lookup"><span data-stu-id="73a75-735">Bool</span></span> | <span data-ttu-id="73a75-736">False</span><span class="sxs-lookup"><span data-stu-id="73a75-736">False</span></span> |

<a id="string" />

## <a name="string"></a><span data-ttu-id="73a75-737">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-737">string</span></span>
`string(valueToConvert)`

<span data-ttu-id="73a75-738">Konverterar hello anges värdet tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-738">Converts hello specified value tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-739">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-739">Parameters</span></span>

| <span data-ttu-id="73a75-740">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-740">Parameter</span></span> | <span data-ttu-id="73a75-741">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-741">Required</span></span> | <span data-ttu-id="73a75-742">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-742">Type</span></span> | <span data-ttu-id="73a75-743">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-743">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-744">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="73a75-744">valueToConvert</span></span> |<span data-ttu-id="73a75-745">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-745">Yes</span></span> | <span data-ttu-id="73a75-746">Alla</span><span class="sxs-lookup"><span data-stu-id="73a75-746">Any</span></span> |<span data-ttu-id="73a75-747">hello värdet tooconvert toostring.</span><span class="sxs-lookup"><span data-stu-id="73a75-747">hello value tooconvert toostring.</span></span> <span data-ttu-id="73a75-748">Någon typ av värde kan konverteras, inklusive objekt och matriser.</span><span class="sxs-lookup"><span data-stu-id="73a75-748">Any type of value can be converted, including objects and arrays.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-749">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-749">Return value</span></span>

<span data-ttu-id="73a75-750">En sträng med hello konverteras värdet.</span><span class="sxs-lookup"><span data-stu-id="73a75-750">A string of hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-751">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-751">Examples</span></span>

<span data-ttu-id="73a75-752">hello som följande exempel visar hur tooconvert olika typer av värden toostrings:</span><span class="sxs-lookup"><span data-stu-id="73a75-752">hello following example shows how tooconvert different types of values toostrings:</span></span>

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

<span data-ttu-id="73a75-753">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-753">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-754">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-754">Name</span></span> | <span data-ttu-id="73a75-755">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-755">Type</span></span> | <span data-ttu-id="73a75-756">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-756">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-757">objectOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-757">objectOutput</span></span> | <span data-ttu-id="73a75-758">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-758">String</span></span> | <span data-ttu-id="73a75-759">{”valueA”: 10, ”valueB”: ”exempeltext”}</span><span class="sxs-lookup"><span data-stu-id="73a75-759">{"valueA":10,"valueB":"Example Text"}</span></span> |
| <span data-ttu-id="73a75-760">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-760">arrayOutput</span></span> | <span data-ttu-id="73a75-761">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-761">String</span></span> | <span data-ttu-id="73a75-762">[”a”, ”b”, ”c”]</span><span class="sxs-lookup"><span data-stu-id="73a75-762">["a","b","c"]</span></span> |
| <span data-ttu-id="73a75-763">intOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-763">intOutput</span></span> | <span data-ttu-id="73a75-764">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-764">String</span></span> | <span data-ttu-id="73a75-765">5</span><span class="sxs-lookup"><span data-stu-id="73a75-765">5</span></span> |

<a id="substring" />

## <a name="substring"></a><span data-ttu-id="73a75-766">delsträngen</span><span class="sxs-lookup"><span data-stu-id="73a75-766">substring</span></span>
`substring(stringToParse, startIndex, length)`

<span data-ttu-id="73a75-767">Returnerar en understräng som börjar på hello angetts teckenposition och innehåller hello angivet antal tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-767">Returns a substring that starts at hello specified character position and contains hello specified number of characters.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-768">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-768">Parameters</span></span>

| <span data-ttu-id="73a75-769">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-769">Parameter</span></span> | <span data-ttu-id="73a75-770">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-770">Required</span></span> | <span data-ttu-id="73a75-771">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-771">Type</span></span> | <span data-ttu-id="73a75-772">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-772">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-773">stringToParse</span><span class="sxs-lookup"><span data-stu-id="73a75-773">stringToParse</span></span> |<span data-ttu-id="73a75-774">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-774">Yes</span></span> |<span data-ttu-id="73a75-775">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-775">string</span></span> |<span data-ttu-id="73a75-776">hello ursprungliga sträng från vilken hello understräng ska extraheras.</span><span class="sxs-lookup"><span data-stu-id="73a75-776">hello original string from which hello substring is extracted.</span></span> |
| <span data-ttu-id="73a75-777">startIndex</span><span class="sxs-lookup"><span data-stu-id="73a75-777">startIndex</span></span> |<span data-ttu-id="73a75-778">Nej</span><span class="sxs-lookup"><span data-stu-id="73a75-778">No</span></span> |<span data-ttu-id="73a75-779">int</span><span class="sxs-lookup"><span data-stu-id="73a75-779">int</span></span> |<span data-ttu-id="73a75-780">hello nollbaserade tecken startposition hello delsträngen.</span><span class="sxs-lookup"><span data-stu-id="73a75-780">hello zero-based starting character position for hello substring.</span></span> |
| <span data-ttu-id="73a75-781">Längd</span><span class="sxs-lookup"><span data-stu-id="73a75-781">length</span></span> |<span data-ttu-id="73a75-782">Nej</span><span class="sxs-lookup"><span data-stu-id="73a75-782">No</span></span> |<span data-ttu-id="73a75-783">int</span><span class="sxs-lookup"><span data-stu-id="73a75-783">int</span></span> |<span data-ttu-id="73a75-784">hello antal tecken för hello delsträngen.</span><span class="sxs-lookup"><span data-stu-id="73a75-784">hello number of characters for hello substring.</span></span> <span data-ttu-id="73a75-785">Måste referera tooa plats inom strängen hello.</span><span class="sxs-lookup"><span data-stu-id="73a75-785">Must refer tooa location within hello string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-786">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-786">Return value</span></span>

<span data-ttu-id="73a75-787">hello delsträngen.</span><span class="sxs-lookup"><span data-stu-id="73a75-787">hello substring.</span></span>

### <a name="remarks"></a><span data-ttu-id="73a75-788">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="73a75-788">Remarks</span></span>

<span data-ttu-id="73a75-789">hello funktionen misslyckas när hello delsträngen sträcker sig utanför hello slutet av hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-789">hello function fails when hello substring extends beyond hello end of hello string.</span></span> <span data-ttu-id="73a75-790">följande exempel hello misslyckas med hello fel ”hello index och längd måste referera tooa plats inom strängen hello.</span><span class="sxs-lookup"><span data-stu-id="73a75-790">hello following example fails with hello error "hello index and length parameters must refer tooa location within hello string.</span></span> <span data-ttu-id="73a75-791">Hej indexparametern: '0' hello längdparameter: 11, hello längden på strängparametern hello: ”10” ”..</span><span class="sxs-lookup"><span data-stu-id="73a75-791">hello index parameter: '0', hello length parameter: '11', hello length of hello string parameter: '10'.".</span></span>

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a><span data-ttu-id="73a75-792">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-792">Examples</span></span>

<span data-ttu-id="73a75-793">följande exempel hello extraherar en understräng från en parameter.</span><span class="sxs-lookup"><span data-stu-id="73a75-793">hello following example extracts a substring from a parameter.</span></span>

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

<span data-ttu-id="73a75-794">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-794">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-795">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-795">Name</span></span> | <span data-ttu-id="73a75-796">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-796">Type</span></span> | <span data-ttu-id="73a75-797">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-797">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-798">substringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-798">substringOutput</span></span> | <span data-ttu-id="73a75-799">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-799">String</span></span> | <span data-ttu-id="73a75-800">två</span><span class="sxs-lookup"><span data-stu-id="73a75-800">two</span></span> |


<a id="take" />

## <a name="take"></a><span data-ttu-id="73a75-801">ta</span><span class="sxs-lookup"><span data-stu-id="73a75-801">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="73a75-802">Returnerar en sträng med hello angivet antal tecken från början hello hello sträng eller en matris med hello angivna antalet element från hello start av hello matris.</span><span class="sxs-lookup"><span data-stu-id="73a75-802">Returns a string with hello specified number of characters from hello start of hello string, or an array with hello specified number of elements from hello start of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-803">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-803">Parameters</span></span>

| <span data-ttu-id="73a75-804">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-804">Parameter</span></span> | <span data-ttu-id="73a75-805">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-805">Required</span></span> | <span data-ttu-id="73a75-806">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-806">Type</span></span> | <span data-ttu-id="73a75-807">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-807">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-808">Ursprungligt värde</span><span class="sxs-lookup"><span data-stu-id="73a75-808">originalValue</span></span> |<span data-ttu-id="73a75-809">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-809">Yes</span></span> |<span data-ttu-id="73a75-810">matris eller sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-810">array or string</span></span> |<span data-ttu-id="73a75-811">Hej array eller string tootake hello element från.</span><span class="sxs-lookup"><span data-stu-id="73a75-811">hello array or string tootake hello elements from.</span></span> |
| <span data-ttu-id="73a75-812">numberToTake</span><span class="sxs-lookup"><span data-stu-id="73a75-812">numberToTake</span></span> |<span data-ttu-id="73a75-813">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-813">Yes</span></span> |<span data-ttu-id="73a75-814">int</span><span class="sxs-lookup"><span data-stu-id="73a75-814">int</span></span> |<span data-ttu-id="73a75-815">hello antal element eller tecken tootake.</span><span class="sxs-lookup"><span data-stu-id="73a75-815">hello number of elements or characters tootake.</span></span> <span data-ttu-id="73a75-816">Om det här värdet är 0 eller mindre, returneras en tom matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-816">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="73a75-817">Om den är större än hello hello matris eller sträng returneras alla hello-element i hello array eller string.</span><span class="sxs-lookup"><span data-stu-id="73a75-817">If it is larger than hello length of hello given array or string, all hello elements in hello array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-818">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-818">Return value</span></span>

<span data-ttu-id="73a75-819">En matris eller sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-819">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-820">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-820">Examples</span></span>

<span data-ttu-id="73a75-821">följande exempel tar hello hello angivet antal element från hello matris och tecken från en sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-821">hello following example takes hello specified number of elements from hello array, and characters from a string.</span></span>

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

<span data-ttu-id="73a75-822">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-822">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-823">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-823">Name</span></span> | <span data-ttu-id="73a75-824">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-824">Type</span></span> | <span data-ttu-id="73a75-825">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-825">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-826">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-826">arrayOutput</span></span> | <span data-ttu-id="73a75-827">Matris</span><span class="sxs-lookup"><span data-stu-id="73a75-827">Array</span></span> | <span data-ttu-id="73a75-828">[””, ”två”]</span><span class="sxs-lookup"><span data-stu-id="73a75-828">["one", "two"]</span></span> |
| <span data-ttu-id="73a75-829">stringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-829">stringOutput</span></span> | <span data-ttu-id="73a75-830">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-830">String</span></span> | <span data-ttu-id="73a75-831">på</span><span class="sxs-lookup"><span data-stu-id="73a75-831">on</span></span> |

<a id="tolower" />

## <a name="tolower"></a><span data-ttu-id="73a75-832">toLower</span><span class="sxs-lookup"><span data-stu-id="73a75-832">toLower</span></span>
`toLower(stringToChange)`

<span data-ttu-id="73a75-833">Konverterar hello angetts sträng toolower fallet.</span><span class="sxs-lookup"><span data-stu-id="73a75-833">Converts hello specified string toolower case.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-834">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-834">Parameters</span></span>

| <span data-ttu-id="73a75-835">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-835">Parameter</span></span> | <span data-ttu-id="73a75-836">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-836">Required</span></span> | <span data-ttu-id="73a75-837">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-837">Type</span></span> | <span data-ttu-id="73a75-838">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-838">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-839">stringToChange</span><span class="sxs-lookup"><span data-stu-id="73a75-839">stringToChange</span></span> |<span data-ttu-id="73a75-840">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-840">Yes</span></span> |<span data-ttu-id="73a75-841">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-841">string</span></span> |<span data-ttu-id="73a75-842">hello värdet tooconvert toolower case.</span><span class="sxs-lookup"><span data-stu-id="73a75-842">hello value tooconvert toolower case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-843">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-843">Return value</span></span>

<span data-ttu-id="73a75-844">toolower fall konvertera hello-strängen.</span><span class="sxs-lookup"><span data-stu-id="73a75-844">hello string converted toolower case.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-845">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-845">Examples</span></span>

<span data-ttu-id="73a75-846">hello följande exempel konverterar en parameter värdet toolower fallet och tooupper fallet.</span><span class="sxs-lookup"><span data-stu-id="73a75-846">hello following example converts a parameter value toolower case and tooupper case.</span></span>

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

<span data-ttu-id="73a75-847">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-847">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-848">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-848">Name</span></span> | <span data-ttu-id="73a75-849">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-849">Type</span></span> | <span data-ttu-id="73a75-850">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-850">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-851">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-851">toLowerOutput</span></span> | <span data-ttu-id="73a75-852">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-852">String</span></span> | <span data-ttu-id="73a75-853">Ett två tre</span><span class="sxs-lookup"><span data-stu-id="73a75-853">one two three</span></span> |
| <span data-ttu-id="73a75-854">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-854">toUpperOutput</span></span> | <span data-ttu-id="73a75-855">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-855">String</span></span> | <span data-ttu-id="73a75-856">ETT TVÅ TRE</span><span class="sxs-lookup"><span data-stu-id="73a75-856">ONE TWO THREE</span></span> |

<a id="toupper" />

## <a name="toupper"></a><span data-ttu-id="73a75-857">toUpper</span><span class="sxs-lookup"><span data-stu-id="73a75-857">toUpper</span></span>
`toUpper(stringToChange)`

<span data-ttu-id="73a75-858">Konverterar hello angetts sträng tooupper fallet.</span><span class="sxs-lookup"><span data-stu-id="73a75-858">Converts hello specified string tooupper case.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-859">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-859">Parameters</span></span>

| <span data-ttu-id="73a75-860">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-860">Parameter</span></span> | <span data-ttu-id="73a75-861">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-861">Required</span></span> | <span data-ttu-id="73a75-862">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-862">Type</span></span> | <span data-ttu-id="73a75-863">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-863">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-864">stringToChange</span><span class="sxs-lookup"><span data-stu-id="73a75-864">stringToChange</span></span> |<span data-ttu-id="73a75-865">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-865">Yes</span></span> |<span data-ttu-id="73a75-866">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-866">string</span></span> |<span data-ttu-id="73a75-867">hello värdet tooconvert tooupper case.</span><span class="sxs-lookup"><span data-stu-id="73a75-867">hello value tooconvert tooupper case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-868">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-868">Return value</span></span>

<span data-ttu-id="73a75-869">tooupper fall konvertera hello-strängen.</span><span class="sxs-lookup"><span data-stu-id="73a75-869">hello string converted tooupper case.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-870">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-870">Examples</span></span>

<span data-ttu-id="73a75-871">hello följande exempel konverterar en parameter värdet toolower fallet och tooupper fallet.</span><span class="sxs-lookup"><span data-stu-id="73a75-871">hello following example converts a parameter value toolower case and tooupper case.</span></span>

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

<span data-ttu-id="73a75-872">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-872">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-873">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-873">Name</span></span> | <span data-ttu-id="73a75-874">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-874">Type</span></span> | <span data-ttu-id="73a75-875">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-875">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-876">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-876">toLowerOutput</span></span> | <span data-ttu-id="73a75-877">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-877">String</span></span> | <span data-ttu-id="73a75-878">Ett två tre</span><span class="sxs-lookup"><span data-stu-id="73a75-878">one two three</span></span> |
| <span data-ttu-id="73a75-879">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-879">toUpperOutput</span></span> | <span data-ttu-id="73a75-880">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-880">String</span></span> | <span data-ttu-id="73a75-881">ETT TVÅ TRE</span><span class="sxs-lookup"><span data-stu-id="73a75-881">ONE TWO THREE</span></span> |

<a id="trim" />

## <a name="trim"></a><span data-ttu-id="73a75-882">Rensa</span><span class="sxs-lookup"><span data-stu-id="73a75-882">trim</span></span>
`trim (stringToTrim)`

<span data-ttu-id="73a75-883">Tar bort alla inledande och avslutande blanksteg från hello angivna strängen.</span><span class="sxs-lookup"><span data-stu-id="73a75-883">Removes all leading and trailing white-space characters from hello specified string.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-884">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-884">Parameters</span></span>

| <span data-ttu-id="73a75-885">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-885">Parameter</span></span> | <span data-ttu-id="73a75-886">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-886">Required</span></span> | <span data-ttu-id="73a75-887">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-887">Type</span></span> | <span data-ttu-id="73a75-888">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-888">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-889">stringToTrim</span><span class="sxs-lookup"><span data-stu-id="73a75-889">stringToTrim</span></span> |<span data-ttu-id="73a75-890">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-890">Yes</span></span> |<span data-ttu-id="73a75-891">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-891">string</span></span> |<span data-ttu-id="73a75-892">hello värdet tootrim.</span><span class="sxs-lookup"><span data-stu-id="73a75-892">hello value tootrim.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-893">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-893">Return value</span></span>

<span data-ttu-id="73a75-894">hello strängen utan inledande och avslutande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="73a75-894">hello string without leading and trailing white-space characters.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-895">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-895">Examples</span></span>

<span data-ttu-id="73a75-896">hello följande exempel tar bort hello blanksteg från hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="73a75-896">hello following example trims hello white-space characters from hello parameter.</span></span>

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

<span data-ttu-id="73a75-897">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-897">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-898">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-898">Name</span></span> | <span data-ttu-id="73a75-899">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-899">Type</span></span> | <span data-ttu-id="73a75-900">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-900">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-901">Returnera</span><span class="sxs-lookup"><span data-stu-id="73a75-901">return</span></span> | <span data-ttu-id="73a75-902">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-902">String</span></span> | <span data-ttu-id="73a75-903">Ett två tre</span><span class="sxs-lookup"><span data-stu-id="73a75-903">one two three</span></span> |

<a id="uniquestring" />

## <a name="uniquestring"></a><span data-ttu-id="73a75-904">uniqueString</span><span class="sxs-lookup"><span data-stu-id="73a75-904">uniqueString</span></span>
`uniqueString (baseString, ...)`

<span data-ttu-id="73a75-905">Skapar en deterministisk hash-sträng baserat på hello värden som parametrar.</span><span class="sxs-lookup"><span data-stu-id="73a75-905">Creates a deterministic hash string based on hello values provided as parameters.</span></span> 

### <a name="parameters"></a><span data-ttu-id="73a75-906">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-906">Parameters</span></span>

| <span data-ttu-id="73a75-907">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-907">Parameter</span></span> | <span data-ttu-id="73a75-908">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-908">Required</span></span> | <span data-ttu-id="73a75-909">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-909">Type</span></span> | <span data-ttu-id="73a75-910">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-910">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-911">baseString</span><span class="sxs-lookup"><span data-stu-id="73a75-911">baseString</span></span> |<span data-ttu-id="73a75-912">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-912">Yes</span></span> |<span data-ttu-id="73a75-913">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-913">string</span></span> |<span data-ttu-id="73a75-914">hello-värde som används i hello hash-funktionen toocreate en unik sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-914">hello value used in hello hash function toocreate a unique string.</span></span> |
| <span data-ttu-id="73a75-915">ytterligare parametrar som krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-915">additional parameters as needed</span></span> |<span data-ttu-id="73a75-916">Nej</span><span class="sxs-lookup"><span data-stu-id="73a75-916">No</span></span> |<span data-ttu-id="73a75-917">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-917">string</span></span> |<span data-ttu-id="73a75-918">Du kan lägga till så många strängar som behövs toocreate hello-värde som anger hello unikhet.</span><span class="sxs-lookup"><span data-stu-id="73a75-918">You can add as many strings as needed toocreate hello value that specifies hello level of uniqueness.</span></span> |

### <a name="remarks"></a><span data-ttu-id="73a75-919">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="73a75-919">Remarks</span></span>

<span data-ttu-id="73a75-920">Den här funktionen är användbart när du behöver toocreate ett unikt namn för en resurs.</span><span class="sxs-lookup"><span data-stu-id="73a75-920">This function is helpful when you need toocreate a unique name for a resource.</span></span> <span data-ttu-id="73a75-921">Du kan ange parametervärden som begränsar hello omfång unikhet för hello resultat.</span><span class="sxs-lookup"><span data-stu-id="73a75-921">You provide parameter values that limit hello scope of uniqueness for hello result.</span></span> <span data-ttu-id="73a75-922">Du kan ange om hello namn är unikt ned toosubscription, resursgrupp eller distribution.</span><span class="sxs-lookup"><span data-stu-id="73a75-922">You can specify whether hello name is unique down toosubscription, resource group, or deployment.</span></span> 

<span data-ttu-id="73a75-923">hello returnerade värdet är inte en slumpmässig sträng, men i stället hello resultatet av en hashfunktionen.</span><span class="sxs-lookup"><span data-stu-id="73a75-923">hello returned value is not a random string, but rather hello result of a hash function.</span></span> <span data-ttu-id="73a75-924">hello returnerade värdet är 13 tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-924">hello returned value is 13 characters long.</span></span> <span data-ttu-id="73a75-925">Det är inte globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="73a75-925">It is not globally unique.</span></span> <span data-ttu-id="73a75-926">Du kanske vill toocombine hello-värde med ett prefix från din naming convention toocreate ett beskrivande namn.</span><span class="sxs-lookup"><span data-stu-id="73a75-926">You may want toocombine hello value with a prefix from your naming convention toocreate a name that is meaningful.</span></span> <span data-ttu-id="73a75-927">hello visar följande exempel hello format hello returnerade värde.</span><span class="sxs-lookup"><span data-stu-id="73a75-927">hello following example shows hello format of hello returned value.</span></span> <span data-ttu-id="73a75-928">hello faktiska värdet varierar beroende på hello angivna parametrar.</span><span class="sxs-lookup"><span data-stu-id="73a75-928">hello actual value varies by hello provided parameters.</span></span>

    tcvhiyu5h2o5o

<span data-ttu-id="73a75-929">hello som följande exempel visar hur toouse uniqueString toocreate ett unikt värde för ofta använda nivåer.</span><span class="sxs-lookup"><span data-stu-id="73a75-929">hello following examples show how toouse uniqueString toocreate a unique value for commonly used levels.</span></span>

<span data-ttu-id="73a75-930">Unikt begränsade toosubscription</span><span class="sxs-lookup"><span data-stu-id="73a75-930">Unique scoped toosubscription</span></span>

```json
"[uniqueString(subscription().subscriptionId)]"
```

<span data-ttu-id="73a75-931">Unikt begränsade tooresource grupp</span><span class="sxs-lookup"><span data-stu-id="73a75-931">Unique scoped tooresource group</span></span>

```json
"[uniqueString(resourceGroup().id)]"
```

<span data-ttu-id="73a75-932">Unikt begränsade toodeployment för en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="73a75-932">Unique scoped toodeployment for a resource group</span></span>

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

<span data-ttu-id="73a75-933">hello som följande exempel visar hur toocreate ett unikt namn för ett lagringskonto baserat på din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="73a75-933">hello following example shows how toocreate a unique name for a storage account based on your resource group.</span></span> <span data-ttu-id="73a75-934">Inuti hello resursgruppen hello namn är inte unikt om konstruerat hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="73a75-934">Inside hello resource group, hello name is not unique if constructed hello same way.</span></span>

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a><span data-ttu-id="73a75-935">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-935">Return value</span></span>

<span data-ttu-id="73a75-936">En sträng som innehåller 13 tecken.</span><span class="sxs-lookup"><span data-stu-id="73a75-936">A string containing 13 characters.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-937">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-937">Examples</span></span>

<span data-ttu-id="73a75-938">hello följande exempel returnerar resultat från uniquestring:</span><span class="sxs-lookup"><span data-stu-id="73a75-938">hello following example returns results from uniquestring:</span></span>

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

## <a name="uri"></a><span data-ttu-id="73a75-939">URI: N</span><span class="sxs-lookup"><span data-stu-id="73a75-939">uri</span></span>
`uri (baseUri, relativeUri)`

<span data-ttu-id="73a75-940">Skapar en absolut URI genom att kombinera hello baseUri och hello relativeUri sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-940">Creates an absolute URI by combining hello baseUri and hello relativeUri string.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-941">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-941">Parameters</span></span>

| <span data-ttu-id="73a75-942">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-942">Parameter</span></span> | <span data-ttu-id="73a75-943">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-943">Required</span></span> | <span data-ttu-id="73a75-944">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-944">Type</span></span> | <span data-ttu-id="73a75-945">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-945">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-946">baseUri</span><span class="sxs-lookup"><span data-stu-id="73a75-946">baseUri</span></span> |<span data-ttu-id="73a75-947">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-947">Yes</span></span> |<span data-ttu-id="73a75-948">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-948">string</span></span> |<span data-ttu-id="73a75-949">hello bas-uri-sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-949">hello base uri string.</span></span> |
| <span data-ttu-id="73a75-950">relativeUri</span><span class="sxs-lookup"><span data-stu-id="73a75-950">relativeUri</span></span> |<span data-ttu-id="73a75-951">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-951">Yes</span></span> |<span data-ttu-id="73a75-952">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-952">string</span></span> |<span data-ttu-id="73a75-953">hello relativ uri sträng tooadd toohello bas-uri-strängen.</span><span class="sxs-lookup"><span data-stu-id="73a75-953">hello relative uri string tooadd toohello base uri string.</span></span> |

<span data-ttu-id="73a75-954">Hej värde för hello **baseUri** parameter kan innehålla en viss fil, men endast hello bassökväg används när man skapar hello URI.</span><span class="sxs-lookup"><span data-stu-id="73a75-954">hello value for hello **baseUri** parameter can include a specific file, but only hello base path is used when constructing hello URI.</span></span> <span data-ttu-id="73a75-955">Till exempel skicka `http://contoso.com/resources/azuredeploy.json` som hello baseUri parametern resultat i en bas-URI för `http://contoso.com/resources/`.</span><span class="sxs-lookup"><span data-stu-id="73a75-955">For example, passing `http://contoso.com/resources/azuredeploy.json` as hello baseUri parameter results in a base URI of `http://contoso.com/resources/`.</span></span>

### <a name="return-value"></a><span data-ttu-id="73a75-956">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-956">Return value</span></span>

<span data-ttu-id="73a75-957">En sträng som representerar hello absolut URI för hello grundläggande och relativa värden.</span><span class="sxs-lookup"><span data-stu-id="73a75-957">A string representing hello absolute URI for hello base and relative values.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-958">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-958">Examples</span></span>

<span data-ttu-id="73a75-959">hello som följande exempel visar hur tooconstruct en länk tooa kapslad mall baserat på hello värdet för hello överordnade mallen.</span><span class="sxs-lookup"><span data-stu-id="73a75-959">hello following example shows how tooconstruct a link tooa nested template based on hello value of hello parent template.</span></span>

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

<span data-ttu-id="73a75-960">följande exempel visar hur hello toouse uri, uriComponent, och uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="73a75-960">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="73a75-961">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-961">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-962">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-962">Name</span></span> | <span data-ttu-id="73a75-963">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-963">Type</span></span> | <span data-ttu-id="73a75-964">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-964">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-965">uriOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-965">uriOutput</span></span> | <span data-ttu-id="73a75-966">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-966">String</span></span> | <span data-ttu-id="73a75-967">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-967">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="73a75-968">componentOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-968">componentOutput</span></span> | <span data-ttu-id="73a75-969">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-969">String</span></span> | <span data-ttu-id="73a75-970">HTTP%3a%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-970">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="73a75-971">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-971">toStringOutput</span></span> | <span data-ttu-id="73a75-972">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-972">String</span></span> | <span data-ttu-id="73a75-973">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-973">http://contoso.com/resources/nested/azuredeploy.json</span></span> |

<a id="uricomponent" />

## <a name="uricomponent"></a><span data-ttu-id="73a75-974">uriComponent</span><span class="sxs-lookup"><span data-stu-id="73a75-974">uriComponent</span></span>
`uricomponent(stringToEncode)`

<span data-ttu-id="73a75-975">Kodar en URI.</span><span class="sxs-lookup"><span data-stu-id="73a75-975">Encodes a URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-976">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-976">Parameters</span></span>

| <span data-ttu-id="73a75-977">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-977">Parameter</span></span> | <span data-ttu-id="73a75-978">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-978">Required</span></span> | <span data-ttu-id="73a75-979">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-979">Type</span></span> | <span data-ttu-id="73a75-980">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-980">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-981">stringToEncode</span><span class="sxs-lookup"><span data-stu-id="73a75-981">stringToEncode</span></span> |<span data-ttu-id="73a75-982">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-982">Yes</span></span> |<span data-ttu-id="73a75-983">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-983">string</span></span> |<span data-ttu-id="73a75-984">hello värdet tooencode.</span><span class="sxs-lookup"><span data-stu-id="73a75-984">hello value tooencode.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-985">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-985">Return value</span></span>

<span data-ttu-id="73a75-986">En sträng med hello URI kodad värde.</span><span class="sxs-lookup"><span data-stu-id="73a75-986">A string of hello URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-987">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-987">Examples</span></span>

<span data-ttu-id="73a75-988">följande exempel visar hur hello toouse uri, uriComponent, och uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="73a75-988">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="73a75-989">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-989">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-990">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-990">Name</span></span> | <span data-ttu-id="73a75-991">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-991">Type</span></span> | <span data-ttu-id="73a75-992">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-992">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-993">uriOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-993">uriOutput</span></span> | <span data-ttu-id="73a75-994">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-994">String</span></span> | <span data-ttu-id="73a75-995">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-995">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="73a75-996">componentOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-996">componentOutput</span></span> | <span data-ttu-id="73a75-997">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-997">String</span></span> | <span data-ttu-id="73a75-998">HTTP%3a%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-998">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="73a75-999">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-999">toStringOutput</span></span> | <span data-ttu-id="73a75-1000">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-1000">String</span></span> | <span data-ttu-id="73a75-1001">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-1001">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a><span data-ttu-id="73a75-1002">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="73a75-1002">uriComponentToString</span></span>
`uriComponentToString(uriEncodedString)`

<span data-ttu-id="73a75-1003">Returnerar en sträng med en URI-kodade värde.</span><span class="sxs-lookup"><span data-stu-id="73a75-1003">Returns a string of a URI encoded value.</span></span>

### <a name="parameters"></a><span data-ttu-id="73a75-1004">Parametrar</span><span class="sxs-lookup"><span data-stu-id="73a75-1004">Parameters</span></span>

| <span data-ttu-id="73a75-1005">Parameter</span><span class="sxs-lookup"><span data-stu-id="73a75-1005">Parameter</span></span> | <span data-ttu-id="73a75-1006">Krävs</span><span class="sxs-lookup"><span data-stu-id="73a75-1006">Required</span></span> | <span data-ttu-id="73a75-1007">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-1007">Type</span></span> | <span data-ttu-id="73a75-1008">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73a75-1008">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="73a75-1009">uriEncodedString</span><span class="sxs-lookup"><span data-stu-id="73a75-1009">uriEncodedString</span></span> |<span data-ttu-id="73a75-1010">Ja</span><span class="sxs-lookup"><span data-stu-id="73a75-1010">Yes</span></span> |<span data-ttu-id="73a75-1011">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-1011">string</span></span> |<span data-ttu-id="73a75-1012">hello URI-kodad värdet tooconvert tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="73a75-1012">hello URI encoded value tooconvert tooa string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="73a75-1013">Returvärde</span><span class="sxs-lookup"><span data-stu-id="73a75-1013">Return value</span></span>

<span data-ttu-id="73a75-1014">Den avkodade strängen URI kodad värde.</span><span class="sxs-lookup"><span data-stu-id="73a75-1014">A decoded string of URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="73a75-1015">Exempel</span><span class="sxs-lookup"><span data-stu-id="73a75-1015">Examples</span></span>

<span data-ttu-id="73a75-1016">följande exempel visar hur hello toouse uri, uriComponent, och uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="73a75-1016">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="73a75-1017">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="73a75-1017">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="73a75-1018">Namn</span><span class="sxs-lookup"><span data-stu-id="73a75-1018">Name</span></span> | <span data-ttu-id="73a75-1019">Typ</span><span class="sxs-lookup"><span data-stu-id="73a75-1019">Type</span></span> | <span data-ttu-id="73a75-1020">Värde</span><span class="sxs-lookup"><span data-stu-id="73a75-1020">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="73a75-1021">uriOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-1021">uriOutput</span></span> | <span data-ttu-id="73a75-1022">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-1022">String</span></span> | <span data-ttu-id="73a75-1023">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-1023">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="73a75-1024">componentOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-1024">componentOutput</span></span> | <span data-ttu-id="73a75-1025">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-1025">String</span></span> | <span data-ttu-id="73a75-1026">HTTP%3a%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-1026">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="73a75-1027">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="73a75-1027">toStringOutput</span></span> | <span data-ttu-id="73a75-1028">Sträng</span><span class="sxs-lookup"><span data-stu-id="73a75-1028">String</span></span> | <span data-ttu-id="73a75-1029">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="73a75-1029">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


## <a name="next-steps"></a><span data-ttu-id="73a75-1030">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73a75-1030">Next steps</span></span>
* <span data-ttu-id="73a75-1031">En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="73a75-1031">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="73a75-1032">flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="73a75-1032">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="73a75-1033">tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="73a75-1033">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="73a75-1034">toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="73a75-1034">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

