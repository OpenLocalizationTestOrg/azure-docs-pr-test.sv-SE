---
title: Azure Resource Manager Mallfunktioner - numeriska | Microsoft Docs
description: "Beskriver funktionerna du använder i en Azure Resource Manager-mall för att arbeta med siffror."
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
ms.openlocfilehash: ae0261134b8d4a934048f58d6c679a48a904950b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="4d749-103">Numeriska funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="4d749-103">Numeric functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="4d749-104">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med heltal:</span><span class="sxs-lookup"><span data-stu-id="4d749-104">Resource Manager provides the following functions for working with integers:</span></span>

* [<span data-ttu-id="4d749-105">Lägg till</span><span class="sxs-lookup"><span data-stu-id="4d749-105">add</span></span>](#add)
* [<span data-ttu-id="4d749-106">copyIndex</span><span class="sxs-lookup"><span data-stu-id="4d749-106">copyIndex</span></span>](#copyindex)
* [<span data-ttu-id="4d749-107">div</span><span class="sxs-lookup"><span data-stu-id="4d749-107">div</span></span>](#div)
* [<span data-ttu-id="4d749-108">flyttal</span><span class="sxs-lookup"><span data-stu-id="4d749-108">float</span></span>](#float)
* [<span data-ttu-id="4d749-109">int</span><span class="sxs-lookup"><span data-stu-id="4d749-109">int</span></span>](#int)
* [<span data-ttu-id="4d749-110">Min</span><span class="sxs-lookup"><span data-stu-id="4d749-110">min</span></span>](#min)
* [<span data-ttu-id="4d749-111">Max</span><span class="sxs-lookup"><span data-stu-id="4d749-111">max</span></span>](#max)
* [<span data-ttu-id="4d749-112">MOD</span><span class="sxs-lookup"><span data-stu-id="4d749-112">mod</span></span>](#mod)
* [<span data-ttu-id="4d749-113">mul</span><span class="sxs-lookup"><span data-stu-id="4d749-113">mul</span></span>](#mul)
* [<span data-ttu-id="4d749-114">Sub</span><span class="sxs-lookup"><span data-stu-id="4d749-114">sub</span></span>](#sub)

<a id="add" />

## <a name="add"></a><span data-ttu-id="4d749-115">Lägg till</span><span class="sxs-lookup"><span data-stu-id="4d749-115">add</span></span>
`add(operand1, operand2)`

<span data-ttu-id="4d749-116">Returnerar summan av de två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-116">Returns the sum of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-117">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-117">Parameters</span></span>

| <span data-ttu-id="4d749-118">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-118">Parameter</span></span> | <span data-ttu-id="4d749-119">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-119">Required</span></span> | <span data-ttu-id="4d749-120">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-120">Type</span></span> | <span data-ttu-id="4d749-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-121">Description</span></span> |
|:--- |:--- |:--- |:--- | 
|<span data-ttu-id="4d749-122">operand1</span><span class="sxs-lookup"><span data-stu-id="4d749-122">operand1</span></span> |<span data-ttu-id="4d749-123">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-123">Yes</span></span> |<span data-ttu-id="4d749-124">int</span><span class="sxs-lookup"><span data-stu-id="4d749-124">int</span></span> |<span data-ttu-id="4d749-125">Första numret att lägga till.</span><span class="sxs-lookup"><span data-stu-id="4d749-125">First number to add.</span></span> |
|<span data-ttu-id="4d749-126">operand2</span><span class="sxs-lookup"><span data-stu-id="4d749-126">operand2</span></span> |<span data-ttu-id="4d749-127">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-127">Yes</span></span> |<span data-ttu-id="4d749-128">int</span><span class="sxs-lookup"><span data-stu-id="4d749-128">int</span></span> |<span data-ttu-id="4d749-129">Andra talet att lägga till.</span><span class="sxs-lookup"><span data-stu-id="4d749-129">Second number to add.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-130">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-130">Return value</span></span>

<span data-ttu-id="4d749-131">Ett heltal som innehåller summan av parametrar.</span><span class="sxs-lookup"><span data-stu-id="4d749-131">An integer that contains the sum of the parameters.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-132">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-132">Example</span></span>

<span data-ttu-id="4d749-133">I följande exempel läggs två parametrar.</span><span class="sxs-lookup"><span data-stu-id="4d749-133">The following example adds two parameters.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
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

<span data-ttu-id="4d749-134">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="4d749-134">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4d749-135">Namn</span><span class="sxs-lookup"><span data-stu-id="4d749-135">Name</span></span> | <span data-ttu-id="4d749-136">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-136">Type</span></span> | <span data-ttu-id="4d749-137">Värde</span><span class="sxs-lookup"><span data-stu-id="4d749-137">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4d749-138">addResult</span><span class="sxs-lookup"><span data-stu-id="4d749-138">addResult</span></span> | <span data-ttu-id="4d749-139">int</span><span class="sxs-lookup"><span data-stu-id="4d749-139">Int</span></span> | <span data-ttu-id="4d749-140">8</span><span class="sxs-lookup"><span data-stu-id="4d749-140">8</span></span> |

<a id="copyindex" />

## <a name="copyindex"></a><span data-ttu-id="4d749-141">copyIndex</span><span class="sxs-lookup"><span data-stu-id="4d749-141">copyIndex</span></span>
`copyIndex(loopName, offset)`

<span data-ttu-id="4d749-142">Returnerar index för en upprepning loop.</span><span class="sxs-lookup"><span data-stu-id="4d749-142">Returns the index of an iteration loop.</span></span> 

### <a name="parameters"></a><span data-ttu-id="4d749-143">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-143">Parameters</span></span>

| <span data-ttu-id="4d749-144">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-144">Parameter</span></span> | <span data-ttu-id="4d749-145">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-145">Required</span></span> | <span data-ttu-id="4d749-146">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-146">Type</span></span> | <span data-ttu-id="4d749-147">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-147">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-148">loopName</span><span class="sxs-lookup"><span data-stu-id="4d749-148">loopName</span></span> | <span data-ttu-id="4d749-149">Nej</span><span class="sxs-lookup"><span data-stu-id="4d749-149">No</span></span> | <span data-ttu-id="4d749-150">Sträng</span><span class="sxs-lookup"><span data-stu-id="4d749-150">string</span></span> | <span data-ttu-id="4d749-151">Namnet på slingan för att hämta upprepning.</span><span class="sxs-lookup"><span data-stu-id="4d749-151">The name of the loop for getting the iteration.</span></span> |
| <span data-ttu-id="4d749-152">förskjutning</span><span class="sxs-lookup"><span data-stu-id="4d749-152">offset</span></span> |<span data-ttu-id="4d749-153">Nej</span><span class="sxs-lookup"><span data-stu-id="4d749-153">No</span></span> |<span data-ttu-id="4d749-154">int</span><span class="sxs-lookup"><span data-stu-id="4d749-154">int</span></span> |<span data-ttu-id="4d749-155">Siffra och lägger till värdet nollbaserade iteration.</span><span class="sxs-lookup"><span data-stu-id="4d749-155">The number to add to the zero-based iteration value.</span></span> |

### <a name="remarks"></a><span data-ttu-id="4d749-156">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="4d749-156">Remarks</span></span>

<span data-ttu-id="4d749-157">Den här funktionen används alltid med en **kopiera** objekt.</span><span class="sxs-lookup"><span data-stu-id="4d749-157">This function is always used with a **copy** object.</span></span> <span data-ttu-id="4d749-158">Om inget värde har angetts för **offset**, det aktuella värdet för upprepning returneras.</span><span class="sxs-lookup"><span data-stu-id="4d749-158">If no value is provided for **offset**, the current iteration value is returned.</span></span> <span data-ttu-id="4d749-159">Värdet för upprepning börjar på noll.</span><span class="sxs-lookup"><span data-stu-id="4d749-159">The iteration value starts at zero.</span></span>

<span data-ttu-id="4d749-160">Den **loopName** egenskapen kan du ange om copyIndex hänvisar till en resurs iteration eller egenskapen iteration.</span><span class="sxs-lookup"><span data-stu-id="4d749-160">The **loopName** property enables you to specify whether copyIndex is referring to a resource iteration or property iteration.</span></span> <span data-ttu-id="4d749-161">Om inget värde har angetts för **loopName**, den aktuella resursen typen iterationen används.</span><span class="sxs-lookup"><span data-stu-id="4d749-161">If no value is provided for **loopName**, the current resource type iteration is used.</span></span> <span data-ttu-id="4d749-162">Ange ett värde för **loopName** när iteration av en egenskap.</span><span class="sxs-lookup"><span data-stu-id="4d749-162">Provide a value for **loopName** when iterating on a property.</span></span> 
 
<span data-ttu-id="4d749-163">En fullständig beskrivning av hur du använder **copyIndex**, se [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="4d749-163">For a complete description of how you use **copyIndex**, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

### <a name="example"></a><span data-ttu-id="4d749-164">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-164">Example</span></span>

<span data-ttu-id="4d749-165">I följande exempel visas en kopia loop och indexvärdet ingår i namnet.</span><span class="sxs-lookup"><span data-stu-id="4d749-165">The following example shows a copy loop and the index value included in the name.</span></span> 

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

### <a name="return-value"></a><span data-ttu-id="4d749-166">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-166">Return value</span></span>

<span data-ttu-id="4d749-167">Ett heltal som representerar det aktuella indexet för upprepning.</span><span class="sxs-lookup"><span data-stu-id="4d749-167">An integer representing the current index of the iteration.</span></span>

<a id="div" />

## <a name="div"></a><span data-ttu-id="4d749-168">div</span><span class="sxs-lookup"><span data-stu-id="4d749-168">div</span></span>
`div(operand1, operand2)`

<span data-ttu-id="4d749-169">Returnerar heltalsdivisionen av två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-169">Returns the integer division of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-170">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-170">Parameters</span></span>

| <span data-ttu-id="4d749-171">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-171">Parameter</span></span> | <span data-ttu-id="4d749-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-172">Required</span></span> | <span data-ttu-id="4d749-173">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-173">Type</span></span> | <span data-ttu-id="4d749-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-174">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-175">operand1</span><span class="sxs-lookup"><span data-stu-id="4d749-175">operand1</span></span> |<span data-ttu-id="4d749-176">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-176">Yes</span></span> |<span data-ttu-id="4d749-177">int</span><span class="sxs-lookup"><span data-stu-id="4d749-177">int</span></span> |<span data-ttu-id="4d749-178">Det tal som delas.</span><span class="sxs-lookup"><span data-stu-id="4d749-178">The number being divided.</span></span> |
| <span data-ttu-id="4d749-179">operand2</span><span class="sxs-lookup"><span data-stu-id="4d749-179">operand2</span></span> |<span data-ttu-id="4d749-180">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-180">Yes</span></span> |<span data-ttu-id="4d749-181">int</span><span class="sxs-lookup"><span data-stu-id="4d749-181">int</span></span> |<span data-ttu-id="4d749-182">Det tal som används för att dela upp.</span><span class="sxs-lookup"><span data-stu-id="4d749-182">The number that is used to divide.</span></span> <span data-ttu-id="4d749-183">Det går inte att vara 0.</span><span class="sxs-lookup"><span data-stu-id="4d749-183">Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-184">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-184">Return value</span></span>

<span data-ttu-id="4d749-185">Ett heltal som representerar delningen.</span><span class="sxs-lookup"><span data-stu-id="4d749-185">An integer representing the division.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-186">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-186">Example</span></span>

<span data-ttu-id="4d749-187">I följande exempel delar upp en parameter av en annan parametern.</span><span class="sxs-lookup"><span data-stu-id="4d749-187">The following example divides one parameter by another parameter.</span></span>

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
                "description": "Integer used to divide"
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

<span data-ttu-id="4d749-188">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="4d749-188">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4d749-189">Namn</span><span class="sxs-lookup"><span data-stu-id="4d749-189">Name</span></span> | <span data-ttu-id="4d749-190">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-190">Type</span></span> | <span data-ttu-id="4d749-191">Värde</span><span class="sxs-lookup"><span data-stu-id="4d749-191">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4d749-192">divResult</span><span class="sxs-lookup"><span data-stu-id="4d749-192">divResult</span></span> | <span data-ttu-id="4d749-193">int</span><span class="sxs-lookup"><span data-stu-id="4d749-193">Int</span></span> | <span data-ttu-id="4d749-194">2</span><span class="sxs-lookup"><span data-stu-id="4d749-194">2</span></span> |

<a id="float" />

## <a name="float"></a><span data-ttu-id="4d749-195">flyttal</span><span class="sxs-lookup"><span data-stu-id="4d749-195">float</span></span>
`float(arg1)`

<span data-ttu-id="4d749-196">Konverterar värdet till ett flyttal peka nummer.</span><span class="sxs-lookup"><span data-stu-id="4d749-196">Converts the value to a floating point number.</span></span> <span data-ttu-id="4d749-197">Du kan bara använda den här funktionen vid sändning av anpassade parametrar till ett program, till exempel en Logikapp.</span><span class="sxs-lookup"><span data-stu-id="4d749-197">You only use this function when passing custom parameters to an application, such as a Logic App.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-198">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-198">Parameters</span></span>

| <span data-ttu-id="4d749-199">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-199">Parameter</span></span> | <span data-ttu-id="4d749-200">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-200">Required</span></span> | <span data-ttu-id="4d749-201">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-201">Type</span></span> | <span data-ttu-id="4d749-202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-202">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-203">arg1</span><span class="sxs-lookup"><span data-stu-id="4d749-203">arg1</span></span> |<span data-ttu-id="4d749-204">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-204">Yes</span></span> |<span data-ttu-id="4d749-205">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="4d749-205">string or int</span></span> |<span data-ttu-id="4d749-206">Värdet som ska konverteras till ett flyttal peka nummer.</span><span class="sxs-lookup"><span data-stu-id="4d749-206">The value to convert to a floating point number.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-207">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-207">Return value</span></span>
<span data-ttu-id="4d749-208">En flytande peka nummer.</span><span class="sxs-lookup"><span data-stu-id="4d749-208">A floating point number.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-209">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-209">Example</span></span>

<span data-ttu-id="4d749-210">I följande exempel visas hur du använder float för att skicka parametrar till en Logikapp:</span><span class="sxs-lookup"><span data-stu-id="4d749-210">The following example shows how to use float to pass parameters to a Logic App:</span></span>

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

## <a name="int"></a><span data-ttu-id="4d749-211">int</span><span class="sxs-lookup"><span data-stu-id="4d749-211">int</span></span>
`int(valueToConvert)`

<span data-ttu-id="4d749-212">Konverterar det angivna värdet till ett heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-212">Converts the specified value to an integer.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-213">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-213">Parameters</span></span>

| <span data-ttu-id="4d749-214">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-214">Parameter</span></span> | <span data-ttu-id="4d749-215">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-215">Required</span></span> | <span data-ttu-id="4d749-216">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-216">Type</span></span> | <span data-ttu-id="4d749-217">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-217">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-218">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="4d749-218">valueToConvert</span></span> |<span data-ttu-id="4d749-219">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-219">Yes</span></span> |<span data-ttu-id="4d749-220">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="4d749-220">string or int</span></span> |<span data-ttu-id="4d749-221">Värdet som ska konverteras till ett heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-221">The value to convert to an integer.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-222">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-222">Return value</span></span>

<span data-ttu-id="4d749-223">Ett heltal av det konvertera värdet.</span><span class="sxs-lookup"><span data-stu-id="4d749-223">An integer of the converted value.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-224">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-224">Example</span></span>

<span data-ttu-id="4d749-225">I följande exempel konverterar som användaren tillhandahållit parametervärdet heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-225">The following example converts the user-provided parameter value to integer.</span></span>

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

<span data-ttu-id="4d749-226">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="4d749-226">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4d749-227">Namn</span><span class="sxs-lookup"><span data-stu-id="4d749-227">Name</span></span> | <span data-ttu-id="4d749-228">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-228">Type</span></span> | <span data-ttu-id="4d749-229">Värde</span><span class="sxs-lookup"><span data-stu-id="4d749-229">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4d749-230">intResult</span><span class="sxs-lookup"><span data-stu-id="4d749-230">intResult</span></span> | <span data-ttu-id="4d749-231">int</span><span class="sxs-lookup"><span data-stu-id="4d749-231">Int</span></span> | <span data-ttu-id="4d749-232">4</span><span class="sxs-lookup"><span data-stu-id="4d749-232">4</span></span> |


<a id="min" />

## <a name="min"></a><span data-ttu-id="4d749-233">min.</span><span class="sxs-lookup"><span data-stu-id="4d749-233">min</span></span>
`min (arg1)`

<span data-ttu-id="4d749-234">Returnerar det lägsta värdet från en heltalsmatris eller en kommaavgränsad lista med heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-234">Returns the minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-235">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-235">Parameters</span></span>

| <span data-ttu-id="4d749-236">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-236">Parameter</span></span> | <span data-ttu-id="4d749-237">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-237">Required</span></span> | <span data-ttu-id="4d749-238">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-238">Type</span></span> | <span data-ttu-id="4d749-239">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-239">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-240">arg1</span><span class="sxs-lookup"><span data-stu-id="4d749-240">arg1</span></span> |<span data-ttu-id="4d749-241">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-241">Yes</span></span> |<span data-ttu-id="4d749-242">matris med heltal eller en kommaavgränsad lista med heltal</span><span class="sxs-lookup"><span data-stu-id="4d749-242">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="4d749-243">Samlingen för att hämta det minsta värdet.</span><span class="sxs-lookup"><span data-stu-id="4d749-243">The collection to get the minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-244">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-244">Return value</span></span>

<span data-ttu-id="4d749-245">Ett heltal som representerar minimivärdet från samlingen.</span><span class="sxs-lookup"><span data-stu-id="4d749-245">An integer representing minimum value from the collection.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-246">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-246">Example</span></span>

<span data-ttu-id="4d749-247">I följande exempel visas hur du använder min med en matris och en lista med heltal:</span><span class="sxs-lookup"><span data-stu-id="4d749-247">The following example shows how to use min with an array and a list of integers:</span></span>

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

<span data-ttu-id="4d749-248">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="4d749-248">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4d749-249">Namn</span><span class="sxs-lookup"><span data-stu-id="4d749-249">Name</span></span> | <span data-ttu-id="4d749-250">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-250">Type</span></span> | <span data-ttu-id="4d749-251">Värde</span><span class="sxs-lookup"><span data-stu-id="4d749-251">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4d749-252">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="4d749-252">arrayOutput</span></span> | <span data-ttu-id="4d749-253">int</span><span class="sxs-lookup"><span data-stu-id="4d749-253">Int</span></span> | <span data-ttu-id="4d749-254">0</span><span class="sxs-lookup"><span data-stu-id="4d749-254">0</span></span> |
| <span data-ttu-id="4d749-255">intOutput</span><span class="sxs-lookup"><span data-stu-id="4d749-255">intOutput</span></span> | <span data-ttu-id="4d749-256">int</span><span class="sxs-lookup"><span data-stu-id="4d749-256">Int</span></span> | <span data-ttu-id="4d749-257">0</span><span class="sxs-lookup"><span data-stu-id="4d749-257">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="4d749-258">Max</span><span class="sxs-lookup"><span data-stu-id="4d749-258">max</span></span>
`max (arg1)`

<span data-ttu-id="4d749-259">Returnerar det största värdet från en matris av heltal eller en kommaavgränsad lista med heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-259">Returns the maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-260">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-260">Parameters</span></span>

| <span data-ttu-id="4d749-261">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-261">Parameter</span></span> | <span data-ttu-id="4d749-262">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-262">Required</span></span> | <span data-ttu-id="4d749-263">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-263">Type</span></span> | <span data-ttu-id="4d749-264">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-264">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-265">arg1</span><span class="sxs-lookup"><span data-stu-id="4d749-265">arg1</span></span> |<span data-ttu-id="4d749-266">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-266">Yes</span></span> |<span data-ttu-id="4d749-267">matris med heltal eller en kommaavgränsad lista med heltal</span><span class="sxs-lookup"><span data-stu-id="4d749-267">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="4d749-268">Samlingen för att hämta det högsta värdet.</span><span class="sxs-lookup"><span data-stu-id="4d749-268">The collection to get the maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-269">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-269">Return value</span></span>

<span data-ttu-id="4d749-270">Ett heltal som representerar det högsta värdet från samlingen.</span><span class="sxs-lookup"><span data-stu-id="4d749-270">An integer representing the maximum value from the collection.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-271">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-271">Example</span></span>

<span data-ttu-id="4d749-272">I följande exempel visas hur du använder max med en matris och en lista med heltal:</span><span class="sxs-lookup"><span data-stu-id="4d749-272">The following example shows how to use max with an array and a list of integers:</span></span>

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

<span data-ttu-id="4d749-273">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="4d749-273">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4d749-274">Namn</span><span class="sxs-lookup"><span data-stu-id="4d749-274">Name</span></span> | <span data-ttu-id="4d749-275">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-275">Type</span></span> | <span data-ttu-id="4d749-276">Värde</span><span class="sxs-lookup"><span data-stu-id="4d749-276">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4d749-277">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="4d749-277">arrayOutput</span></span> | <span data-ttu-id="4d749-278">int</span><span class="sxs-lookup"><span data-stu-id="4d749-278">Int</span></span> | <span data-ttu-id="4d749-279">5</span><span class="sxs-lookup"><span data-stu-id="4d749-279">5</span></span> |
| <span data-ttu-id="4d749-280">intOutput</span><span class="sxs-lookup"><span data-stu-id="4d749-280">intOutput</span></span> | <span data-ttu-id="4d749-281">int</span><span class="sxs-lookup"><span data-stu-id="4d749-281">Int</span></span> | <span data-ttu-id="4d749-282">5</span><span class="sxs-lookup"><span data-stu-id="4d749-282">5</span></span> |

<a id="mod" />

## <a name="mod"></a><span data-ttu-id="4d749-283">MOD</span><span class="sxs-lookup"><span data-stu-id="4d749-283">mod</span></span>
`mod(operand1, operand2)`

<span data-ttu-id="4d749-284">Returnerar resten av Heltalsdivision med två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-284">Returns the remainder of the integer division using the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-285">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-285">Parameters</span></span>

| <span data-ttu-id="4d749-286">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-286">Parameter</span></span> | <span data-ttu-id="4d749-287">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-287">Required</span></span> | <span data-ttu-id="4d749-288">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-288">Type</span></span> | <span data-ttu-id="4d749-289">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-289">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-290">operand1</span><span class="sxs-lookup"><span data-stu-id="4d749-290">operand1</span></span> |<span data-ttu-id="4d749-291">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-291">Yes</span></span> |<span data-ttu-id="4d749-292">int</span><span class="sxs-lookup"><span data-stu-id="4d749-292">int</span></span> |<span data-ttu-id="4d749-293">Det tal som delas.</span><span class="sxs-lookup"><span data-stu-id="4d749-293">The number being divided.</span></span> |
| <span data-ttu-id="4d749-294">operand2</span><span class="sxs-lookup"><span data-stu-id="4d749-294">operand2</span></span> |<span data-ttu-id="4d749-295">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-295">Yes</span></span> |<span data-ttu-id="4d749-296">int</span><span class="sxs-lookup"><span data-stu-id="4d749-296">int</span></span> |<span data-ttu-id="4d749-297">Talet som används för att dela upp, får inte vara 0.</span><span class="sxs-lookup"><span data-stu-id="4d749-297">The number that is used to divide, Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-298">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-298">Return value</span></span>
<span data-ttu-id="4d749-299">Ett heltal som representerar resten.</span><span class="sxs-lookup"><span data-stu-id="4d749-299">An integer representing the remainder.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-300">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-300">Example</span></span>

<span data-ttu-id="4d749-301">I följande exempel returnerar resten av att dividera en parameter av en annan parametern.</span><span class="sxs-lookup"><span data-stu-id="4d749-301">The following example returns the remainder of dividing one parameter by another parameter.</span></span>

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
                "description": "Integer used to divide"
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

<span data-ttu-id="4d749-302">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="4d749-302">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4d749-303">Namn</span><span class="sxs-lookup"><span data-stu-id="4d749-303">Name</span></span> | <span data-ttu-id="4d749-304">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-304">Type</span></span> | <span data-ttu-id="4d749-305">Värde</span><span class="sxs-lookup"><span data-stu-id="4d749-305">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4d749-306">modResult</span><span class="sxs-lookup"><span data-stu-id="4d749-306">modResult</span></span> | <span data-ttu-id="4d749-307">int</span><span class="sxs-lookup"><span data-stu-id="4d749-307">Int</span></span> | <span data-ttu-id="4d749-308">1</span><span class="sxs-lookup"><span data-stu-id="4d749-308">1</span></span> |

<a id="mul" />

## <a name="mul"></a><span data-ttu-id="4d749-309">mul</span><span class="sxs-lookup"><span data-stu-id="4d749-309">mul</span></span>
`mul(operand1, operand2)`

<span data-ttu-id="4d749-310">Returnerar multiplicering av två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-310">Returns the multiplication of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-311">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-311">Parameters</span></span>

| <span data-ttu-id="4d749-312">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-312">Parameter</span></span> | <span data-ttu-id="4d749-313">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-313">Required</span></span> | <span data-ttu-id="4d749-314">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-314">Type</span></span> | <span data-ttu-id="4d749-315">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-315">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-316">operand1</span><span class="sxs-lookup"><span data-stu-id="4d749-316">operand1</span></span> |<span data-ttu-id="4d749-317">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-317">Yes</span></span> |<span data-ttu-id="4d749-318">int</span><span class="sxs-lookup"><span data-stu-id="4d749-318">int</span></span> |<span data-ttu-id="4d749-319">Första talet som multipliceras.</span><span class="sxs-lookup"><span data-stu-id="4d749-319">First number to multiply.</span></span> |
| <span data-ttu-id="4d749-320">operand2</span><span class="sxs-lookup"><span data-stu-id="4d749-320">operand2</span></span> |<span data-ttu-id="4d749-321">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-321">Yes</span></span> |<span data-ttu-id="4d749-322">int</span><span class="sxs-lookup"><span data-stu-id="4d749-322">int</span></span> |<span data-ttu-id="4d749-323">Andra talet som multipliceras.</span><span class="sxs-lookup"><span data-stu-id="4d749-323">Second number to multiply.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-324">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-324">Return value</span></span>

<span data-ttu-id="4d749-325">Ett heltal som representerar multiplikationen.</span><span class="sxs-lookup"><span data-stu-id="4d749-325">An integer representing the multiplication.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-326">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-326">Example</span></span>

<span data-ttu-id="4d749-327">I följande exempel multiplicerar en parameter av en annan parametern.</span><span class="sxs-lookup"><span data-stu-id="4d749-327">The following example multiplies one parameter by another parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
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

<span data-ttu-id="4d749-328">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="4d749-328">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4d749-329">Namn</span><span class="sxs-lookup"><span data-stu-id="4d749-329">Name</span></span> | <span data-ttu-id="4d749-330">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-330">Type</span></span> | <span data-ttu-id="4d749-331">Värde</span><span class="sxs-lookup"><span data-stu-id="4d749-331">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4d749-332">mulResult</span><span class="sxs-lookup"><span data-stu-id="4d749-332">mulResult</span></span> | <span data-ttu-id="4d749-333">int</span><span class="sxs-lookup"><span data-stu-id="4d749-333">Int</span></span> | <span data-ttu-id="4d749-334">15</span><span class="sxs-lookup"><span data-stu-id="4d749-334">15</span></span> |

<a id="sub" />

## <a name="sub"></a><span data-ttu-id="4d749-335">Sub</span><span class="sxs-lookup"><span data-stu-id="4d749-335">sub</span></span>
`sub(operand1, operand2)`

<span data-ttu-id="4d749-336">Returnerar subtraktion av två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="4d749-336">Returns the subtraction of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="4d749-337">Parametrar</span><span class="sxs-lookup"><span data-stu-id="4d749-337">Parameters</span></span>

| <span data-ttu-id="4d749-338">Parameter</span><span class="sxs-lookup"><span data-stu-id="4d749-338">Parameter</span></span> | <span data-ttu-id="4d749-339">Krävs</span><span class="sxs-lookup"><span data-stu-id="4d749-339">Required</span></span> | <span data-ttu-id="4d749-340">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-340">Type</span></span> | <span data-ttu-id="4d749-341">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d749-341">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4d749-342">operand1</span><span class="sxs-lookup"><span data-stu-id="4d749-342">operand1</span></span> |<span data-ttu-id="4d749-343">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-343">Yes</span></span> |<span data-ttu-id="4d749-344">int</span><span class="sxs-lookup"><span data-stu-id="4d749-344">int</span></span> |<span data-ttu-id="4d749-345">Talet som subtraktionen från.</span><span class="sxs-lookup"><span data-stu-id="4d749-345">The number that is subtracted from.</span></span> |
| <span data-ttu-id="4d749-346">operand2</span><span class="sxs-lookup"><span data-stu-id="4d749-346">operand2</span></span> |<span data-ttu-id="4d749-347">Ja</span><span class="sxs-lookup"><span data-stu-id="4d749-347">Yes</span></span> |<span data-ttu-id="4d749-348">int</span><span class="sxs-lookup"><span data-stu-id="4d749-348">int</span></span> |<span data-ttu-id="4d749-349">Numret som subtraheras.</span><span class="sxs-lookup"><span data-stu-id="4d749-349">The number that is subtracted.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4d749-350">Returvärde</span><span class="sxs-lookup"><span data-stu-id="4d749-350">Return value</span></span>
<span data-ttu-id="4d749-351">Ett heltal som representerar subtraktion.</span><span class="sxs-lookup"><span data-stu-id="4d749-351">An integer representing the subtraction.</span></span>

### <a name="example"></a><span data-ttu-id="4d749-352">Exempel</span><span class="sxs-lookup"><span data-stu-id="4d749-352">Example</span></span>

<span data-ttu-id="4d749-353">I följande exempel tar bort en parameter från en annan parameter.</span><span class="sxs-lookup"><span data-stu-id="4d749-353">The following example subtracts one parameter from another parameter.</span></span>

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
                "description": "Integer to subtract"
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

<span data-ttu-id="4d749-354">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="4d749-354">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4d749-355">Namn</span><span class="sxs-lookup"><span data-stu-id="4d749-355">Name</span></span> | <span data-ttu-id="4d749-356">Typ</span><span class="sxs-lookup"><span data-stu-id="4d749-356">Type</span></span> | <span data-ttu-id="4d749-357">Värde</span><span class="sxs-lookup"><span data-stu-id="4d749-357">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4d749-358">subResult</span><span class="sxs-lookup"><span data-stu-id="4d749-358">subResult</span></span> | <span data-ttu-id="4d749-359">int</span><span class="sxs-lookup"><span data-stu-id="4d749-359">Int</span></span> | <span data-ttu-id="4d749-360">4</span><span class="sxs-lookup"><span data-stu-id="4d749-360">4</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4d749-361">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d749-361">Next steps</span></span>
* <span data-ttu-id="4d749-362">En beskrivning av avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4d749-362">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="4d749-363">Om du vill slå samman flera mallar, se [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4d749-363">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="4d749-364">Iterera ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="4d749-364">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="4d749-365">Information om hur du distribuerar mallen som du har skapat finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="4d749-365">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

