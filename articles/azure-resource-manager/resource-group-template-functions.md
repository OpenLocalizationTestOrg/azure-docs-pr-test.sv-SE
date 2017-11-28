---
title: "Hanteraren för filserverresurser Mallfunktioner | Microsoft Docs"
description: "Beskriver funktionerna du använder i en Azure Resource Manager-mall för att hämta värden, arbeta med strängar och siffror, och hämta information om distribution."
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
ms.openlocfilehash: 1324bed07e991e9d84cb6832afe78bdb2d3348fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-resource-manager-template-functions"></a><span data-ttu-id="65303-103">Azure Resource Manager Mallfunktioner</span><span class="sxs-lookup"><span data-stu-id="65303-103">Azure Resource Manager template functions</span></span>
<span data-ttu-id="65303-104">Det här avsnittet beskrivs de funktioner som du kan använda i en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="65303-104">This topic describes all the functions you can use in an Azure Resource Manager template.</span></span>

<span data-ttu-id="65303-105">Du lägger till funktioner i dina mallar genom att skriva dem inom hakparenteser: `[` och `]`respektive.</span><span class="sxs-lookup"><span data-stu-id="65303-105">You add functions in your templates by enclosing them within brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="65303-106">Uttrycket utvärderas under distributionen.</span><span class="sxs-lookup"><span data-stu-id="65303-106">The expression is evaluated during deployment.</span></span> <span data-ttu-id="65303-107">Medan skrivs som en teckensträng kan resultat av utvärderingen av uttrycket vara av en annan JSON-typ, till exempel en matris, objekt eller heltal.</span><span class="sxs-lookup"><span data-stu-id="65303-107">While written as a string literal, the result of evaluating the expression can be of a different JSON type, such as an array, object, or integer.</span></span> <span data-ttu-id="65303-108">Precis som i JavaScript-funktionsanrop som är formaterade som `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="65303-108">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="65303-109">Du kan referera egenskaper genom att använda operatorerna punkt och [index].</span><span class="sxs-lookup"><span data-stu-id="65303-109">You reference properties by using the dot and [index] operators.</span></span>

<span data-ttu-id="65303-110">Ett malluttryck får inte överskrida 24,576 tecken.</span><span class="sxs-lookup"><span data-stu-id="65303-110">A template expression cannot exceed 24,576 characters.</span></span>

<span data-ttu-id="65303-111">Mallfunktioner och deras parametrar är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="65303-111">Template functions and their parameters are case-insensitive.</span></span> <span data-ttu-id="65303-112">Till exempel Resource Manager matchar **variables('var1')** och **VARIABLES('VAR1')** samma.</span><span class="sxs-lookup"><span data-stu-id="65303-112">For example, Resource Manager resolves **variables('var1')** and **VARIABLES('VAR1')** as the same.</span></span> <span data-ttu-id="65303-113">När utvärderas om funktionen ändrar uttryckligen skiftläge (till exempel toUpper eller toLower), funktionen bevarar skiftläge.</span><span class="sxs-lookup"><span data-stu-id="65303-113">When evaluated, unless the function expressly modifies case (such as toUpper or toLower), the function preserves the case.</span></span> <span data-ttu-id="65303-114">Vissa typer av resurser kan ha case krav oavsett hur funktioner utvärderas.</span><span class="sxs-lookup"><span data-stu-id="65303-114">Certain resource types may have case requirements irrespective of how functions are evaluated.</span></span>

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

## <a name="array-and-object-functions"></a><span data-ttu-id="65303-115">Array- och funktioner</span><span class="sxs-lookup"><span data-stu-id="65303-115">Array and object functions</span></span>
<span data-ttu-id="65303-116">Resource Manager innehåller flera funktioner för att arbeta med matriser och -objekt.</span><span class="sxs-lookup"><span data-stu-id="65303-116">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="65303-117">matris</span><span class="sxs-lookup"><span data-stu-id="65303-117">array</span></span>](resource-group-template-functions-array.md#array)
* [<span data-ttu-id="65303-118">Slå samman</span><span class="sxs-lookup"><span data-stu-id="65303-118">coalesce</span></span>](resource-group-template-functions-array.md#coalesce)
* [<span data-ttu-id="65303-119">concat</span><span class="sxs-lookup"><span data-stu-id="65303-119">concat</span></span>](resource-group-template-functions-array.md#concat)
* [<span data-ttu-id="65303-120">innehåller</span><span class="sxs-lookup"><span data-stu-id="65303-120">contains</span></span>](resource-group-template-functions-array.md#contains)
* [<span data-ttu-id="65303-121">createArray</span><span class="sxs-lookup"><span data-stu-id="65303-121">createArray</span></span>](resource-group-template-functions-array.md#createarray)
* [<span data-ttu-id="65303-122">tom</span><span class="sxs-lookup"><span data-stu-id="65303-122">empty</span></span>](resource-group-template-functions-array.md#empty)
* [<span data-ttu-id="65303-123">första</span><span class="sxs-lookup"><span data-stu-id="65303-123">first</span></span>](resource-group-template-functions-array.md#first)
* [<span data-ttu-id="65303-124">skärningspunkten</span><span class="sxs-lookup"><span data-stu-id="65303-124">intersection</span></span>](resource-group-template-functions-array.md#intersection)
* [<span data-ttu-id="65303-125">JSON</span><span class="sxs-lookup"><span data-stu-id="65303-125">json</span></span>](resource-group-template-functions-array.md#json)
* [<span data-ttu-id="65303-126">senaste</span><span class="sxs-lookup"><span data-stu-id="65303-126">last</span></span>](resource-group-template-functions-array.md#last)
* [<span data-ttu-id="65303-127">längd</span><span class="sxs-lookup"><span data-stu-id="65303-127">length</span></span>](resource-group-template-functions-array.md#length)
* [<span data-ttu-id="65303-128">Min</span><span class="sxs-lookup"><span data-stu-id="65303-128">min</span></span>](resource-group-template-functions-array.md#min)
* [<span data-ttu-id="65303-129">Max</span><span class="sxs-lookup"><span data-stu-id="65303-129">max</span></span>](resource-group-template-functions-array.md#max)
* [<span data-ttu-id="65303-130">intervallet</span><span class="sxs-lookup"><span data-stu-id="65303-130">range</span></span>](resource-group-template-functions-array.md#range)
* [<span data-ttu-id="65303-131">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="65303-131">skip</span></span>](resource-group-template-functions-array.md#skip)
* [<span data-ttu-id="65303-132">ta</span><span class="sxs-lookup"><span data-stu-id="65303-132">take</span></span>](resource-group-template-functions-array.md#take)
* [<span data-ttu-id="65303-133">Union</span><span class="sxs-lookup"><span data-stu-id="65303-133">union</span></span>](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a><span data-ttu-id="65303-134">Jämförelse funktioner</span><span class="sxs-lookup"><span data-stu-id="65303-134">Comparison functions</span></span>
<span data-ttu-id="65303-135">Resource Manager innehåller flera funktioner för att göra jämförelser i dina mallar.</span><span class="sxs-lookup"><span data-stu-id="65303-135">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="65303-136">är lika med</span><span class="sxs-lookup"><span data-stu-id="65303-136">equals</span></span>](resource-group-template-functions-comparison.md#equals)
* [<span data-ttu-id="65303-137">mindre</span><span class="sxs-lookup"><span data-stu-id="65303-137">less</span></span>](resource-group-template-functions-comparison.md#less)
* [<span data-ttu-id="65303-138">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="65303-138">lessOrEquals</span></span>](resource-group-template-functions-comparison.md#lessorequals)
* [<span data-ttu-id="65303-139">större</span><span class="sxs-lookup"><span data-stu-id="65303-139">greater</span></span>](resource-group-template-functions-comparison.md#greater)
* [<span data-ttu-id="65303-140">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="65303-140">greaterOrEquals</span></span>](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a><span data-ttu-id="65303-141">Funktioner för distribution av värdet</span><span class="sxs-lookup"><span data-stu-id="65303-141">Deployment value functions</span></span>
<span data-ttu-id="65303-142">Hanteraren för filserverresurser innehåller följande funktioner för att hämta värden från avsnitt i mallen och värden som rör distributionen:</span><span class="sxs-lookup"><span data-stu-id="65303-142">Resource Manager provides the following functions for getting values from sections of the template and values related to the deployment:</span></span>

* [<span data-ttu-id="65303-143">distribution</span><span class="sxs-lookup"><span data-stu-id="65303-143">deployment</span></span>](resource-group-template-functions-deployment.md#deployment)
* [<span data-ttu-id="65303-144">parametrar</span><span class="sxs-lookup"><span data-stu-id="65303-144">parameters</span></span>](resource-group-template-functions-deployment.md#parameters)
* [<span data-ttu-id="65303-145">variabler</span><span class="sxs-lookup"><span data-stu-id="65303-145">variables</span></span>](resource-group-template-functions-deployment.md#variables)

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

## <a name="logical-functions"></a><span data-ttu-id="65303-146">Logiska funktioner</span><span class="sxs-lookup"><span data-stu-id="65303-146">Logical functions</span></span>
<span data-ttu-id="65303-147">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med logiska villkor:</span><span class="sxs-lookup"><span data-stu-id="65303-147">Resource Manager provides the following functions for working with logical conditions:</span></span>

* [<span data-ttu-id="65303-148">och</span><span class="sxs-lookup"><span data-stu-id="65303-148">and</span></span>](resource-group-template-functions-logical.md#and)
* [<span data-ttu-id="65303-149">bool</span><span class="sxs-lookup"><span data-stu-id="65303-149">bool</span></span>](resource-group-template-functions-logical.md#bool)
* [<span data-ttu-id="65303-150">Om</span><span class="sxs-lookup"><span data-stu-id="65303-150">if</span></span>](resource-group-template-functions-logical.md#if)
* [<span data-ttu-id="65303-151">inte</span><span class="sxs-lookup"><span data-stu-id="65303-151">not</span></span>](resource-group-template-functions-logical.md#not)
* [<span data-ttu-id="65303-152">eller</span><span class="sxs-lookup"><span data-stu-id="65303-152">or</span></span>](resource-group-template-functions-logical.md#or)

## <a name="numeric-functions"></a><span data-ttu-id="65303-153">Numeriska funktioner</span><span class="sxs-lookup"><span data-stu-id="65303-153">Numeric functions</span></span>
<span data-ttu-id="65303-154">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med heltal:</span><span class="sxs-lookup"><span data-stu-id="65303-154">Resource Manager provides the following functions for working with integers:</span></span>

* [<span data-ttu-id="65303-155">Lägg till</span><span class="sxs-lookup"><span data-stu-id="65303-155">add</span></span>](resource-group-template-functions-numeric.md#add)
* [<span data-ttu-id="65303-156">copyIndex</span><span class="sxs-lookup"><span data-stu-id="65303-156">copyIndex</span></span>](resource-group-template-functions-numeric.md#copyindex)
* [<span data-ttu-id="65303-157">div</span><span class="sxs-lookup"><span data-stu-id="65303-157">div</span></span>](resource-group-template-functions-numeric.md#div)
* [<span data-ttu-id="65303-158">flyttal</span><span class="sxs-lookup"><span data-stu-id="65303-158">float</span></span>](resource-group-template-functions-numeric.md#float)
* [<span data-ttu-id="65303-159">int</span><span class="sxs-lookup"><span data-stu-id="65303-159">int</span></span>](resource-group-template-functions-numeric.md#int)
* [<span data-ttu-id="65303-160">Min</span><span class="sxs-lookup"><span data-stu-id="65303-160">min</span></span>](resource-group-template-functions-numeric.md#min)
* [<span data-ttu-id="65303-161">Max</span><span class="sxs-lookup"><span data-stu-id="65303-161">max</span></span>](resource-group-template-functions-numeric.md#max)
* [<span data-ttu-id="65303-162">MOD</span><span class="sxs-lookup"><span data-stu-id="65303-162">mod</span></span>](resource-group-template-functions-numeric.md#mod)
* [<span data-ttu-id="65303-163">mul</span><span class="sxs-lookup"><span data-stu-id="65303-163">mul</span></span>](resource-group-template-functions-numeric.md#mul)
* [<span data-ttu-id="65303-164">Sub</span><span class="sxs-lookup"><span data-stu-id="65303-164">sub</span></span>](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a><span data-ttu-id="65303-165">Resursfunktioner</span><span class="sxs-lookup"><span data-stu-id="65303-165">Resource functions</span></span>
<span data-ttu-id="65303-166">Hanteraren för filserverresurser innehåller följande funktioner för att hämta resurs värden:</span><span class="sxs-lookup"><span data-stu-id="65303-166">Resource Manager provides the following functions for getting resource values:</span></span>

* [<span data-ttu-id="65303-167">listKeys och lista {Value}</span><span class="sxs-lookup"><span data-stu-id="65303-167">listKeys and list{Value}</span></span>](resource-group-template-functions-resource.md#listkeys)
* [<span data-ttu-id="65303-168">providers</span><span class="sxs-lookup"><span data-stu-id="65303-168">providers</span></span>](resource-group-template-functions-resource.md#providers)
* [<span data-ttu-id="65303-169">referens</span><span class="sxs-lookup"><span data-stu-id="65303-169">reference</span></span>](resource-group-template-functions-resource.md#reference)
* [<span data-ttu-id="65303-170">resourceGroup</span><span class="sxs-lookup"><span data-stu-id="65303-170">resourceGroup</span></span>](resource-group-template-functions-resource.md#resourcegroup)
* [<span data-ttu-id="65303-171">resurs-ID</span><span class="sxs-lookup"><span data-stu-id="65303-171">resourceId</span></span>](resource-group-template-functions-resource.md#resourceid)
* [<span data-ttu-id="65303-172">prenumeration</span><span class="sxs-lookup"><span data-stu-id="65303-172">subscription</span></span>](resource-group-template-functions-resource.md#subscription)

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

## <a name="string-functions"></a><span data-ttu-id="65303-173">Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="65303-173">String functions</span></span>
<span data-ttu-id="65303-174">Hanteraren för filserverresurser innehåller följande funktioner för att arbeta med strängar:</span><span class="sxs-lookup"><span data-stu-id="65303-174">Resource Manager provides the following functions for working with strings:</span></span>

* [<span data-ttu-id="65303-175">Base64</span><span class="sxs-lookup"><span data-stu-id="65303-175">base64</span></span>](resource-group-template-functions-string.md#base64)
* [<span data-ttu-id="65303-176">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="65303-176">base64ToJson</span></span>](resource-group-template-functions-string.md#base64tojson)
* [<span data-ttu-id="65303-177">base64ToString</span><span class="sxs-lookup"><span data-stu-id="65303-177">base64ToString</span></span>](resource-group-template-functions-string.md#base64tostring)
* [<span data-ttu-id="65303-178">concat</span><span class="sxs-lookup"><span data-stu-id="65303-178">concat</span></span>](resource-group-template-functions-string.md#concat)
* [<span data-ttu-id="65303-179">innehåller</span><span class="sxs-lookup"><span data-stu-id="65303-179">contains</span></span>](resource-group-template-functions-string.md#contains)
* [<span data-ttu-id="65303-180">dataUri</span><span class="sxs-lookup"><span data-stu-id="65303-180">dataUri</span></span>](resource-group-template-functions-string.md#datauri)
* [<span data-ttu-id="65303-181">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="65303-181">dataUriToString</span></span>](resource-group-template-functions-string.md#datauritostring)
* [<span data-ttu-id="65303-182">tom</span><span class="sxs-lookup"><span data-stu-id="65303-182">empty</span></span>](resource-group-template-functions-string.md#empty)
* [<span data-ttu-id="65303-183">endsWith</span><span class="sxs-lookup"><span data-stu-id="65303-183">endsWith</span></span>](resource-group-template-functions-string.md#endswith)
* [<span data-ttu-id="65303-184">första</span><span class="sxs-lookup"><span data-stu-id="65303-184">first</span></span>](resource-group-template-functions-string.md#first)
* [<span data-ttu-id="65303-185">indexOf</span><span class="sxs-lookup"><span data-stu-id="65303-185">indexOf</span></span>](resource-group-template-functions-string.md#indexof)
* [<span data-ttu-id="65303-186">senaste</span><span class="sxs-lookup"><span data-stu-id="65303-186">last</span></span>](resource-group-template-functions-string.md#last)
* [<span data-ttu-id="65303-187">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="65303-187">lastIndexOf</span></span>](resource-group-template-functions-string.md#lastindexof)
* [<span data-ttu-id="65303-188">längd</span><span class="sxs-lookup"><span data-stu-id="65303-188">length</span></span>](resource-group-template-functions-string.md#length)
* [<span data-ttu-id="65303-189">padLeft</span><span class="sxs-lookup"><span data-stu-id="65303-189">padLeft</span></span>](resource-group-template-functions-string.md#padleft)
* [<span data-ttu-id="65303-190">Ersätt</span><span class="sxs-lookup"><span data-stu-id="65303-190">replace</span></span>](resource-group-template-functions-string.md#replace)
* [<span data-ttu-id="65303-191">Hoppa över</span><span class="sxs-lookup"><span data-stu-id="65303-191">skip</span></span>](resource-group-template-functions-string.md#skip)
* [<span data-ttu-id="65303-192">split</span><span class="sxs-lookup"><span data-stu-id="65303-192">split</span></span>](resource-group-template-functions-string.md#split)
* [<span data-ttu-id="65303-193">startsWith</span><span class="sxs-lookup"><span data-stu-id="65303-193">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="65303-194">sträng</span><span class="sxs-lookup"><span data-stu-id="65303-194">string</span></span>](resource-group-template-functions-string.md#string)
* [<span data-ttu-id="65303-195">delsträngen</span><span class="sxs-lookup"><span data-stu-id="65303-195">substring</span></span>](resource-group-template-functions-string.md#substring)
* [<span data-ttu-id="65303-196">ta</span><span class="sxs-lookup"><span data-stu-id="65303-196">take</span></span>](resource-group-template-functions-string.md#take)
* [<span data-ttu-id="65303-197">toLower</span><span class="sxs-lookup"><span data-stu-id="65303-197">toLower</span></span>](resource-group-template-functions-string.md#tolower)
* [<span data-ttu-id="65303-198">toUpper</span><span class="sxs-lookup"><span data-stu-id="65303-198">toUpper</span></span>](resource-group-template-functions-string.md#toupper)
* [<span data-ttu-id="65303-199">trim</span><span class="sxs-lookup"><span data-stu-id="65303-199">trim</span></span>](resource-group-template-functions-string.md#trim)
* [<span data-ttu-id="65303-200">uniqueString</span><span class="sxs-lookup"><span data-stu-id="65303-200">uniqueString</span></span>](resource-group-template-functions-string.md#uniquestring)
* [<span data-ttu-id="65303-201">URI: n</span><span class="sxs-lookup"><span data-stu-id="65303-201">uri</span></span>](resource-group-template-functions-string.md#uri)
* [<span data-ttu-id="65303-202">uriComponent</span><span class="sxs-lookup"><span data-stu-id="65303-202">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="65303-203">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="65303-203">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)


## <a name="next-steps"></a><span data-ttu-id="65303-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="65303-204">Next steps</span></span>
* <span data-ttu-id="65303-205">En beskrivning av avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="65303-205">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="65303-206">Om du vill slå samman flera mallar finns [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md)</span><span class="sxs-lookup"><span data-stu-id="65303-206">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md)</span></span>
* <span data-ttu-id="65303-207">Iterera ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="65303-207">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>
* <span data-ttu-id="65303-208">Information om hur du distribuerar mallen som du har skapat finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="65303-208">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md)</span></span>

