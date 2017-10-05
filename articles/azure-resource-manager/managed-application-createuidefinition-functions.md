---
title: Azure-hanterade program skapa UI definition funktioner | Microsoft Docs
description: "Beskriver funktionerna som ska användas när man skapar UI definitioner för hanterade program i Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 62ee10eb8e6f33cc4d828cf01b405c846bef8aa4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="createuidefinition-functions"></a><span data-ttu-id="8b143-103">CreateUiDefinition funktioner</span><span class="sxs-lookup"><span data-stu-id="8b143-103">CreateUiDefinition functions</span></span>
<span data-ttu-id="8b143-104">Det här avsnittet innehåller signaturer för alla funktioner som stöds av en CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="8b143-104">This section contains the signatures for all supported functions of a CreateUiDefinition.</span></span>

<span data-ttu-id="8b143-105">Om du vill använda en funktion måste omges av deklaration med hakparenteser.</span><span class="sxs-lookup"><span data-stu-id="8b143-105">To use a function, surround the declaration with square brackets.</span></span> <span data-ttu-id="8b143-106">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8b143-106">For example:</span></span>

```json
"[function()]"
```

<span data-ttu-id="8b143-107">Strängar och andra funktioner kan refereras som parametrar för en funktion, men strängar måste omges av enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="8b143-107">Strings and other functions can be referenced as parameters for a function, but strings must be surrounded in single quotes.</span></span> <span data-ttu-id="8b143-108">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8b143-108">For example:</span></span>

```json
"[fn1(fn2(), 'foobar')]"
```

<span data-ttu-id="8b143-109">Om tillämpligt, kan du referera egenskaper för utdata för en funktion med hjälp av punktoperatorn.</span><span class="sxs-lookup"><span data-stu-id="8b143-109">Where applicable, you can reference properties of the output of a function by using the dot operator.</span></span> <span data-ttu-id="8b143-110">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8b143-110">For example:</span></span>

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a><span data-ttu-id="8b143-111">Refererar till funktioner</span><span class="sxs-lookup"><span data-stu-id="8b143-111">Referencing functions</span></span>
<span data-ttu-id="8b143-112">Dessa funktioner kan användas för att referera till utdata från de egenskaper eller kontexten för en CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="8b143-112">These functions can be used to reference outputs from the properties or context of a CreateUiDefinition.</span></span>

### <a name="basics"></a><span data-ttu-id="8b143-113">Grunderna</span><span class="sxs-lookup"><span data-stu-id="8b143-113">basics</span></span>
<span data-ttu-id="8b143-114">Returnerar utdatavärden för ett element som har definierats i grundläggande steg.</span><span class="sxs-lookup"><span data-stu-id="8b143-114">Returns the output values of an element that is defined in the Basics step.</span></span>

<span data-ttu-id="8b143-115">I följande exempel returneras resultatet av element med namnet `foo` i grundläggande steg:</span><span class="sxs-lookup"><span data-stu-id="8b143-115">The following example returns the output of the element named `foo` in the Basics step:</span></span>

```json
"[basics('foo')]"
```

### <a name="steps"></a><span data-ttu-id="8b143-116">Steg</span><span class="sxs-lookup"><span data-stu-id="8b143-116">steps</span></span>
<span data-ttu-id="8b143-117">Returnerar utdatavärden för ett element som har definierats i det angivna steget.</span><span class="sxs-lookup"><span data-stu-id="8b143-117">Returns the output values of an element that is defined in the specified step.</span></span> <span data-ttu-id="8b143-118">För att få utdatavärden element i grundläggande steg kan använda `basics()` i stället.</span><span class="sxs-lookup"><span data-stu-id="8b143-118">To get the output values of elements in the Basics step, use `basics()` instead.</span></span>

<span data-ttu-id="8b143-119">I följande exempel returneras resultatet av element med namnet `bar` i steg med namnet `foo`:</span><span class="sxs-lookup"><span data-stu-id="8b143-119">The following example returns the output of the element named `bar` in the step named `foo`:</span></span>

```json
"[steps('foo').bar]"
```

### <a name="location"></a><span data-ttu-id="8b143-120">location</span><span class="sxs-lookup"><span data-stu-id="8b143-120">location</span></span>
<span data-ttu-id="8b143-121">Returnerar den plats som valts i steget grunderna eller den aktuella kontexten.</span><span class="sxs-lookup"><span data-stu-id="8b143-121">Returns the location selected in the Basics step or the current context.</span></span>

<span data-ttu-id="8b143-122">I följande exempel kunde returnera `"westus"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-122">The following example could return `"westus"`:</span></span>

```json
"[location()]"
```

## <a name="string-functions"></a><span data-ttu-id="8b143-123">Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="8b143-123">String functions</span></span>
<span data-ttu-id="8b143-124">Dessa funktioner kan endast användas med JSON-strängar.</span><span class="sxs-lookup"><span data-stu-id="8b143-124">These functions can only be used with JSON strings.</span></span>

### <a name="concat"></a><span data-ttu-id="8b143-125">concat</span><span class="sxs-lookup"><span data-stu-id="8b143-125">concat</span></span>
<span data-ttu-id="8b143-126">Sammanfogar en eller flera strängar.</span><span class="sxs-lookup"><span data-stu-id="8b143-126">Concatenates one or more strings.</span></span>

<span data-ttu-id="8b143-127">Till exempel om värdet för `element1` om `"bar"`, och sedan det här exemplet returnerar strängen `"foobar!"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-127">For example, if the output value of `element1` if `"bar"`, then this example returns the string `"foobar!"`:</span></span>

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a><span data-ttu-id="8b143-128">delsträngen</span><span class="sxs-lookup"><span data-stu-id="8b143-128">substring</span></span>
<span data-ttu-id="8b143-129">Returnerar delsträngen av den angivna strängen.</span><span class="sxs-lookup"><span data-stu-id="8b143-129">Returns the substring of the specified string.</span></span> <span data-ttu-id="8b143-130">Delsträngen som börjar med det angivna indexet och har den angivna längden.</span><span class="sxs-lookup"><span data-stu-id="8b143-130">The substring starts at the specified index and has the specified length.</span></span>

<span data-ttu-id="8b143-131">I följande exempel returneras `"ftw"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-131">The following example returns `"ftw"`:</span></span>

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a><span data-ttu-id="8b143-132">Ersätt</span><span class="sxs-lookup"><span data-stu-id="8b143-132">replace</span></span>
<span data-ttu-id="8b143-133">Returnerar en sträng som ersätts alla förekomster av den angivna strängen i den aktuella strängen med en annan sträng.</span><span class="sxs-lookup"><span data-stu-id="8b143-133">Returns a string in which all occurrences of the specified string in the current string are replaced with another string.</span></span>

<span data-ttu-id="8b143-134">I följande exempel returneras `"Everything is awesome!"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-134">The following example returns `"Everything is awesome!"`:</span></span>

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a><span data-ttu-id="8b143-135">GUID</span><span class="sxs-lookup"><span data-stu-id="8b143-135">guid</span></span>
<span data-ttu-id="8b143-136">Genererar en globalt unik sträng (GUID).</span><span class="sxs-lookup"><span data-stu-id="8b143-136">Generates a globally unique string (GUID).</span></span>

<span data-ttu-id="8b143-137">I följande exempel kunde returnera `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-137">The following example could return `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span></span>

```json
"[guid()]"
```

### <a name="tolower"></a><span data-ttu-id="8b143-138">toLower</span><span class="sxs-lookup"><span data-stu-id="8b143-138">toLower</span></span>
<span data-ttu-id="8b143-139">Returnerar en sträng som konverterats till gemener.</span><span class="sxs-lookup"><span data-stu-id="8b143-139">Returns a string converted to lowercase.</span></span>

<span data-ttu-id="8b143-140">I följande exempel returneras `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-140">The following example returns `"foobar"`:</span></span>

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a><span data-ttu-id="8b143-141">toUpper</span><span class="sxs-lookup"><span data-stu-id="8b143-141">toUpper</span></span>
<span data-ttu-id="8b143-142">Returnerar en sträng som är konverterad till versaler.</span><span class="sxs-lookup"><span data-stu-id="8b143-142">Returns a string converted to uppercase.</span></span>

<span data-ttu-id="8b143-143">I följande exempel returneras `"FOOBAR"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-143">The following example returns `"FOOBAR"`:</span></span>

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a><span data-ttu-id="8b143-144">Samlingen funktioner</span><span class="sxs-lookup"><span data-stu-id="8b143-144">Collection functions</span></span>
<span data-ttu-id="8b143-145">Dessa funktioner kan användas med samlingar som JSON-strängar, matriser och -objekt.</span><span class="sxs-lookup"><span data-stu-id="8b143-145">These functions can be used with collections, like JSON strings, arrays and objects.</span></span>

### <a name="contains"></a><span data-ttu-id="8b143-146">Innehåller</span><span class="sxs-lookup"><span data-stu-id="8b143-146">contains</span></span>
<span data-ttu-id="8b143-147">Returnerar `true` om en sträng som innehåller den angivna delsträngen, en matris som innehåller det angivna värdet eller ett objekt som innehåller den angivna nyckeln.</span><span class="sxs-lookup"><span data-stu-id="8b143-147">Returns `true` if a string contains the specified substring, an array contains the specified value, or an object contains the specified key.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="8b143-148">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="8b143-148">Example 1: string</span></span>
<span data-ttu-id="8b143-149">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-149">The following example returns `true`:</span></span>

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="8b143-150">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="8b143-150">Example 2: array</span></span>
<span data-ttu-id="8b143-151">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="8b143-151">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="8b143-152">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-152">The following example returns `false`:</span></span>

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="8b143-153">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="8b143-153">Example 3: object</span></span>
<span data-ttu-id="8b143-154">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="8b143-154">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="8b143-155">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-155">The following example returns `true`:</span></span>

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a><span data-ttu-id="8b143-156">Längd</span><span class="sxs-lookup"><span data-stu-id="8b143-156">length</span></span>
<span data-ttu-id="8b143-157">Returnerar antalet tecken i en sträng, antalet värden i en matris eller antalet nycklar i ett objekt.</span><span class="sxs-lookup"><span data-stu-id="8b143-157">Returns the number of characters in a string, the number of values in an array, or the number of keys in an object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="8b143-158">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="8b143-158">Example 1: string</span></span>
<span data-ttu-id="8b143-159">I följande exempel returneras `6`:</span><span class="sxs-lookup"><span data-stu-id="8b143-159">The following example returns `6`:</span></span>

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="8b143-160">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="8b143-160">Example 2: array</span></span>
<span data-ttu-id="8b143-161">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="8b143-161">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="8b143-162">I följande exempel returneras `3`:</span><span class="sxs-lookup"><span data-stu-id="8b143-162">The following example returns `3`:</span></span>

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="8b143-163">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="8b143-163">Example 3: object</span></span>
<span data-ttu-id="8b143-164">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="8b143-164">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="8b143-165">I följande exempel returneras `2`:</span><span class="sxs-lookup"><span data-stu-id="8b143-165">The following example returns `2`:</span></span>

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a><span data-ttu-id="8b143-166">tom</span><span class="sxs-lookup"><span data-stu-id="8b143-166">empty</span></span>
<span data-ttu-id="8b143-167">Returnerar `true` om den sträng, en matris eller ett objekt är null eller tomt.</span><span class="sxs-lookup"><span data-stu-id="8b143-167">Returns `true` if the string, array, or object is null or empty.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="8b143-168">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="8b143-168">Example 1: string</span></span>
<span data-ttu-id="8b143-169">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-169">The following example returns `true`:</span></span>

```json
"[empty('')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="8b143-170">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="8b143-170">Example 2: array</span></span>
<span data-ttu-id="8b143-171">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="8b143-171">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="8b143-172">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-172">The following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="8b143-173">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="8b143-173">Example 3: object</span></span>
<span data-ttu-id="8b143-174">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="8b143-174">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="8b143-175">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-175">The following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a><span data-ttu-id="8b143-176">Exempel 4: null och odefinierad</span><span class="sxs-lookup"><span data-stu-id="8b143-176">Example 4: null and undefined</span></span>
<span data-ttu-id="8b143-177">Anta att `element1` är `null` eller odefinierad.</span><span class="sxs-lookup"><span data-stu-id="8b143-177">Assume `element1` is `null` or undefined.</span></span> <span data-ttu-id="8b143-178">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-178">The following example returns `true`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a><span data-ttu-id="8b143-179">första</span><span class="sxs-lookup"><span data-stu-id="8b143-179">first</span></span>
<span data-ttu-id="8b143-180">Returnerar det första tecknet i den angivna strängen; första värde för den angivna matrisen; eller den första nyckel och värde för det angivna objektet.</span><span class="sxs-lookup"><span data-stu-id="8b143-180">Returns the first character of the specified string; first value of the specified array; or the first key and value of the specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="8b143-181">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="8b143-181">Example 1: string</span></span>
<span data-ttu-id="8b143-182">I följande exempel returneras `"f"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-182">The following example returns `"f"`:</span></span>

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="8b143-183">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="8b143-183">Example 2: array</span></span>
<span data-ttu-id="8b143-184">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="8b143-184">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="8b143-185">I följande exempel returneras `1`:</span><span class="sxs-lookup"><span data-stu-id="8b143-185">The following example returns `1`:</span></span>

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="8b143-186">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="8b143-186">Example 3: object</span></span>
<span data-ttu-id="8b143-187">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="8b143-187">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="8b143-188">I följande exempel returneras `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="8b143-188">The following example returns `{"key1": "foobar"}`:</span></span>

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a><span data-ttu-id="8b143-189">senaste</span><span class="sxs-lookup"><span data-stu-id="8b143-189">last</span></span>
<span data-ttu-id="8b143-190">Returnerar det sista tecknet i den angivna strängen, det senaste värdet för den angivna matrisen eller den senaste nyckeln och värdet för det angivna objektet.</span><span class="sxs-lookup"><span data-stu-id="8b143-190">Returns the last character of the specified string, the last value of the specified array, or the last key and value of the specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="8b143-191">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="8b143-191">Example 1: string</span></span>
<span data-ttu-id="8b143-192">I följande exempel returneras `"r"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-192">The following example returns `"r"`:</span></span>

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="8b143-193">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="8b143-193">Example 2: array</span></span>
<span data-ttu-id="8b143-194">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="8b143-194">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="8b143-195">I följande exempel returneras `2`:</span><span class="sxs-lookup"><span data-stu-id="8b143-195">The following example returns `2`:</span></span>

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="8b143-196">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="8b143-196">Example 3: object</span></span>
<span data-ttu-id="8b143-197">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="8b143-197">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="8b143-198">I följande exempel returneras `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="8b143-198">The following example returns `{"key2": "raboof"}`:</span></span>

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a><span data-ttu-id="8b143-199">ta</span><span class="sxs-lookup"><span data-stu-id="8b143-199">take</span></span>
<span data-ttu-id="8b143-200">Returnerar ett angivet antal sammanhängande tecken från början av strängen, ett angivet antal sammanhängande värden från början av matrisen eller ett angivet antal sammanhängande nycklar och värden från början av objektet.</span><span class="sxs-lookup"><span data-stu-id="8b143-200">Returns a specified number of contiguous characters from the start of the string, a specified number of contiguous values from the start of the array, or a specified number of contiguous keys and values from the start of the object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="8b143-201">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="8b143-201">Example 1: string</span></span>
<span data-ttu-id="8b143-202">I följande exempel returneras `"foo"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-202">The following example returns `"foo"`:</span></span>

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="8b143-203">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="8b143-203">Example 2: array</span></span>
<span data-ttu-id="8b143-204">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="8b143-204">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="8b143-205">I följande exempel returneras `[1, 2]`:</span><span class="sxs-lookup"><span data-stu-id="8b143-205">The following example returns `[1, 2]`:</span></span>

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="8b143-206">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="8b143-206">Example 3: object</span></span>
<span data-ttu-id="8b143-207">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="8b143-207">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="8b143-208">I följande exempel returneras `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="8b143-208">The following example returns `{"key1": "foobar"}`:</span></span>

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a><span data-ttu-id="8b143-209">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="8b143-209">skip</span></span>
<span data-ttu-id="8b143-210">Kringgår ett angivet antal element i en mängd och returnerar sedan de återstående element.</span><span class="sxs-lookup"><span data-stu-id="8b143-210">Bypasses a specified number of elements in a collection, and then returns the remaining elements.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="8b143-211">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="8b143-211">Example 1: string</span></span>
<span data-ttu-id="8b143-212">I följande exempel returneras `"bar"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-212">The following example returns `"bar"`:</span></span>

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="8b143-213">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="8b143-213">Example 2: array</span></span>
<span data-ttu-id="8b143-214">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="8b143-214">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="8b143-215">I följande exempel returneras `[3]`:</span><span class="sxs-lookup"><span data-stu-id="8b143-215">The following example returns `[3]`:</span></span>

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="8b143-216">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="8b143-216">Example 3: object</span></span>
<span data-ttu-id="8b143-217">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="8b143-217">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="8b143-218">I följande exempel returneras `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="8b143-218">The following example returns `{"key2": "raboof"}`:</span></span>

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a><span data-ttu-id="8b143-219">Logiska funktioner</span><span class="sxs-lookup"><span data-stu-id="8b143-219">Logical functions</span></span>
<span data-ttu-id="8b143-220">Dessa funktioner kan användas i villkorlig sats.</span><span class="sxs-lookup"><span data-stu-id="8b143-220">These functions can be used in conditionals.</span></span> <span data-ttu-id="8b143-221">Vissa funktioner kanske inte stöder alla JSON-datatyper.</span><span class="sxs-lookup"><span data-stu-id="8b143-221">Some functions may not support all JSON data types.</span></span>

### <a name="equals"></a><span data-ttu-id="8b143-222">är lika med</span><span class="sxs-lookup"><span data-stu-id="8b143-222">equals</span></span>
<span data-ttu-id="8b143-223">Returnerar `true` om båda parametrarna har samma typ och värde.</span><span class="sxs-lookup"><span data-stu-id="8b143-223">Returns `true` if both parameters have the same type and value.</span></span> <span data-ttu-id="8b143-224">Den här funktionen stöder alla JSON-datatyper.</span><span class="sxs-lookup"><span data-stu-id="8b143-224">This function supports all JSON data types.</span></span>

<span data-ttu-id="8b143-225">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-225">The following example returns `true`:</span></span>

```json
"[equals(0, 0)]"
```

<span data-ttu-id="8b143-226">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-226">The following example returns `true`:</span></span>

```json
"[equals('foo', 'foo')]"
```

<span data-ttu-id="8b143-227">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-227">The following example returns `false`:</span></span>

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a><span data-ttu-id="8b143-228">mindre</span><span class="sxs-lookup"><span data-stu-id="8b143-228">less</span></span>
<span data-ttu-id="8b143-229">Returnerar `true` om den första parametern är strikt mindre än den andra parametern.</span><span class="sxs-lookup"><span data-stu-id="8b143-229">Returns `true` if the first parameter is strictly less than the second parameter.</span></span> <span data-ttu-id="8b143-230">Den här funktionen har stöd för parametrarna endast för nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="8b143-230">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="8b143-231">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-231">The following example returns `true`:</span></span>

```json
"[less(1, 2)]"
```

<span data-ttu-id="8b143-232">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-232">The following example returns `false`:</span></span>

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a><span data-ttu-id="8b143-233">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="8b143-233">lessOrEquals</span></span>
<span data-ttu-id="8b143-234">Returnerar `true` om den första parametern är mindre än eller lika med den andra parametern.</span><span class="sxs-lookup"><span data-stu-id="8b143-234">Returns `true` if the first parameter is less than or equal to the second parameter.</span></span> <span data-ttu-id="8b143-235">Den här funktionen har stöd för parametrarna endast för nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="8b143-235">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="8b143-236">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-236">The following example returns `true`:</span></span>

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a><span data-ttu-id="8b143-237">större</span><span class="sxs-lookup"><span data-stu-id="8b143-237">greater</span></span>
<span data-ttu-id="8b143-238">Returnerar `true` om den första parametern är strikt större än den andra parametern.</span><span class="sxs-lookup"><span data-stu-id="8b143-238">Returns `true` if the first parameter is strictly greater than the second parameter.</span></span> <span data-ttu-id="8b143-239">Den här funktionen har stöd för parametrarna endast för nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="8b143-239">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="8b143-240">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-240">The following example returns `false`:</span></span>

```json
"[greater(1, 2)]"
```

<span data-ttu-id="8b143-241">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-241">The following example returns `true`:</span></span>

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a><span data-ttu-id="8b143-242">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="8b143-242">greaterOrEquals</span></span>
<span data-ttu-id="8b143-243">Returnerar `true` om den första parametern är större än eller lika med den andra parametern.</span><span class="sxs-lookup"><span data-stu-id="8b143-243">Returns `true` if the first parameter is greater than or equal to the second parameter.</span></span> <span data-ttu-id="8b143-244">Den här funktionen har stöd för parametrarna endast för nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="8b143-244">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="8b143-245">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-245">The following example returns `true`:</span></span>

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a><span data-ttu-id="8b143-246">och</span><span class="sxs-lookup"><span data-stu-id="8b143-246">and</span></span>
<span data-ttu-id="8b143-247">Returnerar `true` om alla parametrar som utvärderas till `true`.</span><span class="sxs-lookup"><span data-stu-id="8b143-247">Returns `true` if all the parameters evaluate to `true`.</span></span> <span data-ttu-id="8b143-248">Den här funktionen har stöd för två eller flera parametrar av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="8b143-248">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="8b143-249">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-249">The following example returns `true`:</span></span>

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="8b143-250">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-250">The following example returns `false`:</span></span>

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a><span data-ttu-id="8b143-251">eller</span><span class="sxs-lookup"><span data-stu-id="8b143-251">or</span></span>
<span data-ttu-id="8b143-252">Returnerar `true` om minst en av parametrarna evalueras till `true`.</span><span class="sxs-lookup"><span data-stu-id="8b143-252">Returns `true` if at least one of the parameters evaluates to `true`.</span></span> <span data-ttu-id="8b143-253">Den här funktionen har stöd för två eller flera parametrar av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="8b143-253">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="8b143-254">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-254">The following example returns `true`:</span></span>

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="8b143-255">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-255">The following example returns `true`:</span></span>

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a><span data-ttu-id="8b143-256">inte</span><span class="sxs-lookup"><span data-stu-id="8b143-256">not</span></span>
<span data-ttu-id="8b143-257">Returnerar `true` om parametern evalueras till `false`.</span><span class="sxs-lookup"><span data-stu-id="8b143-257">Returns `true` if the parameter evaluates to `false`.</span></span> <span data-ttu-id="8b143-258">Den här funktionen har stöd för parametrar av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="8b143-258">This function supports parameters only of type Boolean.</span></span>

<span data-ttu-id="8b143-259">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-259">The following example returns `true`:</span></span>

```json
"[not(false)]"
```

<span data-ttu-id="8b143-260">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-260">The following example returns `false`:</span></span>

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a><span data-ttu-id="8b143-261">Slå samman</span><span class="sxs-lookup"><span data-stu-id="8b143-261">coalesce</span></span>
<span data-ttu-id="8b143-262">Returnerar värdet för den första icke-null-parametern.</span><span class="sxs-lookup"><span data-stu-id="8b143-262">Returns the value of the first non-null parameter.</span></span> <span data-ttu-id="8b143-263">Den här funktionen stöder alla JSON-datatyper.</span><span class="sxs-lookup"><span data-stu-id="8b143-263">This function supports all JSON data types.</span></span>

<span data-ttu-id="8b143-264">Anta att `element1` och `element2` är odefinierad.</span><span class="sxs-lookup"><span data-stu-id="8b143-264">Assume `element1` and `element2` are undefined.</span></span> <span data-ttu-id="8b143-265">I följande exempel returneras `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-265">The following example returns `"foobar"`:</span></span>

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a><span data-ttu-id="8b143-266">Konverteringsfunktioner</span><span class="sxs-lookup"><span data-stu-id="8b143-266">Conversion functions</span></span>
<span data-ttu-id="8b143-267">Dessa funktioner kan användas för att konvertera värden mellan JSON-datatyper och kodningar.</span><span class="sxs-lookup"><span data-stu-id="8b143-267">These functions can be used to convert values between JSON data types and encodings.</span></span>

### <a name="int"></a><span data-ttu-id="8b143-268">int</span><span class="sxs-lookup"><span data-stu-id="8b143-268">int</span></span>
<span data-ttu-id="8b143-269">Konverterar parametern till ett heltal.</span><span class="sxs-lookup"><span data-stu-id="8b143-269">Converts the parameter to an integer.</span></span> <span data-ttu-id="8b143-270">Den här funktionen har stöd för parametrar av typen nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="8b143-270">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="8b143-271">I följande exempel returneras `1`:</span><span class="sxs-lookup"><span data-stu-id="8b143-271">The following example returns `1`:</span></span>

```json
"[int('1')]"
```

<span data-ttu-id="8b143-272">I följande exempel returneras `2`:</span><span class="sxs-lookup"><span data-stu-id="8b143-272">The following example returns `2`:</span></span>

```json
"[int(2.9)]"
```

### <a name="float"></a><span data-ttu-id="8b143-273">flyttal</span><span class="sxs-lookup"><span data-stu-id="8b143-273">float</span></span>
<span data-ttu-id="8b143-274">Konverterar parametern till ett flyttal.</span><span class="sxs-lookup"><span data-stu-id="8b143-274">Converts the parameter to a floating-point.</span></span> <span data-ttu-id="8b143-275">Den här funktionen har stöd för parametrar av typen nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="8b143-275">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="8b143-276">I följande exempel returneras `1.0`:</span><span class="sxs-lookup"><span data-stu-id="8b143-276">The following example returns `1.0`:</span></span>

```json
"[float('1.0')]"
```

<span data-ttu-id="8b143-277">I följande exempel returneras `2.9`:</span><span class="sxs-lookup"><span data-stu-id="8b143-277">The following example returns `2.9`:</span></span>

```json
"[float(2.9)]"
```

### <a name="string"></a><span data-ttu-id="8b143-278">Sträng</span><span class="sxs-lookup"><span data-stu-id="8b143-278">string</span></span>
<span data-ttu-id="8b143-279">Konverterar parametern till en sträng.</span><span class="sxs-lookup"><span data-stu-id="8b143-279">Converts the parameter to a string.</span></span> <span data-ttu-id="8b143-280">Den här funktionen stöder alla datatyper i JSON-parametrar.</span><span class="sxs-lookup"><span data-stu-id="8b143-280">This function supports parameters of all JSON data types.</span></span>

<span data-ttu-id="8b143-281">I följande exempel returneras `"1"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-281">The following example returns `"1"`:</span></span>

```json
"[string(1)]"
```

<span data-ttu-id="8b143-282">I följande exempel returneras `"2.9"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-282">The following example returns `"2.9"`:</span></span>

```json
"[string(2.9)]"
```

<span data-ttu-id="8b143-283">I följande exempel returneras `"[1,2,3]"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-283">The following example returns `"[1,2,3]"`:</span></span>

```json
"[string([1,2,3])]"
```

<span data-ttu-id="8b143-284">I följande exempel returneras `"{"foo":"bar"}"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-284">The following example returns `"{"foo":"bar"}"`:</span></span>

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a><span data-ttu-id="8b143-285">bool</span><span class="sxs-lookup"><span data-stu-id="8b143-285">bool</span></span>
<span data-ttu-id="8b143-286">Konverterar parametern till ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="8b143-286">Converts the parameter to a Boolean.</span></span> <span data-ttu-id="8b143-287">Den här funktionen har stöd för parametrar av typen antal, sträng och booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="8b143-287">This function supports parameters of type number, string, and Boolean.</span></span> <span data-ttu-id="8b143-288">Något värde liknar booleska värden i JavaScript, utom `0` eller `'false'` returnerar `true`.</span><span class="sxs-lookup"><span data-stu-id="8b143-288">Similar to Booleans in JavaScript, any value except `0` or `'false'` returns `true`.</span></span>

<span data-ttu-id="8b143-289">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-289">The following example returns `true`:</span></span>

```json
"[bool(1)]"
```

<span data-ttu-id="8b143-290">I följande exempel returneras `false`:</span><span class="sxs-lookup"><span data-stu-id="8b143-290">The following example returns `false`:</span></span>

```json
"[bool(0)]"
```

<span data-ttu-id="8b143-291">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-291">The following example returns `true`:</span></span>

```json
"[bool(true)]"
```

<span data-ttu-id="8b143-292">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-292">The following example returns `true`:</span></span>

```json
"[bool('true')]"
```

### <a name="parse"></a><span data-ttu-id="8b143-293">parsa</span><span class="sxs-lookup"><span data-stu-id="8b143-293">parse</span></span>
<span data-ttu-id="8b143-294">Konverterar parametern till en inbyggd typ.</span><span class="sxs-lookup"><span data-stu-id="8b143-294">Converts the parameter to a native type.</span></span> <span data-ttu-id="8b143-295">I den här funktionen är med andra ord inversen av `string()`.</span><span class="sxs-lookup"><span data-stu-id="8b143-295">In other words, this function is the inverse of `string()`.</span></span> <span data-ttu-id="8b143-296">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="8b143-296">This function supports parameters only of type string.</span></span>

<span data-ttu-id="8b143-297">I följande exempel returneras `1`:</span><span class="sxs-lookup"><span data-stu-id="8b143-297">The following example returns `1`:</span></span>

```json
"[parse('1')]"
```

<span data-ttu-id="8b143-298">I följande exempel returneras `true`:</span><span class="sxs-lookup"><span data-stu-id="8b143-298">The following example returns `true`:</span></span>

```json
"[parse('true')]"
```

<span data-ttu-id="8b143-299">I följande exempel returneras `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="8b143-299">The following example returns `[1,2,3]`:</span></span>

```json
"[parse('[1,2,3]')]"
```

<span data-ttu-id="8b143-300">I följande exempel returneras `{"foo":"bar"}`:</span><span class="sxs-lookup"><span data-stu-id="8b143-300">The following example returns `{"foo":"bar"}`:</span></span>

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a><span data-ttu-id="8b143-301">encodeBase64</span><span class="sxs-lookup"><span data-stu-id="8b143-301">encodeBase64</span></span>
<span data-ttu-id="8b143-302">Kodar parametern för en Base64-kodad sträng.</span><span class="sxs-lookup"><span data-stu-id="8b143-302">Encodes the parameter to a base-64 encoded string.</span></span> <span data-ttu-id="8b143-303">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="8b143-303">This function supports parameters only of type string.</span></span>

<span data-ttu-id="8b143-304">I följande exempel returneras `"Zm9vYmFy"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-304">The following example returns `"Zm9vYmFy"`:</span></span>

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a><span data-ttu-id="8b143-305">decodeBase64</span><span class="sxs-lookup"><span data-stu-id="8b143-305">decodeBase64</span></span>
<span data-ttu-id="8b143-306">Avkodar parametern från en Base64-kodad sträng.</span><span class="sxs-lookup"><span data-stu-id="8b143-306">Decodes the parameter from a base-64 encoded string.</span></span> <span data-ttu-id="8b143-307">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="8b143-307">This function supports parameters only of type string.</span></span>

<span data-ttu-id="8b143-308">I följande exempel returneras `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-308">The following example returns `"foobar"`:</span></span>

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a><span data-ttu-id="8b143-309">encodeUriComponent</span><span class="sxs-lookup"><span data-stu-id="8b143-309">encodeUriComponent</span></span>
<span data-ttu-id="8b143-310">Kodar parametern till en URL-kodad sträng.</span><span class="sxs-lookup"><span data-stu-id="8b143-310">Encodes the parameter to a URL encoded string.</span></span> <span data-ttu-id="8b143-311">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="8b143-311">This function supports parameters only of type string.</span></span>

<span data-ttu-id="8b143-312">I följande exempel returneras `"https%3A%2F%2Fportal.azure.com%2F"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-312">The following example returns `"https%3A%2F%2Fportal.azure.com%2F"`:</span></span>

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a><span data-ttu-id="8b143-313">decodeUriComponent</span><span class="sxs-lookup"><span data-stu-id="8b143-313">decodeUriComponent</span></span>
<span data-ttu-id="8b143-314">Avkodar parametern från en URL-kodad sträng.</span><span class="sxs-lookup"><span data-stu-id="8b143-314">Decodes the parameter from a URL encoded string.</span></span> <span data-ttu-id="8b143-315">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="8b143-315">This function supports parameters only of type string.</span></span>

<span data-ttu-id="8b143-316">I följande exempel returneras `"https://portal.azure.com/"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-316">The following example returns `"https://portal.azure.com/"`:</span></span>

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a><span data-ttu-id="8b143-317">Matematiska funktioner</span><span class="sxs-lookup"><span data-stu-id="8b143-317">Math functions</span></span>
### <a name="add"></a><span data-ttu-id="8b143-318">Lägg till</span><span class="sxs-lookup"><span data-stu-id="8b143-318">add</span></span>
<span data-ttu-id="8b143-319">Adderar två tal och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="8b143-319">Adds two numbers, and returns the result.</span></span>

<span data-ttu-id="8b143-320">I följande exempel returneras `3`:</span><span class="sxs-lookup"><span data-stu-id="8b143-320">The following example returns `3`:</span></span>

```json
"[add(1, 2)]"
```

### <a name="sub"></a><span data-ttu-id="8b143-321">Sub</span><span class="sxs-lookup"><span data-stu-id="8b143-321">sub</span></span>
<span data-ttu-id="8b143-322">Subtraherar andra tal från det första och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="8b143-322">Subtracts the second number from the first number, and returns the result.</span></span>

<span data-ttu-id="8b143-323">I följande exempel returneras `1`:</span><span class="sxs-lookup"><span data-stu-id="8b143-323">The following example returns `1`:</span></span>

```json
"[sub(3, 2)]"
```

### <a name="mul"></a><span data-ttu-id="8b143-324">mul</span><span class="sxs-lookup"><span data-stu-id="8b143-324">mul</span></span>
<span data-ttu-id="8b143-325">Multiplicerar två tal och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="8b143-325">Multiplies two numbers, and returns the result.</span></span>

<span data-ttu-id="8b143-326">I följande exempel returneras `6`:</span><span class="sxs-lookup"><span data-stu-id="8b143-326">The following example returns `6`:</span></span>

```json
"[mul(2, 3)]"
```

### <a name="div"></a><span data-ttu-id="8b143-327">div</span><span class="sxs-lookup"><span data-stu-id="8b143-327">div</span></span>
<span data-ttu-id="8b143-328">Delar det första talet av den andra siffran, och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="8b143-328">Divides the first number by the second number, and returns the result.</span></span> <span data-ttu-id="8b143-329">Resultatet är alltid ett heltal.</span><span class="sxs-lookup"><span data-stu-id="8b143-329">The result is always an integer.</span></span>

<span data-ttu-id="8b143-330">I följande exempel returneras `2`:</span><span class="sxs-lookup"><span data-stu-id="8b143-330">The following example returns `2`:</span></span>

```json
"[div(6, 3)]"
```

### <a name="mod"></a><span data-ttu-id="8b143-331">MOD</span><span class="sxs-lookup"><span data-stu-id="8b143-331">mod</span></span>
<span data-ttu-id="8b143-332">Delar det första talet av den andra siffran, och returnerar resten.</span><span class="sxs-lookup"><span data-stu-id="8b143-332">Divides the first number by the second number, and returns the remainder.</span></span>

<span data-ttu-id="8b143-333">I följande exempel returneras `0`:</span><span class="sxs-lookup"><span data-stu-id="8b143-333">The following example returns `0`:</span></span>

```json
"[mod(6, 3)]"
```

<span data-ttu-id="8b143-334">I följande exempel returneras `2`:</span><span class="sxs-lookup"><span data-stu-id="8b143-334">The following example returns `2`:</span></span>

```json
"[mod(6, 4)]"
```

### <a name="min"></a><span data-ttu-id="8b143-335">min.</span><span class="sxs-lookup"><span data-stu-id="8b143-335">min</span></span>
<span data-ttu-id="8b143-336">Returnerar små av de två talen.</span><span class="sxs-lookup"><span data-stu-id="8b143-336">Returns the small of the two numbers.</span></span>

<span data-ttu-id="8b143-337">I följande exempel returneras `1`:</span><span class="sxs-lookup"><span data-stu-id="8b143-337">The following example returns `1`:</span></span>

```json
"[min(1, 2)]"
```

### <a name="max"></a><span data-ttu-id="8b143-338">Max</span><span class="sxs-lookup"><span data-stu-id="8b143-338">max</span></span>
<span data-ttu-id="8b143-339">Returnerar högre av de två talen.</span><span class="sxs-lookup"><span data-stu-id="8b143-339">Returns the larger of the two numbers.</span></span>

<span data-ttu-id="8b143-340">I följande exempel returneras `2`:</span><span class="sxs-lookup"><span data-stu-id="8b143-340">The following example returns `2`:</span></span>

```json
"[max(1, 2)]"
```

### <a name="range"></a><span data-ttu-id="8b143-341">intervallet</span><span class="sxs-lookup"><span data-stu-id="8b143-341">range</span></span>
<span data-ttu-id="8b143-342">Genererar en sekvens med integrerad som ligger inom det angivna intervallet.</span><span class="sxs-lookup"><span data-stu-id="8b143-342">Generates a sequence of integral numbers within the specified range.</span></span>

<span data-ttu-id="8b143-343">I följande exempel returneras `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="8b143-343">The following example returns `[1,2,3]`:</span></span>

```json
"[range(1, 3)]"
```

### <a name="rand"></a><span data-ttu-id="8b143-344">SLUMP</span><span class="sxs-lookup"><span data-stu-id="8b143-344">rand</span></span>
<span data-ttu-id="8b143-345">Returnerar ett slumptal integrerad inom det angivna intervallet.</span><span class="sxs-lookup"><span data-stu-id="8b143-345">Returns a random integral number within the specified range.</span></span> <span data-ttu-id="8b143-346">Den här funktionen genererar inte kryptografiskt säker slumptal.</span><span class="sxs-lookup"><span data-stu-id="8b143-346">This function does not generate cryptographically secure random numbers.</span></span>

<span data-ttu-id="8b143-347">I följande exempel kunde returnera `42`:</span><span class="sxs-lookup"><span data-stu-id="8b143-347">The following example could return `42`:</span></span>

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a><span data-ttu-id="8b143-348">våning</span><span class="sxs-lookup"><span data-stu-id="8b143-348">floor</span></span>
<span data-ttu-id="8b143-349">Returnerar det största heltalet mindre än eller lika med det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="8b143-349">Returns the largest integer less than or equal to the specified number.</span></span>

<span data-ttu-id="8b143-350">I följande exempel returneras `3`:</span><span class="sxs-lookup"><span data-stu-id="8b143-350">The following example returns `3`:</span></span>

```json
"[floor(3.14)]"
```

### <a name="ceil"></a><span data-ttu-id="8b143-351">ceil</span><span class="sxs-lookup"><span data-stu-id="8b143-351">ceil</span></span>
<span data-ttu-id="8b143-352">Returnerar det största heltalet större än eller lika med det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="8b143-352">Returns the largest integer greater than or equal to the specified number.</span></span>

<span data-ttu-id="8b143-353">I följande exempel returneras `4`:</span><span class="sxs-lookup"><span data-stu-id="8b143-353">The following example returns `4`:</span></span>

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a><span data-ttu-id="8b143-354">Datumfunktioner</span><span class="sxs-lookup"><span data-stu-id="8b143-354">Date functions</span></span>
### <a name="utcnow"></a><span data-ttu-id="8b143-355">utcNow</span><span class="sxs-lookup"><span data-stu-id="8b143-355">utcNow</span></span>
<span data-ttu-id="8b143-356">Returnerar en sträng i ISO 8601-format för den aktuella datum och tid på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="8b143-356">Returns a string in ISO 8601 format of the current date and time on the local computer.</span></span>

<span data-ttu-id="8b143-357">I följande exempel kunde returnera `"1990-12-31T23:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-357">The following example could return `"1990-12-31T23:59:59.000Z"`:</span></span>

```json
"[utcNow()]"
```

### <a name="addseconds"></a><span data-ttu-id="8b143-358">Lägg_till_sekunder</span><span class="sxs-lookup"><span data-stu-id="8b143-358">addSeconds</span></span>
<span data-ttu-id="8b143-359">Lägger till ett heltal sekunder till den angivna tidsstämpeln.</span><span class="sxs-lookup"><span data-stu-id="8b143-359">Adds an integral number of seconds to the specified timestamp.</span></span>

<span data-ttu-id="8b143-360">I följande exempel returneras `"1991-01-01T00:00:00.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-360">The following example returns `"1991-01-01T00:00:00.000Z"`:</span></span>

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a><span data-ttu-id="8b143-361">addMinutes</span><span class="sxs-lookup"><span data-stu-id="8b143-361">addMinutes</span></span>
<span data-ttu-id="8b143-362">Lägger till ett heltal minuter till den angivna tidsstämpeln.</span><span class="sxs-lookup"><span data-stu-id="8b143-362">Adds an integral number of minutes to the specified timestamp.</span></span>

<span data-ttu-id="8b143-363">I följande exempel returneras `"1991-01-01T00:00:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-363">The following example returns `"1991-01-01T00:00:59.000Z"`:</span></span>

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a><span data-ttu-id="8b143-364">addHours</span><span class="sxs-lookup"><span data-stu-id="8b143-364">addHours</span></span>
<span data-ttu-id="8b143-365">Lägger till ett heltal timmar till den angivna tidsstämpeln.</span><span class="sxs-lookup"><span data-stu-id="8b143-365">Adds an integral number of hours to the specified timestamp.</span></span>

<span data-ttu-id="8b143-366">I följande exempel returneras `"1991-01-01T00:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="8b143-366">The following example returns `"1991-01-01T00:59:59.000Z"`:</span></span>

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a><span data-ttu-id="8b143-367">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b143-367">Next steps</span></span>
* <span data-ttu-id="8b143-368">En introduktion till Azure Resource Manager finns [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8b143-368">For an introduction to Azure Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

