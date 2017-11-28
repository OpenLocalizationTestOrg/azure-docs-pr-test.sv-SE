---
title: aaaAzure hanterat program skapa UI definition funktioner | Microsoft Docs
description: "Beskriver hello funktioner toouse vid UI definitioner för hanterade program i Azure"
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
ms.openlocfilehash: 7a311a25404ccaec8c19c3ed8cd7038f6887c013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="createuidefinition-functions"></a><span data-ttu-id="fd780-103">CreateUiDefinition funktioner</span><span class="sxs-lookup"><span data-stu-id="fd780-103">CreateUiDefinition functions</span></span>
<span data-ttu-id="fd780-104">Det här avsnittet innehåller hello signaturer för alla funktioner som stöds av en CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="fd780-104">This section contains hello signatures for all supported functions of a CreateUiDefinition.</span></span>

<span data-ttu-id="fd780-105">toouse en funktion surround hello-deklaration med hakparenteser.</span><span class="sxs-lookup"><span data-stu-id="fd780-105">toouse a function, surround hello declaration with square brackets.</span></span> <span data-ttu-id="fd780-106">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fd780-106">For example:</span></span>

```json
"[function()]"
```

<span data-ttu-id="fd780-107">Strängar och andra funktioner kan refereras som parametrar för en funktion, men strängar måste omges av enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="fd780-107">Strings and other functions can be referenced as parameters for a function, but strings must be surrounded in single quotes.</span></span> <span data-ttu-id="fd780-108">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fd780-108">For example:</span></span>

```json
"[fn1(fn2(), 'foobar')]"
```

<span data-ttu-id="fd780-109">Om tillämpligt, kan du referera egenskaper för hello utdata för en funktion med hjälp av hello punktoperatorn.</span><span class="sxs-lookup"><span data-stu-id="fd780-109">Where applicable, you can reference properties of hello output of a function by using hello dot operator.</span></span> <span data-ttu-id="fd780-110">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fd780-110">For example:</span></span>

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a><span data-ttu-id="fd780-111">Refererar till funktioner</span><span class="sxs-lookup"><span data-stu-id="fd780-111">Referencing functions</span></span>
<span data-ttu-id="fd780-112">Dessa funktioner kan vara används tooreference utdata från hello egenskaper eller kontexten för en CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="fd780-112">These functions can be used tooreference outputs from hello properties or context of a CreateUiDefinition.</span></span>

### <a name="basics"></a><span data-ttu-id="fd780-113">Grunderna</span><span class="sxs-lookup"><span data-stu-id="fd780-113">basics</span></span>
<span data-ttu-id="fd780-114">Returnerar hello utdatavärden för ett element som har definierats i hello grunderna steg.</span><span class="sxs-lookup"><span data-stu-id="fd780-114">Returns hello output values of an element that is defined in hello Basics step.</span></span>

<span data-ttu-id="fd780-115">hello följande exempel returnerar hello utdata från hello element med namnet `foo` i hello grundläggande steg:</span><span class="sxs-lookup"><span data-stu-id="fd780-115">hello following example returns hello output of hello element named `foo` in hello Basics step:</span></span>

```json
"[basics('foo')]"
```

### <a name="steps"></a><span data-ttu-id="fd780-116">steg</span><span class="sxs-lookup"><span data-stu-id="fd780-116">steps</span></span>
<span data-ttu-id="fd780-117">Returnerar hello utdatavärden för ett element som har definierats i hello specifikt steg.</span><span class="sxs-lookup"><span data-stu-id="fd780-117">Returns hello output values of an element that is defined in hello specified step.</span></span> <span data-ttu-id="fd780-118">tooget hello utdatavärden element i hello grunderna steget Använd `basics()` i stället.</span><span class="sxs-lookup"><span data-stu-id="fd780-118">tooget hello output values of elements in hello Basics step, use `basics()` instead.</span></span>

<span data-ttu-id="fd780-119">hello följande exempel returnerar hello utdata från hello element med namnet `bar` i hello steg med namnet `foo`:</span><span class="sxs-lookup"><span data-stu-id="fd780-119">hello following example returns hello output of hello element named `bar` in hello step named `foo`:</span></span>

```json
"[steps('foo').bar]"
```

### <a name="location"></a><span data-ttu-id="fd780-120">location</span><span class="sxs-lookup"><span data-stu-id="fd780-120">location</span></span>
<span data-ttu-id="fd780-121">Returnerar hello-plats som väljs i hello grunderna steg eller hello aktuell kontext.</span><span class="sxs-lookup"><span data-stu-id="fd780-121">Returns hello location selected in hello Basics step or hello current context.</span></span>

<span data-ttu-id="fd780-122">hello följande exempel kunde returnera `"westus"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-122">hello following example could return `"westus"`:</span></span>

```json
"[location()]"
```

## <a name="string-functions"></a><span data-ttu-id="fd780-123">Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="fd780-123">String functions</span></span>
<span data-ttu-id="fd780-124">Dessa funktioner kan endast användas med JSON-strängar.</span><span class="sxs-lookup"><span data-stu-id="fd780-124">These functions can only be used with JSON strings.</span></span>

### <a name="concat"></a><span data-ttu-id="fd780-125">concat</span><span class="sxs-lookup"><span data-stu-id="fd780-125">concat</span></span>
<span data-ttu-id="fd780-126">Sammanfogar en eller flera strängar.</span><span class="sxs-lookup"><span data-stu-id="fd780-126">Concatenates one or more strings.</span></span>

<span data-ttu-id="fd780-127">Om exempelvis hello utdata värdet för `element1` om `"bar"`, och sedan det här exemplet returnerar hello sträng `"foobar!"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-127">For example, if hello output value of `element1` if `"bar"`, then this example returns hello string `"foobar!"`:</span></span>

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a><span data-ttu-id="fd780-128">delsträngen</span><span class="sxs-lookup"><span data-stu-id="fd780-128">substring</span></span>
<span data-ttu-id="fd780-129">Returnerar hello delsträngen av hello angett sträng.</span><span class="sxs-lookup"><span data-stu-id="fd780-129">Returns hello substring of hello specified string.</span></span> <span data-ttu-id="fd780-130">hello delsträngen börjar vid hello angivet index och angett hello längd.</span><span class="sxs-lookup"><span data-stu-id="fd780-130">hello substring starts at hello specified index and has hello specified length.</span></span>

<span data-ttu-id="fd780-131">hello följande exempel returnerar `"ftw"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-131">hello following example returns `"ftw"`:</span></span>

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a><span data-ttu-id="fd780-132">Ersätt</span><span class="sxs-lookup"><span data-stu-id="fd780-132">replace</span></span>
<span data-ttu-id="fd780-133">Returnerar en sträng i vilken alla förekomster av hello angett sträng i hello aktuella strängen ersätts med en annan sträng.</span><span class="sxs-lookup"><span data-stu-id="fd780-133">Returns a string in which all occurrences of hello specified string in hello current string are replaced with another string.</span></span>

<span data-ttu-id="fd780-134">hello följande exempel returnerar `"Everything is awesome!"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-134">hello following example returns `"Everything is awesome!"`:</span></span>

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a><span data-ttu-id="fd780-135">GUID</span><span class="sxs-lookup"><span data-stu-id="fd780-135">guid</span></span>
<span data-ttu-id="fd780-136">Genererar en globalt unik sträng (GUID).</span><span class="sxs-lookup"><span data-stu-id="fd780-136">Generates a globally unique string (GUID).</span></span>

<span data-ttu-id="fd780-137">hello följande exempel kunde returnera `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-137">hello following example could return `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span></span>

```json
"[guid()]"
```

### <a name="tolower"></a><span data-ttu-id="fd780-138">toLower</span><span class="sxs-lookup"><span data-stu-id="fd780-138">toLower</span></span>
<span data-ttu-id="fd780-139">Returnerar en sträng som konverteras toolowercase.</span><span class="sxs-lookup"><span data-stu-id="fd780-139">Returns a string converted toolowercase.</span></span>

<span data-ttu-id="fd780-140">hello följande exempel returnerar `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-140">hello following example returns `"foobar"`:</span></span>

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a><span data-ttu-id="fd780-141">toUpper</span><span class="sxs-lookup"><span data-stu-id="fd780-141">toUpper</span></span>
<span data-ttu-id="fd780-142">Returnerar en sträng som konverteras toouppercase.</span><span class="sxs-lookup"><span data-stu-id="fd780-142">Returns a string converted toouppercase.</span></span>

<span data-ttu-id="fd780-143">hello följande exempel returnerar `"FOOBAR"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-143">hello following example returns `"FOOBAR"`:</span></span>

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a><span data-ttu-id="fd780-144">Samlingen funktioner</span><span class="sxs-lookup"><span data-stu-id="fd780-144">Collection functions</span></span>
<span data-ttu-id="fd780-145">Dessa funktioner kan användas med samlingar som JSON-strängar, matriser och -objekt.</span><span class="sxs-lookup"><span data-stu-id="fd780-145">These functions can be used with collections, like JSON strings, arrays and objects.</span></span>

### <a name="contains"></a><span data-ttu-id="fd780-146">Innehåller</span><span class="sxs-lookup"><span data-stu-id="fd780-146">contains</span></span>
<span data-ttu-id="fd780-147">Returnerar `true` om en sträng innehåller Hej angivna delsträngen, en matris innehåller hello angivet värde eller ett objekt innehåller hello angiven nyckel.</span><span class="sxs-lookup"><span data-stu-id="fd780-147">Returns `true` if a string contains hello specified substring, an array contains hello specified value, or an object contains hello specified key.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="fd780-148">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="fd780-148">Example 1: string</span></span>
<span data-ttu-id="fd780-149">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-149">hello following example returns `true`:</span></span>

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="fd780-150">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="fd780-150">Example 2: array</span></span>
<span data-ttu-id="fd780-151">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="fd780-151">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="fd780-152">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-152">hello following example returns `false`:</span></span>

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="fd780-153">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="fd780-153">Example 3: object</span></span>
<span data-ttu-id="fd780-154">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="fd780-154">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="fd780-155">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-155">hello following example returns `true`:</span></span>

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a><span data-ttu-id="fd780-156">Längd</span><span class="sxs-lookup"><span data-stu-id="fd780-156">length</span></span>
<span data-ttu-id="fd780-157">Returnerar hello antalet tecken i en sträng, hello antalet värden i en matris eller hello antalet nycklar i ett objekt.</span><span class="sxs-lookup"><span data-stu-id="fd780-157">Returns hello number of characters in a string, hello number of values in an array, or hello number of keys in an object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="fd780-158">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="fd780-158">Example 1: string</span></span>
<span data-ttu-id="fd780-159">hello följande exempel returnerar `6`:</span><span class="sxs-lookup"><span data-stu-id="fd780-159">hello following example returns `6`:</span></span>

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="fd780-160">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="fd780-160">Example 2: array</span></span>
<span data-ttu-id="fd780-161">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="fd780-161">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="fd780-162">hello följande exempel returnerar `3`:</span><span class="sxs-lookup"><span data-stu-id="fd780-162">hello following example returns `3`:</span></span>

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="fd780-163">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="fd780-163">Example 3: object</span></span>
<span data-ttu-id="fd780-164">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="fd780-164">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="fd780-165">hello följande exempel returnerar `2`:</span><span class="sxs-lookup"><span data-stu-id="fd780-165">hello following example returns `2`:</span></span>

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a><span data-ttu-id="fd780-166">tom</span><span class="sxs-lookup"><span data-stu-id="fd780-166">empty</span></span>
<span data-ttu-id="fd780-167">Returnerar `true` om hello sträng, matris eller ett objekt är null eller tomt.</span><span class="sxs-lookup"><span data-stu-id="fd780-167">Returns `true` if hello string, array, or object is null or empty.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="fd780-168">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="fd780-168">Example 1: string</span></span>
<span data-ttu-id="fd780-169">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-169">hello following example returns `true`:</span></span>

```json
"[empty('')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="fd780-170">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="fd780-170">Example 2: array</span></span>
<span data-ttu-id="fd780-171">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="fd780-171">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="fd780-172">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-172">hello following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="fd780-173">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="fd780-173">Example 3: object</span></span>
<span data-ttu-id="fd780-174">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="fd780-174">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="fd780-175">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-175">hello following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a><span data-ttu-id="fd780-176">Exempel 4: null och odefinierad</span><span class="sxs-lookup"><span data-stu-id="fd780-176">Example 4: null and undefined</span></span>
<span data-ttu-id="fd780-177">Anta att `element1` är `null` eller odefinierad.</span><span class="sxs-lookup"><span data-stu-id="fd780-177">Assume `element1` is `null` or undefined.</span></span> <span data-ttu-id="fd780-178">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-178">hello following example returns `true`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a><span data-ttu-id="fd780-179">första</span><span class="sxs-lookup"><span data-stu-id="fd780-179">first</span></span>
<span data-ttu-id="fd780-180">Returnerar hello första tecknet i hello angett sträng; första värde för den angivna matrisen med hello; eller hello första nyckel och värde för hello angivna-objektet.</span><span class="sxs-lookup"><span data-stu-id="fd780-180">Returns hello first character of hello specified string; first value of hello specified array; or hello first key and value of hello specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="fd780-181">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="fd780-181">Example 1: string</span></span>
<span data-ttu-id="fd780-182">hello följande exempel returnerar `"f"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-182">hello following example returns `"f"`:</span></span>

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="fd780-183">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="fd780-183">Example 2: array</span></span>
<span data-ttu-id="fd780-184">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="fd780-184">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="fd780-185">hello följande exempel returnerar `1`:</span><span class="sxs-lookup"><span data-stu-id="fd780-185">hello following example returns `1`:</span></span>

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="fd780-186">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="fd780-186">Example 3: object</span></span>
<span data-ttu-id="fd780-187">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="fd780-187">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="fd780-188">hello följande exempel returnerar `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="fd780-188">hello following example returns `{"key1": "foobar"}`:</span></span>

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a><span data-ttu-id="fd780-189">senaste</span><span class="sxs-lookup"><span data-stu-id="fd780-189">last</span></span>
<span data-ttu-id="fd780-190">Returnerar hello sista tecknet i hello angetts sträng, hello sista värdet för den angivna matrisen med hello, eller hello senaste nyckel och värde för hello angivna-objektet.</span><span class="sxs-lookup"><span data-stu-id="fd780-190">Returns hello last character of hello specified string, hello last value of hello specified array, or hello last key and value of hello specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="fd780-191">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="fd780-191">Example 1: string</span></span>
<span data-ttu-id="fd780-192">hello följande exempel returnerar `"r"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-192">hello following example returns `"r"`:</span></span>

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="fd780-193">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="fd780-193">Example 2: array</span></span>
<span data-ttu-id="fd780-194">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="fd780-194">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="fd780-195">hello följande exempel returnerar `2`:</span><span class="sxs-lookup"><span data-stu-id="fd780-195">hello following example returns `2`:</span></span>

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="fd780-196">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="fd780-196">Example 3: object</span></span>
<span data-ttu-id="fd780-197">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="fd780-197">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="fd780-198">hello följande exempel returnerar `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="fd780-198">hello following example returns `{"key2": "raboof"}`:</span></span>

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a><span data-ttu-id="fd780-199">ta</span><span class="sxs-lookup"><span data-stu-id="fd780-199">take</span></span>
<span data-ttu-id="fd780-200">Returnerar ett angivet antal sammanhängande tecken från hello början av hello sträng, ett angivet antal sammanhängande värden från hello start av hello matris eller ett angivet antal sammanhängande nycklar och värden från hello start av hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="fd780-200">Returns a specified number of contiguous characters from hello start of hello string, a specified number of contiguous values from hello start of hello array, or a specified number of contiguous keys and values from hello start of hello object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="fd780-201">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="fd780-201">Example 1: string</span></span>
<span data-ttu-id="fd780-202">hello följande exempel returnerar `"foo"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-202">hello following example returns `"foo"`:</span></span>

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="fd780-203">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="fd780-203">Example 2: array</span></span>
<span data-ttu-id="fd780-204">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="fd780-204">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="fd780-205">hello följande exempel returnerar `[1, 2]`:</span><span class="sxs-lookup"><span data-stu-id="fd780-205">hello following example returns `[1, 2]`:</span></span>

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="fd780-206">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="fd780-206">Example 3: object</span></span>
<span data-ttu-id="fd780-207">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="fd780-207">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="fd780-208">hello följande exempel returnerar `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="fd780-208">hello following example returns `{"key1": "foobar"}`:</span></span>

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a><span data-ttu-id="fd780-209">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="fd780-209">skip</span></span>
<span data-ttu-id="fd780-210">Kringgår ett angivet antal element i en mängd och returnerar sedan hello återstående element.</span><span class="sxs-lookup"><span data-stu-id="fd780-210">Bypasses a specified number of elements in a collection, and then returns hello remaining elements.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="fd780-211">Exempel 1: sträng</span><span class="sxs-lookup"><span data-stu-id="fd780-211">Example 1: string</span></span>
<span data-ttu-id="fd780-212">hello följande exempel returnerar `"bar"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-212">hello following example returns `"bar"`:</span></span>

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="fd780-213">Exempel 2: matris</span><span class="sxs-lookup"><span data-stu-id="fd780-213">Example 2: array</span></span>
<span data-ttu-id="fd780-214">Anta att `element1` returnerar `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="fd780-214">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="fd780-215">hello följande exempel returnerar `[3]`:</span><span class="sxs-lookup"><span data-stu-id="fd780-215">hello following example returns `[3]`:</span></span>

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="fd780-216">Exempel 3: objektet</span><span class="sxs-lookup"><span data-stu-id="fd780-216">Example 3: object</span></span>
<span data-ttu-id="fd780-217">Anta att `element1` returnerar:</span><span class="sxs-lookup"><span data-stu-id="fd780-217">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="fd780-218">hello följande exempel returnerar `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="fd780-218">hello following example returns `{"key2": "raboof"}`:</span></span>

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a><span data-ttu-id="fd780-219">Logiska funktioner</span><span class="sxs-lookup"><span data-stu-id="fd780-219">Logical functions</span></span>
<span data-ttu-id="fd780-220">Dessa funktioner kan användas i villkorlig sats.</span><span class="sxs-lookup"><span data-stu-id="fd780-220">These functions can be used in conditionals.</span></span> <span data-ttu-id="fd780-221">Vissa funktioner kanske inte stöder alla JSON-datatyper.</span><span class="sxs-lookup"><span data-stu-id="fd780-221">Some functions may not support all JSON data types.</span></span>

### <a name="equals"></a><span data-ttu-id="fd780-222">är lika med</span><span class="sxs-lookup"><span data-stu-id="fd780-222">equals</span></span>
<span data-ttu-id="fd780-223">Returnerar `true` om båda parametrarna har hello samma anger och värde.</span><span class="sxs-lookup"><span data-stu-id="fd780-223">Returns `true` if both parameters have hello same type and value.</span></span> <span data-ttu-id="fd780-224">Den här funktionen stöder alla JSON-datatyper.</span><span class="sxs-lookup"><span data-stu-id="fd780-224">This function supports all JSON data types.</span></span>

<span data-ttu-id="fd780-225">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-225">hello following example returns `true`:</span></span>

```json
"[equals(0, 0)]"
```

<span data-ttu-id="fd780-226">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-226">hello following example returns `true`:</span></span>

```json
"[equals('foo', 'foo')]"
```

<span data-ttu-id="fd780-227">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-227">hello following example returns `false`:</span></span>

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a><span data-ttu-id="fd780-228">mindre</span><span class="sxs-lookup"><span data-stu-id="fd780-228">less</span></span>
<span data-ttu-id="fd780-229">Returnerar `true` om hello första parametern är strikt mindre än andra hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="fd780-229">Returns `true` if hello first parameter is strictly less than hello second parameter.</span></span> <span data-ttu-id="fd780-230">Den här funktionen har stöd för parametrarna endast för nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="fd780-230">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="fd780-231">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-231">hello following example returns `true`:</span></span>

```json
"[less(1, 2)]"
```

<span data-ttu-id="fd780-232">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-232">hello following example returns `false`:</span></span>

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a><span data-ttu-id="fd780-233">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="fd780-233">lessOrEquals</span></span>
<span data-ttu-id="fd780-234">Returnerar `true` om hello första parametern är mindre än eller lika med toohello andra parametern.</span><span class="sxs-lookup"><span data-stu-id="fd780-234">Returns `true` if hello first parameter is less than or equal toohello second parameter.</span></span> <span data-ttu-id="fd780-235">Den här funktionen har stöd för parametrarna endast för nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="fd780-235">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="fd780-236">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-236">hello following example returns `true`:</span></span>

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a><span data-ttu-id="fd780-237">större</span><span class="sxs-lookup"><span data-stu-id="fd780-237">greater</span></span>
<span data-ttu-id="fd780-238">Returnerar `true` om hello första parametern är strikt större än andra hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="fd780-238">Returns `true` if hello first parameter is strictly greater than hello second parameter.</span></span> <span data-ttu-id="fd780-239">Den här funktionen har stöd för parametrarna endast för nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="fd780-239">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="fd780-240">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-240">hello following example returns `false`:</span></span>

```json
"[greater(1, 2)]"
```

<span data-ttu-id="fd780-241">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-241">hello following example returns `true`:</span></span>

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a><span data-ttu-id="fd780-242">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="fd780-242">greaterOrEquals</span></span>
<span data-ttu-id="fd780-243">Returnerar `true` om hello första parametern är större än eller lika toohello andra parametern.</span><span class="sxs-lookup"><span data-stu-id="fd780-243">Returns `true` if hello first parameter is greater than or equal toohello second parameter.</span></span> <span data-ttu-id="fd780-244">Den här funktionen har stöd för parametrarna endast för nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="fd780-244">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="fd780-245">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-245">hello following example returns `true`:</span></span>

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a><span data-ttu-id="fd780-246">och</span><span class="sxs-lookup"><span data-stu-id="fd780-246">and</span></span>
<span data-ttu-id="fd780-247">Returnerar `true` om alla hello parametrarna för`true`.</span><span class="sxs-lookup"><span data-stu-id="fd780-247">Returns `true` if all hello parameters evaluate too`true`.</span></span> <span data-ttu-id="fd780-248">Den här funktionen har stöd för två eller flera parametrar av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="fd780-248">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="fd780-249">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-249">hello following example returns `true`:</span></span>

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="fd780-250">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-250">hello following example returns `false`:</span></span>

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a><span data-ttu-id="fd780-251">eller</span><span class="sxs-lookup"><span data-stu-id="fd780-251">or</span></span>
<span data-ttu-id="fd780-252">Returnerar `true` om minst en av parametrarna hello utvärderar för`true`.</span><span class="sxs-lookup"><span data-stu-id="fd780-252">Returns `true` if at least one of hello parameters evaluates too`true`.</span></span> <span data-ttu-id="fd780-253">Den här funktionen har stöd för två eller flera parametrar av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="fd780-253">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="fd780-254">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-254">hello following example returns `true`:</span></span>

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="fd780-255">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-255">hello following example returns `true`:</span></span>

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a><span data-ttu-id="fd780-256">inte</span><span class="sxs-lookup"><span data-stu-id="fd780-256">not</span></span>
<span data-ttu-id="fd780-257">Returnerar `true` om hello parametern utvärderar för`false`.</span><span class="sxs-lookup"><span data-stu-id="fd780-257">Returns `true` if hello parameter evaluates too`false`.</span></span> <span data-ttu-id="fd780-258">Den här funktionen har stöd för parametrar av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="fd780-258">This function supports parameters only of type Boolean.</span></span>

<span data-ttu-id="fd780-259">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-259">hello following example returns `true`:</span></span>

```json
"[not(false)]"
```

<span data-ttu-id="fd780-260">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-260">hello following example returns `false`:</span></span>

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a><span data-ttu-id="fd780-261">Slå samman</span><span class="sxs-lookup"><span data-stu-id="fd780-261">coalesce</span></span>
<span data-ttu-id="fd780-262">Returnerar hello hello första icke-null-parameterns värde.</span><span class="sxs-lookup"><span data-stu-id="fd780-262">Returns hello value of hello first non-null parameter.</span></span> <span data-ttu-id="fd780-263">Den här funktionen stöder alla JSON-datatyper.</span><span class="sxs-lookup"><span data-stu-id="fd780-263">This function supports all JSON data types.</span></span>

<span data-ttu-id="fd780-264">Anta att `element1` och `element2` är odefinierad.</span><span class="sxs-lookup"><span data-stu-id="fd780-264">Assume `element1` and `element2` are undefined.</span></span> <span data-ttu-id="fd780-265">hello följande exempel returnerar `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-265">hello following example returns `"foobar"`:</span></span>

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a><span data-ttu-id="fd780-266">Konverteringsfunktioner</span><span class="sxs-lookup"><span data-stu-id="fd780-266">Conversion functions</span></span>
<span data-ttu-id="fd780-267">Dessa funktioner kan vara används tooconvert värden mellan JSON-datatyper och kodningar.</span><span class="sxs-lookup"><span data-stu-id="fd780-267">These functions can be used tooconvert values between JSON data types and encodings.</span></span>

### <a name="int"></a><span data-ttu-id="fd780-268">int</span><span class="sxs-lookup"><span data-stu-id="fd780-268">int</span></span>
<span data-ttu-id="fd780-269">Konverterar hello parametern tooan heltal.</span><span class="sxs-lookup"><span data-stu-id="fd780-269">Converts hello parameter tooan integer.</span></span> <span data-ttu-id="fd780-270">Den här funktionen har stöd för parametrar av typen nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="fd780-270">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="fd780-271">hello följande exempel returnerar `1`:</span><span class="sxs-lookup"><span data-stu-id="fd780-271">hello following example returns `1`:</span></span>

```json
"[int('1')]"
```

<span data-ttu-id="fd780-272">hello följande exempel returnerar `2`:</span><span class="sxs-lookup"><span data-stu-id="fd780-272">hello following example returns `2`:</span></span>

```json
"[int(2.9)]"
```

### <a name="float"></a><span data-ttu-id="fd780-273">flyttal</span><span class="sxs-lookup"><span data-stu-id="fd780-273">float</span></span>
<span data-ttu-id="fd780-274">Konverterar hello parametern tooa flyttal.</span><span class="sxs-lookup"><span data-stu-id="fd780-274">Converts hello parameter tooa floating-point.</span></span> <span data-ttu-id="fd780-275">Den här funktionen har stöd för parametrar av typen nummer och strängen.</span><span class="sxs-lookup"><span data-stu-id="fd780-275">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="fd780-276">hello följande exempel returnerar `1.0`:</span><span class="sxs-lookup"><span data-stu-id="fd780-276">hello following example returns `1.0`:</span></span>

```json
"[float('1.0')]"
```

<span data-ttu-id="fd780-277">hello följande exempel returnerar `2.9`:</span><span class="sxs-lookup"><span data-stu-id="fd780-277">hello following example returns `2.9`:</span></span>

```json
"[float(2.9)]"
```

### <a name="string"></a><span data-ttu-id="fd780-278">Sträng</span><span class="sxs-lookup"><span data-stu-id="fd780-278">string</span></span>
<span data-ttu-id="fd780-279">Konverterar hello parametern tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="fd780-279">Converts hello parameter tooa string.</span></span> <span data-ttu-id="fd780-280">Den här funktionen stöder alla datatyper i JSON-parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd780-280">This function supports parameters of all JSON data types.</span></span>

<span data-ttu-id="fd780-281">hello följande exempel returnerar `"1"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-281">hello following example returns `"1"`:</span></span>

```json
"[string(1)]"
```

<span data-ttu-id="fd780-282">hello följande exempel returnerar `"2.9"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-282">hello following example returns `"2.9"`:</span></span>

```json
"[string(2.9)]"
```

<span data-ttu-id="fd780-283">hello följande exempel returnerar `"[1,2,3]"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-283">hello following example returns `"[1,2,3]"`:</span></span>

```json
"[string([1,2,3])]"
```

<span data-ttu-id="fd780-284">hello följande exempel returnerar `"{"foo":"bar"}"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-284">hello following example returns `"{"foo":"bar"}"`:</span></span>

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a><span data-ttu-id="fd780-285">bool</span><span class="sxs-lookup"><span data-stu-id="fd780-285">bool</span></span>
<span data-ttu-id="fd780-286">Konverterar hello parametern tooa booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="fd780-286">Converts hello parameter tooa Boolean.</span></span> <span data-ttu-id="fd780-287">Den här funktionen har stöd för parametrar av typen antal, sträng och booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="fd780-287">This function supports parameters of type number, string, and Boolean.</span></span> <span data-ttu-id="fd780-288">Liknande tooBooleans i JavaScript, vilket värde som helst förutom `0` eller `'false'` returnerar `true`.</span><span class="sxs-lookup"><span data-stu-id="fd780-288">Similar tooBooleans in JavaScript, any value except `0` or `'false'` returns `true`.</span></span>

<span data-ttu-id="fd780-289">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-289">hello following example returns `true`:</span></span>

```json
"[bool(1)]"
```

<span data-ttu-id="fd780-290">hello följande exempel returnerar `false`:</span><span class="sxs-lookup"><span data-stu-id="fd780-290">hello following example returns `false`:</span></span>

```json
"[bool(0)]"
```

<span data-ttu-id="fd780-291">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-291">hello following example returns `true`:</span></span>

```json
"[bool(true)]"
```

<span data-ttu-id="fd780-292">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-292">hello following example returns `true`:</span></span>

```json
"[bool('true')]"
```

### <a name="parse"></a><span data-ttu-id="fd780-293">parsa</span><span class="sxs-lookup"><span data-stu-id="fd780-293">parse</span></span>
<span data-ttu-id="fd780-294">Konverterar hello tooa egna parametertypen.</span><span class="sxs-lookup"><span data-stu-id="fd780-294">Converts hello parameter tooa native type.</span></span> <span data-ttu-id="fd780-295">I den här funktionen är med andra ord hello inversen av `string()`.</span><span class="sxs-lookup"><span data-stu-id="fd780-295">In other words, this function is hello inverse of `string()`.</span></span> <span data-ttu-id="fd780-296">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd780-296">This function supports parameters only of type string.</span></span>

<span data-ttu-id="fd780-297">hello följande exempel returnerar `1`:</span><span class="sxs-lookup"><span data-stu-id="fd780-297">hello following example returns `1`:</span></span>

```json
"[parse('1')]"
```

<span data-ttu-id="fd780-298">hello följande exempel returnerar `true`:</span><span class="sxs-lookup"><span data-stu-id="fd780-298">hello following example returns `true`:</span></span>

```json
"[parse('true')]"
```

<span data-ttu-id="fd780-299">hello följande exempel returnerar `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="fd780-299">hello following example returns `[1,2,3]`:</span></span>

```json
"[parse('[1,2,3]')]"
```

<span data-ttu-id="fd780-300">hello följande exempel returnerar `{"foo":"bar"}`:</span><span class="sxs-lookup"><span data-stu-id="fd780-300">hello following example returns `{"foo":"bar"}`:</span></span>

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a><span data-ttu-id="fd780-301">encodeBase64</span><span class="sxs-lookup"><span data-stu-id="fd780-301">encodeBase64</span></span>
<span data-ttu-id="fd780-302">Kodar hello parametern tooa Base64-kodad sträng.</span><span class="sxs-lookup"><span data-stu-id="fd780-302">Encodes hello parameter tooa base-64 encoded string.</span></span> <span data-ttu-id="fd780-303">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd780-303">This function supports parameters only of type string.</span></span>

<span data-ttu-id="fd780-304">hello följande exempel returnerar `"Zm9vYmFy"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-304">hello following example returns `"Zm9vYmFy"`:</span></span>

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a><span data-ttu-id="fd780-305">decodeBase64</span><span class="sxs-lookup"><span data-stu-id="fd780-305">decodeBase64</span></span>
<span data-ttu-id="fd780-306">Avkodar hello-parameter från en Base64-kodad sträng.</span><span class="sxs-lookup"><span data-stu-id="fd780-306">Decodes hello parameter from a base-64 encoded string.</span></span> <span data-ttu-id="fd780-307">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd780-307">This function supports parameters only of type string.</span></span>

<span data-ttu-id="fd780-308">hello följande exempel returnerar `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-308">hello following example returns `"foobar"`:</span></span>

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a><span data-ttu-id="fd780-309">encodeUriComponent</span><span class="sxs-lookup"><span data-stu-id="fd780-309">encodeUriComponent</span></span>
<span data-ttu-id="fd780-310">Kodar hello parametern tooa URL-kodad sträng.</span><span class="sxs-lookup"><span data-stu-id="fd780-310">Encodes hello parameter tooa URL encoded string.</span></span> <span data-ttu-id="fd780-311">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd780-311">This function supports parameters only of type string.</span></span>

<span data-ttu-id="fd780-312">hello följande exempel returnerar `"https%3A%2F%2Fportal.azure.com%2F"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-312">hello following example returns `"https%3A%2F%2Fportal.azure.com%2F"`:</span></span>

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a><span data-ttu-id="fd780-313">decodeUriComponent</span><span class="sxs-lookup"><span data-stu-id="fd780-313">decodeUriComponent</span></span>
<span data-ttu-id="fd780-314">Avkodar hello-parameter från en URL-kodad sträng.</span><span class="sxs-lookup"><span data-stu-id="fd780-314">Decodes hello parameter from a URL encoded string.</span></span> <span data-ttu-id="fd780-315">Den här funktionen stöder endast av typen string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd780-315">This function supports parameters only of type string.</span></span>

<span data-ttu-id="fd780-316">hello följande exempel returnerar `"https://portal.azure.com/"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-316">hello following example returns `"https://portal.azure.com/"`:</span></span>

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a><span data-ttu-id="fd780-317">Matematiska funktioner</span><span class="sxs-lookup"><span data-stu-id="fd780-317">Math functions</span></span>
### <a name="add"></a><span data-ttu-id="fd780-318">Lägg till</span><span class="sxs-lookup"><span data-stu-id="fd780-318">add</span></span>
<span data-ttu-id="fd780-319">Adderar två tal och returnerar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="fd780-319">Adds two numbers, and returns hello result.</span></span>

<span data-ttu-id="fd780-320">hello följande exempel returnerar `3`:</span><span class="sxs-lookup"><span data-stu-id="fd780-320">hello following example returns `3`:</span></span>

```json
"[add(1, 2)]"
```

### <a name="sub"></a><span data-ttu-id="fd780-321">Sub</span><span class="sxs-lookup"><span data-stu-id="fd780-321">sub</span></span>
<span data-ttu-id="fd780-322">Subtraherar hello andra tal från första hello och returnerar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="fd780-322">Subtracts hello second number from hello first number, and returns hello result.</span></span>

<span data-ttu-id="fd780-323">hello följande exempel returnerar `1`:</span><span class="sxs-lookup"><span data-stu-id="fd780-323">hello following example returns `1`:</span></span>

```json
"[sub(3, 2)]"
```

### <a name="mul"></a><span data-ttu-id="fd780-324">mul</span><span class="sxs-lookup"><span data-stu-id="fd780-324">mul</span></span>
<span data-ttu-id="fd780-325">Multiplicerar två tal och returnerar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="fd780-325">Multiplies two numbers, and returns hello result.</span></span>

<span data-ttu-id="fd780-326">hello följande exempel returnerar `6`:</span><span class="sxs-lookup"><span data-stu-id="fd780-326">hello following example returns `6`:</span></span>

```json
"[mul(2, 3)]"
```

### <a name="div"></a><span data-ttu-id="fd780-327">div</span><span class="sxs-lookup"><span data-stu-id="fd780-327">div</span></span>
<span data-ttu-id="fd780-328">Dividerar hello hello andra talet första tal och returnerar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="fd780-328">Divides hello first number by hello second number, and returns hello result.</span></span> <span data-ttu-id="fd780-329">hello resultatet är alltid ett heltal.</span><span class="sxs-lookup"><span data-stu-id="fd780-329">hello result is always an integer.</span></span>

<span data-ttu-id="fd780-330">hello följande exempel returnerar `2`:</span><span class="sxs-lookup"><span data-stu-id="fd780-330">hello following example returns `2`:</span></span>

```json
"[div(6, 3)]"
```

### <a name="mod"></a><span data-ttu-id="fd780-331">MOD</span><span class="sxs-lookup"><span data-stu-id="fd780-331">mod</span></span>
<span data-ttu-id="fd780-332">Dividerar hello hello andra talet första tal och returnerar hello resten.</span><span class="sxs-lookup"><span data-stu-id="fd780-332">Divides hello first number by hello second number, and returns hello remainder.</span></span>

<span data-ttu-id="fd780-333">hello följande exempel returnerar `0`:</span><span class="sxs-lookup"><span data-stu-id="fd780-333">hello following example returns `0`:</span></span>

```json
"[mod(6, 3)]"
```

<span data-ttu-id="fd780-334">hello följande exempel returnerar `2`:</span><span class="sxs-lookup"><span data-stu-id="fd780-334">hello following example returns `2`:</span></span>

```json
"[mod(6, 4)]"
```

### <a name="min"></a><span data-ttu-id="fd780-335">min.</span><span class="sxs-lookup"><span data-stu-id="fd780-335">min</span></span>
<span data-ttu-id="fd780-336">Returnerar hello liten av hello två tal.</span><span class="sxs-lookup"><span data-stu-id="fd780-336">Returns hello small of hello two numbers.</span></span>

<span data-ttu-id="fd780-337">hello följande exempel returnerar `1`:</span><span class="sxs-lookup"><span data-stu-id="fd780-337">hello following example returns `1`:</span></span>

```json
"[min(1, 2)]"
```

### <a name="max"></a><span data-ttu-id="fd780-338">Max</span><span class="sxs-lookup"><span data-stu-id="fd780-338">max</span></span>
<span data-ttu-id="fd780-339">Returnerar hello större av hello två tal.</span><span class="sxs-lookup"><span data-stu-id="fd780-339">Returns hello larger of hello two numbers.</span></span>

<span data-ttu-id="fd780-340">hello följande exempel returnerar `2`:</span><span class="sxs-lookup"><span data-stu-id="fd780-340">hello following example returns `2`:</span></span>

```json
"[max(1, 2)]"
```

### <a name="range"></a><span data-ttu-id="fd780-341">intervallet</span><span class="sxs-lookup"><span data-stu-id="fd780-341">range</span></span>
<span data-ttu-id="fd780-342">Genererar en sekvens av integral siffror i hello intervall som anges.</span><span class="sxs-lookup"><span data-stu-id="fd780-342">Generates a sequence of integral numbers within hello specified range.</span></span>

<span data-ttu-id="fd780-343">hello följande exempel returnerar `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="fd780-343">hello following example returns `[1,2,3]`:</span></span>

```json
"[range(1, 3)]"
```

### <a name="rand"></a><span data-ttu-id="fd780-344">SLUMP</span><span class="sxs-lookup"><span data-stu-id="fd780-344">rand</span></span>
<span data-ttu-id="fd780-345">Returnerar ett slumpmässigt heltal inom hello intervall som anges.</span><span class="sxs-lookup"><span data-stu-id="fd780-345">Returns a random integral number within hello specified range.</span></span> <span data-ttu-id="fd780-346">Den här funktionen genererar inte kryptografiskt säker slumptal.</span><span class="sxs-lookup"><span data-stu-id="fd780-346">This function does not generate cryptographically secure random numbers.</span></span>

<span data-ttu-id="fd780-347">hello följande exempel kunde returnera `42`:</span><span class="sxs-lookup"><span data-stu-id="fd780-347">hello following example could return `42`:</span></span>

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a><span data-ttu-id="fd780-348">våning</span><span class="sxs-lookup"><span data-stu-id="fd780-348">floor</span></span>
<span data-ttu-id="fd780-349">Returnerar hello största heltal som är mindre än eller lika toohello angetts nummer.</span><span class="sxs-lookup"><span data-stu-id="fd780-349">Returns hello largest integer less than or equal toohello specified number.</span></span>

<span data-ttu-id="fd780-350">hello följande exempel returnerar `3`:</span><span class="sxs-lookup"><span data-stu-id="fd780-350">hello following example returns `3`:</span></span>

```json
"[floor(3.14)]"
```

### <a name="ceil"></a><span data-ttu-id="fd780-351">ceil</span><span class="sxs-lookup"><span data-stu-id="fd780-351">ceil</span></span>
<span data-ttu-id="fd780-352">Returnerar hello största heltal som är större än eller lika med toohello angetts nummer.</span><span class="sxs-lookup"><span data-stu-id="fd780-352">Returns hello largest integer greater than or equal toohello specified number.</span></span>

<span data-ttu-id="fd780-353">hello följande exempel returnerar `4`:</span><span class="sxs-lookup"><span data-stu-id="fd780-353">hello following example returns `4`:</span></span>

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a><span data-ttu-id="fd780-354">Datumfunktioner</span><span class="sxs-lookup"><span data-stu-id="fd780-354">Date functions</span></span>
### <a name="utcnow"></a><span data-ttu-id="fd780-355">utcNow</span><span class="sxs-lookup"><span data-stu-id="fd780-355">utcNow</span></span>
<span data-ttu-id="fd780-356">Returnerar en sträng i ISO 8601-format för hello på hello lokala datorns aktuella datum och tid.</span><span class="sxs-lookup"><span data-stu-id="fd780-356">Returns a string in ISO 8601 format of hello current date and time on hello local computer.</span></span>

<span data-ttu-id="fd780-357">hello följande exempel kunde returnera `"1990-12-31T23:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-357">hello following example could return `"1990-12-31T23:59:59.000Z"`:</span></span>

```json
"[utcNow()]"
```

### <a name="addseconds"></a><span data-ttu-id="fd780-358">Lägg_till_sekunder</span><span class="sxs-lookup"><span data-stu-id="fd780-358">addSeconds</span></span>
<span data-ttu-id="fd780-359">Lägger till ett heltal med sekunder toohello angivna tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="fd780-359">Adds an integral number of seconds toohello specified timestamp.</span></span>

<span data-ttu-id="fd780-360">hello följande exempel returnerar `"1991-01-01T00:00:00.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-360">hello following example returns `"1991-01-01T00:00:00.000Z"`:</span></span>

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a><span data-ttu-id="fd780-361">addMinutes</span><span class="sxs-lookup"><span data-stu-id="fd780-361">addMinutes</span></span>
<span data-ttu-id="fd780-362">Lägger till ett heltal med minuter toohello angivna tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="fd780-362">Adds an integral number of minutes toohello specified timestamp.</span></span>

<span data-ttu-id="fd780-363">hello följande exempel returnerar `"1991-01-01T00:00:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-363">hello following example returns `"1991-01-01T00:00:59.000Z"`:</span></span>

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a><span data-ttu-id="fd780-364">addHours</span><span class="sxs-lookup"><span data-stu-id="fd780-364">addHours</span></span>
<span data-ttu-id="fd780-365">Lägger till ett heltal med timmar toohello angivna tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="fd780-365">Adds an integral number of hours toohello specified timestamp.</span></span>

<span data-ttu-id="fd780-366">hello följande exempel returnerar `"1991-01-01T00:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="fd780-366">hello following example returns `"1991-01-01T00:59:59.000Z"`:</span></span>

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a><span data-ttu-id="fd780-367">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd780-367">Next steps</span></span>
* <span data-ttu-id="fd780-368">En introduktion tooAzure Resource Manager finns [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fd780-368">For an introduction tooAzure Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

