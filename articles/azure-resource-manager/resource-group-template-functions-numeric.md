---
title: aaaAzure Resource Manager Mallfunktioner - numeriska | Microsoft Docs
description: Beskriver hello funktioner toouse i en Azure Resource Manager mallen toowork med siffror.
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
ms.openlocfilehash: 855d5b354d094b9815edc160e3d72efbfd36ba77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="9bd93-103">Numeriska funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="9bd93-103">Numeric functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="9bd93-104">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med heltal hello:</span><span class="sxs-lookup"><span data-stu-id="9bd93-104">Resource Manager provides hello following functions for working with integers:</span></span>

* [<span data-ttu-id="9bd93-105">Lägg till</span><span class="sxs-lookup"><span data-stu-id="9bd93-105">add</span></span>](#add)
* [<span data-ttu-id="9bd93-106">copyIndex</span><span class="sxs-lookup"><span data-stu-id="9bd93-106">copyIndex</span></span>](#copyindex)
* [<span data-ttu-id="9bd93-107">div</span><span class="sxs-lookup"><span data-stu-id="9bd93-107">div</span></span>](#div)
* [<span data-ttu-id="9bd93-108">flyttal</span><span class="sxs-lookup"><span data-stu-id="9bd93-108">float</span></span>](#float)
* [<span data-ttu-id="9bd93-109">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-109">int</span></span>](#int)
* [<span data-ttu-id="9bd93-110">Min</span><span class="sxs-lookup"><span data-stu-id="9bd93-110">min</span></span>](#min)
* [<span data-ttu-id="9bd93-111">Max</span><span class="sxs-lookup"><span data-stu-id="9bd93-111">max</span></span>](#max)
* [<span data-ttu-id="9bd93-112">MOD</span><span class="sxs-lookup"><span data-stu-id="9bd93-112">mod</span></span>](#mod)
* [<span data-ttu-id="9bd93-113">mul</span><span class="sxs-lookup"><span data-stu-id="9bd93-113">mul</span></span>](#mul)
* [<span data-ttu-id="9bd93-114">Sub</span><span class="sxs-lookup"><span data-stu-id="9bd93-114">sub</span></span>](#sub)

<a id="add" />

## <a name="add"></a><span data-ttu-id="9bd93-115">Lägg till</span><span class="sxs-lookup"><span data-stu-id="9bd93-115">add</span></span>
`add(operand1, operand2)`

<span data-ttu-id="9bd93-116">Returnerar hello summan av hello två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-116">Returns hello sum of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-117">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-117">Parameters</span></span>

| <span data-ttu-id="9bd93-118">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-118">Parameter</span></span> | <span data-ttu-id="9bd93-119">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-119">Required</span></span> | <span data-ttu-id="9bd93-120">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-120">Type</span></span> | <span data-ttu-id="9bd93-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-121">Description</span></span> |
|:--- |:--- |:--- |:--- | 
|<span data-ttu-id="9bd93-122">operand1</span><span class="sxs-lookup"><span data-stu-id="9bd93-122">operand1</span></span> |<span data-ttu-id="9bd93-123">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-123">Yes</span></span> |<span data-ttu-id="9bd93-124">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-124">int</span></span> |<span data-ttu-id="9bd93-125">Första nummer tooadd.</span><span class="sxs-lookup"><span data-stu-id="9bd93-125">First number tooadd.</span></span> |
|<span data-ttu-id="9bd93-126">operand2</span><span class="sxs-lookup"><span data-stu-id="9bd93-126">operand2</span></span> |<span data-ttu-id="9bd93-127">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-127">Yes</span></span> |<span data-ttu-id="9bd93-128">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-128">int</span></span> |<span data-ttu-id="9bd93-129">Andra nummer tooadd.</span><span class="sxs-lookup"><span data-stu-id="9bd93-129">Second number tooadd.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-130">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-130">Return value</span></span>

<span data-ttu-id="9bd93-131">Ett heltal som innehåller hello summan av hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="9bd93-131">An integer that contains hello sum of hello parameters.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-132">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-132">Example</span></span>

<span data-ttu-id="9bd93-133">hello följande exempel lägger till två parametrar.</span><span class="sxs-lookup"><span data-stu-id="9bd93-133">hello following example adds two parameters.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer tooadd"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer tooadd"
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

<span data-ttu-id="9bd93-134">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="9bd93-134">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="9bd93-135">Namn</span><span class="sxs-lookup"><span data-stu-id="9bd93-135">Name</span></span> | <span data-ttu-id="9bd93-136">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-136">Type</span></span> | <span data-ttu-id="9bd93-137">Värde</span><span class="sxs-lookup"><span data-stu-id="9bd93-137">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="9bd93-138">addResult</span><span class="sxs-lookup"><span data-stu-id="9bd93-138">addResult</span></span> | <span data-ttu-id="9bd93-139">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-139">Int</span></span> | <span data-ttu-id="9bd93-140">8</span><span class="sxs-lookup"><span data-stu-id="9bd93-140">8</span></span> |

<a id="copyindex" />

## <a name="copyindex"></a><span data-ttu-id="9bd93-141">copyIndex</span><span class="sxs-lookup"><span data-stu-id="9bd93-141">copyIndex</span></span>
`copyIndex(loopName, offset)`

<span data-ttu-id="9bd93-142">Returnerar hello index för en upprepning loop.</span><span class="sxs-lookup"><span data-stu-id="9bd93-142">Returns hello index of an iteration loop.</span></span> 

### <a name="parameters"></a><span data-ttu-id="9bd93-143">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-143">Parameters</span></span>

| <span data-ttu-id="9bd93-144">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-144">Parameter</span></span> | <span data-ttu-id="9bd93-145">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-145">Required</span></span> | <span data-ttu-id="9bd93-146">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-146">Type</span></span> | <span data-ttu-id="9bd93-147">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-147">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-148">loopName</span><span class="sxs-lookup"><span data-stu-id="9bd93-148">loopName</span></span> | <span data-ttu-id="9bd93-149">Nej</span><span class="sxs-lookup"><span data-stu-id="9bd93-149">No</span></span> | <span data-ttu-id="9bd93-150">Sträng</span><span class="sxs-lookup"><span data-stu-id="9bd93-150">string</span></span> | <span data-ttu-id="9bd93-151">hello namnet på hello-slinga för att hämta hello iteration.</span><span class="sxs-lookup"><span data-stu-id="9bd93-151">hello name of hello loop for getting hello iteration.</span></span> |
| <span data-ttu-id="9bd93-152">förskjutning</span><span class="sxs-lookup"><span data-stu-id="9bd93-152">offset</span></span> |<span data-ttu-id="9bd93-153">Nej</span><span class="sxs-lookup"><span data-stu-id="9bd93-153">No</span></span> |<span data-ttu-id="9bd93-154">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-154">int</span></span> |<span data-ttu-id="9bd93-155">Hej tooadd toohello nollbaserade iteration siffra.</span><span class="sxs-lookup"><span data-stu-id="9bd93-155">hello number tooadd toohello zero-based iteration value.</span></span> |

### <a name="remarks"></a><span data-ttu-id="9bd93-156">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="9bd93-156">Remarks</span></span>

<span data-ttu-id="9bd93-157">Den här funktionen används alltid med en **kopiera** objekt.</span><span class="sxs-lookup"><span data-stu-id="9bd93-157">This function is always used with a **copy** object.</span></span> <span data-ttu-id="9bd93-158">Om inget värde har angetts för **offset**, hello aktuella iteration värdet returneras.</span><span class="sxs-lookup"><span data-stu-id="9bd93-158">If no value is provided for **offset**, hello current iteration value is returned.</span></span> <span data-ttu-id="9bd93-159">hello iteration värdet börjar på noll.</span><span class="sxs-lookup"><span data-stu-id="9bd93-159">hello iteration value starts at zero.</span></span>

<span data-ttu-id="9bd93-160">Hej **loopName** egenskapen gör att du toospecify om copyIndex hänvisar tooa resurs iteration eller egenskapen iteration.</span><span class="sxs-lookup"><span data-stu-id="9bd93-160">hello **loopName** property enables you toospecify whether copyIndex is referring tooa resource iteration or property iteration.</span></span> <span data-ttu-id="9bd93-161">Om inget värde har angetts för **loopName**, hello aktuella resurs typen iteration används.</span><span class="sxs-lookup"><span data-stu-id="9bd93-161">If no value is provided for **loopName**, hello current resource type iteration is used.</span></span> <span data-ttu-id="9bd93-162">Ange ett värde för **loopName** när iteration av en egenskap.</span><span class="sxs-lookup"><span data-stu-id="9bd93-162">Provide a value for **loopName** when iterating on a property.</span></span> 
 
<span data-ttu-id="9bd93-163">En fullständig beskrivning av hur du använder **copyIndex**, se [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="9bd93-163">For a complete description of how you use **copyIndex**, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-164">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-164">Example</span></span>

<span data-ttu-id="9bd93-165">hello visas följande exempel en kopia loop och hello indexvärde ingå i hello namn.</span><span class="sxs-lookup"><span data-stu-id="9bd93-165">hello following example shows a copy loop and hello index value included in hello name.</span></span> 

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

### <a name="return-value"></a><span data-ttu-id="9bd93-166">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-166">Return value</span></span>

<span data-ttu-id="9bd93-167">Ett heltal som representerar hello aktuella indexet för hello iteration.</span><span class="sxs-lookup"><span data-stu-id="9bd93-167">An integer representing hello current index of hello iteration.</span></span>

<a id="div" />

## <a name="div"></a><span data-ttu-id="9bd93-168">div</span><span class="sxs-lookup"><span data-stu-id="9bd93-168">div</span></span>
`div(operand1, operand2)`

<span data-ttu-id="9bd93-169">Returnerar hello Heltalsdivision med hello två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-169">Returns hello integer division of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-170">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-170">Parameters</span></span>

| <span data-ttu-id="9bd93-171">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-171">Parameter</span></span> | <span data-ttu-id="9bd93-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-172">Required</span></span> | <span data-ttu-id="9bd93-173">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-173">Type</span></span> | <span data-ttu-id="9bd93-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-174">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-175">operand1</span><span class="sxs-lookup"><span data-stu-id="9bd93-175">operand1</span></span> |<span data-ttu-id="9bd93-176">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-176">Yes</span></span> |<span data-ttu-id="9bd93-177">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-177">int</span></span> |<span data-ttu-id="9bd93-178">hello nummer delas.</span><span class="sxs-lookup"><span data-stu-id="9bd93-178">hello number being divided.</span></span> |
| <span data-ttu-id="9bd93-179">operand2</span><span class="sxs-lookup"><span data-stu-id="9bd93-179">operand2</span></span> |<span data-ttu-id="9bd93-180">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-180">Yes</span></span> |<span data-ttu-id="9bd93-181">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-181">int</span></span> |<span data-ttu-id="9bd93-182">hello-nummer som används toodivide.</span><span class="sxs-lookup"><span data-stu-id="9bd93-182">hello number that is used toodivide.</span></span> <span data-ttu-id="9bd93-183">Det går inte att vara 0.</span><span class="sxs-lookup"><span data-stu-id="9bd93-183">Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-184">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-184">Return value</span></span>

<span data-ttu-id="9bd93-185">Ett heltal som representerar hello division.</span><span class="sxs-lookup"><span data-stu-id="9bd93-185">An integer representing hello division.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-186">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-186">Example</span></span>

<span data-ttu-id="9bd93-187">följande exempel hello delar upp en parameter av en annan parametern.</span><span class="sxs-lookup"><span data-stu-id="9bd93-187">hello following example divides one parameter by another parameter.</span></span>

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
                "description": "Integer used toodivide"
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

<span data-ttu-id="9bd93-188">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="9bd93-188">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="9bd93-189">Namn</span><span class="sxs-lookup"><span data-stu-id="9bd93-189">Name</span></span> | <span data-ttu-id="9bd93-190">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-190">Type</span></span> | <span data-ttu-id="9bd93-191">Värde</span><span class="sxs-lookup"><span data-stu-id="9bd93-191">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="9bd93-192">divResult</span><span class="sxs-lookup"><span data-stu-id="9bd93-192">divResult</span></span> | <span data-ttu-id="9bd93-193">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-193">Int</span></span> | <span data-ttu-id="9bd93-194">2</span><span class="sxs-lookup"><span data-stu-id="9bd93-194">2</span></span> |

<a id="float" />

## <a name="float"></a><span data-ttu-id="9bd93-195">flyttal</span><span class="sxs-lookup"><span data-stu-id="9bd93-195">float</span></span>
`float(arg1)`

<span data-ttu-id="9bd93-196">Konverterar hello värdet tooa flyttal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-196">Converts hello value tooa floating point number.</span></span> <span data-ttu-id="9bd93-197">Du kan bara använda den här funktionen vid sändning av anpassade parametrar tooan program, till exempel en Logikapp.</span><span class="sxs-lookup"><span data-stu-id="9bd93-197">You only use this function when passing custom parameters tooan application, such as a Logic App.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-198">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-198">Parameters</span></span>

| <span data-ttu-id="9bd93-199">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-199">Parameter</span></span> | <span data-ttu-id="9bd93-200">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-200">Required</span></span> | <span data-ttu-id="9bd93-201">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-201">Type</span></span> | <span data-ttu-id="9bd93-202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-202">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-203">arg1</span><span class="sxs-lookup"><span data-stu-id="9bd93-203">arg1</span></span> |<span data-ttu-id="9bd93-204">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-204">Yes</span></span> |<span data-ttu-id="9bd93-205">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="9bd93-205">string or int</span></span> |<span data-ttu-id="9bd93-206">hello värdet tooconvert tooa flyttal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-206">hello value tooconvert tooa floating point number.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-207">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-207">Return value</span></span>
<span data-ttu-id="9bd93-208">En flytande peka nummer.</span><span class="sxs-lookup"><span data-stu-id="9bd93-208">A floating point number.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-209">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-209">Example</span></span>

<span data-ttu-id="9bd93-210">hello som följande exempel visar hur toouse float toopass parametrar tooa Logikapp:</span><span class="sxs-lookup"><span data-stu-id="9bd93-210">hello following example shows how toouse float toopass parameters tooa Logic App:</span></span>

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

## <a name="int"></a><span data-ttu-id="9bd93-211">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-211">int</span></span>
`int(valueToConvert)`

<span data-ttu-id="9bd93-212">Konverterar hello angivet värde tooan heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-212">Converts hello specified value tooan integer.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-213">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-213">Parameters</span></span>

| <span data-ttu-id="9bd93-214">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-214">Parameter</span></span> | <span data-ttu-id="9bd93-215">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-215">Required</span></span> | <span data-ttu-id="9bd93-216">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-216">Type</span></span> | <span data-ttu-id="9bd93-217">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-217">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-218">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="9bd93-218">valueToConvert</span></span> |<span data-ttu-id="9bd93-219">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-219">Yes</span></span> |<span data-ttu-id="9bd93-220">sträng eller ett heltal</span><span class="sxs-lookup"><span data-stu-id="9bd93-220">string or int</span></span> |<span data-ttu-id="9bd93-221">hello värdet tooconvert tooan heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-221">hello value tooconvert tooan integer.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-222">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-222">Return value</span></span>

<span data-ttu-id="9bd93-223">Ett heltal hello konverteras värdet.</span><span class="sxs-lookup"><span data-stu-id="9bd93-223">An integer of hello converted value.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-224">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-224">Example</span></span>

<span data-ttu-id="9bd93-225">hello konverterar följande exempel hello som användaren tillhandahållit parametern värdet toointeger.</span><span class="sxs-lookup"><span data-stu-id="9bd93-225">hello following example converts hello user-provided parameter value toointeger.</span></span>

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

<span data-ttu-id="9bd93-226">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="9bd93-226">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="9bd93-227">Namn</span><span class="sxs-lookup"><span data-stu-id="9bd93-227">Name</span></span> | <span data-ttu-id="9bd93-228">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-228">Type</span></span> | <span data-ttu-id="9bd93-229">Värde</span><span class="sxs-lookup"><span data-stu-id="9bd93-229">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="9bd93-230">intResult</span><span class="sxs-lookup"><span data-stu-id="9bd93-230">intResult</span></span> | <span data-ttu-id="9bd93-231">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-231">Int</span></span> | <span data-ttu-id="9bd93-232">4</span><span class="sxs-lookup"><span data-stu-id="9bd93-232">4</span></span> |


<a id="min" />

## <a name="min"></a><span data-ttu-id="9bd93-233">min.</span><span class="sxs-lookup"><span data-stu-id="9bd93-233">min</span></span>
`min (arg1)`

<span data-ttu-id="9bd93-234">Returnerar hello minimivärdet från en matris av heltal eller en kommaavgränsad lista med heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-234">Returns hello minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-235">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-235">Parameters</span></span>

| <span data-ttu-id="9bd93-236">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-236">Parameter</span></span> | <span data-ttu-id="9bd93-237">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-237">Required</span></span> | <span data-ttu-id="9bd93-238">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-238">Type</span></span> | <span data-ttu-id="9bd93-239">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-239">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-240">arg1</span><span class="sxs-lookup"><span data-stu-id="9bd93-240">arg1</span></span> |<span data-ttu-id="9bd93-241">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-241">Yes</span></span> |<span data-ttu-id="9bd93-242">matris med heltal eller en kommaavgränsad lista med heltal</span><span class="sxs-lookup"><span data-stu-id="9bd93-242">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="9bd93-243">hello samling tooget hello minimivärde.</span><span class="sxs-lookup"><span data-stu-id="9bd93-243">hello collection tooget hello minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-244">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-244">Return value</span></span>

<span data-ttu-id="9bd93-245">Ett heltal som representerar minimivärdet från hello samling.</span><span class="sxs-lookup"><span data-stu-id="9bd93-245">An integer representing minimum value from hello collection.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-246">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-246">Example</span></span>

<span data-ttu-id="9bd93-247">följande exempel visar hur hello toouse min med en matris och en lista med heltal:</span><span class="sxs-lookup"><span data-stu-id="9bd93-247">hello following example shows how toouse min with an array and a list of integers:</span></span>

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

<span data-ttu-id="9bd93-248">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="9bd93-248">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="9bd93-249">Namn</span><span class="sxs-lookup"><span data-stu-id="9bd93-249">Name</span></span> | <span data-ttu-id="9bd93-250">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-250">Type</span></span> | <span data-ttu-id="9bd93-251">Värde</span><span class="sxs-lookup"><span data-stu-id="9bd93-251">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="9bd93-252">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="9bd93-252">arrayOutput</span></span> | <span data-ttu-id="9bd93-253">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-253">Int</span></span> | <span data-ttu-id="9bd93-254">0</span><span class="sxs-lookup"><span data-stu-id="9bd93-254">0</span></span> |
| <span data-ttu-id="9bd93-255">intOutput</span><span class="sxs-lookup"><span data-stu-id="9bd93-255">intOutput</span></span> | <span data-ttu-id="9bd93-256">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-256">Int</span></span> | <span data-ttu-id="9bd93-257">0</span><span class="sxs-lookup"><span data-stu-id="9bd93-257">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="9bd93-258">Max</span><span class="sxs-lookup"><span data-stu-id="9bd93-258">max</span></span>
`max (arg1)`

<span data-ttu-id="9bd93-259">Returnerar hello högsta värde från en heltalsmatris eller en kommaavgränsad lista med heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-259">Returns hello maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-260">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-260">Parameters</span></span>

| <span data-ttu-id="9bd93-261">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-261">Parameter</span></span> | <span data-ttu-id="9bd93-262">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-262">Required</span></span> | <span data-ttu-id="9bd93-263">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-263">Type</span></span> | <span data-ttu-id="9bd93-264">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-264">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-265">arg1</span><span class="sxs-lookup"><span data-stu-id="9bd93-265">arg1</span></span> |<span data-ttu-id="9bd93-266">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-266">Yes</span></span> |<span data-ttu-id="9bd93-267">matris med heltal eller en kommaavgränsad lista med heltal</span><span class="sxs-lookup"><span data-stu-id="9bd93-267">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="9bd93-268">hello samling tooget hello maximivärde.</span><span class="sxs-lookup"><span data-stu-id="9bd93-268">hello collection tooget hello maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-269">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-269">Return value</span></span>

<span data-ttu-id="9bd93-270">Ett heltal som representerar maximivärdet för hello från hello samling.</span><span class="sxs-lookup"><span data-stu-id="9bd93-270">An integer representing hello maximum value from hello collection.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-271">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-271">Example</span></span>

<span data-ttu-id="9bd93-272">följande exempel visar hur hello toouse max med en matris och en lista med heltal:</span><span class="sxs-lookup"><span data-stu-id="9bd93-272">hello following example shows how toouse max with an array and a list of integers:</span></span>

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

<span data-ttu-id="9bd93-273">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="9bd93-273">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="9bd93-274">Namn</span><span class="sxs-lookup"><span data-stu-id="9bd93-274">Name</span></span> | <span data-ttu-id="9bd93-275">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-275">Type</span></span> | <span data-ttu-id="9bd93-276">Värde</span><span class="sxs-lookup"><span data-stu-id="9bd93-276">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="9bd93-277">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="9bd93-277">arrayOutput</span></span> | <span data-ttu-id="9bd93-278">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-278">Int</span></span> | <span data-ttu-id="9bd93-279">5</span><span class="sxs-lookup"><span data-stu-id="9bd93-279">5</span></span> |
| <span data-ttu-id="9bd93-280">intOutput</span><span class="sxs-lookup"><span data-stu-id="9bd93-280">intOutput</span></span> | <span data-ttu-id="9bd93-281">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-281">Int</span></span> | <span data-ttu-id="9bd93-282">5</span><span class="sxs-lookup"><span data-stu-id="9bd93-282">5</span></span> |

<a id="mod" />

## <a name="mod"></a><span data-ttu-id="9bd93-283">MOD</span><span class="sxs-lookup"><span data-stu-id="9bd93-283">mod</span></span>
`mod(operand1, operand2)`

<span data-ttu-id="9bd93-284">Returnerar hello resten av hello Heltalsdivision med hello två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-284">Returns hello remainder of hello integer division using hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-285">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-285">Parameters</span></span>

| <span data-ttu-id="9bd93-286">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-286">Parameter</span></span> | <span data-ttu-id="9bd93-287">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-287">Required</span></span> | <span data-ttu-id="9bd93-288">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-288">Type</span></span> | <span data-ttu-id="9bd93-289">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-289">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-290">operand1</span><span class="sxs-lookup"><span data-stu-id="9bd93-290">operand1</span></span> |<span data-ttu-id="9bd93-291">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-291">Yes</span></span> |<span data-ttu-id="9bd93-292">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-292">int</span></span> |<span data-ttu-id="9bd93-293">hello nummer delas.</span><span class="sxs-lookup"><span data-stu-id="9bd93-293">hello number being divided.</span></span> |
| <span data-ttu-id="9bd93-294">operand2</span><span class="sxs-lookup"><span data-stu-id="9bd93-294">operand2</span></span> |<span data-ttu-id="9bd93-295">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-295">Yes</span></span> |<span data-ttu-id="9bd93-296">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-296">int</span></span> |<span data-ttu-id="9bd93-297">hello-värde som är används toodivide får inte vara 0.</span><span class="sxs-lookup"><span data-stu-id="9bd93-297">hello number that is used toodivide, Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-298">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-298">Return value</span></span>
<span data-ttu-id="9bd93-299">Ett heltal som representerar hello resten.</span><span class="sxs-lookup"><span data-stu-id="9bd93-299">An integer representing hello remainder.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-300">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-300">Example</span></span>

<span data-ttu-id="9bd93-301">hello returneras följande exempel hello återstoden av att dividera en parameter av en annan parametern.</span><span class="sxs-lookup"><span data-stu-id="9bd93-301">hello following example returns hello remainder of dividing one parameter by another parameter.</span></span>

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
                "description": "Integer used toodivide"
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

<span data-ttu-id="9bd93-302">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="9bd93-302">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="9bd93-303">Namn</span><span class="sxs-lookup"><span data-stu-id="9bd93-303">Name</span></span> | <span data-ttu-id="9bd93-304">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-304">Type</span></span> | <span data-ttu-id="9bd93-305">Värde</span><span class="sxs-lookup"><span data-stu-id="9bd93-305">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="9bd93-306">modResult</span><span class="sxs-lookup"><span data-stu-id="9bd93-306">modResult</span></span> | <span data-ttu-id="9bd93-307">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-307">Int</span></span> | <span data-ttu-id="9bd93-308">1</span><span class="sxs-lookup"><span data-stu-id="9bd93-308">1</span></span> |

<a id="mul" />

## <a name="mul"></a><span data-ttu-id="9bd93-309">mul</span><span class="sxs-lookup"><span data-stu-id="9bd93-309">mul</span></span>
`mul(operand1, operand2)`

<span data-ttu-id="9bd93-310">Returnerar hello multiplicering av hello två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-310">Returns hello multiplication of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-311">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-311">Parameters</span></span>

| <span data-ttu-id="9bd93-312">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-312">Parameter</span></span> | <span data-ttu-id="9bd93-313">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-313">Required</span></span> | <span data-ttu-id="9bd93-314">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-314">Type</span></span> | <span data-ttu-id="9bd93-315">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-315">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-316">operand1</span><span class="sxs-lookup"><span data-stu-id="9bd93-316">operand1</span></span> |<span data-ttu-id="9bd93-317">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-317">Yes</span></span> |<span data-ttu-id="9bd93-318">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-318">int</span></span> |<span data-ttu-id="9bd93-319">Första nummer toomultiply.</span><span class="sxs-lookup"><span data-stu-id="9bd93-319">First number toomultiply.</span></span> |
| <span data-ttu-id="9bd93-320">operand2</span><span class="sxs-lookup"><span data-stu-id="9bd93-320">operand2</span></span> |<span data-ttu-id="9bd93-321">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-321">Yes</span></span> |<span data-ttu-id="9bd93-322">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-322">int</span></span> |<span data-ttu-id="9bd93-323">Andra nummer toomultiply.</span><span class="sxs-lookup"><span data-stu-id="9bd93-323">Second number toomultiply.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-324">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-324">Return value</span></span>

<span data-ttu-id="9bd93-325">Ett heltal som representerar hello multiplikation.</span><span class="sxs-lookup"><span data-stu-id="9bd93-325">An integer representing hello multiplication.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-326">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-326">Example</span></span>

<span data-ttu-id="9bd93-327">följande exempel hello multiplicerar en parameter av en annan parametern.</span><span class="sxs-lookup"><span data-stu-id="9bd93-327">hello following example multiplies one parameter by another parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer toomultiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer toomultiply"
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

<span data-ttu-id="9bd93-328">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="9bd93-328">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="9bd93-329">Namn</span><span class="sxs-lookup"><span data-stu-id="9bd93-329">Name</span></span> | <span data-ttu-id="9bd93-330">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-330">Type</span></span> | <span data-ttu-id="9bd93-331">Värde</span><span class="sxs-lookup"><span data-stu-id="9bd93-331">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="9bd93-332">mulResult</span><span class="sxs-lookup"><span data-stu-id="9bd93-332">mulResult</span></span> | <span data-ttu-id="9bd93-333">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-333">Int</span></span> | <span data-ttu-id="9bd93-334">15</span><span class="sxs-lookup"><span data-stu-id="9bd93-334">15</span></span> |

<a id="sub" />

## <a name="sub"></a><span data-ttu-id="9bd93-335">Sub</span><span class="sxs-lookup"><span data-stu-id="9bd93-335">sub</span></span>
`sub(operand1, operand2)`

<span data-ttu-id="9bd93-336">Returnerar hello subtraktion av hello två angivna heltal.</span><span class="sxs-lookup"><span data-stu-id="9bd93-336">Returns hello subtraction of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="9bd93-337">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9bd93-337">Parameters</span></span>

| <span data-ttu-id="9bd93-338">Parameter</span><span class="sxs-lookup"><span data-stu-id="9bd93-338">Parameter</span></span> | <span data-ttu-id="9bd93-339">Krävs</span><span class="sxs-lookup"><span data-stu-id="9bd93-339">Required</span></span> | <span data-ttu-id="9bd93-340">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-340">Type</span></span> | <span data-ttu-id="9bd93-341">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9bd93-341">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9bd93-342">operand1</span><span class="sxs-lookup"><span data-stu-id="9bd93-342">operand1</span></span> |<span data-ttu-id="9bd93-343">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-343">Yes</span></span> |<span data-ttu-id="9bd93-344">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-344">int</span></span> |<span data-ttu-id="9bd93-345">hello-nummer som dras från.</span><span class="sxs-lookup"><span data-stu-id="9bd93-345">hello number that is subtracted from.</span></span> |
| <span data-ttu-id="9bd93-346">operand2</span><span class="sxs-lookup"><span data-stu-id="9bd93-346">operand2</span></span> |<span data-ttu-id="9bd93-347">Ja</span><span class="sxs-lookup"><span data-stu-id="9bd93-347">Yes</span></span> |<span data-ttu-id="9bd93-348">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-348">int</span></span> |<span data-ttu-id="9bd93-349">hello-nummer som subtraheras.</span><span class="sxs-lookup"><span data-stu-id="9bd93-349">hello number that is subtracted.</span></span> |

### <a name="return-value"></a><span data-ttu-id="9bd93-350">Returvärde</span><span class="sxs-lookup"><span data-stu-id="9bd93-350">Return value</span></span>
<span data-ttu-id="9bd93-351">Ett heltal som representerar hello subtraktion.</span><span class="sxs-lookup"><span data-stu-id="9bd93-351">An integer representing hello subtraction.</span></span>

### <a name="example"></a><span data-ttu-id="9bd93-352">Exempel</span><span class="sxs-lookup"><span data-stu-id="9bd93-352">Example</span></span>

<span data-ttu-id="9bd93-353">följande exempel hello subtraherar en parameter från en annan parameter.</span><span class="sxs-lookup"><span data-stu-id="9bd93-353">hello following example subtracts one parameter from another parameter.</span></span>

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
                "description": "Integer toosubtract"
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

<span data-ttu-id="9bd93-354">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="9bd93-354">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="9bd93-355">Namn</span><span class="sxs-lookup"><span data-stu-id="9bd93-355">Name</span></span> | <span data-ttu-id="9bd93-356">Typ</span><span class="sxs-lookup"><span data-stu-id="9bd93-356">Type</span></span> | <span data-ttu-id="9bd93-357">Värde</span><span class="sxs-lookup"><span data-stu-id="9bd93-357">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="9bd93-358">subResult</span><span class="sxs-lookup"><span data-stu-id="9bd93-358">subResult</span></span> | <span data-ttu-id="9bd93-359">int</span><span class="sxs-lookup"><span data-stu-id="9bd93-359">Int</span></span> | <span data-ttu-id="9bd93-360">4</span><span class="sxs-lookup"><span data-stu-id="9bd93-360">4</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9bd93-361">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9bd93-361">Next steps</span></span>
* <span data-ttu-id="9bd93-362">En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9bd93-362">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="9bd93-363">flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9bd93-363">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="9bd93-364">tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="9bd93-364">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="9bd93-365">toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="9bd93-365">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

