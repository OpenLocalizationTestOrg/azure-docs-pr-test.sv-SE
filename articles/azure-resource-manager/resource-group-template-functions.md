---
title: aaaResource Manager Mallfunktioner | Microsoft Docs
description: "Beskriver hello funktioner toouse arbeta med strängar och siffror i en Azure Resource Manager mallen tooretrieve värden och hämta information om distribution."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: tomfitz
ms.openlocfilehash: d1b2e68a33d75058f83d6972dadb33a6390d49b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-template-functions"></a><span data-ttu-id="36cb8-103">Azure Resource Manager Mallfunktioner</span><span class="sxs-lookup"><span data-stu-id="36cb8-103">Azure Resource Manager template functions</span></span>
<span data-ttu-id="36cb8-104">Det här avsnittet beskriver alla hello-funktioner som du kan använda i en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="36cb8-104">This topic describes all hello functions you can use in an Azure Resource Manager template.</span></span>

<span data-ttu-id="36cb8-105">Du lägger till funktioner i dina mallar genom att skriva dem inom hakparenteser: `[` och `]`respektive.</span><span class="sxs-lookup"><span data-stu-id="36cb8-105">You add functions in your templates by enclosing them within brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="36cb8-106">hello uttrycket utvärderas under distributionen.</span><span class="sxs-lookup"><span data-stu-id="36cb8-106">hello expression is evaluated during deployment.</span></span> <span data-ttu-id="36cb8-107">Medan skrivs som en teckensträng kan hello resultat av utvärderingen av hello uttryck vara av en annan JSON-typ, till exempel en matris, objekt eller heltal.</span><span class="sxs-lookup"><span data-stu-id="36cb8-107">While written as a string literal, hello result of evaluating hello expression can be of a different JSON type, such as an array, object, or integer.</span></span> <span data-ttu-id="36cb8-108">Precis som i JavaScript-funktionsanrop som är formaterade som `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="36cb8-108">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="36cb8-109">Du kan referera egenskaper med hjälp av hello punkt och [index] operatörer.</span><span class="sxs-lookup"><span data-stu-id="36cb8-109">You reference properties by using hello dot and [index] operators.</span></span>

<span data-ttu-id="36cb8-110">Ett malluttryck får inte överskrida 24,576 tecken.</span><span class="sxs-lookup"><span data-stu-id="36cb8-110">A template expression cannot exceed 24,576 characters.</span></span>

<span data-ttu-id="36cb8-111">Mallfunktioner och deras parametrar är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="36cb8-111">Template functions and their parameters are case-insensitive.</span></span> <span data-ttu-id="36cb8-112">Till exempel Resource Manager matchar **variables('var1')** och **VARIABLES('VAR1')** som hello samma.</span><span class="sxs-lookup"><span data-stu-id="36cb8-112">For example, Resource Manager resolves **variables('var1')** and **VARIABLES('VAR1')** as hello same.</span></span> <span data-ttu-id="36cb8-113">När utvärderas om hello funktionen ändrar uttryckligen skiftläge (till exempel toUpper eller toLower), hello funktionen behåller hello skiftläge.</span><span class="sxs-lookup"><span data-stu-id="36cb8-113">When evaluated, unless hello function expressly modifies case (such as toUpper or toLower), hello function preserves hello case.</span></span> <span data-ttu-id="36cb8-114">Vissa typer av resurser kan ha case krav oavsett hur funktioner utvärderas.</span><span class="sxs-lookup"><span data-stu-id="36cb8-114">Certain resource types may have case requirements irrespective of how functions are evaluated.</span></span>

<a id="array" />
<a id="coalesce" />
<a id="concatarray" />
<a id="contains" />
<a id="createarray" />
<a id="empty" />
<a id="first" />
<a id="intersection" />
<a id="last" />
<a id="length" />
<a id="min" />
<a id="max" />
<a id="range" />
<a id="skip" />
<a id="take" />
<a id="union" />

## <a name="array-and-object-functions"></a><span data-ttu-id="36cb8-115">Array- och funktioner</span><span class="sxs-lookup"><span data-stu-id="36cb8-115">Array and object functions</span></span>
<span data-ttu-id="36cb8-116">Resource Manager innehåller flera funktioner för att arbeta med matriser och -objekt.</span><span class="sxs-lookup"><span data-stu-id="36cb8-116">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="36cb8-117">matris</span><span class="sxs-lookup"><span data-stu-id="36cb8-117">array</span></span>](resource-group-template-functions-array.md#array)
* [<span data-ttu-id="36cb8-118">Slå samman</span><span class="sxs-lookup"><span data-stu-id="36cb8-118">coalesce</span></span>](resource-group-template-functions-array.md#coalesce)
* [<span data-ttu-id="36cb8-119">concat</span><span class="sxs-lookup"><span data-stu-id="36cb8-119">concat</span></span>](resource-group-template-functions-array.md#concat)
* [<span data-ttu-id="36cb8-120">innehåller</span><span class="sxs-lookup"><span data-stu-id="36cb8-120">contains</span></span>](resource-group-template-functions-array.md#contains)
* [<span data-ttu-id="36cb8-121">createArray</span><span class="sxs-lookup"><span data-stu-id="36cb8-121">createArray</span></span>](resource-group-template-functions-array.md#createarray)
* [<span data-ttu-id="36cb8-122">tom</span><span class="sxs-lookup"><span data-stu-id="36cb8-122">empty</span></span>](resource-group-template-functions-array.md#empty)
* [<span data-ttu-id="36cb8-123">första</span><span class="sxs-lookup"><span data-stu-id="36cb8-123">first</span></span>](resource-group-template-functions-array.md#first)
* [<span data-ttu-id="36cb8-124">skärningspunkten</span><span class="sxs-lookup"><span data-stu-id="36cb8-124">intersection</span></span>](resource-group-template-functions-array.md#intersection)
* [<span data-ttu-id="36cb8-125">JSON</span><span class="sxs-lookup"><span data-stu-id="36cb8-125">json</span></span>](resource-group-template-functions-array.md#json)
* [<span data-ttu-id="36cb8-126">senaste</span><span class="sxs-lookup"><span data-stu-id="36cb8-126">last</span></span>](resource-group-template-functions-array.md#last)
* [<span data-ttu-id="36cb8-127">längd</span><span class="sxs-lookup"><span data-stu-id="36cb8-127">length</span></span>](resource-group-template-functions-array.md#length)
* [<span data-ttu-id="36cb8-128">Min</span><span class="sxs-lookup"><span data-stu-id="36cb8-128">min</span></span>](resource-group-template-functions-array.md#min)
* [<span data-ttu-id="36cb8-129">Max</span><span class="sxs-lookup"><span data-stu-id="36cb8-129">max</span></span>](resource-group-template-functions-array.md#max)
* [<span data-ttu-id="36cb8-130">intervallet</span><span class="sxs-lookup"><span data-stu-id="36cb8-130">range</span></span>](resource-group-template-functions-array.md#range)
* [<span data-ttu-id="36cb8-131">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="36cb8-131">skip</span></span>](resource-group-template-functions-array.md#skip)
* [<span data-ttu-id="36cb8-132">ta</span><span class="sxs-lookup"><span data-stu-id="36cb8-132">take</span></span>](resource-group-template-functions-array.md#take)
* [<span data-ttu-id="36cb8-133">Union</span><span class="sxs-lookup"><span data-stu-id="36cb8-133">union</span></span>](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a><span data-ttu-id="36cb8-134">Jämförelse funktioner</span><span class="sxs-lookup"><span data-stu-id="36cb8-134">Comparison functions</span></span>
<span data-ttu-id="36cb8-135">Resource Manager innehåller flera funktioner för att göra jämförelser i dina mallar.</span><span class="sxs-lookup"><span data-stu-id="36cb8-135">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="36cb8-136">är lika med</span><span class="sxs-lookup"><span data-stu-id="36cb8-136">equals</span></span>](resource-group-template-functions-comparison.md#equals)
* [<span data-ttu-id="36cb8-137">mindre</span><span class="sxs-lookup"><span data-stu-id="36cb8-137">less</span></span>](resource-group-template-functions-comparison.md#less)
* [<span data-ttu-id="36cb8-138">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="36cb8-138">lessOrEquals</span></span>](resource-group-template-functions-comparison.md#lessorequals)
* [<span data-ttu-id="36cb8-139">större</span><span class="sxs-lookup"><span data-stu-id="36cb8-139">greater</span></span>](resource-group-template-functions-comparison.md#greater)
* [<span data-ttu-id="36cb8-140">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="36cb8-140">greaterOrEquals</span></span>](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a><span data-ttu-id="36cb8-141">Funktioner för distribution av värdet</span><span class="sxs-lookup"><span data-stu-id="36cb8-141">Deployment value functions</span></span>
<span data-ttu-id="36cb8-142">Resource Manager tillhandahåller hello följande funktioner för att hämta värden från delar av hello mall och värden relaterade toohello distribution:</span><span class="sxs-lookup"><span data-stu-id="36cb8-142">Resource Manager provides hello following functions for getting values from sections of hello template and values related toohello deployment:</span></span>

* [<span data-ttu-id="36cb8-143">distribution</span><span class="sxs-lookup"><span data-stu-id="36cb8-143">deployment</span></span>](resource-group-template-functions-deployment.md#deployment)
* [<span data-ttu-id="36cb8-144">parametrar</span><span class="sxs-lookup"><span data-stu-id="36cb8-144">parameters</span></span>](resource-group-template-functions-deployment.md#parameters)
* [<span data-ttu-id="36cb8-145">variabler</span><span class="sxs-lookup"><span data-stu-id="36cb8-145">variables</span></span>](resource-group-template-functions-deployment.md#variables)

<a id="add" />
<a id="copyindex" />
<a id="div" />
<a id="float" />
<a id="int" />
<a id="minint" />
<a id="maxint" />
<a id="mod" />
<a id="mul" />
<a id="sub" />

## <a name="logical-functions"></a><span data-ttu-id="36cb8-146">Logiska funktioner</span><span class="sxs-lookup"><span data-stu-id="36cb8-146">Logical functions</span></span>
<span data-ttu-id="36cb8-147">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med logiska villkor hello:</span><span class="sxs-lookup"><span data-stu-id="36cb8-147">Resource Manager provides hello following functions for working with logical conditions:</span></span>

* [<span data-ttu-id="36cb8-148">och</span><span class="sxs-lookup"><span data-stu-id="36cb8-148">and</span></span>](resource-group-template-functions-logical.md#and)
* [<span data-ttu-id="36cb8-149">bool</span><span class="sxs-lookup"><span data-stu-id="36cb8-149">bool</span></span>](resource-group-template-functions-logical.md#bool)
* [<span data-ttu-id="36cb8-150">Om</span><span class="sxs-lookup"><span data-stu-id="36cb8-150">if</span></span>](resource-group-template-functions-logical.md#if)
* [<span data-ttu-id="36cb8-151">inte</span><span class="sxs-lookup"><span data-stu-id="36cb8-151">not</span></span>](resource-group-template-functions-logical.md#not)
* [<span data-ttu-id="36cb8-152">eller</span><span class="sxs-lookup"><span data-stu-id="36cb8-152">or</span></span>](resource-group-template-functions-logical.md#or)

## <a name="numeric-functions"></a><span data-ttu-id="36cb8-153">Numeriska funktioner</span><span class="sxs-lookup"><span data-stu-id="36cb8-153">Numeric functions</span></span>
<span data-ttu-id="36cb8-154">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med heltal hello:</span><span class="sxs-lookup"><span data-stu-id="36cb8-154">Resource Manager provides hello following functions for working with integers:</span></span>

* [<span data-ttu-id="36cb8-155">Lägg till</span><span class="sxs-lookup"><span data-stu-id="36cb8-155">add</span></span>](resource-group-template-functions-numeric.md#add)
* [<span data-ttu-id="36cb8-156">copyIndex</span><span class="sxs-lookup"><span data-stu-id="36cb8-156">copyIndex</span></span>](resource-group-template-functions-numeric.md#copyindex)
* [<span data-ttu-id="36cb8-157">div</span><span class="sxs-lookup"><span data-stu-id="36cb8-157">div</span></span>](resource-group-template-functions-numeric.md#div)
* [<span data-ttu-id="36cb8-158">flyttal</span><span class="sxs-lookup"><span data-stu-id="36cb8-158">float</span></span>](resource-group-template-functions-numeric.md#float)
* [<span data-ttu-id="36cb8-159">int</span><span class="sxs-lookup"><span data-stu-id="36cb8-159">int</span></span>](resource-group-template-functions-numeric.md#int)
* [<span data-ttu-id="36cb8-160">Min</span><span class="sxs-lookup"><span data-stu-id="36cb8-160">min</span></span>](resource-group-template-functions-numeric.md#min)
* [<span data-ttu-id="36cb8-161">Max</span><span class="sxs-lookup"><span data-stu-id="36cb8-161">max</span></span>](resource-group-template-functions-numeric.md#max)
* [<span data-ttu-id="36cb8-162">MOD</span><span class="sxs-lookup"><span data-stu-id="36cb8-162">mod</span></span>](resource-group-template-functions-numeric.md#mod)
* [<span data-ttu-id="36cb8-163">mul</span><span class="sxs-lookup"><span data-stu-id="36cb8-163">mul</span></span>](resource-group-template-functions-numeric.md#mul)
* [<span data-ttu-id="36cb8-164">Sub</span><span class="sxs-lookup"><span data-stu-id="36cb8-164">sub</span></span>](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a><span data-ttu-id="36cb8-165">Resursfunktioner</span><span class="sxs-lookup"><span data-stu-id="36cb8-165">Resource functions</span></span>
<span data-ttu-id="36cb8-166">Resource Manager tillhandahåller hello följande funktioner för att hämta resurs värden:</span><span class="sxs-lookup"><span data-stu-id="36cb8-166">Resource Manager provides hello following functions for getting resource values:</span></span>

* [<span data-ttu-id="36cb8-167">listKeys och lista {Value}</span><span class="sxs-lookup"><span data-stu-id="36cb8-167">listKeys and list{Value}</span></span>](resource-group-template-functions-resource.md#listkeys)
* [<span data-ttu-id="36cb8-168">providers</span><span class="sxs-lookup"><span data-stu-id="36cb8-168">providers</span></span>](resource-group-template-functions-resource.md#providers)
* [<span data-ttu-id="36cb8-169">referens</span><span class="sxs-lookup"><span data-stu-id="36cb8-169">reference</span></span>](resource-group-template-functions-resource.md#reference)
* [<span data-ttu-id="36cb8-170">resourceGroup</span><span class="sxs-lookup"><span data-stu-id="36cb8-170">resourceGroup</span></span>](resource-group-template-functions-resource.md#resourcegroup)
* [<span data-ttu-id="36cb8-171">resurs-ID</span><span class="sxs-lookup"><span data-stu-id="36cb8-171">resourceId</span></span>](resource-group-template-functions-resource.md#resourceid)
* [<span data-ttu-id="36cb8-172">prenumeration</span><span class="sxs-lookup"><span data-stu-id="36cb8-172">subscription</span></span>](resource-group-template-functions-resource.md#subscription)

<a id="base64" />
<a id="base64tojson" />
<a id="base64tostring" />
<a id="concat" />
<a id="containsstring" />
<a id="datauri" />
<a id="datauritostring" />
<a id="emptystring" />
<a id="endswith" />
<a id="firststring" />
<a id="indexof" />
<a id="laststring" />
<a id="lastindexof" />
<a id="lengthstring" />
<a id="padleft" />
<a id="replace" />
<a id="skipstring" />
<a id="split" />
<a id="startswith" />
<a id="string" />
<a id="substring" />
<a id="takestring" />
<a id="tolower" />
<a id="toupper" />
<a id="trim" />
<a id="uniquestring" />
<a id="uri" />
<a id="uricomponent" />
<a id="uricomponenttostring" />

## <a name="string-functions"></a><span data-ttu-id="36cb8-173">Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="36cb8-173">String functions</span></span>
<span data-ttu-id="36cb8-174">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med strängar hello:</span><span class="sxs-lookup"><span data-stu-id="36cb8-174">Resource Manager provides hello following functions for working with strings:</span></span>

* [<span data-ttu-id="36cb8-175">Base64</span><span class="sxs-lookup"><span data-stu-id="36cb8-175">base64</span></span>](resource-group-template-functions-string.md#base64)
* [<span data-ttu-id="36cb8-176">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="36cb8-176">base64ToJson</span></span>](resource-group-template-functions-string.md#base64tojson)
* [<span data-ttu-id="36cb8-177">base64ToString</span><span class="sxs-lookup"><span data-stu-id="36cb8-177">base64ToString</span></span>](resource-group-template-functions-string.md#base64tostring)
* [<span data-ttu-id="36cb8-178">concat</span><span class="sxs-lookup"><span data-stu-id="36cb8-178">concat</span></span>](resource-group-template-functions-string.md#concat)
* [<span data-ttu-id="36cb8-179">innehåller</span><span class="sxs-lookup"><span data-stu-id="36cb8-179">contains</span></span>](resource-group-template-functions-string.md#contains)
* [<span data-ttu-id="36cb8-180">dataUri</span><span class="sxs-lookup"><span data-stu-id="36cb8-180">dataUri</span></span>](resource-group-template-functions-string.md#datauri)
* [<span data-ttu-id="36cb8-181">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="36cb8-181">dataUriToString</span></span>](resource-group-template-functions-string.md#datauritostring)
* [<span data-ttu-id="36cb8-182">tom</span><span class="sxs-lookup"><span data-stu-id="36cb8-182">empty</span></span>](resource-group-template-functions-string.md#empty)
* [<span data-ttu-id="36cb8-183">endsWith</span><span class="sxs-lookup"><span data-stu-id="36cb8-183">endsWith</span></span>](resource-group-template-functions-string.md#endswith)
* [<span data-ttu-id="36cb8-184">första</span><span class="sxs-lookup"><span data-stu-id="36cb8-184">first</span></span>](resource-group-template-functions-string.md#first)
* [<span data-ttu-id="36cb8-185">indexOf</span><span class="sxs-lookup"><span data-stu-id="36cb8-185">indexOf</span></span>](resource-group-template-functions-string.md#indexof)
* [<span data-ttu-id="36cb8-186">senaste</span><span class="sxs-lookup"><span data-stu-id="36cb8-186">last</span></span>](resource-group-template-functions-string.md#last)
* [<span data-ttu-id="36cb8-187">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="36cb8-187">lastIndexOf</span></span>](resource-group-template-functions-string.md#lastindexof)
* [<span data-ttu-id="36cb8-188">längd</span><span class="sxs-lookup"><span data-stu-id="36cb8-188">length</span></span>](resource-group-template-functions-string.md#length)
* [<span data-ttu-id="36cb8-189">padLeft</span><span class="sxs-lookup"><span data-stu-id="36cb8-189">padLeft</span></span>](resource-group-template-functions-string.md#padleft)
* [<span data-ttu-id="36cb8-190">Ersätt</span><span class="sxs-lookup"><span data-stu-id="36cb8-190">replace</span></span>](resource-group-template-functions-string.md#replace)
* [<span data-ttu-id="36cb8-191">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="36cb8-191">skip</span></span>](resource-group-template-functions-string.md#skip)
* [<span data-ttu-id="36cb8-192">split</span><span class="sxs-lookup"><span data-stu-id="36cb8-192">split</span></span>](resource-group-template-functions-string.md#split)
* [<span data-ttu-id="36cb8-193">startsWith</span><span class="sxs-lookup"><span data-stu-id="36cb8-193">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="36cb8-194">sträng</span><span class="sxs-lookup"><span data-stu-id="36cb8-194">string</span></span>](resource-group-template-functions-string.md#string)
* [<span data-ttu-id="36cb8-195">delsträngen</span><span class="sxs-lookup"><span data-stu-id="36cb8-195">substring</span></span>](resource-group-template-functions-string.md#substring)
* [<span data-ttu-id="36cb8-196">ta</span><span class="sxs-lookup"><span data-stu-id="36cb8-196">take</span></span>](resource-group-template-functions-string.md#take)
* [<span data-ttu-id="36cb8-197">toLower</span><span class="sxs-lookup"><span data-stu-id="36cb8-197">toLower</span></span>](resource-group-template-functions-string.md#tolower)
* [<span data-ttu-id="36cb8-198">toUpper</span><span class="sxs-lookup"><span data-stu-id="36cb8-198">toUpper</span></span>](resource-group-template-functions-string.md#toupper)
* [<span data-ttu-id="36cb8-199">trim</span><span class="sxs-lookup"><span data-stu-id="36cb8-199">trim</span></span>](resource-group-template-functions-string.md#trim)
* [<span data-ttu-id="36cb8-200">uniqueString</span><span class="sxs-lookup"><span data-stu-id="36cb8-200">uniqueString</span></span>](resource-group-template-functions-string.md#uniquestring)
* [<span data-ttu-id="36cb8-201">URI: n</span><span class="sxs-lookup"><span data-stu-id="36cb8-201">uri</span></span>](resource-group-template-functions-string.md#uri)
* [<span data-ttu-id="36cb8-202">uriComponent</span><span class="sxs-lookup"><span data-stu-id="36cb8-202">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="36cb8-203">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="36cb8-203">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)


## <a name="next-steps"></a><span data-ttu-id="36cb8-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36cb8-204">Next steps</span></span>
* <span data-ttu-id="36cb8-205">En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="36cb8-205">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="36cb8-206">toomerge flera mallar finns [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md)</span><span class="sxs-lookup"><span data-stu-id="36cb8-206">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md)</span></span>
* <span data-ttu-id="36cb8-207">tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="36cb8-207">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>
* <span data-ttu-id="36cb8-208">toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="36cb8-208">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md)</span></span>

