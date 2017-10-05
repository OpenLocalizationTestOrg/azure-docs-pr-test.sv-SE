---
title: 'Azure AD Connect-synkronisering: funktioner referens | Microsoft Docs'
description: "Referens för uttryck för deklarativ etablering i Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 926f52ef64eb79205dbfb344edc7d9bece2a6947
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a><span data-ttu-id="eb11a-103">Azure AD Connect-synkronisering: funktioner referens</span><span class="sxs-lookup"><span data-stu-id="eb11a-103">Azure AD Connect sync: Functions Reference</span></span>
<span data-ttu-id="eb11a-104">I Azure AD Connect för funktioner att ändra ett attributvärde under synkroniseringen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-104">In Azure AD Connect, functions are used to manipulate an attribute value during synchronization.</span></span>  
<span data-ttu-id="eb11a-105">Syntaxen för funktionerna som uttrycks i följande format:</span><span class="sxs-lookup"><span data-stu-id="eb11a-105">The Syntax of the functions is expressed using the following format:</span></span>  
`<output type> FunctionName(<input type> <position name>, ..)`

<span data-ttu-id="eb11a-106">Om en funktion är överbelastad och accepterar flera syntax, visas alla giltig syntax.</span><span class="sxs-lookup"><span data-stu-id="eb11a-106">If a function is overloaded and accepts multiple syntaxes, all valid syntaxes are listed.</span></span>  
<span data-ttu-id="eb11a-107">Funktionerna är starkt typbestämd och de bekräftar att typ som skickades matchar den dokumenterade typen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-107">The functions are strongly typed and they verify that the type passed in matches the documented type.</span></span>  
<span data-ttu-id="eb11a-108">Ett fel genereras om typen inte matchar.</span><span class="sxs-lookup"><span data-stu-id="eb11a-108">If the type does not match, an error is thrown.</span></span>

<span data-ttu-id="eb11a-109">Typerna uttrycks med följande syntax:</span><span class="sxs-lookup"><span data-stu-id="eb11a-109">The types are expressed with the following syntax:</span></span>

* <span data-ttu-id="eb11a-110">**bin** – binära</span><span class="sxs-lookup"><span data-stu-id="eb11a-110">**bin** – Binary</span></span>
* <span data-ttu-id="eb11a-111">**bool** – booleskt</span><span class="sxs-lookup"><span data-stu-id="eb11a-111">**bool** – Boolean</span></span>
* <span data-ttu-id="eb11a-112">**DT** – UTC-datum/tid</span><span class="sxs-lookup"><span data-stu-id="eb11a-112">**dt** – UTC Date/Time</span></span>
* <span data-ttu-id="eb11a-113">**uppräkningen** – uppräkning av kända konstanter</span><span class="sxs-lookup"><span data-stu-id="eb11a-113">**enum** – Enumeration of known constants</span></span>
* <span data-ttu-id="eb11a-114">**EXP** – uttryck som utvärderas till ett booleskt värde förväntades</span><span class="sxs-lookup"><span data-stu-id="eb11a-114">**exp** – Expression, which is expected to evaluate to a Boolean</span></span>
* <span data-ttu-id="eb11a-115">**mvbin** – multivärdes binära</span><span class="sxs-lookup"><span data-stu-id="eb11a-115">**mvbin** – Multi-Valued Binary</span></span>
* <span data-ttu-id="eb11a-116">**mvstr** – multivärdes sträng</span><span class="sxs-lookup"><span data-stu-id="eb11a-116">**mvstr** – Multi-Valued String</span></span>
* <span data-ttu-id="eb11a-117">**mvref** – multivärdes-referens</span><span class="sxs-lookup"><span data-stu-id="eb11a-117">**mvref** – Multi-Valued Reference</span></span>
* <span data-ttu-id="eb11a-118">**NUM** – numeriskt</span><span class="sxs-lookup"><span data-stu-id="eb11a-118">**num** – Numeric</span></span>
* <span data-ttu-id="eb11a-119">**REF** – referens</span><span class="sxs-lookup"><span data-stu-id="eb11a-119">**ref** – Reference</span></span>
* <span data-ttu-id="eb11a-120">**str** – sträng</span><span class="sxs-lookup"><span data-stu-id="eb11a-120">**str** – String</span></span>
* <span data-ttu-id="eb11a-121">**var** – en variant av (nästan) en annan typ</span><span class="sxs-lookup"><span data-stu-id="eb11a-121">**var** – A variant of (almost) any other type</span></span>
* <span data-ttu-id="eb11a-122">**void** – returnerar inte ett värde</span><span class="sxs-lookup"><span data-stu-id="eb11a-122">**void** – doesn’t return a value</span></span>

<span data-ttu-id="eb11a-123">Funktioner med typer **mvbin**, **mvstr**, och **mvref** fungerar bara på flera värden attribut.</span><span class="sxs-lookup"><span data-stu-id="eb11a-123">The functions with the types **mvbin**, **mvstr**, and **mvref** can only work on multi-valued attributes.</span></span> <span data-ttu-id="eb11a-124">Fungerar med **bin**, **str**, och **ref** fungerar på både enstaka och flera värden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-124">Functions with **bin**, **str**, and **ref** work on both single-valued and multi-valued attributes.</span></span>

## <a name="functions-reference"></a><span data-ttu-id="eb11a-125">Referens för funktioner</span><span class="sxs-lookup"><span data-stu-id="eb11a-125">Functions Reference</span></span>
| <span data-ttu-id="eb11a-126">Lista över funktioner</span><span class="sxs-lookup"><span data-stu-id="eb11a-126">List of functions</span></span> |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="eb11a-127">**Certifikat**</span><span class="sxs-lookup"><span data-stu-id="eb11a-127">**Certificate**</span></span> | | | | |
| [<span data-ttu-id="eb11a-128">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="eb11a-128">CertExtensionOids</span></span>](#certextensionoids) |[<span data-ttu-id="eb11a-129">CertFormat</span><span class="sxs-lookup"><span data-stu-id="eb11a-129">CertFormat</span></span>](#certformat) |[<span data-ttu-id="eb11a-130">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="eb11a-130">CertFriendlyName</span></span>](#certfriendlyname) |[<span data-ttu-id="eb11a-131">CertHashString</span><span class="sxs-lookup"><span data-stu-id="eb11a-131">CertHashString</span></span>](#certhashstring) | |
| [<span data-ttu-id="eb11a-132">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="eb11a-132">CertIssuer</span></span>](#certissuer) |[<span data-ttu-id="eb11a-133">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="eb11a-133">CertIssuerDN</span></span>](#certissuerdn) |[<span data-ttu-id="eb11a-134">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-134">CertIssuerOid</span></span>](#certissueroid) |[<span data-ttu-id="eb11a-135">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="eb11a-135">CertKeyAlgorithm</span></span>](#certkeyalgorithm) | |
| [<span data-ttu-id="eb11a-136">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="eb11a-136">CertKeyAlgorithmParams</span></span>](#certkeyalgorithmparams) |[<span data-ttu-id="eb11a-137">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="eb11a-137">CertNameInfo</span></span>](#certnameinfo) |[<span data-ttu-id="eb11a-138">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="eb11a-138">CertNotAfter</span></span>](#certnotafter) |[<span data-ttu-id="eb11a-139">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="eb11a-139">CertNotBefore</span></span>](#certnotbefore) | |
| [<span data-ttu-id="eb11a-140">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-140">CertPublicKeyOid</span></span>](#certpublickeyoid) |[<span data-ttu-id="eb11a-141">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-141">CertPublicKeyParametersOid</span></span>](#certpublickeyparametersoid) |[<span data-ttu-id="eb11a-142">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="eb11a-142">CertSerialNumber</span></span>](#certserialnumber) |[<span data-ttu-id="eb11a-143">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-143">CertSignatureAlgorithmOid</span></span>](#certsignaturealgorithmoid) | |
| [<span data-ttu-id="eb11a-144">CertSubject</span><span class="sxs-lookup"><span data-stu-id="eb11a-144">CertSubject</span></span>](#certsubject) |[<span data-ttu-id="eb11a-145">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="eb11a-145">CertSubjectNameDN</span></span>](#certsubjectnamedn) |[<span data-ttu-id="eb11a-146">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-146">CertSubjectNameOid</span></span>](#certsubjectnameoid) |[<span data-ttu-id="eb11a-147">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="eb11a-147">CertThumbprint</span></span>](#certthumbprint) | |
[<span data-ttu-id="eb11a-148">CertVersion</span><span class="sxs-lookup"><span data-stu-id="eb11a-148"> CertVersion</span></span>](#certversion) |[<span data-ttu-id="eb11a-149">IsCert</span><span class="sxs-lookup"><span data-stu-id="eb11a-149">IsCert</span></span>](#iscert) | | | |
| <span data-ttu-id="eb11a-150">**Konvertering**</span><span class="sxs-lookup"><span data-stu-id="eb11a-150">**Conversion**</span></span> | | | | |
| [<span data-ttu-id="eb11a-151">CBool</span><span class="sxs-lookup"><span data-stu-id="eb11a-151">CBool</span></span>](#cbool) |[<span data-ttu-id="eb11a-152">CDate</span><span class="sxs-lookup"><span data-stu-id="eb11a-152">CDate</span></span>](#cdate) |[<span data-ttu-id="eb11a-153">CGuid</span><span class="sxs-lookup"><span data-stu-id="eb11a-153">CGuid</span></span>](#cguid) |[<span data-ttu-id="eb11a-154">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="eb11a-154">ConvertFromBase64</span></span>](#convertfrombase64) | |
| [<span data-ttu-id="eb11a-155">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="eb11a-155">ConvertToBase64</span></span>](#converttobase64) |[<span data-ttu-id="eb11a-156">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="eb11a-156">ConvertFromUTF8Hex</span></span>](#convertfromutf8hex) |[<span data-ttu-id="eb11a-157">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="eb11a-157">ConvertToUTF8Hex</span></span>](#converttoutf8hex) |[<span data-ttu-id="eb11a-158">CNum</span><span class="sxs-lookup"><span data-stu-id="eb11a-158">CNum</span></span>](#cnum) | |
| [<span data-ttu-id="eb11a-159">CRef</span><span class="sxs-lookup"><span data-stu-id="eb11a-159">CRef</span></span>](#cref) |[<span data-ttu-id="eb11a-160">CStr</span><span class="sxs-lookup"><span data-stu-id="eb11a-160">CStr</span></span>](#cstr) |[<span data-ttu-id="eb11a-161">GUIDFromString</span><span class="sxs-lookup"><span data-stu-id="eb11a-161">StringFromGuid</span></span>](#StringFromGuid) |[<span data-ttu-id="eb11a-162">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="eb11a-162">StringFromSid</span></span>](#stringfromsid) | |
| <span data-ttu-id="eb11a-163">**Datum / tid**</span><span class="sxs-lookup"><span data-stu-id="eb11a-163">**Date / Time**</span></span> | | | | |
| [<span data-ttu-id="eb11a-164">DateAdd</span><span class="sxs-lookup"><span data-stu-id="eb11a-164">DateAdd</span></span>](#dateadd) |[<span data-ttu-id="eb11a-165">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="eb11a-165">DateFromNum</span></span>](#datefromnum) |[<span data-ttu-id="eb11a-166">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="eb11a-166">FormatDateTime</span></span>](#formatdatetime) |[<span data-ttu-id="eb11a-167">Nu</span><span class="sxs-lookup"><span data-stu-id="eb11a-167">Now</span></span>](#now) | |
| [<span data-ttu-id="eb11a-168">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="eb11a-168">NumFromDate</span></span>](#numfromdate) | | | | |
| <span data-ttu-id="eb11a-169">**Directory**</span><span class="sxs-lookup"><span data-stu-id="eb11a-169">**Directory**</span></span> | | | | |
| [<span data-ttu-id="eb11a-170">DNComponent</span><span class="sxs-lookup"><span data-stu-id="eb11a-170">DNComponent</span></span>](#dncomponent) |[<span data-ttu-id="eb11a-171">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="eb11a-171">DNComponentRev</span></span>](#dncomponentrev) |[<span data-ttu-id="eb11a-172">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="eb11a-172">EscapeDNComponent</span></span>](#escapedncomponent) | | |
| <span data-ttu-id="eb11a-173">**Utvärdering**</span><span class="sxs-lookup"><span data-stu-id="eb11a-173">**Evaluation**</span></span> | | | | |
| [<span data-ttu-id="eb11a-174">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="eb11a-174">IsBitSet</span></span>](#isbitset) |[<span data-ttu-id="eb11a-175">IsDate</span><span class="sxs-lookup"><span data-stu-id="eb11a-175">IsDate</span></span>](#isdate) |[<span data-ttu-id="eb11a-176">IsEmpty</span><span class="sxs-lookup"><span data-stu-id="eb11a-176">IsEmpty</span></span>](#isempty) |[<span data-ttu-id="eb11a-177">IsGuid</span><span class="sxs-lookup"><span data-stu-id="eb11a-177">IsGuid</span></span>](#isguid) | |
| [<span data-ttu-id="eb11a-178">IsNull</span><span class="sxs-lookup"><span data-stu-id="eb11a-178">IsNull</span></span>](#isnull) |[<span data-ttu-id="eb11a-179">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="eb11a-179">IsNullOrEmpty</span></span>](#isnullorempty) |[<span data-ttu-id="eb11a-180">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="eb11a-180">IsNumeric</span></span>](#isnumeric) |[<span data-ttu-id="eb11a-181">IsPresent</span><span class="sxs-lookup"><span data-stu-id="eb11a-181">IsPresent</span></span>](#ispresent) | |
| [<span data-ttu-id="eb11a-182">IsString</span><span class="sxs-lookup"><span data-stu-id="eb11a-182">IsString</span></span>](#isstring) | | | | |
| <span data-ttu-id="eb11a-183">**Math**</span><span class="sxs-lookup"><span data-stu-id="eb11a-183">**Math**</span></span> | | | | |
| [<span data-ttu-id="eb11a-184">BitAnd</span><span class="sxs-lookup"><span data-stu-id="eb11a-184">BitAnd</span></span>](#bitand) |[<span data-ttu-id="eb11a-185">BitOr</span><span class="sxs-lookup"><span data-stu-id="eb11a-185">BitOr</span></span>](#bitor) |[<span data-ttu-id="eb11a-186">RandomNum</span><span class="sxs-lookup"><span data-stu-id="eb11a-186">RandomNum</span></span>](#randomnum) | | |
| <span data-ttu-id="eb11a-187">**Flera värden**</span><span class="sxs-lookup"><span data-stu-id="eb11a-187">**Multi-valued**</span></span> | | | | |
| [<span data-ttu-id="eb11a-188">Innehåller</span><span class="sxs-lookup"><span data-stu-id="eb11a-188">Contains</span></span>](#contains) |[<span data-ttu-id="eb11a-189">Antal</span><span class="sxs-lookup"><span data-stu-id="eb11a-189">Count</span></span>](#count) |[<span data-ttu-id="eb11a-190">Objektet</span><span class="sxs-lookup"><span data-stu-id="eb11a-190">Item</span></span>](#item) |[<span data-ttu-id="eb11a-191">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="eb11a-191">ItemOrNull</span></span>](#itemornull) | |
| [<span data-ttu-id="eb11a-192">Anslut dig</span><span class="sxs-lookup"><span data-stu-id="eb11a-192">Join</span></span>](#join) |[<span data-ttu-id="eb11a-193">RemoveDuplicates</span><span class="sxs-lookup"><span data-stu-id="eb11a-193">RemoveDuplicates</span></span>](#removeduplicates) |[<span data-ttu-id="eb11a-194">Dela</span><span class="sxs-lookup"><span data-stu-id="eb11a-194">Split</span></span>](#split) | | |
| <span data-ttu-id="eb11a-195">**Programflöde**</span><span class="sxs-lookup"><span data-stu-id="eb11a-195">**Program Flow**</span></span> | | | | |
| [<span data-ttu-id="eb11a-196">Fel</span><span class="sxs-lookup"><span data-stu-id="eb11a-196">Error</span></span>](#error) |[<span data-ttu-id="eb11a-197">IIF</span><span class="sxs-lookup"><span data-stu-id="eb11a-197">IIF</span></span>](#iif) |[<span data-ttu-id="eb11a-198">Välj</span><span class="sxs-lookup"><span data-stu-id="eb11a-198">Select</span></span>](#select) |[<span data-ttu-id="eb11a-199">Växel</span><span class="sxs-lookup"><span data-stu-id="eb11a-199">Switch</span></span>](#switch) | |
| [<span data-ttu-id="eb11a-200">Där</span><span class="sxs-lookup"><span data-stu-id="eb11a-200">Where</span></span>](#where) |[<span data-ttu-id="eb11a-201">Med</span><span class="sxs-lookup"><span data-stu-id="eb11a-201">With</span></span>](#with) | | | |
| <span data-ttu-id="eb11a-202">**Text**</span><span class="sxs-lookup"><span data-stu-id="eb11a-202">**Text**</span></span> | | | | |
| [<span data-ttu-id="eb11a-203">GUID</span><span class="sxs-lookup"><span data-stu-id="eb11a-203">GUID</span></span>](#guid) |[<span data-ttu-id="eb11a-204">InStr</span><span class="sxs-lookup"><span data-stu-id="eb11a-204">InStr</span></span>](#instr) |[<span data-ttu-id="eb11a-205">InStrRev</span><span class="sxs-lookup"><span data-stu-id="eb11a-205">InStrRev</span></span>](#instrrev) |[<span data-ttu-id="eb11a-206">LCase</span><span class="sxs-lookup"><span data-stu-id="eb11a-206">LCase</span></span>](#lcase) | |
| [<span data-ttu-id="eb11a-207">Vänster</span><span class="sxs-lookup"><span data-stu-id="eb11a-207">Left</span></span>](#left) |[<span data-ttu-id="eb11a-208">Len</span><span class="sxs-lookup"><span data-stu-id="eb11a-208">Len</span></span>](#len) |[<span data-ttu-id="eb11a-209">LTrim</span><span class="sxs-lookup"><span data-stu-id="eb11a-209">LTrim</span></span>](#ltrim) |[<span data-ttu-id="eb11a-210">Mid</span><span class="sxs-lookup"><span data-stu-id="eb11a-210">Mid</span></span>](#mid) | |
| [<span data-ttu-id="eb11a-211">PadLeft</span><span class="sxs-lookup"><span data-stu-id="eb11a-211">PadLeft</span></span>](#padleft) |[<span data-ttu-id="eb11a-212">PadRight</span><span class="sxs-lookup"><span data-stu-id="eb11a-212">PadRight</span></span>](#padright) |[<span data-ttu-id="eb11a-213">PCase</span><span class="sxs-lookup"><span data-stu-id="eb11a-213">PCase</span></span>](#pcase) |[<span data-ttu-id="eb11a-214">Ersätt</span><span class="sxs-lookup"><span data-stu-id="eb11a-214">Replace</span></span>](#replace) | |
| [<span data-ttu-id="eb11a-215">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="eb11a-215">ReplaceChars</span></span>](#replacechars) |[<span data-ttu-id="eb11a-216">Höger</span><span class="sxs-lookup"><span data-stu-id="eb11a-216">Right</span></span>](#right) |[<span data-ttu-id="eb11a-217">RTrim</span><span class="sxs-lookup"><span data-stu-id="eb11a-217">RTrim</span></span>](#rtrim) |[<span data-ttu-id="eb11a-218">Rensa</span><span class="sxs-lookup"><span data-stu-id="eb11a-218">Trim</span></span>](#trim) | |
| [<span data-ttu-id="eb11a-219">UCase</span><span class="sxs-lookup"><span data-stu-id="eb11a-219">UCase</span></span>](#ucase) |[<span data-ttu-id="eb11a-220">Word</span><span class="sxs-lookup"><span data-stu-id="eb11a-220">Word</span></span>](#word) | | | |

- - -
### <a name="bitand"></a><span data-ttu-id="eb11a-221">BitAnd</span><span class="sxs-lookup"><span data-stu-id="eb11a-221">BitAnd</span></span>
<span data-ttu-id="eb11a-222">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-222">**Description:**</span></span>  
<span data-ttu-id="eb11a-223">Funktionen BitAnd anger angivna bits på ett värde.</span><span class="sxs-lookup"><span data-stu-id="eb11a-223">The BitAnd function sets specified bits on a value.</span></span>

<span data-ttu-id="eb11a-224">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-224">**Syntax:**</span></span>  
`num BitAnd(num value1, num value2)`

* <span data-ttu-id="eb11a-225">value1, value2: numeriska värden som ska vara AND'ed tillsammans</span><span class="sxs-lookup"><span data-stu-id="eb11a-225">value1, value2: numeric values that should be AND’ed together</span></span>

<span data-ttu-id="eb11a-226">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-226">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-227">Den här funktionen konverteras båda parametrarna till binär representation och anger lite till:</span><span class="sxs-lookup"><span data-stu-id="eb11a-227">This function converts both parameters to the binary representation and sets a bit to:</span></span>

* <span data-ttu-id="eb11a-228">0 – om en eller båda av motsvarande bitar i *mask* och *flaggan* är 0</span><span class="sxs-lookup"><span data-stu-id="eb11a-228">0 - if one or both of the corresponding bits in *mask* and *flag* are 0</span></span>
* <span data-ttu-id="eb11a-229">1 – om båda motsvarande bitar är 1.</span><span class="sxs-lookup"><span data-stu-id="eb11a-229">1 - if both of the corresponding bits are 1.</span></span>

<span data-ttu-id="eb11a-230">Med andra ord returneras 0, utom när de motsvarande bitarna i båda parametrarna är 1.</span><span class="sxs-lookup"><span data-stu-id="eb11a-230">In other words, it returns 0 in all cases except when the corresponding bits of both parameters are 1.</span></span>

<span data-ttu-id="eb11a-231">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-231">**Example:**</span></span>  
`BitAnd(&HF, &HF7)`  
<span data-ttu-id="eb11a-232">Returnerar 7 eftersom hexadecimala ”F” och ”F7” utvärderas till det här värdet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-232">Returns 7 because hexadecimal "F" AND "F7" evaluate to this value.</span></span>

- - -
### <a name="bitor"></a><span data-ttu-id="eb11a-233">BitOr</span><span class="sxs-lookup"><span data-stu-id="eb11a-233">BitOr</span></span>
<span data-ttu-id="eb11a-234">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-234">**Description:**</span></span>  
<span data-ttu-id="eb11a-235">BITOR-funktion anger angivna bits på ett värde.</span><span class="sxs-lookup"><span data-stu-id="eb11a-235">The BitOr function sets specified bits on a value.</span></span>

<span data-ttu-id="eb11a-236">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-236">**Syntax:**</span></span>  
`num BitOr(num value1, num value2)`

* <span data-ttu-id="eb11a-237">value1, value2: numeriska värden som ska vara sammansatta med or tillsammans</span><span class="sxs-lookup"><span data-stu-id="eb11a-237">value1, value2: numeric values that should be OR’ed together</span></span>

<span data-ttu-id="eb11a-238">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-238">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-239">Den här funktionen konverteras båda parametrarna till binär representation och anger en bit 1 om en eller båda motsvarande bitar i mask och flaggan är mellan 1 och 0 om båda motsvarande bits är 0.</span><span class="sxs-lookup"><span data-stu-id="eb11a-239">This function converts both parameters to the binary representation and sets a bit to 1 if one or both of the corresponding bits in mask and flag are 1, and to 0 if both of the corresponding bits are 0.</span></span> <span data-ttu-id="eb11a-240">Med andra ord returnerar 1, utom där motsvarande bitarna av båda parametrarna är 0.</span><span class="sxs-lookup"><span data-stu-id="eb11a-240">In other words, it returns 1 in all cases except where the corresponding bits of both parameters are 0.</span></span>

- - -
### <a name="cbool"></a><span data-ttu-id="eb11a-241">CBool</span><span class="sxs-lookup"><span data-stu-id="eb11a-241">CBool</span></span>
<span data-ttu-id="eb11a-242">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-242">**Description:**</span></span>  
<span data-ttu-id="eb11a-243">Funktionen CBool returnerar ett booleskt värde baserat på det utvärderade uttrycket</span><span class="sxs-lookup"><span data-stu-id="eb11a-243">The CBool function returns a Boolean based on the evaluated expression</span></span>

<span data-ttu-id="eb11a-244">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-244">**Syntax:**</span></span>  
`bool CBool(exp Expression)`

<span data-ttu-id="eb11a-245">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-245">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-246">Om uttrycket utvärderas till ett annat värde CBool returnerar True, annars returneras False.</span><span class="sxs-lookup"><span data-stu-id="eb11a-246">If the expression evaluates to a nonzero value, then CBool returns True, else it returns False.</span></span>

<span data-ttu-id="eb11a-247">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-247">**Example:**</span></span>  
`CBool([attrib1] = [attrib2])`  

<span data-ttu-id="eb11a-248">Returnerar True om båda attribut har samma värde.</span><span class="sxs-lookup"><span data-stu-id="eb11a-248">Returns True if both attributes have the same value.</span></span>

- - -
### <a name="cdate"></a><span data-ttu-id="eb11a-249">CDate</span><span class="sxs-lookup"><span data-stu-id="eb11a-249">CDate</span></span>
<span data-ttu-id="eb11a-250">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-250">**Description:**</span></span>  
<span data-ttu-id="eb11a-251">Funktionen CDate returnerar UTC DateTime från en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-251">The CDate function returns a UTC DateTime from a string.</span></span> <span data-ttu-id="eb11a-252">DateTime är inte en inbyggd attributtyp synkroniserade men används av vissa funktioner.</span><span class="sxs-lookup"><span data-stu-id="eb11a-252">DateTime is not a native attribute type in Sync but is used by some functions.</span></span>

<span data-ttu-id="eb11a-253">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-253">**Syntax:**</span></span>  
`dt CDate(str value)`

* <span data-ttu-id="eb11a-254">Värde: En sträng med ett datum, tid och du kan också tidszon</span><span class="sxs-lookup"><span data-stu-id="eb11a-254">Value: A string with a date, time, and optionally time zone</span></span>

<span data-ttu-id="eb11a-255">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-255">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-256">Den returnerade strängen är alltid i UTC.</span><span class="sxs-lookup"><span data-stu-id="eb11a-256">The returned string is always in UTC.</span></span>

<span data-ttu-id="eb11a-257">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-257">**Example:**</span></span>  
`CDate([employeeStartTime])`  
<span data-ttu-id="eb11a-258">Returnerar ett datetime-värde baserat på medarbetarens starttid</span><span class="sxs-lookup"><span data-stu-id="eb11a-258">Returns a DateTime based on the employee’s start time</span></span>

`CDate("2013-01-10 4:00 PM -8")`  
<span data-ttu-id="eb11a-259">Returnerar en DateTime som representerar ”2013-01-11 12:00:00”</span><span class="sxs-lookup"><span data-stu-id="eb11a-259">Returns a DateTime representing "2013-01-11 12:00 AM"</span></span>








- - -
### <a name="certextensionoids"></a><span data-ttu-id="eb11a-260">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="eb11a-260">CertExtensionOids</span></span>
<span data-ttu-id="eb11a-261">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-261">**Description:**</span></span>  
<span data-ttu-id="eb11a-262">Returnerar Oid-värden för alla kritiska tillägg av ett certifikatobjekt.</span><span class="sxs-lookup"><span data-stu-id="eb11a-262">Returns the Oid values of all the critical extensions of a certificate object.</span></span>

<span data-ttu-id="eb11a-263">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-263">**Syntax:**</span></span>  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-264">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-264">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-265">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-265">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certformat"></a><span data-ttu-id="eb11a-266">CertFormat</span><span class="sxs-lookup"><span data-stu-id="eb11a-266">CertFormat</span></span>
<span data-ttu-id="eb11a-267">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-267">**Description:**</span></span>  
<span data-ttu-id="eb11a-268">Returnerar namnet på formatet för den här X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-268">Returns the name of the format of this X.509v3 certificate.</span></span>

<span data-ttu-id="eb11a-269">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-269">**Syntax:**</span></span>  
`str CertFormat(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-270">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-270">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-271">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-271">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certfriendlyname"></a><span data-ttu-id="eb11a-272">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="eb11a-272">CertFriendlyName</span></span>
<span data-ttu-id="eb11a-273">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-273">**Description:**</span></span>  
<span data-ttu-id="eb11a-274">Returnerar det associera aliaset för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-274">Returns the associated alias for a certificate.</span></span>

<span data-ttu-id="eb11a-275">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-275">**Syntax:**</span></span>  
`str CertFriendlyName(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-276">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-276">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-277">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-277">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certhashstring"></a><span data-ttu-id="eb11a-278">CertHashString</span><span class="sxs-lookup"><span data-stu-id="eb11a-278">CertHashString</span></span>
<span data-ttu-id="eb11a-279">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-279">**Description:**</span></span>  
<span data-ttu-id="eb11a-280">Returnerar SHA1-hash-värdet för X.509v3-certifikat som en hexadecimal sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-280">Returns the SHA1 hash value for the X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="eb11a-281">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-281">**Syntax:**</span></span>  
`str CertHashString(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-282">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-282">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-283">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-283">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuer"></a><span data-ttu-id="eb11a-284">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="eb11a-284">CertIssuer</span></span>
<span data-ttu-id="eb11a-285">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-285">**Description:**</span></span>  
<span data-ttu-id="eb11a-286">Returnerar namnet på den certifikatutfärdare som utfärdade X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-286">Returns the name of the certificate authority that issued the X.509v3 certificate.</span></span>

<span data-ttu-id="eb11a-287">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-287">**Syntax:**</span></span>  
`str CertIssuer(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-288">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-288">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-289">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-289">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuerdn"></a><span data-ttu-id="eb11a-290">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="eb11a-290">CertIssuerDN</span></span>
<span data-ttu-id="eb11a-291">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-291">**Description:**</span></span>  
<span data-ttu-id="eb11a-292">Returnerar det unika namnet på certifikatutfärdaren.</span><span class="sxs-lookup"><span data-stu-id="eb11a-292">Returns the distinguished name of the certificate issuer.</span></span>

<span data-ttu-id="eb11a-293">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-293">**Syntax:**</span></span>  
`str CertIssuerDN(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-294">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-294">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-295">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-295">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissueroid"></a><span data-ttu-id="eb11a-296">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-296">CertIssuerOid</span></span>
<span data-ttu-id="eb11a-297">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-297">**Description:**</span></span>  
<span data-ttu-id="eb11a-298">Returnerar Oid för certifikatutfärdaren.</span><span class="sxs-lookup"><span data-stu-id="eb11a-298">Returns the Oid of the certificate issuer.</span></span>

<span data-ttu-id="eb11a-299">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-299">**Syntax:**</span></span>  
`str CertIssuerOid(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-300">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-300">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-301">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-301">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithm"></a><span data-ttu-id="eb11a-302">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="eb11a-302">CertKeyAlgorithm</span></span>
<span data-ttu-id="eb11a-303">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-303">**Description:**</span></span>  
<span data-ttu-id="eb11a-304">Returnerar nyckelalgoritm informationen för den här X.509v3-certifikat som en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-304">Returns the key algorithm information for this X.509v3 certificate as a string.</span></span>

<span data-ttu-id="eb11a-305">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-305">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-306">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-306">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-307">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-307">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithmparams"></a><span data-ttu-id="eb11a-308">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="eb11a-308">CertKeyAlgorithmParams</span></span>
<span data-ttu-id="eb11a-309">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-309">**Description:**</span></span>  
<span data-ttu-id="eb11a-310">Returnerar nyckelalgoritm parametrar för X.509v3-certifikat som en hexadecimal sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-310">Returns the key algorithm parameters for the X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="eb11a-311">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-311">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-312">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-312">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-313">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-313">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnameinfo"></a><span data-ttu-id="eb11a-314">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="eb11a-314">CertNameInfo</span></span>
<span data-ttu-id="eb11a-315">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-315">**Description:**</span></span>  
<span data-ttu-id="eb11a-316">Returnerar ämne och Utfärdarens namn från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-316">Returns the subject and issuer names from a certificate.</span></span>

<span data-ttu-id="eb11a-317">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-317">**Syntax:**</span></span>  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   <span data-ttu-id="eb11a-318">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-318">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-319">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-319">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
*   <span data-ttu-id="eb11a-320">X509NameType: X509NameType värdet för ämnet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-320">X509NameType: The X509NameType value for the subject.</span></span>
*   <span data-ttu-id="eb11a-321">includesIssuerName: true för att inkludera utfärdarnamnet; Annars, FALSKT.</span><span class="sxs-lookup"><span data-stu-id="eb11a-321">includesIssuerName: true to include the issuer name; otherwise, false.</span></span>

- - -
### <a name="certnotafter"></a><span data-ttu-id="eb11a-322">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="eb11a-322">CertNotAfter</span></span>
<span data-ttu-id="eb11a-323">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-323">**Description:**</span></span>  
<span data-ttu-id="eb11a-324">Returnerar datumet i lokal tid som ett certifikat är inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="eb11a-324">Returns the date in local time after which a certificate is no longer valid.</span></span>

<span data-ttu-id="eb11a-325">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-325">**Syntax:**</span></span>  
`dt CertNotAfter(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-326">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-326">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-327">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-327">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnotbefore"></a><span data-ttu-id="eb11a-328">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="eb11a-328">CertNotBefore</span></span>
<span data-ttu-id="eb11a-329">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-329">**Description:**</span></span>  
<span data-ttu-id="eb11a-330">Returnerar datumet i lokal tid som ett certifikat börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="eb11a-330">Returns the date in local time on which a certificate becomes valid.</span></span>

<span data-ttu-id="eb11a-331">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-331">**Syntax:**</span></span>  
`dt CertNotBefore(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-332">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-332">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-333">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-333">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyoid"></a><span data-ttu-id="eb11a-334">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-334">CertPublicKeyOid</span></span>
<span data-ttu-id="eb11a-335">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-335">**Description:**</span></span>  
<span data-ttu-id="eb11a-336">Returnerar Oid för den offentliga nyckeln för X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-336">Returns the Oid of the public key for the X.509v3 certificate.</span></span>

<span data-ttu-id="eb11a-337">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-337">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-338">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-338">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-339">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-339">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyparametersoid"></a><span data-ttu-id="eb11a-340">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-340">CertPublicKeyParametersOid</span></span>
<span data-ttu-id="eb11a-341">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-341">**Description:**</span></span>  
<span data-ttu-id="eb11a-342">Returnerar Oid för parametrarna för offentlig nyckel för X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-342">Returns the Oid of the public key parameters for the X.509v3 certificate.</span></span>

<span data-ttu-id="eb11a-343">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-343">**Syntax:**</span></span>  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-344">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-344">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-345">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-345">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certserialnumber"></a><span data-ttu-id="eb11a-346">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="eb11a-346">CertSerialNumber</span></span>
<span data-ttu-id="eb11a-347">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-347">**Description:**</span></span>  
<span data-ttu-id="eb11a-348">Returnerar serienumret för X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-348">Returns the serial number of the X.509v3 certificate.</span></span>

<span data-ttu-id="eb11a-349">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-349">**Syntax:**</span></span>  
`str CertSerialNumber(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-350">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-350">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-351">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-351">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsignaturealgorithmoid"></a><span data-ttu-id="eb11a-352">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-352">CertSignatureAlgorithmOid</span></span>
<span data-ttu-id="eb11a-353">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-353">**Description:**</span></span>  
<span data-ttu-id="eb11a-354">Returnerar Oid av algoritmen som används för att skapa signaturen på ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-354">Returns the Oid of the algorithm used to create the signature of a certificate.</span></span>

<span data-ttu-id="eb11a-355">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-355">**Syntax:**</span></span>  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-356">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-356">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-357">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-357">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubject"></a><span data-ttu-id="eb11a-358">CertSubject</span><span class="sxs-lookup"><span data-stu-id="eb11a-358">CertSubject</span></span>
<span data-ttu-id="eb11a-359">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-359">**Description:**</span></span>  
<span data-ttu-id="eb11a-360">Hämtar det unika ämnesnamnet från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-360">Gets the subject distinguished name from a certificate.</span></span>

<span data-ttu-id="eb11a-361">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-361">**Syntax:**</span></span>  
`str CertSubject(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-362">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-362">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-363">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-363">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnamedn"></a><span data-ttu-id="eb11a-364">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="eb11a-364">CertSubjectNameDN</span></span>
<span data-ttu-id="eb11a-365">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-365">**Description:**</span></span>  
<span data-ttu-id="eb11a-366">Returnerar det unika ämnesnamnet från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-366">Returns the subject distinguished name from a certificate.</span></span>

<span data-ttu-id="eb11a-367">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-367">**Syntax:**</span></span>  
`str CertSubjectNameDN(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-368">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-368">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-369">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-369">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnameoid"></a><span data-ttu-id="eb11a-370">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="eb11a-370">CertSubjectNameOid</span></span>
<span data-ttu-id="eb11a-371">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-371">**Description:**</span></span>  
<span data-ttu-id="eb11a-372">Returnerar Oid för ämnesnamnet från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-372">Returns the Oid of the subject name from a certificate.</span></span>

<span data-ttu-id="eb11a-373">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-373">**Syntax:**</span></span>  
`str CertSubjectNameOid(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-374">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-374">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-375">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-375">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certthumbprint"></a><span data-ttu-id="eb11a-376">certThumbprint</span><span class="sxs-lookup"><span data-stu-id="eb11a-376">CertThumbprint</span></span>
<span data-ttu-id="eb11a-377">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-377">**Description:**</span></span>  
<span data-ttu-id="eb11a-378">Returnerar tumavtrycket för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-378">Returns the thumbprint of a certificate.</span></span>

<span data-ttu-id="eb11a-379">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-379">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-380">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-380">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-381">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-381">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certversion"></a><span data-ttu-id="eb11a-382">CertVersion</span><span class="sxs-lookup"><span data-stu-id="eb11a-382">CertVersion</span></span>
<span data-ttu-id="eb11a-383">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-383">**Description:**</span></span>  
<span data-ttu-id="eb11a-384">Returnerar X.509-Formatversion för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-384">Returns the X.509 format version of a certificate.</span></span>

<span data-ttu-id="eb11a-385">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-385">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-386">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-386">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-387">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-387">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="cguid"></a><span data-ttu-id="eb11a-388">CGuid</span><span class="sxs-lookup"><span data-stu-id="eb11a-388">CGuid</span></span>
<span data-ttu-id="eb11a-389">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-389">**Description:**</span></span>  
<span data-ttu-id="eb11a-390">Funktionen CGuid konverterar strängrepresentation av en GUID till dess binär representation.</span><span class="sxs-lookup"><span data-stu-id="eb11a-390">The CGuid function converts the string representation of a GUID to its binary representation.</span></span>

<span data-ttu-id="eb11a-391">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-391">**Syntax:**</span></span>  
`bin CGuid(str GUID)`

* <span data-ttu-id="eb11a-392">En sträng formaterad i det här mönstret: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx eller {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="eb11a-392">A String formatted in this pattern: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

- - -
### <a name="contains"></a><span data-ttu-id="eb11a-393">Contains</span><span class="sxs-lookup"><span data-stu-id="eb11a-393">Contains</span></span>
<span data-ttu-id="eb11a-394">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-394">**Description:**</span></span>  
<span data-ttu-id="eb11a-395">Innehåller funktionen söker efter en sträng i ett flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="eb11a-395">The Contains function finds a string inside a multi-valued attribute</span></span>

<span data-ttu-id="eb11a-396">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-396">**Syntax:**</span></span>  
<span data-ttu-id="eb11a-397">`num Contains (mvstring attribute, str search)`-skiftlägeskänsligt</span><span class="sxs-lookup"><span data-stu-id="eb11a-397">`num Contains (mvstring attribute, str search)` - case-sensitive</span></span>  
`num Contains (mvstring attribute, str search, enum Casetype)`  
<span data-ttu-id="eb11a-398">`num Contains (mvref attribute, str search)`-skiftlägeskänsligt</span><span class="sxs-lookup"><span data-stu-id="eb11a-398">`num Contains (mvref attribute, str search)` - case-sensitive</span></span>

* <span data-ttu-id="eb11a-399">attributet: flervärdesattribut att söka.</span><span class="sxs-lookup"><span data-stu-id="eb11a-399">attribute: the multi-valued attribute to search.</span></span>
* <span data-ttu-id="eb11a-400">Sök: sträng att söka efter i attributet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-400">search: string to find in the attribute.</span></span>
* <span data-ttu-id="eb11a-401">Casetype: CaseInsensitive- eller CaseSensitive.</span><span class="sxs-lookup"><span data-stu-id="eb11a-401">Casetype: CaseInsensitive or CaseSensitive.</span></span>

<span data-ttu-id="eb11a-402">Returnerar index i attributet med flera värden där strängen hittades.</span><span class="sxs-lookup"><span data-stu-id="eb11a-402">Returns index in the multi-valued attribute where the string was found.</span></span> <span data-ttu-id="eb11a-403">0 returneras om strängen inte hittas.</span><span class="sxs-lookup"><span data-stu-id="eb11a-403">0 is returned if the string is not found.</span></span>

<span data-ttu-id="eb11a-404">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-404">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-405">Söker efter delsträngar i värdena för flera värden strängattribut sökningen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-405">For multi-valued string attributes, the search finds substrings in the values.</span></span>  
<span data-ttu-id="eb11a-406">Den sökta strängen måste exakt matcha värdet för att anses vara en matchning för referensattribut.</span><span class="sxs-lookup"><span data-stu-id="eb11a-406">For reference attributes, the searched string must exactly match the value to be considered a match.</span></span>

<span data-ttu-id="eb11a-407">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-407">**Example:**</span></span>  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
<span data-ttu-id="eb11a-408">Om attributet proxyAddresses har en primär e-postadress (anges med versaler ”SMTP”:), returneras attributet proxyAddress, annars returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="eb11a-408">If the proxyAddresses attribute has a primary email address (indicated by uppercase "SMTP:"), then return the proxyAddress attribute, else return an error.</span></span>

- - -
### <a name="convertfrombase64"></a><span data-ttu-id="eb11a-409">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="eb11a-409">ConvertFromBase64</span></span>
<span data-ttu-id="eb11a-410">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-410">**Description:**</span></span>  
<span data-ttu-id="eb11a-411">Funktionen ConvertFromBase64 konverterar angivna base64-kodade värdet till en vanlig sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-411">The ConvertFromBase64 function converts the specified base64 encoded value to a regular string.</span></span>

<span data-ttu-id="eb11a-412">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-412">**Syntax:**</span></span>  
<span data-ttu-id="eb11a-413">`str ConvertFromBase64(str source)`-förutsätter Unicode för kodning</span><span class="sxs-lookup"><span data-stu-id="eb11a-413">`str ConvertFromBase64(str source)` - assumes Unicode for encoding</span></span>  
`str ConvertFromBase64(str source, enum Encoding)`

* <span data-ttu-id="eb11a-414">källa: Base64-kodad sträng</span><span class="sxs-lookup"><span data-stu-id="eb11a-414">source: Base64 encoded string</span></span>  
* <span data-ttu-id="eb11a-415">Kodning: Unicode, ASCII, UTF8</span><span class="sxs-lookup"><span data-stu-id="eb11a-415">Encoding: Unicode, ASCII, UTF8</span></span>

<span data-ttu-id="eb11a-416">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="eb11a-416">**Example**</span></span>  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

<span data-ttu-id="eb11a-417">Båda exempel returnera ”*Hello world!*”</span><span class="sxs-lookup"><span data-stu-id="eb11a-417">Both examples return "*Hello world!*"</span></span>

- - -
### <a name="convertfromutf8hex"></a><span data-ttu-id="eb11a-418">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="eb11a-418">ConvertFromUTF8Hex</span></span>
<span data-ttu-id="eb11a-419">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-419">**Description:**</span></span>  
<span data-ttu-id="eb11a-420">Funktionen ConvertFromUTF8Hex konverterar det angivna Hex UTF8-kodade värdet till en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-420">The ConvertFromUTF8Hex function converts the specified UTF8 Hex encoded value to a string.</span></span>

<span data-ttu-id="eb11a-421">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-421">**Syntax:**</span></span>  
`str ConvertFromUTF8Hex(str source)`

* <span data-ttu-id="eb11a-422">källa: UTF8 2 byte kodade förekomster av textsträngen</span><span class="sxs-lookup"><span data-stu-id="eb11a-422">source: UTF8 2-byte encoded sting</span></span>

<span data-ttu-id="eb11a-423">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-423">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-424">Skillnaden mellan den här funktionen och ConvertFromBase64([],UTF8) i som resultatet är eget för DN-attributet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-424">The difference between this function and ConvertFromBase64([],UTF8) in that the result is friendly for the DN attribute.</span></span>  
<span data-ttu-id="eb11a-425">Det här formatet används av Azure Active Directory som unikt namn.</span><span class="sxs-lookup"><span data-stu-id="eb11a-425">This format is used by Azure Active Directory as DN.</span></span>

<span data-ttu-id="eb11a-426">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-426">**Example:**</span></span>  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
<span data-ttu-id="eb11a-427">Returnerar ”*Hello world!*”</span><span class="sxs-lookup"><span data-stu-id="eb11a-427">Returns "*Hello world!*"</span></span>

- - -
### <a name="converttobase64"></a><span data-ttu-id="eb11a-428">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="eb11a-428">ConvertToBase64</span></span>
<span data-ttu-id="eb11a-429">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-429">**Description:**</span></span>  
<span data-ttu-id="eb11a-430">Funktionen ConvertToBase64 konverterar en sträng till en Unicode-base64-sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-430">The ConvertToBase64 function converts a string to a Unicode base64 string.</span></span>  
<span data-ttu-id="eb11a-431">Konverterar värdet för en heltalsmatris till dess motsvarande strängrepresentation som kodats med Base64-siffror.</span><span class="sxs-lookup"><span data-stu-id="eb11a-431">Converts the value of an array of integers to its equivalent string representation that is encoded with base-64 digits.</span></span>

<span data-ttu-id="eb11a-432">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-432">**Syntax:**</span></span>  
`str ConvertToBase64(str source)`

<span data-ttu-id="eb11a-433">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-433">**Example:**</span></span>  
`ConvertToBase64("Hello world!")`  
<span data-ttu-id="eb11a-434">Returnerar ”SABlAGwAbABvACAAdwBvAHIAbABkACEA”</span><span class="sxs-lookup"><span data-stu-id="eb11a-434">Returns "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span></span>

- - -
### <a name="converttoutf8hex"></a><span data-ttu-id="eb11a-435">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="eb11a-435">ConvertToUTF8Hex</span></span>
<span data-ttu-id="eb11a-436">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-436">**Description:**</span></span>  
<span data-ttu-id="eb11a-437">Funktionen ConvertToUTF8Hex konverterar en sträng till ett värde för Hex UTF8-kodade.</span><span class="sxs-lookup"><span data-stu-id="eb11a-437">The ConvertToUTF8Hex function converts a string to a UTF8 Hex encoded value.</span></span>

<span data-ttu-id="eb11a-438">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-438">**Syntax:**</span></span>  
`str ConvertToUTF8Hex(str source)`

<span data-ttu-id="eb11a-439">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-439">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-440">Utdataformatet för den här funktionen används av Azure Active Directory som DN attribute-format.</span><span class="sxs-lookup"><span data-stu-id="eb11a-440">The output format of this function is used by Azure Active Directory as DN attribute format.</span></span>

<span data-ttu-id="eb11a-441">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-441">**Example:**</span></span>  
`ConvertToUTF8Hex("Hello world!")`  
<span data-ttu-id="eb11a-442">Returnerar 48656C6C6F20776F726C6421</span><span class="sxs-lookup"><span data-stu-id="eb11a-442">Returns 48656C6C6F20776F726C6421</span></span>

- - -
### <a name="count"></a><span data-ttu-id="eb11a-443">Antal</span><span class="sxs-lookup"><span data-stu-id="eb11a-443">Count</span></span>
<span data-ttu-id="eb11a-444">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-444">**Description:**</span></span>  
<span data-ttu-id="eb11a-445">Count-funktionen returnerar antalet element i ett flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="eb11a-445">The Count function returns the number of elements in a multi-valued attribute</span></span>

<span data-ttu-id="eb11a-446">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-446">**Syntax:**</span></span>  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a><span data-ttu-id="eb11a-447">CNum</span><span class="sxs-lookup"><span data-stu-id="eb11a-447">CNum</span></span>
<span data-ttu-id="eb11a-448">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-448">**Description:**</span></span>  
<span data-ttu-id="eb11a-449">Funktionen CNum tar en sträng och returnerar en numerisk datatyp.</span><span class="sxs-lookup"><span data-stu-id="eb11a-449">The CNum function takes a string and returns a numeric data type.</span></span>

<span data-ttu-id="eb11a-450">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-450">**Syntax:**</span></span>  
`num CNum(str value)`

- - -
### <a name="cref"></a><span data-ttu-id="eb11a-451">CRef</span><span class="sxs-lookup"><span data-stu-id="eb11a-451">CRef</span></span>
<span data-ttu-id="eb11a-452">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-452">**Description:**</span></span>  
<span data-ttu-id="eb11a-453">Konverterar en sträng till ett referensattribut</span><span class="sxs-lookup"><span data-stu-id="eb11a-453">Converts a string to a reference attribute</span></span>

<span data-ttu-id="eb11a-454">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-454">**Syntax:**</span></span>  
`ref CRef(str value)`

<span data-ttu-id="eb11a-455">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-455">**Example:**</span></span>  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a><span data-ttu-id="eb11a-456">CStr</span><span class="sxs-lookup"><span data-stu-id="eb11a-456">CStr</span></span>
<span data-ttu-id="eb11a-457">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-457">**Description:**</span></span>  
<span data-ttu-id="eb11a-458">Funktionen CStr konverteras till datatypen string.</span><span class="sxs-lookup"><span data-stu-id="eb11a-458">The CStr function converts to a string data type.</span></span>

<span data-ttu-id="eb11a-459">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-459">**Syntax:**</span></span>  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* <span data-ttu-id="eb11a-460">värde: kan vara ett numeriskt värde, referensattribut eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="eb11a-460">value: Can be a numeric value, reference attribute, or Boolean.</span></span>

<span data-ttu-id="eb11a-461">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-461">**Example:**</span></span>  
`CStr([dn])`  
<span data-ttu-id="eb11a-462">Kan returnera ”cn = Johan, dc = contoso, dc = com”</span><span class="sxs-lookup"><span data-stu-id="eb11a-462">Could return "cn=Joe,dc=contoso,dc=com"</span></span>

- - -
### <a name="dateadd"></a><span data-ttu-id="eb11a-463">DateAdd</span><span class="sxs-lookup"><span data-stu-id="eb11a-463">DateAdd</span></span>
<span data-ttu-id="eb11a-464">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-464">**Description:**</span></span>  
<span data-ttu-id="eb11a-465">Returnerar ett datum som innehåller ett datum som ett angivet tidsintervall har lagts till.</span><span class="sxs-lookup"><span data-stu-id="eb11a-465">Returns a Date containing a date to which a specified time interval has been added.</span></span>

<span data-ttu-id="eb11a-466">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-466">**Syntax:**</span></span>  
`dt DateAdd(str interval, num value, dt date)`

* <span data-ttu-id="eb11a-467">intervall: stränguttryck som är tidsintervall du vill lägga till.</span><span class="sxs-lookup"><span data-stu-id="eb11a-467">interval: String expression that is the interval of time you want to add.</span></span> <span data-ttu-id="eb11a-468">Strängen måste ha något av följande värden:</span><span class="sxs-lookup"><span data-stu-id="eb11a-468">The string must have one of the following values:</span></span>
  * <span data-ttu-id="eb11a-469">åååå år</span><span class="sxs-lookup"><span data-stu-id="eb11a-469">yyyy Year</span></span>
  * <span data-ttu-id="eb11a-470">q kvartal</span><span class="sxs-lookup"><span data-stu-id="eb11a-470">q Quarter</span></span>
  * <span data-ttu-id="eb11a-471">m månad</span><span class="sxs-lookup"><span data-stu-id="eb11a-471">m Month</span></span>
  * <span data-ttu-id="eb11a-472">y dagen på året</span><span class="sxs-lookup"><span data-stu-id="eb11a-472">y Day of year</span></span>
  * <span data-ttu-id="eb11a-473">d dag</span><span class="sxs-lookup"><span data-stu-id="eb11a-473">d Day</span></span>
  * <span data-ttu-id="eb11a-474">w veckodag</span><span class="sxs-lookup"><span data-stu-id="eb11a-474">w Weekday</span></span>
  * <span data-ttu-id="eb11a-475">ww vecka</span><span class="sxs-lookup"><span data-stu-id="eb11a-475">ww Week</span></span>
  * <span data-ttu-id="eb11a-476">h timme</span><span class="sxs-lookup"><span data-stu-id="eb11a-476">h Hour</span></span>
  * <span data-ttu-id="eb11a-477">n minut</span><span class="sxs-lookup"><span data-stu-id="eb11a-477">n Minute</span></span>
  * <span data-ttu-id="eb11a-478">s andra</span><span class="sxs-lookup"><span data-stu-id="eb11a-478">s Second</span></span>
* <span data-ttu-id="eb11a-479">värde: antalet enheter som du vill lägga till.</span><span class="sxs-lookup"><span data-stu-id="eb11a-479">value: The number of units you want to add.</span></span> <span data-ttu-id="eb11a-480">Det kan vara positivt (för att hämta framtida datum) eller negativt (för att hämta tidigare datum).</span><span class="sxs-lookup"><span data-stu-id="eb11a-480">It can be positive (to get dates in the future) or negative (to get dates in the past).</span></span>
* <span data-ttu-id="eb11a-481">datum: DateTime som representerar som intervallet ska läggas till.</span><span class="sxs-lookup"><span data-stu-id="eb11a-481">date: DateTime representing date to which the interval is added.</span></span>

<span data-ttu-id="eb11a-482">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-482">**Example:**</span></span>  
`DateAdd("m", 3, CDate("2001-01-01"))`  
<span data-ttu-id="eb11a-483">Lägger till tre månader och returnerar ett datetime-värde som representerar ”2001-04-01”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-483">Adds 3 months and returns a DateTime representing "2001-04-01".</span></span>

- - -
### <a name="datefromnum"></a><span data-ttu-id="eb11a-484">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="eb11a-484">DateFromNum</span></span>
<span data-ttu-id="eb11a-485">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-485">**Description:**</span></span>  
<span data-ttu-id="eb11a-486">Funktionen DateFromNum konverterar ett värde i Annonsens datum-format till ett DateTime-typen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-486">The DateFromNum function converts a value in AD’s date format to a DateTime type.</span></span>

<span data-ttu-id="eb11a-487">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-487">**Syntax:**</span></span>  
`dt DateFromNum(num value)`

<span data-ttu-id="eb11a-488">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-488">**Example:**</span></span>  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
<span data-ttu-id="eb11a-489">Returnerar ett datetime-värde som representerar 2012-01-01 23:00:00</span><span class="sxs-lookup"><span data-stu-id="eb11a-489">Returns a DateTime representing 2012-01-01 23:00:00</span></span>

- - -
### <a name="dncomponent"></a><span data-ttu-id="eb11a-490">DNComponent</span><span class="sxs-lookup"><span data-stu-id="eb11a-490">DNComponent</span></span>
<span data-ttu-id="eb11a-491">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-491">**Description:**</span></span>  
<span data-ttu-id="eb11a-492">Funktionen DNComponent returnerar värdet för en angiven DN-komponent från vänster.</span><span class="sxs-lookup"><span data-stu-id="eb11a-492">The DNComponent function returns the value of a specified DN component going from left.</span></span>

<span data-ttu-id="eb11a-493">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-493">**Syntax:**</span></span>  
`str DNComponent(ref dn, num ComponentNumber)`

* <span data-ttu-id="eb11a-494">DN: referensattribut att tolka</span><span class="sxs-lookup"><span data-stu-id="eb11a-494">dn: the reference attribute to interpret</span></span>
* <span data-ttu-id="eb11a-495">ComponentNumber: Komponenten i DN ska returneras</span><span class="sxs-lookup"><span data-stu-id="eb11a-495">ComponentNumber: The component in the DN to return</span></span>

<span data-ttu-id="eb11a-496">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-496">**Example:**</span></span>  
`DNComponent([dn],1)`  
<span data-ttu-id="eb11a-497">Om dn är ”cn = Johan, ou =...”, returneras Joe</span><span class="sxs-lookup"><span data-stu-id="eb11a-497">If dn is "cn=Joe,ou=…," it returns Joe</span></span>

- - -
### <a name="dncomponentrev"></a><span data-ttu-id="eb11a-498">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="eb11a-498">DNComponentRev</span></span>
<span data-ttu-id="eb11a-499">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-499">**Description:**</span></span>  
<span data-ttu-id="eb11a-500">Funktionen DNComponentRev returnerar värdet för en angiven DN-komponent från höger (end).</span><span class="sxs-lookup"><span data-stu-id="eb11a-500">The DNComponentRev function returns the value of a specified DN component going from right (the end).</span></span>

<span data-ttu-id="eb11a-501">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-501">**Syntax:**</span></span>  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* <span data-ttu-id="eb11a-502">DN: referensattribut att tolka</span><span class="sxs-lookup"><span data-stu-id="eb11a-502">dn: the reference attribute to interpret</span></span>
* <span data-ttu-id="eb11a-503">ComponentNumber - komponenten i DN ska returneras</span><span class="sxs-lookup"><span data-stu-id="eb11a-503">ComponentNumber - The component in the DN to return</span></span>
* <span data-ttu-id="eb11a-504">Alternativ: DC – Ignorera alla komponenter med ”dc =”</span><span class="sxs-lookup"><span data-stu-id="eb11a-504">Options: DC – Ignore all components with "dc="</span></span>

<span data-ttu-id="eb11a-505">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-505">**Example:**</span></span>  
<span data-ttu-id="eb11a-506">Om dn är ”cn = Joe, ou = Atlanta, ou = GA, ou = US, dc = contoso, dc = com” sedan</span><span class="sxs-lookup"><span data-stu-id="eb11a-506">If dn is "cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com" then</span></span>  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
<span data-ttu-id="eb11a-507">Returnerar båda oss.</span><span class="sxs-lookup"><span data-stu-id="eb11a-507">Both return US.</span></span>

- - -
### <a name="error"></a><span data-ttu-id="eb11a-508">Fel</span><span class="sxs-lookup"><span data-stu-id="eb11a-508">Error</span></span>
<span data-ttu-id="eb11a-509">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-509">**Description:**</span></span>  
<span data-ttu-id="eb11a-510">Fel-funktionen används för att returnera ett anpassat fel.</span><span class="sxs-lookup"><span data-stu-id="eb11a-510">The Error function is used to return a custom error.</span></span>

<span data-ttu-id="eb11a-511">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-511">**Syntax:**</span></span>  
`void Error(str ErrorMessage)`

<span data-ttu-id="eb11a-512">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-512">**Example:**</span></span>  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
<span data-ttu-id="eb11a-513">Om attributet accountName inte finns genereras ett fel i objektet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-513">If the attribute accountName is not present, throw an error on the object.</span></span>

- - -
### <a name="escapedncomponent"></a><span data-ttu-id="eb11a-514">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="eb11a-514">EscapeDNComponent</span></span>
<span data-ttu-id="eb11a-515">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-515">**Description:**</span></span>  
<span data-ttu-id="eb11a-516">Funktionen EscapeDNComponent tar en komponent i ett unikt namn och hoppas det så att den kan representeras i LDAP.</span><span class="sxs-lookup"><span data-stu-id="eb11a-516">The EscapeDNComponent function takes one component of a DN and escapes it so it can be represented in LDAP.</span></span>

<span data-ttu-id="eb11a-517">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-517">**Syntax:**</span></span>  
`str EscapeDNComponent(str value)`

<span data-ttu-id="eb11a-518">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-518">**Example:**</span></span>  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
<span data-ttu-id="eb11a-519">Ser till att objektet kan skapas i en LDAP-katalog, även om attributet visningsnamn innehåller tecken som måste hoppas i LDAP.</span><span class="sxs-lookup"><span data-stu-id="eb11a-519">Makes sure the object can be created in an LDAP directory even if the displayName attribute has characters that must be escaped in LDAP.</span></span>

- - -
### <a name="formatdatetime"></a><span data-ttu-id="eb11a-520">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="eb11a-520">FormatDateTime</span></span>
<span data-ttu-id="eb11a-521">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-521">**Description:**</span></span>  
<span data-ttu-id="eb11a-522">Funktionen FormatDateTime används för att formatera ett datetime-värde till en sträng med ett bestämt format</span><span class="sxs-lookup"><span data-stu-id="eb11a-522">The FormatDateTime function is used to format a DateTime to a string with a specified format</span></span>

<span data-ttu-id="eb11a-523">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-523">**Syntax:**</span></span>  
`str FormatDateTime(dt value, str format)`

* <span data-ttu-id="eb11a-524">värde: ett värde i DateTime-format</span><span class="sxs-lookup"><span data-stu-id="eb11a-524">value: a value in the DateTime format</span></span>
* <span data-ttu-id="eb11a-525">format: en sträng som representerar det format som ska konverteras till.</span><span class="sxs-lookup"><span data-stu-id="eb11a-525">format: a string representing the format to convert to.</span></span>

<span data-ttu-id="eb11a-526">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-526">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-527">De möjliga värdena för formatet finns här: [User-defined datum/tid-format (Format-funktionen)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span><span class="sxs-lookup"><span data-stu-id="eb11a-527">The possible values for the format can be found here: [User-Defined Date/Time Formats (Format Function)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span></span>

<span data-ttu-id="eb11a-528">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-528">**Example:**</span></span>  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
<span data-ttu-id="eb11a-529">Resulterar i ”2007-12-25”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-529">Results in "2007-12-25".</span></span>

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
<span data-ttu-id="eb11a-530">Kan medföra ”20140905081453.0Z”</span><span class="sxs-lookup"><span data-stu-id="eb11a-530">Can result in "20140905081453.0Z"</span></span>

- - -
### <a name="guid"></a><span data-ttu-id="eb11a-531">GUID</span><span class="sxs-lookup"><span data-stu-id="eb11a-531">GUID</span></span>
<span data-ttu-id="eb11a-532">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-532">**Description:**</span></span>  
<span data-ttu-id="eb11a-533">Funktionen GUID genererar en ny, slumpmässig GUID</span><span class="sxs-lookup"><span data-stu-id="eb11a-533">The function GUID generates a new random GUID</span></span>

<span data-ttu-id="eb11a-534">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-534">**Syntax:**</span></span>  
`str GUID()`

- - -
### <a name="iif"></a><span data-ttu-id="eb11a-535">IIF</span><span class="sxs-lookup"><span data-stu-id="eb11a-535">IIF</span></span>
<span data-ttu-id="eb11a-536">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-536">**Description:**</span></span>  
<span data-ttu-id="eb11a-537">Funktionen IIF returnerar ett av en uppsättning möjliga värden baserat på ett angivet villkor.</span><span class="sxs-lookup"><span data-stu-id="eb11a-537">The IIF function returns one of a set of possible values based on a specified condition.</span></span>

<span data-ttu-id="eb11a-538">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-538">**Syntax:**</span></span>  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* <span data-ttu-id="eb11a-539">villkor: ett värde eller uttryck som kan utvärderas till true eller false.</span><span class="sxs-lookup"><span data-stu-id="eb11a-539">condition: any value or expression that can be evaluated to true or false.</span></span>
* <span data-ttu-id="eb11a-540">värde_om_sant: om villkoret utvärderas till SANT, det returnerade värdet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-540">valueIfTrue: If the condition evaluates to true, the returned value.</span></span>
* <span data-ttu-id="eb11a-541">värde_om_falskt: om villkoret utvärderas till false, värdet som returneras.</span><span class="sxs-lookup"><span data-stu-id="eb11a-541">valueIfFalse: If the condition evaluates to false, the returned value.</span></span>

<span data-ttu-id="eb11a-542">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-542">**Example:**</span></span>  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 <span data-ttu-id="eb11a-543">Om användaren är en intern, returnerar alias för en användare med ”t-” läggs till i början av det, annars returnerar användarens alias som är.</span><span class="sxs-lookup"><span data-stu-id="eb11a-543">If the user is an intern, returns the alias of a user with "t-" added to the beginning of it, else returns the user’s alias as is.</span></span>

- - -
### <a name="instr"></a><span data-ttu-id="eb11a-544">InStr</span><span class="sxs-lookup"><span data-stu-id="eb11a-544">InStr</span></span>
<span data-ttu-id="eb11a-545">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-545">**Description:**</span></span>  
<span data-ttu-id="eb11a-546">Funktionen InStr söker efter den första förekomsten av en understräng i en sträng</span><span class="sxs-lookup"><span data-stu-id="eb11a-546">The InStr function finds the first occurrence of a substring in a string</span></span>

<span data-ttu-id="eb11a-547">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-547">**Syntax:**</span></span>  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* <span data-ttu-id="eb11a-548">stringcheck: strängen som ska genomsökas</span><span class="sxs-lookup"><span data-stu-id="eb11a-548">stringcheck: string to be searched</span></span>
* <span data-ttu-id="eb11a-549">stringmatch: strängen som ska returneras</span><span class="sxs-lookup"><span data-stu-id="eb11a-549">stringmatch: string to be found</span></span>
* <span data-ttu-id="eb11a-550">Starta: startposition för att hitta delsträngen</span><span class="sxs-lookup"><span data-stu-id="eb11a-550">start: starting position to find the substring</span></span>
* <span data-ttu-id="eb11a-551">Jämför: i vbTextCompare eller i vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="eb11a-551">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="eb11a-552">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-552">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-553">Returnerar positionen där delsträngen hittades eller 0 om inte hittas.</span><span class="sxs-lookup"><span data-stu-id="eb11a-553">Returns the position where the substring was found or 0 if not found.</span></span>

<span data-ttu-id="eb11a-554">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-554">**Example:**</span></span>  
`InStr("The quick brown fox","quick")`  
<span data-ttu-id="eb11a-555">Evalues till 5</span><span class="sxs-lookup"><span data-stu-id="eb11a-555">Evalues to 5</span></span>

`InStr("repEated","e",3,vbBinaryCompare)`  
<span data-ttu-id="eb11a-556">Utvärderar till 7</span><span class="sxs-lookup"><span data-stu-id="eb11a-556">Evaluates to 7</span></span>

- - -
### <a name="instrrev"></a><span data-ttu-id="eb11a-557">InStrRev</span><span class="sxs-lookup"><span data-stu-id="eb11a-557">InStrRev</span></span>
<span data-ttu-id="eb11a-558">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-558">**Description:**</span></span>  
<span data-ttu-id="eb11a-559">Funktionen InStrRev söker efter den sista förekomsten av en understräng i en sträng</span><span class="sxs-lookup"><span data-stu-id="eb11a-559">The InStrRev function finds the last occurrence of a substring in a string</span></span>

<span data-ttu-id="eb11a-560">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-560">**Syntax:**</span></span>  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* <span data-ttu-id="eb11a-561">stringcheck: strängen som ska genomsökas</span><span class="sxs-lookup"><span data-stu-id="eb11a-561">stringcheck: string to be searched</span></span>
* <span data-ttu-id="eb11a-562">stringmatch: strängen som ska returneras</span><span class="sxs-lookup"><span data-stu-id="eb11a-562">stringmatch: string to be found</span></span>
* <span data-ttu-id="eb11a-563">Starta: startposition för att hitta delsträngen</span><span class="sxs-lookup"><span data-stu-id="eb11a-563">start: starting position to find the substring</span></span>
* <span data-ttu-id="eb11a-564">Jämför: i vbTextCompare eller i vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="eb11a-564">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="eb11a-565">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-565">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-566">Returnerar positionen där delsträngen hittades eller 0 om inte hittas.</span><span class="sxs-lookup"><span data-stu-id="eb11a-566">Returns the position where the substring was found or 0 if not found.</span></span>

<span data-ttu-id="eb11a-567">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-567">**Example:**</span></span>  
`InStrRev("abbcdbbbef","bb")`  
<span data-ttu-id="eb11a-568">Returnerar 7</span><span class="sxs-lookup"><span data-stu-id="eb11a-568">Returns 7</span></span>

- - -
### <a name="isbitset"></a><span data-ttu-id="eb11a-569">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="eb11a-569">IsBitSet</span></span>
<span data-ttu-id="eb11a-570">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-570">**Description:**</span></span>  
<span data-ttu-id="eb11a-571">Funktionen IsBitSet nätverkstester om en stund anges eller inte</span><span class="sxs-lookup"><span data-stu-id="eb11a-571">The function IsBitSet Tests if a bit is set or not</span></span>

<span data-ttu-id="eb11a-572">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-572">**Syntax:**</span></span>  
`bool IsBitSet(num value, num flag)`

* <span data-ttu-id="eb11a-573">värde: ett numeriskt värde som är evaluated.flag: ett numeriskt värde som har bitars som ska utvärderas</span><span class="sxs-lookup"><span data-stu-id="eb11a-573">value: a numeric value that is evaluated.flag: a numeric value that has the bit to be evaluated</span></span>

<span data-ttu-id="eb11a-574">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-574">**Example:**</span></span>  
`IsBitSet(&HF,4)`  
<span data-ttu-id="eb11a-575">Returnerar SANT eftersom ”4”-biten är aktiverad i det hexadecimala värdet ”F”</span><span class="sxs-lookup"><span data-stu-id="eb11a-575">Returns True because bit "4" is set in the hexadecimal value "F"</span></span>

- - -
### <a name="isdate"></a><span data-ttu-id="eb11a-576">IsDate</span><span class="sxs-lookup"><span data-stu-id="eb11a-576">IsDate</span></span>
<span data-ttu-id="eb11a-577">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-577">**Description:**</span></span>  
<span data-ttu-id="eb11a-578">Om uttrycket kan vara utvärderas som en DateTime-typ, och sedan funktionen IsDate utvärderas till SANT.</span><span class="sxs-lookup"><span data-stu-id="eb11a-578">If the expression can be evaluates as a DateTime type, then the IsDate function evaluates to True.</span></span>

<span data-ttu-id="eb11a-579">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-579">**Syntax:**</span></span>  
`bool IsDate(var Expression)`

<span data-ttu-id="eb11a-580">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-580">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-581">Används för att avgöra om CDate() kan genomföras.</span><span class="sxs-lookup"><span data-stu-id="eb11a-581">Used to determine if CDate() can be successful.</span></span>

- - -
### <a name="iscert"></a><span data-ttu-id="eb11a-582">IsCert</span><span class="sxs-lookup"><span data-stu-id="eb11a-582">IsCert</span></span>
<span data-ttu-id="eb11a-583">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-583">**Description:**</span></span>  
<span data-ttu-id="eb11a-584">Returnerar true om rådata kan serialiseras till .NET X509Certificate2 certifikatobjekt.</span><span class="sxs-lookup"><span data-stu-id="eb11a-584">Returns true if the raw data can be serialized into .NET X509Certificate2 certificate object.</span></span>

<span data-ttu-id="eb11a-585">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-585">**Syntax:**</span></span>  
`bool CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="eb11a-586">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-586">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="eb11a-587">Bytematrisen kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="eb11a-587">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
- - -
### <a name="isempty"></a><span data-ttu-id="eb11a-588">IsEmpty</span><span class="sxs-lookup"><span data-stu-id="eb11a-588">IsEmpty</span></span>
<span data-ttu-id="eb11a-589">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-589">**Description:**</span></span>  
<span data-ttu-id="eb11a-590">Om attributet finns i CS eller MV men evalueras till en tom sträng, utvärderar funktionen IsEmpty till True.</span><span class="sxs-lookup"><span data-stu-id="eb11a-590">If the attribute is present in the CS or MV but evaluates to an empty string, then the IsEmpty function evaluates to True.</span></span>

<span data-ttu-id="eb11a-591">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-591">**Syntax:**</span></span>  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a><span data-ttu-id="eb11a-592">IsGuid</span><span class="sxs-lookup"><span data-stu-id="eb11a-592">IsGuid</span></span>
<span data-ttu-id="eb11a-593">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-593">**Description:**</span></span>  
<span data-ttu-id="eb11a-594">Om strängen kan konverteras till ett GUID, utvärderas funktionen IsGuid till true.</span><span class="sxs-lookup"><span data-stu-id="eb11a-594">If the string could be converted to a GUID, then the IsGuid function evaluated to true.</span></span>

<span data-ttu-id="eb11a-595">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-595">**Syntax:**</span></span>  
`bool IsGuid(str GUID)`

<span data-ttu-id="eb11a-596">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-596">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-597">Ett GUID som har definierats som en sträng som följer någon av dessa mönster: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx eller {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="eb11a-597">A GUID is defined as a string following one of these patterns: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

<span data-ttu-id="eb11a-598">Används för att avgöra om CGuid() kan genomföras.</span><span class="sxs-lookup"><span data-stu-id="eb11a-598">Used to determine if CGuid() can be successful.</span></span>

<span data-ttu-id="eb11a-599">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-599">**Example:**</span></span>  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
<span data-ttu-id="eb11a-600">Om StrAttribute har en GUID-format, returnera en a binär representation, annars returnerar Null.</span><span class="sxs-lookup"><span data-stu-id="eb11a-600">If the StrAttribute has a GUID format, return a binary representation, otherwise return a Null.</span></span>

- - -
### <a name="isnull"></a><span data-ttu-id="eb11a-601">IsNull</span><span class="sxs-lookup"><span data-stu-id="eb11a-601">IsNull</span></span>
<span data-ttu-id="eb11a-602">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-602">**Description:**</span></span>  
<span data-ttu-id="eb11a-603">Om uttrycket utvärderas till Null, returnerar funktionen IsNull true.</span><span class="sxs-lookup"><span data-stu-id="eb11a-603">If the expression evaluates to Null, then the IsNull function returns true.</span></span>

<span data-ttu-id="eb11a-604">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-604">**Syntax:**</span></span>  
`bool IsNull(var Expression)`

<span data-ttu-id="eb11a-605">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-605">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-606">Ett null-värde anges för ett attribut av attributet saknas.</span><span class="sxs-lookup"><span data-stu-id="eb11a-606">For an attribute, a Null is expressed by the absence of the attribute.</span></span>

<span data-ttu-id="eb11a-607">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-607">**Example:**</span></span>  
`IsNull([displayName])`  
<span data-ttu-id="eb11a-608">Returnerar True om attributet inte finns i CS eller MV.</span><span class="sxs-lookup"><span data-stu-id="eb11a-608">Returns True if the attribute is not present in the CS or MV.</span></span>

- - -
### <a name="isnullorempty"></a><span data-ttu-id="eb11a-609">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="eb11a-609">IsNullOrEmpty</span></span>
<span data-ttu-id="eb11a-610">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-610">**Description:**</span></span>  
<span data-ttu-id="eb11a-611">Om uttrycket är null eller en tom sträng, returnerar funktionen IsNullOrEmpty SANT.</span><span class="sxs-lookup"><span data-stu-id="eb11a-611">If the expression is null or an empty string, then the IsNullOrEmpty function returns true.</span></span>

<span data-ttu-id="eb11a-612">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-612">**Syntax:**</span></span>  
`bool IsNullOrEmpty(var Expression)`

<span data-ttu-id="eb11a-613">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-613">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-614">För ett attribut, skulle detta utvärderas till True om attributet saknas eller finns men är en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-614">For an attribute, this would evaluate to True if the attribute is absent or is present but is an empty string.</span></span>  
<span data-ttu-id="eb11a-615">Inversen till den här funktionen kallas IsPresent.</span><span class="sxs-lookup"><span data-stu-id="eb11a-615">The inverse of this function is named IsPresent.</span></span>

<span data-ttu-id="eb11a-616">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-616">**Example:**</span></span>  
`IsNullOrEmpty([displayName])`  
<span data-ttu-id="eb11a-617">Returnerar True om attributet finns inte eller är en tom sträng i CS eller MV.</span><span class="sxs-lookup"><span data-stu-id="eb11a-617">Returns True if the attribute is not present or is an empty string in the CS or MV.</span></span>

- - -
### <a name="isnumeric"></a><span data-ttu-id="eb11a-618">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="eb11a-618">IsNumeric</span></span>
<span data-ttu-id="eb11a-619">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-619">**Description:**</span></span>  
<span data-ttu-id="eb11a-620">Funktionen IsNumeric returnerar ett booleskt värde som anger om ett uttryck kan utvärderas som tal av typen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-620">The IsNumeric function returns a Boolean value indicating whether an expression can be evaluated as a number type.</span></span>

<span data-ttu-id="eb11a-621">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-621">**Syntax:**</span></span>  
`bool IsNumeric(var Expression)`

<span data-ttu-id="eb11a-622">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-622">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-623">Används för att avgöra om CNum() kan vara lyckade att parsa uttrycket.</span><span class="sxs-lookup"><span data-stu-id="eb11a-623">Used to determine if CNum() can be successful to parse the expression.</span></span>

- - -
### <a name="isstring"></a><span data-ttu-id="eb11a-624">IsString</span><span class="sxs-lookup"><span data-stu-id="eb11a-624">IsString</span></span>
<span data-ttu-id="eb11a-625">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-625">**Description:**</span></span>  
<span data-ttu-id="eb11a-626">Om uttrycket kan utvärderas till en strängtyp, utvärderar funktionen IsString till True.</span><span class="sxs-lookup"><span data-stu-id="eb11a-626">If the expression can be evaluated to a string type, then the IsString function evaluates to True.</span></span>

<span data-ttu-id="eb11a-627">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-627">**Syntax:**</span></span>  
`bool IsString(var expression)`

<span data-ttu-id="eb11a-628">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-628">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-629">Används för att avgöra om CStr() kan vara lyckade att parsa uttrycket.</span><span class="sxs-lookup"><span data-stu-id="eb11a-629">Used to determine if CStr() can be successful to parse the expression.</span></span>

- - -
### <a name="ispresent"></a><span data-ttu-id="eb11a-630">IsPresent</span><span class="sxs-lookup"><span data-stu-id="eb11a-630">IsPresent</span></span>
<span data-ttu-id="eb11a-631">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-631">**Description:**</span></span>  
<span data-ttu-id="eb11a-632">Om uttrycket utvärderas till en sträng som inte är Null och inte är tom, returnerar funktionen IsPresent SANT.</span><span class="sxs-lookup"><span data-stu-id="eb11a-632">If the expression evaluates to a string that is not Null and is not empty, then the IsPresent function returns true.</span></span>

<span data-ttu-id="eb11a-633">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-633">**Syntax:**</span></span>  
`bool IsPresent(var expression)`

<span data-ttu-id="eb11a-634">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-634">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-635">Inversen till den här funktionen kallas IsNullOrEmpty.</span><span class="sxs-lookup"><span data-stu-id="eb11a-635">The inverse of this function is named IsNullOrEmpty.</span></span>

<span data-ttu-id="eb11a-636">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-636">**Example:**</span></span>  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a><span data-ttu-id="eb11a-637">Objekt</span><span class="sxs-lookup"><span data-stu-id="eb11a-637">Item</span></span>
<span data-ttu-id="eb11a-638">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-638">**Description:**</span></span>  
<span data-ttu-id="eb11a-639">Funktionen Item returnerar ett objekt från ett med flera värden strängattribut.</span><span class="sxs-lookup"><span data-stu-id="eb11a-639">The Item function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="eb11a-640">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-640">**Syntax:**</span></span>  
`var Item(mvstr attribute, num index)`

* <span data-ttu-id="eb11a-641">attributet: flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="eb11a-641">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="eb11a-642">index: index till ett objekt i strängen med flera värden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-642">index: index to an item in the multi-valued string.</span></span>

<span data-ttu-id="eb11a-643">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-643">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-644">Funktionen Item används tillsammans med funktionen innehåller eftersom funktionen senare returnerar index till ett objekt i attributet med flera värden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-644">The Item function is useful together with the Contains function since the latter function returns the index to an item in the multi-valued attribute.</span></span>

<span data-ttu-id="eb11a-645">Genererar ett fel om index är utanför intervallet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-645">Throws an error if index is out of bounds.</span></span>

<span data-ttu-id="eb11a-646">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-646">**Example:**</span></span>  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
<span data-ttu-id="eb11a-647">Returnerar den primära e-postadressen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-647">Returns the primary email address.</span></span>

- - -
### <a name="itemornull"></a><span data-ttu-id="eb11a-648">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="eb11a-648">ItemOrNull</span></span>
<span data-ttu-id="eb11a-649">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-649">**Description:**</span></span>  
<span data-ttu-id="eb11a-650">Funktionen ItemOrNull returnerar ett objekt från ett med flera värden strängattribut.</span><span class="sxs-lookup"><span data-stu-id="eb11a-650">The ItemOrNull function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="eb11a-651">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-651">**Syntax:**</span></span>  
`var ItemOrNull(mvstr attribute, num index)`

* <span data-ttu-id="eb11a-652">attributet: flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="eb11a-652">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="eb11a-653">index: index till ett objekt i strängen med flera värden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-653">index: index to an item in the multi-valued string.</span></span>

<span data-ttu-id="eb11a-654">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-654">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-655">Funktionen ItemOrNull används tillsammans med funktionen innehåller eftersom funktionen senare returnerar index till ett objekt i attributet med flera värden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-655">The ItemOrNull function is useful together with the Contains function since the latter function returns the index to an item in the multi-valued attribute.</span></span>

<span data-ttu-id="eb11a-656">Om index är utanför intervallet, returnerar du värdet Null.</span><span class="sxs-lookup"><span data-stu-id="eb11a-656">If index is out of bounds, then returns a Null value.</span></span>

- - -
### <a name="join"></a><span data-ttu-id="eb11a-657">Slå ihop</span><span class="sxs-lookup"><span data-stu-id="eb11a-657">Join</span></span>
<span data-ttu-id="eb11a-658">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-658">**Description:**</span></span>  
<span data-ttu-id="eb11a-659">Funktionen Anslut till en sträng med flera värden och returnerar en enstaka sträng med angiven avgränsare infogas mellan varje element.</span><span class="sxs-lookup"><span data-stu-id="eb11a-659">The Join function takes a multi-valued string and returns a single-valued string with specified separator inserted between each item.</span></span>

<span data-ttu-id="eb11a-660">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-660">**Syntax:**</span></span>  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* <span data-ttu-id="eb11a-661">attributet: flervärdesattribut som innehåller strängar som ska sammanfogas.</span><span class="sxs-lookup"><span data-stu-id="eb11a-661">attribute: Multi-valued attribute containing strings to be joined.</span></span>
* <span data-ttu-id="eb11a-662">en avgränsare: alla strängar som används för att avgränsa delsträngar i den returnerade strängen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-662">delimiter: Any string, used to separate the substrings in the returned string.</span></span> <span data-ttu-id="eb11a-663">Om det utelämnas används blanksteg (””) används.</span><span class="sxs-lookup"><span data-stu-id="eb11a-663">If omitted, the space character (" ") is used.</span></span> <span data-ttu-id="eb11a-664">Om en avgränsare för en tom sträng (””) eller något, sammanfogas alla objekt i listan med ingen avgränsare.</span><span class="sxs-lookup"><span data-stu-id="eb11a-664">If Delimiter is a zero-length string ("") or Nothing, all items in the list are concatenated with no delimiters.</span></span>

<span data-ttu-id="eb11a-665">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="eb11a-665">**Remarks**</span></span>  
<span data-ttu-id="eb11a-666">Det finns paritet mellan koppling och dela funktioner.</span><span class="sxs-lookup"><span data-stu-id="eb11a-666">There is parity between the Join and Split functions.</span></span> <span data-ttu-id="eb11a-667">Funktionen Anslut till en matris med strängar och slår ihop dem med hjälp av en sträng som avgränsare, för att returnera en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-667">The Join function takes an array of strings and joins them using a delimiter string, to return a single string.</span></span> <span data-ttu-id="eb11a-668">Funktionen Dela tar en sträng och skiljer den på avgränsaren, som returnerar en matris med strängar.</span><span class="sxs-lookup"><span data-stu-id="eb11a-668">The Split function takes a string and separates it at the delimiter, to return an array of strings.</span></span> <span data-ttu-id="eb11a-669">Viktigaste skillnaden är dock att kopplingen kan sammanfoga strängar med valfri avgränsare sträng, dela kan endast separata strängar som använder en avgränsare för en enstaka tecken.</span><span class="sxs-lookup"><span data-stu-id="eb11a-669">However, a key difference is that Join can concatenate strings with any delimiter string, Split can only separate strings using a single character delimiter.</span></span>

<span data-ttu-id="eb11a-670">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-670">**Example:**</span></span>  
`Join([proxyAddresses],",")`  
<span data-ttu-id="eb11a-671">Kan returnera ”:SMTP:john.doe@contoso.com,smtp:jd@contoso.com”</span><span class="sxs-lookup"><span data-stu-id="eb11a-671">Could return: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span></span>

- - -
### <a name="lcase"></a><span data-ttu-id="eb11a-672">LCase</span><span class="sxs-lookup"><span data-stu-id="eb11a-672">LCase</span></span>
<span data-ttu-id="eb11a-673">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-673">**Description:**</span></span>  
<span data-ttu-id="eb11a-674">Funktionen LCase konverterar alla tecken i en textsträng till gemener.</span><span class="sxs-lookup"><span data-stu-id="eb11a-674">The LCase function converts all characters in a string to lower case.</span></span>

<span data-ttu-id="eb11a-675">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-675">**Syntax:**</span></span>  
`str LCase(str value)`

<span data-ttu-id="eb11a-676">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-676">**Example:**</span></span>  
`LCase("TeSt")`  
<span data-ttu-id="eb11a-677">Returnerar ”test”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-677">Returns "test".</span></span>

- - -
### <a name="left"></a><span data-ttu-id="eb11a-678">vänster</span><span class="sxs-lookup"><span data-stu-id="eb11a-678">Left</span></span>
<span data-ttu-id="eb11a-679">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-679">**Description:**</span></span>  
<span data-ttu-id="eb11a-680">Funktionen vänster returnerar ett angivet antal tecken från vänster i en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-680">The Left function returns a specified number of characters from the left of a string.</span></span>

<span data-ttu-id="eb11a-681">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-681">**Syntax:**</span></span>  
`str Left(str string, num NumChars)`

* <span data-ttu-id="eb11a-682">sträng: strängen som ska returnera tecken från</span><span class="sxs-lookup"><span data-stu-id="eb11a-682">string: the string to return characters from</span></span>
* <span data-ttu-id="eb11a-683">NumChars: ett tal som identifierar antalet tecken ska returneras från början (vänster) av sträng</span><span class="sxs-lookup"><span data-stu-id="eb11a-683">NumChars: a number identifying the number of characters to return from the beginning (left) of string</span></span>

<span data-ttu-id="eb11a-684">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-684">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-685">En sträng som innehåller de första numChars tecken i strängen:</span><span class="sxs-lookup"><span data-stu-id="eb11a-685">A string containing the first numChars characters in string:</span></span>

* <span data-ttu-id="eb11a-686">Om numChars = 0, returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-686">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="eb11a-687">Om numChars < 0, returnerar Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-687">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="eb11a-688">Om strängen är null returnera tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-688">If string is null, return empty string.</span></span>

<span data-ttu-id="eb11a-689">Om strängen innehåller färre tecken än antalet angivna i numChars, returneras en sträng som är identisk med sträng (som är, som innehåller alla tecken i parameter 1).</span><span class="sxs-lookup"><span data-stu-id="eb11a-689">If string contains fewer characters than the number specified in numChars, a string identical to string (that is, containing all characters in parameter 1) is returned.</span></span>

<span data-ttu-id="eb11a-690">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-690">**Example:**</span></span>  
`Left("John Doe", 3)`  
<span data-ttu-id="eb11a-691">Returnerar ”Joh”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-691">Returns "Joh".</span></span>

- - -
### <a name="len"></a><span data-ttu-id="eb11a-692">Len</span><span class="sxs-lookup"><span data-stu-id="eb11a-692">Len</span></span>
<span data-ttu-id="eb11a-693">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-693">**Description:**</span></span>  
<span data-ttu-id="eb11a-694">Funktionen längd returnerar antalet tecken i en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-694">The Len function returns number of characters in a string.</span></span>

<span data-ttu-id="eb11a-695">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-695">**Syntax:**</span></span>  
`num Len(str value)`

<span data-ttu-id="eb11a-696">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-696">**Example:**</span></span>  
`Len("John Doe")`  
<span data-ttu-id="eb11a-697">Returnerar 8</span><span class="sxs-lookup"><span data-stu-id="eb11a-697">Returns 8</span></span>

- - -
### <a name="ltrim"></a><span data-ttu-id="eb11a-698">LTrim</span><span class="sxs-lookup"><span data-stu-id="eb11a-698">LTrim</span></span>
<span data-ttu-id="eb11a-699">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-699">**Description:**</span></span>  
<span data-ttu-id="eb11a-700">Funktionen LTrim tar bort inledande blanksteg från en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-700">The LTrim function removes leading white spaces from a string.</span></span>

<span data-ttu-id="eb11a-701">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-701">**Syntax:**</span></span>  
`str LTrim(str value)`

<span data-ttu-id="eb11a-702">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-702">**Example:**</span></span>  
`LTrim(" Test ")`  
<span data-ttu-id="eb11a-703">Returnerar ”Test”</span><span class="sxs-lookup"><span data-stu-id="eb11a-703">Returns "Test "</span></span>

- - -
### <a name="mid"></a><span data-ttu-id="eb11a-704">Mid</span><span class="sxs-lookup"><span data-stu-id="eb11a-704">Mid</span></span>
<span data-ttu-id="eb11a-705">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-705">**Description:**</span></span>  
<span data-ttu-id="eb11a-706">Funktionen Mid returnerar ett angivet antal tecken från en angiven position i en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-706">The Mid function returns a specified number of characters from a specified position in a string.</span></span>

<span data-ttu-id="eb11a-707">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-707">**Syntax:**</span></span>  
`str Mid(str string, num start, num NumChars)`

* <span data-ttu-id="eb11a-708">sträng: strängen som ska returnera tecken från</span><span class="sxs-lookup"><span data-stu-id="eb11a-708">string: the string to return characters from</span></span>
* <span data-ttu-id="eb11a-709">Starta: ett tal som identifierar Start position i strängen att returnera tecken från</span><span class="sxs-lookup"><span data-stu-id="eb11a-709">start: a number identifying the starting position in string to return characters from</span></span>
* <span data-ttu-id="eb11a-710">NumChars: ett tal som identifierar antalet tecken ska returneras från position i strängen</span><span class="sxs-lookup"><span data-stu-id="eb11a-710">NumChars: a number identifying the number of characters to return from position in string</span></span>

<span data-ttu-id="eb11a-711">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-711">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-712">Returnera numChars tecken med början från början position i strängen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-712">Return numChars characters starting from position start in string.</span></span>  
<span data-ttu-id="eb11a-713">En sträng som innehåller numChars tecken från början position i strängen:</span><span class="sxs-lookup"><span data-stu-id="eb11a-713">A string containing numChars characters from position start in string:</span></span>

* <span data-ttu-id="eb11a-714">Om numChars = 0, returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-714">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="eb11a-715">Om numChars < 0, returnerar Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-715">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="eb11a-716">Om start > längden på strängen, returnera Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-716">If start > the length of string, return input string.</span></span>
* <span data-ttu-id="eb11a-717">Om starta < = 0, returnera Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-717">If start <= 0, return input string.</span></span>
* <span data-ttu-id="eb11a-718">Om strängen är null returnera tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-718">If string is null, return empty string.</span></span>

<span data-ttu-id="eb11a-719">Om det inte numChar tecken returneras i strängen position, så många tecken som möjligt.</span><span class="sxs-lookup"><span data-stu-id="eb11a-719">If there are not numChar characters remaining in string from position start, as many characters as possible are returned.</span></span>

<span data-ttu-id="eb11a-720">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-720">**Example:**</span></span>  
`Mid("John Doe", 3, 5)`  
<span data-ttu-id="eb11a-721">Returnerar ”hn gör”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-721">Returns "hn Do".</span></span>

`Mid("John Doe", 6, 999)`  
<span data-ttu-id="eb11a-722">Returnerar ”Berg”</span><span class="sxs-lookup"><span data-stu-id="eb11a-722">Returns "Doe"</span></span>

- - -
### <a name="now"></a><span data-ttu-id="eb11a-723">Nu</span><span class="sxs-lookup"><span data-stu-id="eb11a-723">Now</span></span>
<span data-ttu-id="eb11a-724">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-724">**Description:**</span></span>  
<span data-ttu-id="eb11a-725">Funktionen nu returnerar ett datetime-värde som anger aktuellt datum och tid, enligt systemets datum och klockslag för på datorn.</span><span class="sxs-lookup"><span data-stu-id="eb11a-725">The Now function returns a DateTime specifying the current date and time, according to your computer's system date and time.</span></span>

<span data-ttu-id="eb11a-726">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-726">**Syntax:**</span></span>  
`dt Now()`

- - -
### <a name="numfromdate"></a><span data-ttu-id="eb11a-727">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="eb11a-727">NumFromDate</span></span>
<span data-ttu-id="eb11a-728">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-728">**Description:**</span></span>  
<span data-ttu-id="eb11a-729">Funktionen NumFromDate returnerar ett datum i Annonsens datumformat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-729">The NumFromDate function returns a date in AD’s date format.</span></span>

<span data-ttu-id="eb11a-730">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-730">**Syntax:**</span></span>  
`num NumFromDate(dt value)`

<span data-ttu-id="eb11a-731">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-731">**Example:**</span></span>  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
<span data-ttu-id="eb11a-732">Returnerar 129699324000000000</span><span class="sxs-lookup"><span data-stu-id="eb11a-732">Returns 129699324000000000</span></span>

- - -
### <a name="padleft"></a><span data-ttu-id="eb11a-733">padLeft</span><span class="sxs-lookup"><span data-stu-id="eb11a-733">PadLeft</span></span>
<span data-ttu-id="eb11a-734">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-734">**Description:**</span></span>  
<span data-ttu-id="eb11a-735">De PadLeft funktionen vänster-Pad en sträng till en angiven längd med hjälp av angivna utfyllnadstecknet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-735">The PadLeft function left-pads a string to a specified length using a provided padding character.</span></span>

<span data-ttu-id="eb11a-736">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-736">**Syntax:**</span></span>  
`str PadLeft(str string, num length, str padCharacter)`

* <span data-ttu-id="eb11a-737">sträng: strängen utfyllnad.</span><span class="sxs-lookup"><span data-stu-id="eb11a-737">string: the string to pad.</span></span>
* <span data-ttu-id="eb11a-738">längd: ett heltal som representerar den önskade längden på strängen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-738">length: An integer representing the desired length of string.</span></span>
* <span data-ttu-id="eb11a-739">padCharacter: en sträng som består av ett enskilt tecken som ska användas som pad tecken</span><span class="sxs-lookup"><span data-stu-id="eb11a-739">padCharacter: A string consisting of a single character to use as the pad character</span></span>

<span data-ttu-id="eb11a-740">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-740">**Remarks:**</span></span>

* <span data-ttu-id="eb11a-741">Om längden på strängen är mindre än längden, läggs padCharacter flera gånger i början (vänster) strängens förrän den har en längd lika med längden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-741">If the length of string is less than length, then padCharacter is repeatedly appended to the beginning (left) of string until it has a length equal to length.</span></span>
* <span data-ttu-id="eb11a-742">padCharacter kan vara ett blanksteg, men den kan inte vara ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="eb11a-742">PadCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="eb11a-743">Om längden på strängen är lika med eller större än längden, returneras strängen oförändrat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-743">If the length of string is equal to or greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="eb11a-744">Om strängen har en längd som är större än eller lika med längden, returneras en sträng som är identiska till sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-744">If string has a length greater than or equal to length, a string identical to string is returned.</span></span>
* <span data-ttu-id="eb11a-745">Om längden på strängen är mindre än längden, returnerade en ny sträng med längden som innehåller strängen fylls ut med en padCharacter.</span><span class="sxs-lookup"><span data-stu-id="eb11a-745">If the length of string is less than length, then a new string of the desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="eb11a-746">Om strängen är null returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-746">If string is null, the function returns an empty string.</span></span>

<span data-ttu-id="eb11a-747">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-747">**Example:**</span></span>  
`PadLeft("User", 10, "0")`  
<span data-ttu-id="eb11a-748">Returnerar ”000000User”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-748">Returns "000000User".</span></span>

- - -
### <a name="padright"></a><span data-ttu-id="eb11a-749">PadRight</span><span class="sxs-lookup"><span data-stu-id="eb11a-749">PadRight</span></span>
<span data-ttu-id="eb11a-750">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-750">**Description:**</span></span>  
<span data-ttu-id="eb11a-751">De PadRight funktionen höger-Pad en sträng till en angiven längd med hjälp av angivna utfyllnadstecknet.</span><span class="sxs-lookup"><span data-stu-id="eb11a-751">The PadRight function right-pads a string to a specified length using a provided padding character.</span></span>

<span data-ttu-id="eb11a-752">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-752">**Syntax:**</span></span>  
`str PadRight(str string, num length, str padCharacter)`

* <span data-ttu-id="eb11a-753">sträng: strängen utfyllnad.</span><span class="sxs-lookup"><span data-stu-id="eb11a-753">string: the string to pad.</span></span>
* <span data-ttu-id="eb11a-754">längd: ett heltal som representerar den önskade längden på strängen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-754">length: An integer representing the desired length of string.</span></span>
* <span data-ttu-id="eb11a-755">padCharacter: en sträng som består av ett enskilt tecken som ska användas som pad tecken</span><span class="sxs-lookup"><span data-stu-id="eb11a-755">padCharacter: A string consisting of a single character to use as the pad character</span></span>

<span data-ttu-id="eb11a-756">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-756">**Remarks:**</span></span>

* <span data-ttu-id="eb11a-757">Om längden på strängen är mindre än längden, sedan läggs padCharacter upprepade gånger till (höger) slutet av strängen förrän den har en längd som är lika med längden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-757">If the length of string is less than length, then padCharacter is repeatedly appended to the end (right) of string until it has a length equal to length.</span></span>
* <span data-ttu-id="eb11a-758">padCharacter kan vara ett blanksteg, men den kan inte vara ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="eb11a-758">padCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="eb11a-759">Om längden på strängen är lika med eller större än längden, returneras strängen oförändrat.</span><span class="sxs-lookup"><span data-stu-id="eb11a-759">If the length of string is equal to or greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="eb11a-760">Om strängen har en längd som är större än eller lika med längden, returneras en sträng som är identiska till sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-760">If string has a length greater than or equal to length, a string identical to string is returned.</span></span>
* <span data-ttu-id="eb11a-761">Om längden på strängen är mindre än längden, returnerade en ny sträng med längden som innehåller strängen fylls ut med en padCharacter.</span><span class="sxs-lookup"><span data-stu-id="eb11a-761">If the length of string is less than length, then a new string of the desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="eb11a-762">Om strängen är null returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-762">If string is null, the function returns an empty string.</span></span>

<span data-ttu-id="eb11a-763">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-763">**Example:**</span></span>  
`PadRight("User", 10, "0")`  
<span data-ttu-id="eb11a-764">Returnerar ”User000000”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-764">Returns "User000000".</span></span>

- - -
### <a name="pcase"></a><span data-ttu-id="eb11a-765">PCase</span><span class="sxs-lookup"><span data-stu-id="eb11a-765">PCase</span></span>
<span data-ttu-id="eb11a-766">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-766">**Description:**</span></span>  
<span data-ttu-id="eb11a-767">Funktionen PCase konverterar det första tecknet i varje blankstegsavgränsad ord i en sträng till versaler och alla andra tecken har konverterats till gemener.</span><span class="sxs-lookup"><span data-stu-id="eb11a-767">The PCase function converts the first character of each space delimited word in a string to upper case, and all other characters are converted to lower case.</span></span>

<span data-ttu-id="eb11a-768">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-768">**Syntax:**</span></span>  
`String PCase(string)`

<span data-ttu-id="eb11a-769">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-769">**Remarks:**</span></span>

* <span data-ttu-id="eb11a-770">Den här funktionen ger inte rätt skiftläge om du vill konvertera ett ord som är helt versaler, till exempel en förkortning för närvarande.</span><span class="sxs-lookup"><span data-stu-id="eb11a-770">This function does not currently provide proper casing to convert a word that is entirely uppercase, such as an acronym.</span></span>

<span data-ttu-id="eb11a-771">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-771">**Example:**</span></span>  
`PCase("TEsT")`  
<span data-ttu-id="eb11a-772">Returnerar ”Test”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-772">Returns "Test".</span></span>

`PCase(LCase("TEST"))`  
<span data-ttu-id="eb11a-773">Returnerar ”Test”</span><span class="sxs-lookup"><span data-stu-id="eb11a-773">Returns "Test"</span></span>

- - -
### <a name="randomnum"></a><span data-ttu-id="eb11a-774">RandomNum</span><span class="sxs-lookup"><span data-stu-id="eb11a-774">RandomNum</span></span>
<span data-ttu-id="eb11a-775">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-775">**Description:**</span></span>  
<span data-ttu-id="eb11a-776">Funktionen RandomNum returnerar ett slumptal mellan ett visst intervall.</span><span class="sxs-lookup"><span data-stu-id="eb11a-776">The RandomNum function returns a random number between a specified interval.</span></span>

<span data-ttu-id="eb11a-777">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-777">**Syntax:**</span></span>  
`num RandomNum(num start, num end)`

* <span data-ttu-id="eb11a-778">Starta: ett tal som identifierar den nedre gränsen för slumpmässigt värde att generera</span><span class="sxs-lookup"><span data-stu-id="eb11a-778">start: a number identifying the lower limit of the random value to generate</span></span>
* <span data-ttu-id="eb11a-779">End: ett tal som identifierar den övre gränsen för slumpmässigt värde att generera</span><span class="sxs-lookup"><span data-stu-id="eb11a-779">end: a number identifying the upper limit of the random value to generate</span></span>

<span data-ttu-id="eb11a-780">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-780">**Example:**</span></span>  
`Random(100,999)`  
<span data-ttu-id="eb11a-781">Returnera 734.</span><span class="sxs-lookup"><span data-stu-id="eb11a-781">Can return 734.</span></span>

- - -
### <a name="removeduplicates"></a><span data-ttu-id="eb11a-782">RemoveDuplicates</span><span class="sxs-lookup"><span data-stu-id="eb11a-782">RemoveDuplicates</span></span>
<span data-ttu-id="eb11a-783">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-783">**Description:**</span></span>  
<span data-ttu-id="eb11a-784">Funktionen RemoveDuplicates tar en sträng med flera värden och kontrollera att varje värdet är unikt.</span><span class="sxs-lookup"><span data-stu-id="eb11a-784">The RemoveDuplicates function takes a multi-valued string and make sure each value is unique.</span></span>

<span data-ttu-id="eb11a-785">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-785">**Syntax:**</span></span>  
`mvstr RemoveDuplicates(mvstr attribute)`

<span data-ttu-id="eb11a-786">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-786">**Example:**</span></span>  
`RemoveDuplicates([proxyAddresses])`  
<span data-ttu-id="eb11a-787">Returnerar ett språkoberoende proxyAddress attribut där alla dubblettvärden har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="eb11a-787">Returns a sanitized proxyAddress attribute where all duplicate values have been removed.</span></span>

- - -
### <a name="replace"></a><span data-ttu-id="eb11a-788">Ersätt</span><span class="sxs-lookup"><span data-stu-id="eb11a-788">Replace</span></span>
<span data-ttu-id="eb11a-789">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-789">**Description:**</span></span>  
<span data-ttu-id="eb11a-790">Funktionen Ersätt ersätter alla förekomster av en textsträng med en annan sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-790">The Replace function replaces all occurrences of a string to another string.</span></span>

<span data-ttu-id="eb11a-791">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-791">**Syntax:**</span></span>  
`str Replace(str string, str OldValue, str NewValue)`

* <span data-ttu-id="eb11a-792">sträng: Ersätt värden i en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-792">string: A string to replace values in.</span></span>
* <span data-ttu-id="eb11a-793">OldValue: En sträng att söka efter och ersätta.</span><span class="sxs-lookup"><span data-stu-id="eb11a-793">OldValue: The string to search for and to replace.</span></span>
* <span data-ttu-id="eb11a-794">NewValue: Strängen som ska ersätta.</span><span class="sxs-lookup"><span data-stu-id="eb11a-794">NewValue: The string to replace to.</span></span>

<span data-ttu-id="eb11a-795">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-795">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-796">Funktionen godtar följande särskilda monikrar:</span><span class="sxs-lookup"><span data-stu-id="eb11a-796">The function recognizes the following special monikers:</span></span>

* <span data-ttu-id="eb11a-797">\n – ny rad</span><span class="sxs-lookup"><span data-stu-id="eb11a-797">\n – New Line</span></span>
* <span data-ttu-id="eb11a-798">\r – vagnretur</span><span class="sxs-lookup"><span data-stu-id="eb11a-798">\r – Carriage Return</span></span>
* <span data-ttu-id="eb11a-799">\t – fliken</span><span class="sxs-lookup"><span data-stu-id="eb11a-799">\t – Tab</span></span>

<span data-ttu-id="eb11a-800">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-800">**Example:**</span></span>  
`Replace([address],"\r\n",", ")`  
<span data-ttu-id="eb11a-801">Ersätter CRLF med ett kommatecken och blanksteg och kan leda till ”en Microsoft sätt, Redmond, WA, USA”</span><span class="sxs-lookup"><span data-stu-id="eb11a-801">Replaces CRLF with a comma and space, and could lead to "One Microsoft Way, Redmond, WA, USA"</span></span>

- - -
### <a name="replacechars"></a><span data-ttu-id="eb11a-802">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="eb11a-802">ReplaceChars</span></span>
<span data-ttu-id="eb11a-803">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-803">**Description:**</span></span>  
<span data-ttu-id="eb11a-804">Funktionen ReplaceChars ersätter alla förekomster av tecken hittades i ReplacePattern-sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-804">The ReplaceChars function replaces all occurrences of characters found in the ReplacePattern string.</span></span>

<span data-ttu-id="eb11a-805">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-805">**Syntax:**</span></span>  
`str ReplaceChars(str string, str ReplacePattern)`

* <span data-ttu-id="eb11a-806">sträng: en sträng som ska ersätta tecken i.</span><span class="sxs-lookup"><span data-stu-id="eb11a-806">string: A string to replace characters in.</span></span>
* <span data-ttu-id="eb11a-807">ReplacePattern: en sträng som innehåller en ordlista med tecken som ska ersätta.</span><span class="sxs-lookup"><span data-stu-id="eb11a-807">ReplacePattern: a string containing a dictionary with characters to replace.</span></span>

<span data-ttu-id="eb11a-808">Formatet är {källa1}: {target1}, {källa2}: {target2}, {källan}, {targetN} där källan är tecknet för att hitta och rikta den sträng som ska ersättas med.</span><span class="sxs-lookup"><span data-stu-id="eb11a-808">The format is {source1}:{target1},{source2}:{target2},{sourceN},{targetN} where source is the character to find and target the string to replace with.</span></span>

<span data-ttu-id="eb11a-809">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-809">**Remarks:**</span></span>

* <span data-ttu-id="eb11a-810">Funktionen tar varje förekomst av definierade källor och ersätter dem med mål.</span><span class="sxs-lookup"><span data-stu-id="eb11a-810">The function takes each occurrence of defined sources and replaces them with the targets.</span></span>
* <span data-ttu-id="eb11a-811">Källan måste vara exakt ett tecken för (unicode).</span><span class="sxs-lookup"><span data-stu-id="eb11a-811">The source must be exactly one (unicode) character.</span></span>
* <span data-ttu-id="eb11a-812">Källan kan inte vara tomt eller längre än ett tecken (tolkningsfel).</span><span class="sxs-lookup"><span data-stu-id="eb11a-812">The source cannot be empty or longer than one character (parsing error).</span></span>
* <span data-ttu-id="eb11a-813">Målet kan ha flera tecken, till exempel ö:oe, β:ss.</span><span class="sxs-lookup"><span data-stu-id="eb11a-813">The target can have multiple characters, for example ö:oe, β:ss.</span></span>
* <span data-ttu-id="eb11a-814">Målet kan vara tom som anger tecknet som ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="eb11a-814">The target can be empty indicating that the character should be removed.</span></span>
* <span data-ttu-id="eb11a-815">Källan är skiftlägeskänsligt och måste vara en exakt matchning.</span><span class="sxs-lookup"><span data-stu-id="eb11a-815">The source is case-sensitive and must be an exact match.</span></span>
* <span data-ttu-id="eb11a-816">I (semikolonavgränsad) och: (kolon) är reserverade tecken som inte kan ersättas med den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-816">The , (comma) and : (colon) are reserved characters and cannot be replaced using this function.</span></span>
* <span data-ttu-id="eb11a-817">Blanksteg och andra vit tecken i strängen ReplacePattern ignoreras.</span><span class="sxs-lookup"><span data-stu-id="eb11a-817">Spaces and other white characters in the ReplacePattern string are ignored.</span></span>

<span data-ttu-id="eb11a-818">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-818">**Example:**</span></span>  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
<span data-ttu-id="eb11a-819">Returnerar Raksmorgas</span><span class="sxs-lookup"><span data-stu-id="eb11a-819">Returns Raksmorgas</span></span>

`ReplaceChars("O’Neil",%ReplaceString%)`  
<span data-ttu-id="eb11a-820">Returnerar ”ONeil”, enskild skalstreck definieras till att tas bort.</span><span class="sxs-lookup"><span data-stu-id="eb11a-820">Returns "ONeil", the single tick is defined to be removed.</span></span>

- - -
### <a name="right"></a><span data-ttu-id="eb11a-821">Höger</span><span class="sxs-lookup"><span data-stu-id="eb11a-821">Right</span></span>
<span data-ttu-id="eb11a-822">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-822">**Description:**</span></span>  
<span data-ttu-id="eb11a-823">Funktionen höger returnerar ett angivet antal tecken från höger (end) i en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-823">The Right function returns a specified number of characters from the right (end) of a string.</span></span>

<span data-ttu-id="eb11a-824">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-824">**Syntax:**</span></span>  
`str Right(str string, num NumChars)`

* <span data-ttu-id="eb11a-825">sträng: strängen som ska returnera tecken från</span><span class="sxs-lookup"><span data-stu-id="eb11a-825">string: the string to return characters from</span></span>
* <span data-ttu-id="eb11a-826">NumChars: ett tal som identifierar antalet tecken ska returneras från (höger) slutet av strängen</span><span class="sxs-lookup"><span data-stu-id="eb11a-826">NumChars: a number identifying the number of characters to return from the end (right) of string</span></span>

<span data-ttu-id="eb11a-827">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-827">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-828">NumChars tecken som ska returneras från den senaste positionen i strängen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-828">NumChars characters are returned from the last position of string.</span></span>

<span data-ttu-id="eb11a-829">En sträng som innehåller de senaste numChars tecken i strängen:</span><span class="sxs-lookup"><span data-stu-id="eb11a-829">A string containing the last numChars characters in string:</span></span>

* <span data-ttu-id="eb11a-830">Om numChars = 0, returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-830">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="eb11a-831">Om numChars < 0, returnerar Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="eb11a-831">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="eb11a-832">Om strängen är null returnera tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-832">If string is null, return empty string.</span></span>

<span data-ttu-id="eb11a-833">Om strängen innehåller färre tecken än antalet angivna i NumChars, returneras en sträng som är identiska till sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-833">If string contains fewer characters than the number specified in NumChars, a string identical to string is returned.</span></span>

<span data-ttu-id="eb11a-834">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-834">**Example:**</span></span>  
`Right("John Doe", 3)`  
<span data-ttu-id="eb11a-835">Returnerar ”Berg”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-835">Returns "Doe".</span></span>

- - -
### <a name="rtrim"></a><span data-ttu-id="eb11a-836">RTrim</span><span class="sxs-lookup"><span data-stu-id="eb11a-836">RTrim</span></span>
<span data-ttu-id="eb11a-837">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-837">**Description:**</span></span>  
<span data-ttu-id="eb11a-838">RTrim funktionen tar bort avslutande blanksteg från en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-838">The RTrim function removes trailing white spaces from a string.</span></span>

<span data-ttu-id="eb11a-839">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-839">**Syntax:**</span></span>  
`str RTrim(str value)`

<span data-ttu-id="eb11a-840">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-840">**Example:**</span></span>  
`RTrim(" Test ")`  
<span data-ttu-id="eb11a-841">Returnerar ”Test”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-841">Returns " Test".</span></span>

- - -
### <a name="select"></a><span data-ttu-id="eb11a-842">Välj</span><span class="sxs-lookup"><span data-stu-id="eb11a-842">Select</span></span>
<span data-ttu-id="eb11a-843">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-843">**Description:**</span></span>  
<span data-ttu-id="eb11a-844">Processen för alla värden i en flervärdesfält attribut (eller utdata för ett uttryck) baserat på funktionen som anges.</span><span class="sxs-lookup"><span data-stu-id="eb11a-844">Process all values in a multi-valued attribute (or output of an expression) based on function specified.</span></span>

<span data-ttu-id="eb11a-845">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-845">**Syntax:**</span></span>  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* <span data-ttu-id="eb11a-846">objektet: representerar ett element i attributet med flera värden</span><span class="sxs-lookup"><span data-stu-id="eb11a-846">item: Represents an element in the multi-valued attribute</span></span>
* <span data-ttu-id="eb11a-847">attributet: attributet med flera värden</span><span class="sxs-lookup"><span data-stu-id="eb11a-847">attribute: the multi-valued attribute</span></span>
* <span data-ttu-id="eb11a-848">uttryck: ett uttryck som returnerar en mängd med värden</span><span class="sxs-lookup"><span data-stu-id="eb11a-848">expression: an expression that returns a collection of values</span></span>
* <span data-ttu-id="eb11a-849">villkor: en funktion som kan bearbeta ett objekt i attributet</span><span class="sxs-lookup"><span data-stu-id="eb11a-849">condition: any function that can process an item in the attribute</span></span>

<span data-ttu-id="eb11a-850">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-850">**Examples:**</span></span>  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
<span data-ttu-id="eb11a-851">Returnera alla värden i flervärdesattribut Annantelefon när bindestreck (-) har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="eb11a-851">Return all the values in the multi-valued attribute otherPhone after hyphens (-) have been removed.</span></span>

- - -
### <a name="split"></a><span data-ttu-id="eb11a-852">Dela</span><span class="sxs-lookup"><span data-stu-id="eb11a-852">Split</span></span>
<span data-ttu-id="eb11a-853">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-853">**Description:**</span></span>  
<span data-ttu-id="eb11a-854">Funktionen Dela en sträng som avgränsas med en avgränsare och det är en sträng med flera värden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-854">The Split function takes a string separated with a delimiter and makes it a multi-valued string.</span></span>

<span data-ttu-id="eb11a-855">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-855">**Syntax:**</span></span>  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* <span data-ttu-id="eb11a-856">värde: sträng med ett avgränsningstecken för att avgränsa.</span><span class="sxs-lookup"><span data-stu-id="eb11a-856">value: the string with a delimiter character to separate.</span></span>
* <span data-ttu-id="eb11a-857">en avgränsare: valfritt tecken som ska användas som avgränsare.</span><span class="sxs-lookup"><span data-stu-id="eb11a-857">delimiter: single character to be used as the delimiter.</span></span>
* <span data-ttu-id="eb11a-858">gränsen: maximala antalet värden som kan returnera.</span><span class="sxs-lookup"><span data-stu-id="eb11a-858">limit: maximum number of values that can return.</span></span>

<span data-ttu-id="eb11a-859">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-859">**Example:**</span></span>  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
<span data-ttu-id="eb11a-860">Returnerar en sträng som har flera värden med 2 element användbart för attributet proxyAddress.</span><span class="sxs-lookup"><span data-stu-id="eb11a-860">Returns a multi-valued string with 2 elements useful for the proxyAddress attribute.</span></span>

- - -
### <a name="stringfromguid"></a><span data-ttu-id="eb11a-861">GUIDFromString</span><span class="sxs-lookup"><span data-stu-id="eb11a-861">StringFromGuid</span></span>
<span data-ttu-id="eb11a-862">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-862">**Description:**</span></span>  
<span data-ttu-id="eb11a-863">Funktionen GUIDFromString tar en binär GUID och konverterar den till en sträng</span><span class="sxs-lookup"><span data-stu-id="eb11a-863">The StringFromGuid function takes a binary GUID and converts it to a string</span></span>

<span data-ttu-id="eb11a-864">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-864">**Syntax:**</span></span>  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a><span data-ttu-id="eb11a-865">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="eb11a-865">StringFromSid</span></span>
<span data-ttu-id="eb11a-866">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-866">**Description:**</span></span>  
<span data-ttu-id="eb11a-867">Funktionen StringFromSid konverterar en bytematris som innehåller en säkerhetsidentifierare till en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-867">The StringFromSid function converts a byte array containing a security identifier to a string.</span></span>

<span data-ttu-id="eb11a-868">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-868">**Syntax:**</span></span>  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a><span data-ttu-id="eb11a-869">Växel</span><span class="sxs-lookup"><span data-stu-id="eb11a-869">Switch</span></span>
<span data-ttu-id="eb11a-870">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-870">**Description:**</span></span>  
<span data-ttu-id="eb11a-871">Växeln-funktionen används för att returnera ett enstaka värde baserat på utvärderade villkoren.</span><span class="sxs-lookup"><span data-stu-id="eb11a-871">The Switch function is used to return a single value based on evaluated conditions.</span></span>

<span data-ttu-id="eb11a-872">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-872">**Syntax:**</span></span>  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* <span data-ttu-id="eb11a-873">uttryck: Variant-uttryck som du vill utvärdera.</span><span class="sxs-lookup"><span data-stu-id="eb11a-873">expr: Variant expression you want to evaluate.</span></span>
* <span data-ttu-id="eb11a-874">värde: värde som ska returneras om motsvarande uttryck är True.</span><span class="sxs-lookup"><span data-stu-id="eb11a-874">value: Value to be returned if the corresponding expression is True.</span></span>

<span data-ttu-id="eb11a-875">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-875">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-876">Växeln-funktionen argumentlistan består av par med uttryck och värden.</span><span class="sxs-lookup"><span data-stu-id="eb11a-876">The Switch function argument list consists of pairs of expressions and values.</span></span> <span data-ttu-id="eb11a-877">Uttryck som utvärderas från vänster till höger och returneras värdet som associeras med det första uttrycket ska utvärderas till SANT.</span><span class="sxs-lookup"><span data-stu-id="eb11a-877">The expressions are evaluated from left to right, and the value associated with the first expression to evaluate to True is returned.</span></span> <span data-ttu-id="eb11a-878">Om delarna inte är korrekt paras ihop, inträffar ett fel under körning.</span><span class="sxs-lookup"><span data-stu-id="eb11a-878">If the parts aren't properly paired, a run-time error occurs.</span></span>

<span data-ttu-id="eb11a-879">Om Uttr1 är SANT returnerar växeln value1.</span><span class="sxs-lookup"><span data-stu-id="eb11a-879">For example, if expr1 is True, Switch returns value1.</span></span> <span data-ttu-id="eb11a-880">Om uttryck-1 är False, men uttryck-2 är sant, returnerar växeln värdet 2 och så vidare.</span><span class="sxs-lookup"><span data-stu-id="eb11a-880">If expr-1 is False, but expr-2 is True, Switch returns value-2, and so on.</span></span>

<span data-ttu-id="eb11a-881">Växeln returnerar en ingenting om:</span><span class="sxs-lookup"><span data-stu-id="eb11a-881">Switch returns a Nothing if:</span></span>

* <span data-ttu-id="eb11a-882">Ingen av uttrycken är sant.</span><span class="sxs-lookup"><span data-stu-id="eb11a-882">None of the expressions are True.</span></span>
* <span data-ttu-id="eb11a-883">Det första värdet är SANT uttrycket har ett motsvarande värde som är Null.</span><span class="sxs-lookup"><span data-stu-id="eb11a-883">The first True expression has a corresponding value that is Null.</span></span>

<span data-ttu-id="eb11a-884">Växeln utvärderar alla uttryck, även om den returnerar endast en av dem.</span><span class="sxs-lookup"><span data-stu-id="eb11a-884">Switch evaluates all expressions, even though it returns only one of them.</span></span> <span data-ttu-id="eb11a-885">Därför bör du titta på för oönskade sidoeffekter.</span><span class="sxs-lookup"><span data-stu-id="eb11a-885">For this reason, you should watch for undesirable side effects.</span></span> <span data-ttu-id="eb11a-886">Utvärderingen av ett uttryck som resulterar i en division med noll-fel, inträffar ett fel.</span><span class="sxs-lookup"><span data-stu-id="eb11a-886">For example, if the evaluation of any expression results in a division by zero error, an error occurs.</span></span>

<span data-ttu-id="eb11a-887">Värdet kan också vara funktionen fel som returnerar en anpassad sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-887">Value can also be the Error function, which would return a custom string.</span></span>

<span data-ttu-id="eb11a-888">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-888">**Example:**</span></span>  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
<span data-ttu-id="eb11a-889">Returnerar det språk som talas i vissa större städer, annars returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="eb11a-889">Returns the language spoken in some major cities, otherwise returns an Error.</span></span>

- - -
### <a name="trim"></a><span data-ttu-id="eb11a-890">Rensa</span><span class="sxs-lookup"><span data-stu-id="eb11a-890">Trim</span></span>
<span data-ttu-id="eb11a-891">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-891">**Description:**</span></span>  
<span data-ttu-id="eb11a-892">Funktionen Rensa tar bort inledande och avslutande blanksteg från en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-892">The Trim function removes leading and trailing white spaces from a string.</span></span>

<span data-ttu-id="eb11a-893">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-893">**Syntax:**</span></span>  
`str Trim(str value)`  

<span data-ttu-id="eb11a-894">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-894">**Example:**</span></span>  
`Trim(" Test ")`  
<span data-ttu-id="eb11a-895">Returnerar ”Test”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-895">Returns "Test".</span></span>

`Trim([proxyAddresses])`  
<span data-ttu-id="eb11a-896">Tar bort inledande och avslutande blanksteg för varje värde i attributet proxyAddress.</span><span class="sxs-lookup"><span data-stu-id="eb11a-896">Removes leading and trailing spaces for each value in the proxyAddress attribute.</span></span>

- - -
### <a name="ucase"></a><span data-ttu-id="eb11a-897">UCase</span><span class="sxs-lookup"><span data-stu-id="eb11a-897">UCase</span></span>
<span data-ttu-id="eb11a-898">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-898">**Description:**</span></span>  
<span data-ttu-id="eb11a-899">Funktionen UCase konverterar alla tecken i en textsträng till versaler.</span><span class="sxs-lookup"><span data-stu-id="eb11a-899">The UCase function converts all characters in a string to upper case.</span></span>

<span data-ttu-id="eb11a-900">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-900">**Syntax:**</span></span>  
`str UCase(str string)`

<span data-ttu-id="eb11a-901">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-901">**Example:**</span></span>  
`UCase("TeSt")`  
<span data-ttu-id="eb11a-902">Returnerar ”TEST”.</span><span class="sxs-lookup"><span data-stu-id="eb11a-902">Returns "TEST".</span></span>

- - -
### <a name="where"></a><span data-ttu-id="eb11a-903">där</span><span class="sxs-lookup"><span data-stu-id="eb11a-903">Where</span></span>

<span data-ttu-id="eb11a-904">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-904">**Description:**</span></span>  
<span data-ttu-id="eb11a-905">Returnerar en delmängd av värden från ett med flera värden attribut (eller utdata för ett uttryck) som baserat på specifika villkor.</span><span class="sxs-lookup"><span data-stu-id="eb11a-905">Returns a subset of values from a multi-valued attribute (or output of an expression) based on specific condition.</span></span>

<span data-ttu-id="eb11a-906">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-906">**Syntax:**</span></span>  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* <span data-ttu-id="eb11a-907">objektet: representerar ett element i attributet med flera värden</span><span class="sxs-lookup"><span data-stu-id="eb11a-907">item: Represents an element in the multi-valued attribute</span></span>
* <span data-ttu-id="eb11a-908">attributet: attributet med flera värden</span><span class="sxs-lookup"><span data-stu-id="eb11a-908">attribute: the multi-valued attribute</span></span>
* <span data-ttu-id="eb11a-909">villkor: ett uttryck som kan utvärderas till true eller false</span><span class="sxs-lookup"><span data-stu-id="eb11a-909">condition: any expression that can be evaluated to true or false</span></span>
* <span data-ttu-id="eb11a-910">uttryck: ett uttryck som returnerar en mängd med värden</span><span class="sxs-lookup"><span data-stu-id="eb11a-910">expression: an expression that returns a collection of values</span></span>

<span data-ttu-id="eb11a-911">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-911">**Example:**</span></span>  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
<span data-ttu-id="eb11a-912">Returvärden certifikatet i den flervärdesattribut userCertificate som inte har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="eb11a-912">Return the certificate values in the multi-valued attribute userCertificate which aren’t expired.</span></span>

- - -
### <a name="with"></a><span data-ttu-id="eb11a-913">med</span><span class="sxs-lookup"><span data-stu-id="eb11a-913">With</span></span>
<span data-ttu-id="eb11a-914">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-914">**Description:**</span></span>  
<span data-ttu-id="eb11a-915">Med funktionen är ett sätt att förenkla ett komplext uttryck med hjälp av en variabel som representerar ett deluttryck som visas en eller flera gånger i ett komplext uttryck.</span><span class="sxs-lookup"><span data-stu-id="eb11a-915">The With function provides a way to simplify a complex expression by using a variable to represent a subexpression which appears one or more times in the complex expression.</span></span>

<span data-ttu-id="eb11a-916">**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`</span><span class="sxs-lookup"><span data-stu-id="eb11a-916">**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`</span></span>  
* <span data-ttu-id="eb11a-917">variabel: representerar underuttryck.</span><span class="sxs-lookup"><span data-stu-id="eb11a-917">variable: Represents the subexpression.</span></span>
* <span data-ttu-id="eb11a-918">underuttryck: deluttryck som representeras av variabeln.</span><span class="sxs-lookup"><span data-stu-id="eb11a-918">subExpression: subexpression represented by variable.</span></span>
* <span data-ttu-id="eb11a-919">complexExpression: ett komplext uttryck.</span><span class="sxs-lookup"><span data-stu-id="eb11a-919">complexExpression: A complex expression.</span></span>

<span data-ttu-id="eb11a-920">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-920">**Example:**</span></span>  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
<span data-ttu-id="eb11a-921">Har samma funktioner som:</span><span class="sxs-lookup"><span data-stu-id="eb11a-921">Is functionally equivalent to:</span></span>  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
<span data-ttu-id="eb11a-922">Som returnerar endast återstående certifikat värden i attributet userCertificate.</span><span class="sxs-lookup"><span data-stu-id="eb11a-922">Which returns only unexpired certificate values in the userCertificate attribute.</span></span>


- - -
### <a name="word"></a><span data-ttu-id="eb11a-923">Word</span><span class="sxs-lookup"><span data-stu-id="eb11a-923">Word</span></span>
<span data-ttu-id="eb11a-924">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-924">**Description:**</span></span>  
<span data-ttu-id="eb11a-925">Funktionen Word returnerar ett ord som ingår i en sträng, baserat på parametrarna som beskriver avgränsare ska användas och word-nummer för att returnera.</span><span class="sxs-lookup"><span data-stu-id="eb11a-925">The Word function returns a word contained within a string, based on parameters describing the delimiters to use and the word number to return.</span></span>

<span data-ttu-id="eb11a-926">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-926">**Syntax:**</span></span>  
`str Word(str string, num WordNumber, str delimiters)`

* <span data-ttu-id="eb11a-927">sträng: strängen som ska returnera ett ord från.</span><span class="sxs-lookup"><span data-stu-id="eb11a-927">string: the string to return a word from.</span></span>
* <span data-ttu-id="eb11a-928">WordNumber: ett tal som identifierar vilka word-numret ska returnera.</span><span class="sxs-lookup"><span data-stu-id="eb11a-928">WordNumber: a number identifying which word number should return.</span></span>
* <span data-ttu-id="eb11a-929">avgränsare: en sträng som representerar delimiter(s) som ska användas för att identifiera ord</span><span class="sxs-lookup"><span data-stu-id="eb11a-929">delimiters: a string representing the delimiter(s) that should be used to identify words</span></span>

<span data-ttu-id="eb11a-930">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-930">**Remarks:**</span></span>  
<span data-ttu-id="eb11a-931">Varje sträng med tecken i strängen avgränsade med något av tecknen i avgränsare identifieras som ord:</span><span class="sxs-lookup"><span data-stu-id="eb11a-931">Each string of characters in string separated by the one of the characters in delimiters are identified as words:</span></span>

* <span data-ttu-id="eb11a-932">Om number < 1, returnerar tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-932">If number < 1, returns empty string.</span></span>
* <span data-ttu-id="eb11a-933">Om strängen är null returnerar tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-933">If string is null, returns empty string.</span></span>

<span data-ttu-id="eb11a-934">Om strängen innehåller mindre än antalet ord eller strängen innehåller inte några ord som identifieras av avgränsare, returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="eb11a-934">If string contains less than number words, or string does not contain any words identified by delimiters, an empty string is returned.</span></span>

<span data-ttu-id="eb11a-935">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="eb11a-935">**Example:**</span></span>  
`Word("The quick brown fox",3," ")`  
<span data-ttu-id="eb11a-936">Returnerar ”Jansson”</span><span class="sxs-lookup"><span data-stu-id="eb11a-936">Returns "brown"</span></span>

`Word("This,string!has&many separators",3,",!&#")`  
<span data-ttu-id="eb11a-937">Returnerar ”har”</span><span class="sxs-lookup"><span data-stu-id="eb11a-937">Would return "has"</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eb11a-938">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="eb11a-938">Additional Resources</span></span>
* [<span data-ttu-id="eb11a-939">Förstå uttryck för deklarativ etablering</span><span class="sxs-lookup"><span data-stu-id="eb11a-939">Understanding Declarative Provisioning Expressions</span></span>](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [<span data-ttu-id="eb11a-940">Azure AD Connect Sync: Anpassa synkroniseringsalternativ</span><span class="sxs-lookup"><span data-stu-id="eb11a-940">Azure AD Connect Sync: Customizing Synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="eb11a-941">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eb11a-941">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
