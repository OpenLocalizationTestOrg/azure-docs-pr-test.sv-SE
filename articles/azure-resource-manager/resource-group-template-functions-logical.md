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
# <a name="logical-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="7bb00-103">Logiska funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="7bb00-103">Logical functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="7bb00-104">Resource Manager innehåller flera funktioner för att göra jämförelser i dina mallar.</span><span class="sxs-lookup"><span data-stu-id="7bb00-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="7bb00-105">och</span><span class="sxs-lookup"><span data-stu-id="7bb00-105">and</span></span>](#and)
* [<span data-ttu-id="7bb00-106">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-106">bool</span></span>](#bool)
* [<span data-ttu-id="7bb00-107">Om</span><span class="sxs-lookup"><span data-stu-id="7bb00-107">if</span></span>](#if)
* [<span data-ttu-id="7bb00-108">inte</span><span class="sxs-lookup"><span data-stu-id="7bb00-108">not</span></span>](#not)
* [<span data-ttu-id="7bb00-109">eller</span><span class="sxs-lookup"><span data-stu-id="7bb00-109">or</span></span>](#or)

## <a name="and"></a><span data-ttu-id="7bb00-110">och</span><span class="sxs-lookup"><span data-stu-id="7bb00-110">and</span></span>
`and(arg1, arg2)`

<span data-ttu-id="7bb00-111">Kontrollerar om båda parametervärden är true.</span><span class="sxs-lookup"><span data-stu-id="7bb00-111">Checks whether both parameter values are true.</span></span>

### <a name="parameters"></a><span data-ttu-id="7bb00-112">Parametrar</span><span class="sxs-lookup"><span data-stu-id="7bb00-112">Parameters</span></span>

| <span data-ttu-id="7bb00-113">Parameter</span><span class="sxs-lookup"><span data-stu-id="7bb00-113">Parameter</span></span> | <span data-ttu-id="7bb00-114">Krävs</span><span class="sxs-lookup"><span data-stu-id="7bb00-114">Required</span></span> | <span data-ttu-id="7bb00-115">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-115">Type</span></span> | <span data-ttu-id="7bb00-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7bb00-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="7bb00-117">arg1</span><span class="sxs-lookup"><span data-stu-id="7bb00-117">arg1</span></span> |<span data-ttu-id="7bb00-118">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-118">Yes</span></span> |<span data-ttu-id="7bb00-119">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-119">boolean</span></span> |<span data-ttu-id="7bb00-120">Hej första värde toocheck om är true.</span><span class="sxs-lookup"><span data-stu-id="7bb00-120">hello first value toocheck whether is true.</span></span> |
| <span data-ttu-id="7bb00-121">arg2</span><span class="sxs-lookup"><span data-stu-id="7bb00-121">arg2</span></span> |<span data-ttu-id="7bb00-122">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-122">Yes</span></span> |<span data-ttu-id="7bb00-123">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-123">boolean</span></span> |<span data-ttu-id="7bb00-124">Hej andra värdet toocheck om är true.</span><span class="sxs-lookup"><span data-stu-id="7bb00-124">hello second value toocheck whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="7bb00-125">Returvärde</span><span class="sxs-lookup"><span data-stu-id="7bb00-125">Return value</span></span>

<span data-ttu-id="7bb00-126">Returnerar **SANT** om båda värdena är true, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="7bb00-126">Returns **True** if both values are true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="7bb00-127">Exempel</span><span class="sxs-lookup"><span data-stu-id="7bb00-127">Examples</span></span>

<span data-ttu-id="7bb00-128">följande exempel visar hur hello toouse logiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="7bb00-128">hello following example shows how toouse logical functions.</span></span>

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

<span data-ttu-id="7bb00-129">hello utdata från hello föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="7bb00-129">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="7bb00-130">Namn</span><span class="sxs-lookup"><span data-stu-id="7bb00-130">Name</span></span> | <span data-ttu-id="7bb00-131">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-131">Type</span></span> | <span data-ttu-id="7bb00-132">Värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-132">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="7bb00-133">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-133">andExampleOutput</span></span> | <span data-ttu-id="7bb00-134">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-134">Bool</span></span> | <span data-ttu-id="7bb00-135">False</span><span class="sxs-lookup"><span data-stu-id="7bb00-135">False</span></span> |
| <span data-ttu-id="7bb00-136">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-136">orExampleOutput</span></span> | <span data-ttu-id="7bb00-137">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-137">Bool</span></span> | <span data-ttu-id="7bb00-138">True</span><span class="sxs-lookup"><span data-stu-id="7bb00-138">True</span></span> |
| <span data-ttu-id="7bb00-139">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-139">notExampleOutput</span></span> | <span data-ttu-id="7bb00-140">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-140">Bool</span></span> | <span data-ttu-id="7bb00-141">False</span><span class="sxs-lookup"><span data-stu-id="7bb00-141">False</span></span> |


## <a name="bool"></a><span data-ttu-id="7bb00-142">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-142">bool</span></span>
`bool(arg1)`

<span data-ttu-id="7bb00-143">Konverterar hello parametern tooa boolean.</span><span class="sxs-lookup"><span data-stu-id="7bb00-143">Converts hello parameter tooa boolean.</span></span>

### <a name="parameters"></a><span data-ttu-id="7bb00-144">Parametrar</span><span class="sxs-lookup"><span data-stu-id="7bb00-144">Parameters</span></span>

| <span data-ttu-id="7bb00-145">Parameter</span><span class="sxs-lookup"><span data-stu-id="7bb00-145">Parameter</span></span> | <span data-ttu-id="7bb00-146">Krävs</span><span class="sxs-lookup"><span data-stu-id="7bb00-146">Required</span></span> | <span data-ttu-id="7bb00-147">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-147">Type</span></span> | <span data-ttu-id="7bb00-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7bb00-148">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="7bb00-149">arg1</span><span class="sxs-lookup"><span data-stu-id="7bb00-149">arg1</span></span> |<span data-ttu-id="7bb00-150">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-150">Yes</span></span> |<span data-ttu-id="7bb00-151">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="7bb00-151">string or int</span></span> |<span data-ttu-id="7bb00-152">Hej värdet tooconvert tooa boolean.</span><span class="sxs-lookup"><span data-stu-id="7bb00-152">hello value tooconvert tooa boolean.</span></span> |

### <a name="return-value"></a><span data-ttu-id="7bb00-153">Returvärde</span><span class="sxs-lookup"><span data-stu-id="7bb00-153">Return value</span></span>
<span data-ttu-id="7bb00-154">Ett booleskt värde för hello konverteras värdet.</span><span class="sxs-lookup"><span data-stu-id="7bb00-154">A boolean of hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="7bb00-155">Exempel</span><span class="sxs-lookup"><span data-stu-id="7bb00-155">Examples</span></span>

<span data-ttu-id="7bb00-156">följande exempel visar hur hello toouse bool med en sträng eller ett heltal.</span><span class="sxs-lookup"><span data-stu-id="7bb00-156">hello following example shows how toouse bool with a string or integer.</span></span>

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

<span data-ttu-id="7bb00-157">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="7bb00-157">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="7bb00-158">Namn</span><span class="sxs-lookup"><span data-stu-id="7bb00-158">Name</span></span> | <span data-ttu-id="7bb00-159">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-159">Type</span></span> | <span data-ttu-id="7bb00-160">Värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-160">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="7bb00-161">trueString</span><span class="sxs-lookup"><span data-stu-id="7bb00-161">trueString</span></span> | <span data-ttu-id="7bb00-162">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-162">Bool</span></span> | <span data-ttu-id="7bb00-163">True</span><span class="sxs-lookup"><span data-stu-id="7bb00-163">True</span></span> |
| <span data-ttu-id="7bb00-164">falseString</span><span class="sxs-lookup"><span data-stu-id="7bb00-164">falseString</span></span> | <span data-ttu-id="7bb00-165">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-165">Bool</span></span> | <span data-ttu-id="7bb00-166">False</span><span class="sxs-lookup"><span data-stu-id="7bb00-166">False</span></span> |
| <span data-ttu-id="7bb00-167">trueInt</span><span class="sxs-lookup"><span data-stu-id="7bb00-167">trueInt</span></span> | <span data-ttu-id="7bb00-168">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-168">Bool</span></span> | <span data-ttu-id="7bb00-169">True</span><span class="sxs-lookup"><span data-stu-id="7bb00-169">True</span></span> |
| <span data-ttu-id="7bb00-170">falseInt</span><span class="sxs-lookup"><span data-stu-id="7bb00-170">falseInt</span></span> | <span data-ttu-id="7bb00-171">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-171">Bool</span></span> | <span data-ttu-id="7bb00-172">False</span><span class="sxs-lookup"><span data-stu-id="7bb00-172">False</span></span> |

## <a name="if"></a><span data-ttu-id="7bb00-173">Om</span><span class="sxs-lookup"><span data-stu-id="7bb00-173">if</span></span>
`if(condition, trueValue, falseValue)`

<span data-ttu-id="7bb00-174">Returnerar ett värde baserat på om ett villkor är SANT eller FALSKT.</span><span class="sxs-lookup"><span data-stu-id="7bb00-174">Returns a value based on whether a condition is true or false.</span></span>

### <a name="parameters"></a><span data-ttu-id="7bb00-175">Parametrar</span><span class="sxs-lookup"><span data-stu-id="7bb00-175">Parameters</span></span>

| <span data-ttu-id="7bb00-176">Parameter</span><span class="sxs-lookup"><span data-stu-id="7bb00-176">Parameter</span></span> | <span data-ttu-id="7bb00-177">Krävs</span><span class="sxs-lookup"><span data-stu-id="7bb00-177">Required</span></span> | <span data-ttu-id="7bb00-178">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-178">Type</span></span> | <span data-ttu-id="7bb00-179">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7bb00-179">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="7bb00-180">Villkor</span><span class="sxs-lookup"><span data-stu-id="7bb00-180">condition</span></span> |<span data-ttu-id="7bb00-181">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-181">Yes</span></span> |<span data-ttu-id="7bb00-182">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-182">boolean</span></span> |<span data-ttu-id="7bb00-183">Hej värdet toocheck om det inte stämmer.</span><span class="sxs-lookup"><span data-stu-id="7bb00-183">hello value toocheck whether it is true.</span></span> |
| <span data-ttu-id="7bb00-184">trueValue</span><span class="sxs-lookup"><span data-stu-id="7bb00-184">trueValue</span></span> |<span data-ttu-id="7bb00-185">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-185">Yes</span></span> | <span data-ttu-id="7bb00-186">sträng, int, objekt eller matris</span><span class="sxs-lookup"><span data-stu-id="7bb00-186">string, int, object, or array</span></span> |<span data-ttu-id="7bb00-187">hello värdet tooreturn när hello villkoret är sant.</span><span class="sxs-lookup"><span data-stu-id="7bb00-187">hello value tooreturn when hello condition is true.</span></span> |
| <span data-ttu-id="7bb00-188">falseValue</span><span class="sxs-lookup"><span data-stu-id="7bb00-188">falseValue</span></span> |<span data-ttu-id="7bb00-189">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-189">Yes</span></span> | <span data-ttu-id="7bb00-190">sträng, int, objekt eller matris</span><span class="sxs-lookup"><span data-stu-id="7bb00-190">string, int, object, or array</span></span> |<span data-ttu-id="7bb00-191">hello värdet tooreturn när hello villkoret är FALSKT.</span><span class="sxs-lookup"><span data-stu-id="7bb00-191">hello value tooreturn when hello condition is false.</span></span> |

### <a name="return-value"></a><span data-ttu-id="7bb00-192">Returvärde</span><span class="sxs-lookup"><span data-stu-id="7bb00-192">Return value</span></span>

<span data-ttu-id="7bb00-193">Returnerar andra parameter när den första parametern är **SANT**, annars returnerar tredje parametern.</span><span class="sxs-lookup"><span data-stu-id="7bb00-193">Returns second parameter when first parameter is **True**; otherwise, returns third parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="7bb00-194">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="7bb00-194">Remarks</span></span>

<span data-ttu-id="7bb00-195">Du kan använda den här funktionen tooconditionally en resursegenskap.</span><span class="sxs-lookup"><span data-stu-id="7bb00-195">You can use this function tooconditionally set a resource property.</span></span> <span data-ttu-id="7bb00-196">hello följande exempel är inte en fullständig mall, men den visar hello relevanta delar för att ange villkorligt hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7bb00-196">hello following example is not a full template, but it shows hello relevant portions for conditionally setting hello availability set.</span></span>

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

### <a name="examples"></a><span data-ttu-id="7bb00-197">Exempel</span><span class="sxs-lookup"><span data-stu-id="7bb00-197">Examples</span></span>

<span data-ttu-id="7bb00-198">följande exempel visar hur hello toouse hello `if` funktion.</span><span class="sxs-lookup"><span data-stu-id="7bb00-198">hello following example shows how toouse hello `if` function.</span></span>

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

<span data-ttu-id="7bb00-199">hello utdata från hello föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="7bb00-199">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="7bb00-200">Namn</span><span class="sxs-lookup"><span data-stu-id="7bb00-200">Name</span></span> | <span data-ttu-id="7bb00-201">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-201">Type</span></span> | <span data-ttu-id="7bb00-202">Värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-202">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="7bb00-203">yesOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-203">yesOutput</span></span> | <span data-ttu-id="7bb00-204">Sträng</span><span class="sxs-lookup"><span data-stu-id="7bb00-204">String</span></span> | <span data-ttu-id="7bb00-205">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-205">yes</span></span> |
| <span data-ttu-id="7bb00-206">noOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-206">noOutput</span></span> | <span data-ttu-id="7bb00-207">Sträng</span><span class="sxs-lookup"><span data-stu-id="7bb00-207">String</span></span> | <span data-ttu-id="7bb00-208">Nej</span><span class="sxs-lookup"><span data-stu-id="7bb00-208">no</span></span> |


## <a name="not"></a><span data-ttu-id="7bb00-209">inte</span><span class="sxs-lookup"><span data-stu-id="7bb00-209">not</span></span>
`not(arg1)`

<span data-ttu-id="7bb00-210">Konverterar booleskt värde tooits motsatt värde.</span><span class="sxs-lookup"><span data-stu-id="7bb00-210">Converts boolean value tooits opposite value.</span></span>

### <a name="parameters"></a><span data-ttu-id="7bb00-211">Parametrar</span><span class="sxs-lookup"><span data-stu-id="7bb00-211">Parameters</span></span>

| <span data-ttu-id="7bb00-212">Parameter</span><span class="sxs-lookup"><span data-stu-id="7bb00-212">Parameter</span></span> | <span data-ttu-id="7bb00-213">Krävs</span><span class="sxs-lookup"><span data-stu-id="7bb00-213">Required</span></span> | <span data-ttu-id="7bb00-214">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-214">Type</span></span> | <span data-ttu-id="7bb00-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7bb00-215">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="7bb00-216">arg1</span><span class="sxs-lookup"><span data-stu-id="7bb00-216">arg1</span></span> |<span data-ttu-id="7bb00-217">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-217">Yes</span></span> |<span data-ttu-id="7bb00-218">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-218">boolean</span></span> |<span data-ttu-id="7bb00-219">hello värdet tooconvert.</span><span class="sxs-lookup"><span data-stu-id="7bb00-219">hello value tooconvert.</span></span> |


### <a name="return-value"></a><span data-ttu-id="7bb00-220">Returvärde</span><span class="sxs-lookup"><span data-stu-id="7bb00-220">Return value</span></span>

<span data-ttu-id="7bb00-221">Returnerar **SANT** när parametern är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="7bb00-221">Returns **True** when parameter is **False**.</span></span> <span data-ttu-id="7bb00-222">Returnerar **FALSKT** när parametern är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="7bb00-222">Returns **False** when parameter is **True**.</span></span>

### <a name="examples"></a><span data-ttu-id="7bb00-223">Exempel</span><span class="sxs-lookup"><span data-stu-id="7bb00-223">Examples</span></span>

<span data-ttu-id="7bb00-224">följande exempel visar hur hello toouse logiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="7bb00-224">hello following example shows how toouse logical functions.</span></span>

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

<span data-ttu-id="7bb00-225">hello utdata från hello föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="7bb00-225">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="7bb00-226">Namn</span><span class="sxs-lookup"><span data-stu-id="7bb00-226">Name</span></span> | <span data-ttu-id="7bb00-227">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-227">Type</span></span> | <span data-ttu-id="7bb00-228">Värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-228">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="7bb00-229">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-229">andExampleOutput</span></span> | <span data-ttu-id="7bb00-230">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-230">Bool</span></span> | <span data-ttu-id="7bb00-231">False</span><span class="sxs-lookup"><span data-stu-id="7bb00-231">False</span></span> |
| <span data-ttu-id="7bb00-232">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-232">orExampleOutput</span></span> | <span data-ttu-id="7bb00-233">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-233">Bool</span></span> | <span data-ttu-id="7bb00-234">True</span><span class="sxs-lookup"><span data-stu-id="7bb00-234">True</span></span> |
| <span data-ttu-id="7bb00-235">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-235">notExampleOutput</span></span> | <span data-ttu-id="7bb00-236">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-236">Bool</span></span> | <span data-ttu-id="7bb00-237">False</span><span class="sxs-lookup"><span data-stu-id="7bb00-237">False</span></span> |

<span data-ttu-id="7bb00-238">hello följande exempel används **inte** med [är lika med](resource-group-template-functions-comparison.md#equals).</span><span class="sxs-lookup"><span data-stu-id="7bb00-238">hello following example uses **not** with [equals](resource-group-template-functions-comparison.md#equals).</span></span>

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

<span data-ttu-id="7bb00-239">hello utdata från hello föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="7bb00-239">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="7bb00-240">Namn</span><span class="sxs-lookup"><span data-stu-id="7bb00-240">Name</span></span> | <span data-ttu-id="7bb00-241">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-241">Type</span></span> | <span data-ttu-id="7bb00-242">Värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-242">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="7bb00-243">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="7bb00-243">checkNotEquals</span></span> | <span data-ttu-id="7bb00-244">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-244">Bool</span></span> | <span data-ttu-id="7bb00-245">True</span><span class="sxs-lookup"><span data-stu-id="7bb00-245">True</span></span> |


## <a name="or"></a><span data-ttu-id="7bb00-246">eller</span><span class="sxs-lookup"><span data-stu-id="7bb00-246">or</span></span>
`or(arg1, arg2)`

<span data-ttu-id="7bb00-247">Kontrollerar om antingen parametervärdet är true.</span><span class="sxs-lookup"><span data-stu-id="7bb00-247">Checks whether either parameter value is true.</span></span>

### <a name="parameters"></a><span data-ttu-id="7bb00-248">Parametrar</span><span class="sxs-lookup"><span data-stu-id="7bb00-248">Parameters</span></span>

| <span data-ttu-id="7bb00-249">Parameter</span><span class="sxs-lookup"><span data-stu-id="7bb00-249">Parameter</span></span> | <span data-ttu-id="7bb00-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="7bb00-250">Required</span></span> | <span data-ttu-id="7bb00-251">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-251">Type</span></span> | <span data-ttu-id="7bb00-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7bb00-252">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="7bb00-253">arg1</span><span class="sxs-lookup"><span data-stu-id="7bb00-253">arg1</span></span> |<span data-ttu-id="7bb00-254">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-254">Yes</span></span> |<span data-ttu-id="7bb00-255">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-255">boolean</span></span> |<span data-ttu-id="7bb00-256">Hej första värde toocheck om är true.</span><span class="sxs-lookup"><span data-stu-id="7bb00-256">hello first value toocheck whether is true.</span></span> |
| <span data-ttu-id="7bb00-257">arg2</span><span class="sxs-lookup"><span data-stu-id="7bb00-257">arg2</span></span> |<span data-ttu-id="7bb00-258">Ja</span><span class="sxs-lookup"><span data-stu-id="7bb00-258">Yes</span></span> |<span data-ttu-id="7bb00-259">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-259">boolean</span></span> |<span data-ttu-id="7bb00-260">Hej andra värdet toocheck om är true.</span><span class="sxs-lookup"><span data-stu-id="7bb00-260">hello second value toocheck whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="7bb00-261">Returvärde</span><span class="sxs-lookup"><span data-stu-id="7bb00-261">Return value</span></span>

<span data-ttu-id="7bb00-262">Returnerar **SANT** om antingen värdet är true, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="7bb00-262">Returns **True** if either value is true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="7bb00-263">Exempel</span><span class="sxs-lookup"><span data-stu-id="7bb00-263">Examples</span></span>

<span data-ttu-id="7bb00-264">följande exempel visar hur hello toouse logiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="7bb00-264">hello following example shows how toouse logical functions.</span></span>

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

<span data-ttu-id="7bb00-265">hello utdata från hello föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="7bb00-265">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="7bb00-266">Namn</span><span class="sxs-lookup"><span data-stu-id="7bb00-266">Name</span></span> | <span data-ttu-id="7bb00-267">Typ</span><span class="sxs-lookup"><span data-stu-id="7bb00-267">Type</span></span> | <span data-ttu-id="7bb00-268">Värde</span><span class="sxs-lookup"><span data-stu-id="7bb00-268">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="7bb00-269">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-269">andExampleOutput</span></span> | <span data-ttu-id="7bb00-270">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-270">Bool</span></span> | <span data-ttu-id="7bb00-271">False</span><span class="sxs-lookup"><span data-stu-id="7bb00-271">False</span></span> |
| <span data-ttu-id="7bb00-272">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-272">orExampleOutput</span></span> | <span data-ttu-id="7bb00-273">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-273">Bool</span></span> | <span data-ttu-id="7bb00-274">True</span><span class="sxs-lookup"><span data-stu-id="7bb00-274">True</span></span> |
| <span data-ttu-id="7bb00-275">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="7bb00-275">notExampleOutput</span></span> | <span data-ttu-id="7bb00-276">bool</span><span class="sxs-lookup"><span data-stu-id="7bb00-276">Bool</span></span> | <span data-ttu-id="7bb00-277">False</span><span class="sxs-lookup"><span data-stu-id="7bb00-277">False</span></span> |


## <a name="next-steps"></a><span data-ttu-id="7bb00-278">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7bb00-278">Next steps</span></span>
* <span data-ttu-id="7bb00-279">En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7bb00-279">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="7bb00-280">flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7bb00-280">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="7bb00-281">tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="7bb00-281">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="7bb00-282">toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="7bb00-282">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

