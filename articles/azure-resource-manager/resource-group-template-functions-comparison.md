---
title: "Mallfunktioner Azure Resource Manager - jämförelse | Microsoft Docs"
description: "Beskriver funktionerna du använder i en Azure Resource Manager-mall för att jämföra värden."
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
ms.openlocfilehash: 521e5ed06c138bcd374913588f06a2e6c1e99963
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="2ca6a-103">Jämförelse funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="2ca6a-103">Comparison functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="2ca6a-104">Resource Manager innehåller flera funktioner för att göra jämförelser i dina mallar.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="2ca6a-105">är lika med</span><span class="sxs-lookup"><span data-stu-id="2ca6a-105">equals</span></span>](#equals)
* [<span data-ttu-id="2ca6a-106">större</span><span class="sxs-lookup"><span data-stu-id="2ca6a-106">greater</span></span>](#greater)
* [<span data-ttu-id="2ca6a-107">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="2ca6a-107">greaterOrEquals</span></span>](#greaterorequals)
* [<span data-ttu-id="2ca6a-108">mindre</span><span class="sxs-lookup"><span data-stu-id="2ca6a-108">less</span></span>](#less)
* [<span data-ttu-id="2ca6a-109">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="2ca6a-109">lessOrEquals</span></span>](#lessorequals)

## <a name="equals"></a><span data-ttu-id="2ca6a-110">är lika med</span><span class="sxs-lookup"><span data-stu-id="2ca6a-110">equals</span></span>
`equals(arg1, arg2)`

<span data-ttu-id="2ca6a-111">Kontrollerar om två värden är lika med varandra.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-111">Checks whether two values equal each other.</span></span>

### <a name="parameters"></a><span data-ttu-id="2ca6a-112">Parametrar</span><span class="sxs-lookup"><span data-stu-id="2ca6a-112">Parameters</span></span>

| <span data-ttu-id="2ca6a-113">Parameter</span><span class="sxs-lookup"><span data-stu-id="2ca6a-113">Parameter</span></span> | <span data-ttu-id="2ca6a-114">Krävs</span><span class="sxs-lookup"><span data-stu-id="2ca6a-114">Required</span></span> | <span data-ttu-id="2ca6a-115">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-115">Type</span></span> | <span data-ttu-id="2ca6a-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2ca6a-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2ca6a-117">arg1</span><span class="sxs-lookup"><span data-stu-id="2ca6a-117">arg1</span></span> |<span data-ttu-id="2ca6a-118">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-118">Yes</span></span> |<span data-ttu-id="2ca6a-119">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="2ca6a-119">int, string, array, or object</span></span> |<span data-ttu-id="2ca6a-120">Det första värdet att söka efter likhet.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-120">The first value to check for equality.</span></span> |
| <span data-ttu-id="2ca6a-121">arg2</span><span class="sxs-lookup"><span data-stu-id="2ca6a-121">arg2</span></span> |<span data-ttu-id="2ca6a-122">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-122">Yes</span></span> |<span data-ttu-id="2ca6a-123">int, string, matris eller objekt</span><span class="sxs-lookup"><span data-stu-id="2ca6a-123">int, string, array, or object</span></span> |<span data-ttu-id="2ca6a-124">Det andra värdet att söka efter likhet.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-124">The second value to check for equality.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2ca6a-125">Returvärde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-125">Return value</span></span>

<span data-ttu-id="2ca6a-126">Returnerar **SANT** om värdena är lika, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-126">Returns **True** if the values are equal; otherwise, **False**.</span></span>

### <a name="remarks"></a><span data-ttu-id="2ca6a-127">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="2ca6a-127">Remarks</span></span>

<span data-ttu-id="2ca6a-128">Funktionen är lika med används ofta med den `condition` element för att testa om en resurs har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-128">The equals function is often used with the `condition` element to test whether a resource is deployed.</span></span>

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

### <a name="example"></a><span data-ttu-id="2ca6a-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="2ca6a-129">Example</span></span>

<span data-ttu-id="2ca6a-130">Exempel mallen kontrollerar olika typer av värden sinsemellan.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-130">The example template checks different types of values for equality.</span></span> <span data-ttu-id="2ca6a-131">Alla standardvärden returnera True.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-131">All the default values return True.</span></span>

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

<span data-ttu-id="2ca6a-132">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="2ca6a-132">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="2ca6a-133">Namn</span><span class="sxs-lookup"><span data-stu-id="2ca6a-133">Name</span></span> | <span data-ttu-id="2ca6a-134">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-134">Type</span></span> | <span data-ttu-id="2ca6a-135">Värde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-135">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2ca6a-136">checkInts</span><span class="sxs-lookup"><span data-stu-id="2ca6a-136">checkInts</span></span> | <span data-ttu-id="2ca6a-137">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-137">Bool</span></span> | <span data-ttu-id="2ca6a-138">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-138">True</span></span> |
| <span data-ttu-id="2ca6a-139">checkStrings</span><span class="sxs-lookup"><span data-stu-id="2ca6a-139">checkStrings</span></span> | <span data-ttu-id="2ca6a-140">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-140">Bool</span></span> | <span data-ttu-id="2ca6a-141">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-141">True</span></span> |
| <span data-ttu-id="2ca6a-142">checkArrays</span><span class="sxs-lookup"><span data-stu-id="2ca6a-142">checkArrays</span></span> | <span data-ttu-id="2ca6a-143">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-143">Bool</span></span> | <span data-ttu-id="2ca6a-144">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-144">True</span></span> |
| <span data-ttu-id="2ca6a-145">checkObjects</span><span class="sxs-lookup"><span data-stu-id="2ca6a-145">checkObjects</span></span> | <span data-ttu-id="2ca6a-146">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-146">Bool</span></span> | <span data-ttu-id="2ca6a-147">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-147">True</span></span> |


<span data-ttu-id="2ca6a-148">I följande exempel används [inte](resource-group-template-functions-logical.md#not) med **är lika med**.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-148">The following example uses [not](resource-group-template-functions-logical.md#not) with **equals**.</span></span>

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

<span data-ttu-id="2ca6a-149">Utdata från föregående exempel är:</span><span class="sxs-lookup"><span data-stu-id="2ca6a-149">The output from the preceding example is:</span></span>

| <span data-ttu-id="2ca6a-150">Namn</span><span class="sxs-lookup"><span data-stu-id="2ca6a-150">Name</span></span> | <span data-ttu-id="2ca6a-151">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-151">Type</span></span> | <span data-ttu-id="2ca6a-152">Värde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2ca6a-153">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="2ca6a-153">checkNotEquals</span></span> | <span data-ttu-id="2ca6a-154">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-154">Bool</span></span> | <span data-ttu-id="2ca6a-155">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-155">True</span></span> |


## <a name="greater"></a><span data-ttu-id="2ca6a-156">större</span><span class="sxs-lookup"><span data-stu-id="2ca6a-156">greater</span></span>
`greater(arg1, arg2)`

<span data-ttu-id="2ca6a-157">Kontrollerar om det första värdet är större än det andra värdet.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-157">Checks whether the first value is greater than the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="2ca6a-158">Parametrar</span><span class="sxs-lookup"><span data-stu-id="2ca6a-158">Parameters</span></span>

| <span data-ttu-id="2ca6a-159">Parameter</span><span class="sxs-lookup"><span data-stu-id="2ca6a-159">Parameter</span></span> | <span data-ttu-id="2ca6a-160">Krävs</span><span class="sxs-lookup"><span data-stu-id="2ca6a-160">Required</span></span> | <span data-ttu-id="2ca6a-161">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-161">Type</span></span> | <span data-ttu-id="2ca6a-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2ca6a-162">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2ca6a-163">arg1</span><span class="sxs-lookup"><span data-stu-id="2ca6a-163">arg1</span></span> |<span data-ttu-id="2ca6a-164">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-164">Yes</span></span> |<span data-ttu-id="2ca6a-165">int eller string</span><span class="sxs-lookup"><span data-stu-id="2ca6a-165">int or string</span></span> |<span data-ttu-id="2ca6a-166">Det första värdet för större jämförelsen.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-166">The first value for the greater comparison.</span></span> |
| <span data-ttu-id="2ca6a-167">arg2</span><span class="sxs-lookup"><span data-stu-id="2ca6a-167">arg2</span></span> |<span data-ttu-id="2ca6a-168">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-168">Yes</span></span> |<span data-ttu-id="2ca6a-169">int eller string</span><span class="sxs-lookup"><span data-stu-id="2ca6a-169">int or string</span></span> |<span data-ttu-id="2ca6a-170">Det andra värdet för större jämförelsen.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-170">The second value for the greater comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2ca6a-171">Returvärde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-171">Return value</span></span>

<span data-ttu-id="2ca6a-172">Returnerar **SANT** om det första värdet är större än det andra värdet, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-172">Returns **True** if the first value is greater than the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="2ca6a-173">Exempel</span><span class="sxs-lookup"><span data-stu-id="2ca6a-173">Example</span></span>

<span data-ttu-id="2ca6a-174">Exempel mallen kontrollerar om ett värde är större än den andra.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-174">The example template checks whether the one value is greater than the other.</span></span>

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

<span data-ttu-id="2ca6a-175">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="2ca6a-175">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="2ca6a-176">Namn</span><span class="sxs-lookup"><span data-stu-id="2ca6a-176">Name</span></span> | <span data-ttu-id="2ca6a-177">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-177">Type</span></span> | <span data-ttu-id="2ca6a-178">Värde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-178">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2ca6a-179">checkInts</span><span class="sxs-lookup"><span data-stu-id="2ca6a-179">checkInts</span></span> | <span data-ttu-id="2ca6a-180">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-180">Bool</span></span> | <span data-ttu-id="2ca6a-181">False</span><span class="sxs-lookup"><span data-stu-id="2ca6a-181">False</span></span> |
| <span data-ttu-id="2ca6a-182">checkStrings</span><span class="sxs-lookup"><span data-stu-id="2ca6a-182">checkStrings</span></span> | <span data-ttu-id="2ca6a-183">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-183">Bool</span></span> | <span data-ttu-id="2ca6a-184">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-184">True</span></span> |


## <a name="greaterorequals"></a><span data-ttu-id="2ca6a-185">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="2ca6a-185">greaterOrEquals</span></span>
`greaterOrEquals(arg1, arg2)`

<span data-ttu-id="2ca6a-186">Kontrollerar om det första värdet är större än eller lika med det andra värdet.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-186">Checks whether the first value is greater than or equal to the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="2ca6a-187">Parametrar</span><span class="sxs-lookup"><span data-stu-id="2ca6a-187">Parameters</span></span>

| <span data-ttu-id="2ca6a-188">Parameter</span><span class="sxs-lookup"><span data-stu-id="2ca6a-188">Parameter</span></span> | <span data-ttu-id="2ca6a-189">Krävs</span><span class="sxs-lookup"><span data-stu-id="2ca6a-189">Required</span></span> | <span data-ttu-id="2ca6a-190">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-190">Type</span></span> | <span data-ttu-id="2ca6a-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2ca6a-191">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2ca6a-192">arg1</span><span class="sxs-lookup"><span data-stu-id="2ca6a-192">arg1</span></span> |<span data-ttu-id="2ca6a-193">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-193">Yes</span></span> |<span data-ttu-id="2ca6a-194">int eller string</span><span class="sxs-lookup"><span data-stu-id="2ca6a-194">int or string</span></span> |<span data-ttu-id="2ca6a-195">Det första värdet för jämförelse större eller lika med.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-195">The first value for the greater or equal comparison.</span></span> |
| <span data-ttu-id="2ca6a-196">arg2</span><span class="sxs-lookup"><span data-stu-id="2ca6a-196">arg2</span></span> |<span data-ttu-id="2ca6a-197">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-197">Yes</span></span> |<span data-ttu-id="2ca6a-198">int eller string</span><span class="sxs-lookup"><span data-stu-id="2ca6a-198">int or string</span></span> |<span data-ttu-id="2ca6a-199">Det andra värdet för jämförelse större eller lika med.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-199">The second value for the greater or equal comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2ca6a-200">Returvärde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-200">Return value</span></span>

<span data-ttu-id="2ca6a-201">Returnerar **SANT** om det första värdet är större än eller lika med det andra värdet, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-201">Returns **True** if the first value is greater than or equal to the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="2ca6a-202">Exempel</span><span class="sxs-lookup"><span data-stu-id="2ca6a-202">Example</span></span>

<span data-ttu-id="2ca6a-203">Exempel mallen kontrollerar om ett värde är större än eller lika med den andra.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-203">The example template checks whether the one value is greater than or equal to the other.</span></span>

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

<span data-ttu-id="2ca6a-204">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="2ca6a-204">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="2ca6a-205">Namn</span><span class="sxs-lookup"><span data-stu-id="2ca6a-205">Name</span></span> | <span data-ttu-id="2ca6a-206">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-206">Type</span></span> | <span data-ttu-id="2ca6a-207">Värde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-207">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2ca6a-208">checkInts</span><span class="sxs-lookup"><span data-stu-id="2ca6a-208">checkInts</span></span> | <span data-ttu-id="2ca6a-209">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-209">Bool</span></span> | <span data-ttu-id="2ca6a-210">False</span><span class="sxs-lookup"><span data-stu-id="2ca6a-210">False</span></span> |
| <span data-ttu-id="2ca6a-211">checkStrings</span><span class="sxs-lookup"><span data-stu-id="2ca6a-211">checkStrings</span></span> | <span data-ttu-id="2ca6a-212">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-212">Bool</span></span> | <span data-ttu-id="2ca6a-213">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-213">True</span></span> |



## <a name="less"></a><span data-ttu-id="2ca6a-214">mindre</span><span class="sxs-lookup"><span data-stu-id="2ca6a-214">less</span></span>
`less(arg1, arg2)`

<span data-ttu-id="2ca6a-215">Kontrollerar om det första värdet är mindre än det andra värdet.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-215">Checks whether the first value is less than the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="2ca6a-216">Parametrar</span><span class="sxs-lookup"><span data-stu-id="2ca6a-216">Parameters</span></span>

| <span data-ttu-id="2ca6a-217">Parameter</span><span class="sxs-lookup"><span data-stu-id="2ca6a-217">Parameter</span></span> | <span data-ttu-id="2ca6a-218">Krävs</span><span class="sxs-lookup"><span data-stu-id="2ca6a-218">Required</span></span> | <span data-ttu-id="2ca6a-219">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-219">Type</span></span> | <span data-ttu-id="2ca6a-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2ca6a-220">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2ca6a-221">arg1</span><span class="sxs-lookup"><span data-stu-id="2ca6a-221">arg1</span></span> |<span data-ttu-id="2ca6a-222">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-222">Yes</span></span> |<span data-ttu-id="2ca6a-223">int eller string</span><span class="sxs-lookup"><span data-stu-id="2ca6a-223">int or string</span></span> |<span data-ttu-id="2ca6a-224">Det första värdet för mindre jämförelsen.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-224">The first value for the less comparison.</span></span> |
| <span data-ttu-id="2ca6a-225">arg2</span><span class="sxs-lookup"><span data-stu-id="2ca6a-225">arg2</span></span> |<span data-ttu-id="2ca6a-226">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-226">Yes</span></span> |<span data-ttu-id="2ca6a-227">int eller string</span><span class="sxs-lookup"><span data-stu-id="2ca6a-227">int or string</span></span> |<span data-ttu-id="2ca6a-228">Det andra värdet för mindre jämförelsen.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-228">The second value for the less comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2ca6a-229">Returvärde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-229">Return value</span></span>

<span data-ttu-id="2ca6a-230">Returnerar **SANT** om det första värdet är mindre än det andra värdet, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-230">Returns **True** if the first value is less than the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="2ca6a-231">Exempel</span><span class="sxs-lookup"><span data-stu-id="2ca6a-231">Example</span></span>

<span data-ttu-id="2ca6a-232">Exempel mallen kontrollerar om ett värde är mindre än den andra.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-232">The example template checks whether the one value is less than the other.</span></span>

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

<span data-ttu-id="2ca6a-233">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="2ca6a-233">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="2ca6a-234">Namn</span><span class="sxs-lookup"><span data-stu-id="2ca6a-234">Name</span></span> | <span data-ttu-id="2ca6a-235">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-235">Type</span></span> | <span data-ttu-id="2ca6a-236">Värde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-236">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2ca6a-237">checkInts</span><span class="sxs-lookup"><span data-stu-id="2ca6a-237">checkInts</span></span> | <span data-ttu-id="2ca6a-238">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-238">Bool</span></span> | <span data-ttu-id="2ca6a-239">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-239">True</span></span> |
| <span data-ttu-id="2ca6a-240">checkStrings</span><span class="sxs-lookup"><span data-stu-id="2ca6a-240">checkStrings</span></span> | <span data-ttu-id="2ca6a-241">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-241">Bool</span></span> | <span data-ttu-id="2ca6a-242">False</span><span class="sxs-lookup"><span data-stu-id="2ca6a-242">False</span></span> |


## <a name="lessorequals"></a><span data-ttu-id="2ca6a-243">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="2ca6a-243">lessOrEquals</span></span>
`lessOrEquals(arg1, arg2)`

<span data-ttu-id="2ca6a-244">Kontrollerar om det första värdet är mindre än eller lika med det andra värdet.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-244">Checks whether the first value is less than or equal to the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="2ca6a-245">Parametrar</span><span class="sxs-lookup"><span data-stu-id="2ca6a-245">Parameters</span></span>

| <span data-ttu-id="2ca6a-246">Parameter</span><span class="sxs-lookup"><span data-stu-id="2ca6a-246">Parameter</span></span> | <span data-ttu-id="2ca6a-247">Krävs</span><span class="sxs-lookup"><span data-stu-id="2ca6a-247">Required</span></span> | <span data-ttu-id="2ca6a-248">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-248">Type</span></span> | <span data-ttu-id="2ca6a-249">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2ca6a-249">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2ca6a-250">arg1</span><span class="sxs-lookup"><span data-stu-id="2ca6a-250">arg1</span></span> |<span data-ttu-id="2ca6a-251">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-251">Yes</span></span> |<span data-ttu-id="2ca6a-252">int eller string</span><span class="sxs-lookup"><span data-stu-id="2ca6a-252">int or string</span></span> |<span data-ttu-id="2ca6a-253">Den första värde för mindre eller lika med jämförelse.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-253">The first value for the less or equals comparison.</span></span> |
| <span data-ttu-id="2ca6a-254">arg2</span><span class="sxs-lookup"><span data-stu-id="2ca6a-254">arg2</span></span> |<span data-ttu-id="2ca6a-255">Ja</span><span class="sxs-lookup"><span data-stu-id="2ca6a-255">Yes</span></span> |<span data-ttu-id="2ca6a-256">int eller string</span><span class="sxs-lookup"><span data-stu-id="2ca6a-256">int or string</span></span> |<span data-ttu-id="2ca6a-257">Andra värdet för mindre eller lika med jämförelse.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-257">The second value for the less or equals comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2ca6a-258">Returvärde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-258">Return value</span></span>

<span data-ttu-id="2ca6a-259">Returnerar **SANT** om det första värdet är mindre än eller lika med det andra värdet, annars **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-259">Returns **True** if the first value is less than or equal to the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="2ca6a-260">Exempel</span><span class="sxs-lookup"><span data-stu-id="2ca6a-260">Example</span></span>

<span data-ttu-id="2ca6a-261">Exempel mallen kontrollerar om ett värde är mindre än eller lika med den andra.</span><span class="sxs-lookup"><span data-stu-id="2ca6a-261">The example template checks whether the one value is less than or equal to the other.</span></span>

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

<span data-ttu-id="2ca6a-262">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="2ca6a-262">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="2ca6a-263">Namn</span><span class="sxs-lookup"><span data-stu-id="2ca6a-263">Name</span></span> | <span data-ttu-id="2ca6a-264">Typ</span><span class="sxs-lookup"><span data-stu-id="2ca6a-264">Type</span></span> | <span data-ttu-id="2ca6a-265">Värde</span><span class="sxs-lookup"><span data-stu-id="2ca6a-265">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2ca6a-266">checkInts</span><span class="sxs-lookup"><span data-stu-id="2ca6a-266">checkInts</span></span> | <span data-ttu-id="2ca6a-267">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-267">Bool</span></span> | <span data-ttu-id="2ca6a-268">True</span><span class="sxs-lookup"><span data-stu-id="2ca6a-268">True</span></span> |
| <span data-ttu-id="2ca6a-269">checkStrings</span><span class="sxs-lookup"><span data-stu-id="2ca6a-269">checkStrings</span></span> | <span data-ttu-id="2ca6a-270">bool</span><span class="sxs-lookup"><span data-stu-id="2ca6a-270">Bool</span></span> | <span data-ttu-id="2ca6a-271">False</span><span class="sxs-lookup"><span data-stu-id="2ca6a-271">False</span></span> |



## <a name="next-steps"></a><span data-ttu-id="2ca6a-272">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ca6a-272">Next steps</span></span>
* <span data-ttu-id="2ca6a-273">En beskrivning av avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2ca6a-273">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="2ca6a-274">Om du vill slå samman flera mallar, se [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2ca6a-274">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="2ca6a-275">Iterera ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="2ca6a-275">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="2ca6a-276">Information om hur du distribuerar mallen som du har skapat finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="2ca6a-276">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

