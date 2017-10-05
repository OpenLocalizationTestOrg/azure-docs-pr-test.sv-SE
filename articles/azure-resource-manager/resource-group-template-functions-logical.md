---
title: Azure Resource Manager Mallfunktioner - logiska | Microsoft Docs
description: "Beskriver funktionerna du använder i en Azure Resource Manager-mall för att fastställa logiska värden."
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
ms.openlocfilehash: 313601ad99cdc12c4b50f5469959d37a9fa70d35
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="f1e35-103">Logiska funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="f1e35-103">Logical functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="f1e35-104">Resource Manager innehåller flera funktioner för att göra jämförelser i dina mallar.</span><span class="sxs-lookup"><span data-stu-id="f1e35-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="f1e35-105">och</span><span class="sxs-lookup"><span data-stu-id="f1e35-105">and</span></span>](#and)
* [<span data-ttu-id="f1e35-106">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-106">bool</span></span>](#bool)
* [<span data-ttu-id="f1e35-107">Om</span><span class="sxs-lookup"><span data-stu-id="f1e35-107">if</span></span>](#if)
* [<span data-ttu-id="f1e35-108">inte</span><span class="sxs-lookup"><span data-stu-id="f1e35-108">not</span></span>](#not)
* [<span data-ttu-id="f1e35-109">eller</span><span class="sxs-lookup"><span data-stu-id="f1e35-109">or</span></span>](#or)

## <a name="and"></a><span data-ttu-id="f1e35-110">och</span><span class="sxs-lookup"><span data-stu-id="f1e35-110">and</span></span>
`and(arg1, arg2)`

<span data-ttu-id="f1e35-111">Kontrollerar om båda parametervärden är true.</span><span class="sxs-lookup"><span data-stu-id="f1e35-111">Checks whether both parameter values are true.</span></span>

### <a name="parameters"></a><span data-ttu-id="f1e35-112">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f1e35-112">Parameters</span></span>

| <span data-ttu-id="f1e35-113">Parameter</span><span class="sxs-lookup"><span data-stu-id="f1e35-113">Parameter</span></span> | <span data-ttu-id="f1e35-114">Krävs</span><span class="sxs-lookup"><span data-stu-id="f1e35-114">Required</span></span> | <span data-ttu-id="f1e35-115">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-115">Type</span></span> | <span data-ttu-id="f1e35-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1e35-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="f1e35-117">arg1</span><span class="sxs-lookup"><span data-stu-id="f1e35-117">arg1</span></span> |<span data-ttu-id="f1e35-118">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-118">Yes</span></span> |<span data-ttu-id="f1e35-119">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-119">boolean</span></span> |<span data-ttu-id="f1e35-120">Det första värdet för att kontrollera om är true.</span><span class="sxs-lookup"><span data-stu-id="f1e35-120">The first value to check whether is true.</span></span> |
| <span data-ttu-id="f1e35-121">arg2</span><span class="sxs-lookup"><span data-stu-id="f1e35-121">arg2</span></span> |<span data-ttu-id="f1e35-122">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-122">Yes</span></span> |<span data-ttu-id="f1e35-123">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-123">boolean</span></span> |<span data-ttu-id="f1e35-124">Det andra värdet för att kontrollera om är true.</span><span class="sxs-lookup"><span data-stu-id="f1e35-124">The second value to check whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="f1e35-125">Returvärde</span><span class="sxs-lookup"><span data-stu-id="f1e35-125">Return value</span></span>

<span data-ttu-id="f1e35-126">Returnerar **SANT** om båda värdena är true, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="f1e35-126">Returns **True** if both values are true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="f1e35-127">Exempel</span><span class="sxs-lookup"><span data-stu-id="f1e35-127">Examples</span></span>

<span data-ttu-id="f1e35-128">I följande exempel visas hur du använder logiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="f1e35-128">The following example shows how to use logical functions.</span></span>

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

<span data-ttu-id="f1e35-129">Utdata från föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="f1e35-129">The output from the preceding example is:</span></span>

| <span data-ttu-id="f1e35-130">Namn</span><span class="sxs-lookup"><span data-stu-id="f1e35-130">Name</span></span> | <span data-ttu-id="f1e35-131">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-131">Type</span></span> | <span data-ttu-id="f1e35-132">Värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-132">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="f1e35-133">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-133">andExampleOutput</span></span> | <span data-ttu-id="f1e35-134">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-134">Bool</span></span> | <span data-ttu-id="f1e35-135">False</span><span class="sxs-lookup"><span data-stu-id="f1e35-135">False</span></span> |
| <span data-ttu-id="f1e35-136">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-136">orExampleOutput</span></span> | <span data-ttu-id="f1e35-137">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-137">Bool</span></span> | <span data-ttu-id="f1e35-138">True</span><span class="sxs-lookup"><span data-stu-id="f1e35-138">True</span></span> |
| <span data-ttu-id="f1e35-139">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-139">notExampleOutput</span></span> | <span data-ttu-id="f1e35-140">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-140">Bool</span></span> | <span data-ttu-id="f1e35-141">False</span><span class="sxs-lookup"><span data-stu-id="f1e35-141">False</span></span> |


## <a name="bool"></a><span data-ttu-id="f1e35-142">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-142">bool</span></span>
`bool(arg1)`

<span data-ttu-id="f1e35-143">Konverterar parametern till ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f1e35-143">Converts the parameter to a boolean.</span></span>

### <a name="parameters"></a><span data-ttu-id="f1e35-144">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f1e35-144">Parameters</span></span>

| <span data-ttu-id="f1e35-145">Parameter</span><span class="sxs-lookup"><span data-stu-id="f1e35-145">Parameter</span></span> | <span data-ttu-id="f1e35-146">Krävs</span><span class="sxs-lookup"><span data-stu-id="f1e35-146">Required</span></span> | <span data-ttu-id="f1e35-147">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-147">Type</span></span> | <span data-ttu-id="f1e35-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1e35-148">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="f1e35-149">arg1</span><span class="sxs-lookup"><span data-stu-id="f1e35-149">arg1</span></span> |<span data-ttu-id="f1e35-150">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-150">Yes</span></span> |<span data-ttu-id="f1e35-151">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="f1e35-151">string or int</span></span> |<span data-ttu-id="f1e35-152">Värdet som ska konverteras till ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f1e35-152">The value to convert to a boolean.</span></span> |

### <a name="return-value"></a><span data-ttu-id="f1e35-153">Returvärde</span><span class="sxs-lookup"><span data-stu-id="f1e35-153">Return value</span></span>
<span data-ttu-id="f1e35-154">Ett booleskt värde för konverterade värdet.</span><span class="sxs-lookup"><span data-stu-id="f1e35-154">A boolean of the converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="f1e35-155">Exempel</span><span class="sxs-lookup"><span data-stu-id="f1e35-155">Examples</span></span>

<span data-ttu-id="f1e35-156">I följande exempel visas hur du använder bool med en sträng eller ett heltal.</span><span class="sxs-lookup"><span data-stu-id="f1e35-156">The following example shows how to use bool with a string or integer.</span></span>

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

<span data-ttu-id="f1e35-157">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="f1e35-157">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="f1e35-158">Namn</span><span class="sxs-lookup"><span data-stu-id="f1e35-158">Name</span></span> | <span data-ttu-id="f1e35-159">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-159">Type</span></span> | <span data-ttu-id="f1e35-160">Värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-160">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="f1e35-161">trueString</span><span class="sxs-lookup"><span data-stu-id="f1e35-161">trueString</span></span> | <span data-ttu-id="f1e35-162">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-162">Bool</span></span> | <span data-ttu-id="f1e35-163">True</span><span class="sxs-lookup"><span data-stu-id="f1e35-163">True</span></span> |
| <span data-ttu-id="f1e35-164">falseString</span><span class="sxs-lookup"><span data-stu-id="f1e35-164">falseString</span></span> | <span data-ttu-id="f1e35-165">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-165">Bool</span></span> | <span data-ttu-id="f1e35-166">False</span><span class="sxs-lookup"><span data-stu-id="f1e35-166">False</span></span> |
| <span data-ttu-id="f1e35-167">trueInt</span><span class="sxs-lookup"><span data-stu-id="f1e35-167">trueInt</span></span> | <span data-ttu-id="f1e35-168">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-168">Bool</span></span> | <span data-ttu-id="f1e35-169">True</span><span class="sxs-lookup"><span data-stu-id="f1e35-169">True</span></span> |
| <span data-ttu-id="f1e35-170">falseInt</span><span class="sxs-lookup"><span data-stu-id="f1e35-170">falseInt</span></span> | <span data-ttu-id="f1e35-171">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-171">Bool</span></span> | <span data-ttu-id="f1e35-172">False</span><span class="sxs-lookup"><span data-stu-id="f1e35-172">False</span></span> |

## <a name="if"></a><span data-ttu-id="f1e35-173">Om</span><span class="sxs-lookup"><span data-stu-id="f1e35-173">if</span></span>
`if(condition, trueValue, falseValue)`

<span data-ttu-id="f1e35-174">Returnerar ett värde baserat på om ett villkor är SANT eller FALSKT.</span><span class="sxs-lookup"><span data-stu-id="f1e35-174">Returns a value based on whether a condition is true or false.</span></span>

### <a name="parameters"></a><span data-ttu-id="f1e35-175">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f1e35-175">Parameters</span></span>

| <span data-ttu-id="f1e35-176">Parameter</span><span class="sxs-lookup"><span data-stu-id="f1e35-176">Parameter</span></span> | <span data-ttu-id="f1e35-177">Krävs</span><span class="sxs-lookup"><span data-stu-id="f1e35-177">Required</span></span> | <span data-ttu-id="f1e35-178">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-178">Type</span></span> | <span data-ttu-id="f1e35-179">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1e35-179">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="f1e35-180">Villkor</span><span class="sxs-lookup"><span data-stu-id="f1e35-180">condition</span></span> |<span data-ttu-id="f1e35-181">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-181">Yes</span></span> |<span data-ttu-id="f1e35-182">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-182">boolean</span></span> |<span data-ttu-id="f1e35-183">Värdet för att kontrollera om det är true.</span><span class="sxs-lookup"><span data-stu-id="f1e35-183">The value to check whether it is true.</span></span> |
| <span data-ttu-id="f1e35-184">trueValue</span><span class="sxs-lookup"><span data-stu-id="f1e35-184">trueValue</span></span> |<span data-ttu-id="f1e35-185">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-185">Yes</span></span> | <span data-ttu-id="f1e35-186">sträng, int, objekt eller matris</span><span class="sxs-lookup"><span data-stu-id="f1e35-186">string, int, object, or array</span></span> |<span data-ttu-id="f1e35-187">Värdet som returneras när villkoret är sant.</span><span class="sxs-lookup"><span data-stu-id="f1e35-187">The value to return when the condition is true.</span></span> |
| <span data-ttu-id="f1e35-188">falseValue</span><span class="sxs-lookup"><span data-stu-id="f1e35-188">falseValue</span></span> |<span data-ttu-id="f1e35-189">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-189">Yes</span></span> | <span data-ttu-id="f1e35-190">sträng, int, objekt eller matris</span><span class="sxs-lookup"><span data-stu-id="f1e35-190">string, int, object, or array</span></span> |<span data-ttu-id="f1e35-191">Värdet som returneras om villkoret är FALSKT.</span><span class="sxs-lookup"><span data-stu-id="f1e35-191">The value to return when the condition is false.</span></span> |

### <a name="return-value"></a><span data-ttu-id="f1e35-192">Returvärde</span><span class="sxs-lookup"><span data-stu-id="f1e35-192">Return value</span></span>

<span data-ttu-id="f1e35-193">Returnerar andra parameter när den första parametern är **SANT**, annars returnerar tredje parametern.</span><span class="sxs-lookup"><span data-stu-id="f1e35-193">Returns second parameter when first parameter is **True**; otherwise, returns third parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="f1e35-194">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="f1e35-194">Remarks</span></span>

<span data-ttu-id="f1e35-195">Du kan använda den här funktionen för att ange en resursegenskap villkorligt.</span><span class="sxs-lookup"><span data-stu-id="f1e35-195">You can use this function to conditionally set a resource property.</span></span> <span data-ttu-id="f1e35-196">I följande exempel är inte en fullständig mall, men den visar relevanta delar för att ange villkorligt tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f1e35-196">The following example is not a full template, but it shows the relevant portions for conditionally setting the availability set.</span></span>

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

### <a name="examples"></a><span data-ttu-id="f1e35-197">Exempel</span><span class="sxs-lookup"><span data-stu-id="f1e35-197">Examples</span></span>

<span data-ttu-id="f1e35-198">I följande exempel visas hur du använder den `if` funktion.</span><span class="sxs-lookup"><span data-stu-id="f1e35-198">The following example shows how to use the `if` function.</span></span>

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

<span data-ttu-id="f1e35-199">Utdata från föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="f1e35-199">The output from the preceding example is:</span></span>

| <span data-ttu-id="f1e35-200">Namn</span><span class="sxs-lookup"><span data-stu-id="f1e35-200">Name</span></span> | <span data-ttu-id="f1e35-201">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-201">Type</span></span> | <span data-ttu-id="f1e35-202">Värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-202">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="f1e35-203">yesOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-203">yesOutput</span></span> | <span data-ttu-id="f1e35-204">Sträng</span><span class="sxs-lookup"><span data-stu-id="f1e35-204">String</span></span> | <span data-ttu-id="f1e35-205">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-205">yes</span></span> |
| <span data-ttu-id="f1e35-206">noOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-206">noOutput</span></span> | <span data-ttu-id="f1e35-207">Sträng</span><span class="sxs-lookup"><span data-stu-id="f1e35-207">String</span></span> | <span data-ttu-id="f1e35-208">Nej</span><span class="sxs-lookup"><span data-stu-id="f1e35-208">no</span></span> |


## <a name="not"></a><span data-ttu-id="f1e35-209">inte</span><span class="sxs-lookup"><span data-stu-id="f1e35-209">not</span></span>
`not(arg1)`

<span data-ttu-id="f1e35-210">Konverterar booleskt värde till motsatt värde.</span><span class="sxs-lookup"><span data-stu-id="f1e35-210">Converts boolean value to its opposite value.</span></span>

### <a name="parameters"></a><span data-ttu-id="f1e35-211">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f1e35-211">Parameters</span></span>

| <span data-ttu-id="f1e35-212">Parameter</span><span class="sxs-lookup"><span data-stu-id="f1e35-212">Parameter</span></span> | <span data-ttu-id="f1e35-213">Krävs</span><span class="sxs-lookup"><span data-stu-id="f1e35-213">Required</span></span> | <span data-ttu-id="f1e35-214">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-214">Type</span></span> | <span data-ttu-id="f1e35-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1e35-215">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="f1e35-216">arg1</span><span class="sxs-lookup"><span data-stu-id="f1e35-216">arg1</span></span> |<span data-ttu-id="f1e35-217">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-217">Yes</span></span> |<span data-ttu-id="f1e35-218">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-218">boolean</span></span> |<span data-ttu-id="f1e35-219">Värdet som ska konverteras.</span><span class="sxs-lookup"><span data-stu-id="f1e35-219">The value to convert.</span></span> |


### <a name="return-value"></a><span data-ttu-id="f1e35-220">Returvärde</span><span class="sxs-lookup"><span data-stu-id="f1e35-220">Return value</span></span>

<span data-ttu-id="f1e35-221">Returnerar **SANT** när parametern är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="f1e35-221">Returns **True** when parameter is **False**.</span></span> <span data-ttu-id="f1e35-222">Returnerar **FALSKT** när parametern är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="f1e35-222">Returns **False** when parameter is **True**.</span></span>

### <a name="examples"></a><span data-ttu-id="f1e35-223">Exempel</span><span class="sxs-lookup"><span data-stu-id="f1e35-223">Examples</span></span>

<span data-ttu-id="f1e35-224">I följande exempel visas hur du använder logiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="f1e35-224">The following example shows how to use logical functions.</span></span>

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

<span data-ttu-id="f1e35-225">Utdata från föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="f1e35-225">The output from the preceding example is:</span></span>

| <span data-ttu-id="f1e35-226">Namn</span><span class="sxs-lookup"><span data-stu-id="f1e35-226">Name</span></span> | <span data-ttu-id="f1e35-227">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-227">Type</span></span> | <span data-ttu-id="f1e35-228">Värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-228">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="f1e35-229">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-229">andExampleOutput</span></span> | <span data-ttu-id="f1e35-230">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-230">Bool</span></span> | <span data-ttu-id="f1e35-231">False</span><span class="sxs-lookup"><span data-stu-id="f1e35-231">False</span></span> |
| <span data-ttu-id="f1e35-232">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-232">orExampleOutput</span></span> | <span data-ttu-id="f1e35-233">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-233">Bool</span></span> | <span data-ttu-id="f1e35-234">True</span><span class="sxs-lookup"><span data-stu-id="f1e35-234">True</span></span> |
| <span data-ttu-id="f1e35-235">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-235">notExampleOutput</span></span> | <span data-ttu-id="f1e35-236">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-236">Bool</span></span> | <span data-ttu-id="f1e35-237">False</span><span class="sxs-lookup"><span data-stu-id="f1e35-237">False</span></span> |

<span data-ttu-id="f1e35-238">I följande exempel används **inte** med [är lika med](resource-group-template-functions-comparison.md#equals).</span><span class="sxs-lookup"><span data-stu-id="f1e35-238">The following example uses **not** with [equals](resource-group-template-functions-comparison.md#equals).</span></span>

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

<span data-ttu-id="f1e35-239">Utdata från föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="f1e35-239">The output from the preceding example is:</span></span>

| <span data-ttu-id="f1e35-240">Namn</span><span class="sxs-lookup"><span data-stu-id="f1e35-240">Name</span></span> | <span data-ttu-id="f1e35-241">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-241">Type</span></span> | <span data-ttu-id="f1e35-242">Värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-242">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="f1e35-243">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="f1e35-243">checkNotEquals</span></span> | <span data-ttu-id="f1e35-244">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-244">Bool</span></span> | <span data-ttu-id="f1e35-245">True</span><span class="sxs-lookup"><span data-stu-id="f1e35-245">True</span></span> |


## <a name="or"></a><span data-ttu-id="f1e35-246">eller</span><span class="sxs-lookup"><span data-stu-id="f1e35-246">or</span></span>
`or(arg1, arg2)`

<span data-ttu-id="f1e35-247">Kontrollerar om antingen parametervärdet är true.</span><span class="sxs-lookup"><span data-stu-id="f1e35-247">Checks whether either parameter value is true.</span></span>

### <a name="parameters"></a><span data-ttu-id="f1e35-248">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f1e35-248">Parameters</span></span>

| <span data-ttu-id="f1e35-249">Parameter</span><span class="sxs-lookup"><span data-stu-id="f1e35-249">Parameter</span></span> | <span data-ttu-id="f1e35-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="f1e35-250">Required</span></span> | <span data-ttu-id="f1e35-251">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-251">Type</span></span> | <span data-ttu-id="f1e35-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1e35-252">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="f1e35-253">arg1</span><span class="sxs-lookup"><span data-stu-id="f1e35-253">arg1</span></span> |<span data-ttu-id="f1e35-254">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-254">Yes</span></span> |<span data-ttu-id="f1e35-255">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-255">boolean</span></span> |<span data-ttu-id="f1e35-256">Det första värdet för att kontrollera om är true.</span><span class="sxs-lookup"><span data-stu-id="f1e35-256">The first value to check whether is true.</span></span> |
| <span data-ttu-id="f1e35-257">arg2</span><span class="sxs-lookup"><span data-stu-id="f1e35-257">arg2</span></span> |<span data-ttu-id="f1e35-258">Ja</span><span class="sxs-lookup"><span data-stu-id="f1e35-258">Yes</span></span> |<span data-ttu-id="f1e35-259">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-259">boolean</span></span> |<span data-ttu-id="f1e35-260">Det andra värdet för att kontrollera om är true.</span><span class="sxs-lookup"><span data-stu-id="f1e35-260">The second value to check whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="f1e35-261">Returvärde</span><span class="sxs-lookup"><span data-stu-id="f1e35-261">Return value</span></span>

<span data-ttu-id="f1e35-262">Returnerar **SANT** om antingen värdet är true, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="f1e35-262">Returns **True** if either value is true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="f1e35-263">Exempel</span><span class="sxs-lookup"><span data-stu-id="f1e35-263">Examples</span></span>

<span data-ttu-id="f1e35-264">I följande exempel visas hur du använder logiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="f1e35-264">The following example shows how to use logical functions.</span></span>

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

<span data-ttu-id="f1e35-265">Utdata från föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="f1e35-265">The output from the preceding example is:</span></span>

| <span data-ttu-id="f1e35-266">Namn</span><span class="sxs-lookup"><span data-stu-id="f1e35-266">Name</span></span> | <span data-ttu-id="f1e35-267">Typ</span><span class="sxs-lookup"><span data-stu-id="f1e35-267">Type</span></span> | <span data-ttu-id="f1e35-268">Värde</span><span class="sxs-lookup"><span data-stu-id="f1e35-268">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="f1e35-269">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-269">andExampleOutput</span></span> | <span data-ttu-id="f1e35-270">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-270">Bool</span></span> | <span data-ttu-id="f1e35-271">False</span><span class="sxs-lookup"><span data-stu-id="f1e35-271">False</span></span> |
| <span data-ttu-id="f1e35-272">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-272">orExampleOutput</span></span> | <span data-ttu-id="f1e35-273">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-273">Bool</span></span> | <span data-ttu-id="f1e35-274">True</span><span class="sxs-lookup"><span data-stu-id="f1e35-274">True</span></span> |
| <span data-ttu-id="f1e35-275">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="f1e35-275">notExampleOutput</span></span> | <span data-ttu-id="f1e35-276">bool</span><span class="sxs-lookup"><span data-stu-id="f1e35-276">Bool</span></span> | <span data-ttu-id="f1e35-277">False</span><span class="sxs-lookup"><span data-stu-id="f1e35-277">False</span></span> |


## <a name="next-steps"></a><span data-ttu-id="f1e35-278">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1e35-278">Next steps</span></span>
* <span data-ttu-id="f1e35-279">En beskrivning av avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f1e35-279">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="f1e35-280">Om du vill slå samman flera mallar, se [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f1e35-280">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="f1e35-281">Iterera ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="f1e35-281">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="f1e35-282">Information om hur du distribuerar mallen som du har skapat finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="f1e35-282">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

