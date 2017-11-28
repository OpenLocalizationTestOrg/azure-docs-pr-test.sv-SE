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
ms.openlocfilehash: fbe0df856ca2efda965650fb85c7e831a0be32c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a><span data-ttu-id="3f4f9-103">Azure AD Connect-synkronisering: funktioner referens</span><span class="sxs-lookup"><span data-stu-id="3f4f9-103">Azure AD Connect sync: Functions Reference</span></span>
<span data-ttu-id="3f4f9-104">I Azure AD Connect är funktioner används toomanipulate ett attributvärde under synkroniseringen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-104">In Azure AD Connect, functions are used toomanipulate an attribute value during synchronization.</span></span>  
<span data-ttu-id="3f4f9-105">Hej för hello uttrycks med hello följande format:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-105">hello Syntax of hello functions is expressed using hello following format:</span></span>  
`<output type> FunctionName(<input type> <position name>, ..)`

<span data-ttu-id="3f4f9-106">Om en funktion är överbelastad och accepterar flera syntax, visas alla giltig syntax.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-106">If a function is overloaded and accepts multiple syntaxes, all valid syntaxes are listed.</span></span>  
<span data-ttu-id="3f4f9-107">hello funktioner är starkt typbestämd och de bekräftar att hello typ som skickades matchar hello dokumenterade typen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-107">hello functions are strongly typed and they verify that hello type passed in matches hello documented type.</span></span>  
<span data-ttu-id="3f4f9-108">Ett fel returneras om hello typ inte matchar.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-108">If hello type does not match, an error is thrown.</span></span>

<span data-ttu-id="3f4f9-109">hello typer uttrycks med hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-109">hello types are expressed with hello following syntax:</span></span>

* <span data-ttu-id="3f4f9-110">**bin** – binära</span><span class="sxs-lookup"><span data-stu-id="3f4f9-110">**bin** – Binary</span></span>
* <span data-ttu-id="3f4f9-111">**bool** – booleskt</span><span class="sxs-lookup"><span data-stu-id="3f4f9-111">**bool** – Boolean</span></span>
* <span data-ttu-id="3f4f9-112">**DT** – UTC-datum/tid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-112">**dt** – UTC Date/Time</span></span>
* <span data-ttu-id="3f4f9-113">**uppräkningen** – uppräkning av kända konstanter</span><span class="sxs-lookup"><span data-stu-id="3f4f9-113">**enum** – Enumeration of known constants</span></span>
* <span data-ttu-id="3f4f9-114">**EXP** – uttryck, vilket är förväntat tooevaluate tooa booleskt värde</span><span class="sxs-lookup"><span data-stu-id="3f4f9-114">**exp** – Expression, which is expected tooevaluate tooa Boolean</span></span>
* <span data-ttu-id="3f4f9-115">**mvbin** – multivärdes binära</span><span class="sxs-lookup"><span data-stu-id="3f4f9-115">**mvbin** – Multi-Valued Binary</span></span>
* <span data-ttu-id="3f4f9-116">**mvstr** – multivärdes sträng</span><span class="sxs-lookup"><span data-stu-id="3f4f9-116">**mvstr** – Multi-Valued String</span></span>
* <span data-ttu-id="3f4f9-117">**mvref** – multivärdes-referens</span><span class="sxs-lookup"><span data-stu-id="3f4f9-117">**mvref** – Multi-Valued Reference</span></span>
* <span data-ttu-id="3f4f9-118">**NUM** – numeriskt</span><span class="sxs-lookup"><span data-stu-id="3f4f9-118">**num** – Numeric</span></span>
* <span data-ttu-id="3f4f9-119">**REF** – referens</span><span class="sxs-lookup"><span data-stu-id="3f4f9-119">**ref** – Reference</span></span>
* <span data-ttu-id="3f4f9-120">**str** – sträng</span><span class="sxs-lookup"><span data-stu-id="3f4f9-120">**str** – String</span></span>
* <span data-ttu-id="3f4f9-121">**var** – en variant av (nästan) en annan typ</span><span class="sxs-lookup"><span data-stu-id="3f4f9-121">**var** – A variant of (almost) any other type</span></span>
* <span data-ttu-id="3f4f9-122">**void** – returnerar inte ett värde</span><span class="sxs-lookup"><span data-stu-id="3f4f9-122">**void** – doesn’t return a value</span></span>

<span data-ttu-id="3f4f9-123">Hej funktioner med hello typer **mvbin**, **mvstr**, och **mvref** fungerar bara på flera värden attribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-123">hello functions with hello types **mvbin**, **mvstr**, and **mvref** can only work on multi-valued attributes.</span></span> <span data-ttu-id="3f4f9-124">Fungerar med **bin**, **str**, och **ref** fungerar på både enstaka och flera värden.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-124">Functions with **bin**, **str**, and **ref** work on both single-valued and multi-valued attributes.</span></span>

## <a name="functions-reference"></a><span data-ttu-id="3f4f9-125">Referens för funktioner</span><span class="sxs-lookup"><span data-stu-id="3f4f9-125">Functions Reference</span></span>
| <span data-ttu-id="3f4f9-126">Lista över funktioner</span><span class="sxs-lookup"><span data-stu-id="3f4f9-126">List of functions</span></span> |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="3f4f9-127">**Certifikat**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-127">**Certificate**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-128">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="3f4f9-128">CertExtensionOids</span></span>](#certextensionoids) |[<span data-ttu-id="3f4f9-129">CertFormat</span><span class="sxs-lookup"><span data-stu-id="3f4f9-129">CertFormat</span></span>](#certformat) |[<span data-ttu-id="3f4f9-130">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="3f4f9-130">CertFriendlyName</span></span>](#certfriendlyname) |[<span data-ttu-id="3f4f9-131">CertHashString</span><span class="sxs-lookup"><span data-stu-id="3f4f9-131">CertHashString</span></span>](#certhashstring) | |
| [<span data-ttu-id="3f4f9-132">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="3f4f9-132">CertIssuer</span></span>](#certissuer) |[<span data-ttu-id="3f4f9-133">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="3f4f9-133">CertIssuerDN</span></span>](#certissuerdn) |[<span data-ttu-id="3f4f9-134">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-134">CertIssuerOid</span></span>](#certissueroid) |[<span data-ttu-id="3f4f9-135">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="3f4f9-135">CertKeyAlgorithm</span></span>](#certkeyalgorithm) | |
| [<span data-ttu-id="3f4f9-136">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="3f4f9-136">CertKeyAlgorithmParams</span></span>](#certkeyalgorithmparams) |[<span data-ttu-id="3f4f9-137">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="3f4f9-137">CertNameInfo</span></span>](#certnameinfo) |[<span data-ttu-id="3f4f9-138">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="3f4f9-138">CertNotAfter</span></span>](#certnotafter) |[<span data-ttu-id="3f4f9-139">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="3f4f9-139">CertNotBefore</span></span>](#certnotbefore) | |
| [<span data-ttu-id="3f4f9-140">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-140">CertPublicKeyOid</span></span>](#certpublickeyoid) |[<span data-ttu-id="3f4f9-141">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-141">CertPublicKeyParametersOid</span></span>](#certpublickeyparametersoid) |[<span data-ttu-id="3f4f9-142">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="3f4f9-142">CertSerialNumber</span></span>](#certserialnumber) |[<span data-ttu-id="3f4f9-143">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-143">CertSignatureAlgorithmOid</span></span>](#certsignaturealgorithmoid) | |
| [<span data-ttu-id="3f4f9-144">CertSubject</span><span class="sxs-lookup"><span data-stu-id="3f4f9-144">CertSubject</span></span>](#certsubject) |[<span data-ttu-id="3f4f9-145">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="3f4f9-145">CertSubjectNameDN</span></span>](#certsubjectnamedn) |[<span data-ttu-id="3f4f9-146">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-146">CertSubjectNameOid</span></span>](#certsubjectnameoid) |[<span data-ttu-id="3f4f9-147">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="3f4f9-147">CertThumbprint</span></span>](#certthumbprint) | |
[<span data-ttu-id="3f4f9-148">CertVersion</span><span class="sxs-lookup"><span data-stu-id="3f4f9-148"> CertVersion</span></span>](#certversion) |[<span data-ttu-id="3f4f9-149">IsCert</span><span class="sxs-lookup"><span data-stu-id="3f4f9-149">IsCert</span></span>](#iscert) | | | |
| <span data-ttu-id="3f4f9-150">**Konvertering**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-150">**Conversion**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-151">CBool</span><span class="sxs-lookup"><span data-stu-id="3f4f9-151">CBool</span></span>](#cbool) |[<span data-ttu-id="3f4f9-152">CDate</span><span class="sxs-lookup"><span data-stu-id="3f4f9-152">CDate</span></span>](#cdate) |[<span data-ttu-id="3f4f9-153">CGuid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-153">CGuid</span></span>](#cguid) |[<span data-ttu-id="3f4f9-154">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="3f4f9-154">ConvertFromBase64</span></span>](#convertfrombase64) | |
| [<span data-ttu-id="3f4f9-155">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="3f4f9-155">ConvertToBase64</span></span>](#converttobase64) |[<span data-ttu-id="3f4f9-156">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="3f4f9-156">ConvertFromUTF8Hex</span></span>](#convertfromutf8hex) |[<span data-ttu-id="3f4f9-157">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="3f4f9-157">ConvertToUTF8Hex</span></span>](#converttoutf8hex) |[<span data-ttu-id="3f4f9-158">CNum</span><span class="sxs-lookup"><span data-stu-id="3f4f9-158">CNum</span></span>](#cnum) | |
| [<span data-ttu-id="3f4f9-159">CRef</span><span class="sxs-lookup"><span data-stu-id="3f4f9-159">CRef</span></span>](#cref) |[<span data-ttu-id="3f4f9-160">CStr</span><span class="sxs-lookup"><span data-stu-id="3f4f9-160">CStr</span></span>](#cstr) |[<span data-ttu-id="3f4f9-161">GUIDFromString</span><span class="sxs-lookup"><span data-stu-id="3f4f9-161">StringFromGuid</span></span>](#StringFromGuid) |[<span data-ttu-id="3f4f9-162">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-162">StringFromSid</span></span>](#stringfromsid) | |
| <span data-ttu-id="3f4f9-163">**Datum / tid**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-163">**Date / Time**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-164">DateAdd</span><span class="sxs-lookup"><span data-stu-id="3f4f9-164">DateAdd</span></span>](#dateadd) |[<span data-ttu-id="3f4f9-165">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="3f4f9-165">DateFromNum</span></span>](#datefromnum) |[<span data-ttu-id="3f4f9-166">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="3f4f9-166">FormatDateTime</span></span>](#formatdatetime) |[<span data-ttu-id="3f4f9-167">Nu</span><span class="sxs-lookup"><span data-stu-id="3f4f9-167">Now</span></span>](#now) | |
| [<span data-ttu-id="3f4f9-168">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="3f4f9-168">NumFromDate</span></span>](#numfromdate) | | | | |
| <span data-ttu-id="3f4f9-169">**Directory**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-169">**Directory**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-170">DNComponent</span><span class="sxs-lookup"><span data-stu-id="3f4f9-170">DNComponent</span></span>](#dncomponent) |[<span data-ttu-id="3f4f9-171">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="3f4f9-171">DNComponentRev</span></span>](#dncomponentrev) |[<span data-ttu-id="3f4f9-172">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="3f4f9-172">EscapeDNComponent</span></span>](#escapedncomponent) | | |
| <span data-ttu-id="3f4f9-173">**Utvärdering**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-173">**Evaluation**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-174">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="3f4f9-174">IsBitSet</span></span>](#isbitset) |[<span data-ttu-id="3f4f9-175">IsDate</span><span class="sxs-lookup"><span data-stu-id="3f4f9-175">IsDate</span></span>](#isdate) |[<span data-ttu-id="3f4f9-176">IsEmpty</span><span class="sxs-lookup"><span data-stu-id="3f4f9-176">IsEmpty</span></span>](#isempty) |[<span data-ttu-id="3f4f9-177">IsGuid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-177">IsGuid</span></span>](#isguid) | |
| [<span data-ttu-id="3f4f9-178">IsNull</span><span class="sxs-lookup"><span data-stu-id="3f4f9-178">IsNull</span></span>](#isnull) |[<span data-ttu-id="3f4f9-179">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="3f4f9-179">IsNullOrEmpty</span></span>](#isnullorempty) |[<span data-ttu-id="3f4f9-180">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="3f4f9-180">IsNumeric</span></span>](#isnumeric) |[<span data-ttu-id="3f4f9-181">IsPresent</span><span class="sxs-lookup"><span data-stu-id="3f4f9-181">IsPresent</span></span>](#ispresent) | |
| [<span data-ttu-id="3f4f9-182">IsString</span><span class="sxs-lookup"><span data-stu-id="3f4f9-182">IsString</span></span>](#isstring) | | | | |
| <span data-ttu-id="3f4f9-183">**Math**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-183">**Math**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-184">BitAnd</span><span class="sxs-lookup"><span data-stu-id="3f4f9-184">BitAnd</span></span>](#bitand) |[<span data-ttu-id="3f4f9-185">BitOr</span><span class="sxs-lookup"><span data-stu-id="3f4f9-185">BitOr</span></span>](#bitor) |[<span data-ttu-id="3f4f9-186">RandomNum</span><span class="sxs-lookup"><span data-stu-id="3f4f9-186">RandomNum</span></span>](#randomnum) | | |
| <span data-ttu-id="3f4f9-187">**Flera värden**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-187">**Multi-valued**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-188">Innehåller</span><span class="sxs-lookup"><span data-stu-id="3f4f9-188">Contains</span></span>](#contains) |[<span data-ttu-id="3f4f9-189">Antal</span><span class="sxs-lookup"><span data-stu-id="3f4f9-189">Count</span></span>](#count) |[<span data-ttu-id="3f4f9-190">Objektet</span><span class="sxs-lookup"><span data-stu-id="3f4f9-190">Item</span></span>](#item) |[<span data-ttu-id="3f4f9-191">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="3f4f9-191">ItemOrNull</span></span>](#itemornull) | |
| [<span data-ttu-id="3f4f9-192">Anslut dig</span><span class="sxs-lookup"><span data-stu-id="3f4f9-192">Join</span></span>](#join) |[<span data-ttu-id="3f4f9-193">RemoveDuplicates</span><span class="sxs-lookup"><span data-stu-id="3f4f9-193">RemoveDuplicates</span></span>](#removeduplicates) |[<span data-ttu-id="3f4f9-194">Dela</span><span class="sxs-lookup"><span data-stu-id="3f4f9-194">Split</span></span>](#split) | | |
| <span data-ttu-id="3f4f9-195">**Programflöde**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-195">**Program Flow**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-196">Fel</span><span class="sxs-lookup"><span data-stu-id="3f4f9-196">Error</span></span>](#error) |[<span data-ttu-id="3f4f9-197">IIF</span><span class="sxs-lookup"><span data-stu-id="3f4f9-197">IIF</span></span>](#iif) |[<span data-ttu-id="3f4f9-198">Välj</span><span class="sxs-lookup"><span data-stu-id="3f4f9-198">Select</span></span>](#select) |[<span data-ttu-id="3f4f9-199">Växel</span><span class="sxs-lookup"><span data-stu-id="3f4f9-199">Switch</span></span>](#switch) | |
| [<span data-ttu-id="3f4f9-200">Där</span><span class="sxs-lookup"><span data-stu-id="3f4f9-200">Where</span></span>](#where) |[<span data-ttu-id="3f4f9-201">Med</span><span class="sxs-lookup"><span data-stu-id="3f4f9-201">With</span></span>](#with) | | | |
| <span data-ttu-id="3f4f9-202">**Text**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-202">**Text**</span></span> | | | | |
| [<span data-ttu-id="3f4f9-203">GUID</span><span class="sxs-lookup"><span data-stu-id="3f4f9-203">GUID</span></span>](#guid) |[<span data-ttu-id="3f4f9-204">InStr</span><span class="sxs-lookup"><span data-stu-id="3f4f9-204">InStr</span></span>](#instr) |[<span data-ttu-id="3f4f9-205">InStrRev</span><span class="sxs-lookup"><span data-stu-id="3f4f9-205">InStrRev</span></span>](#instrrev) |[<span data-ttu-id="3f4f9-206">LCase</span><span class="sxs-lookup"><span data-stu-id="3f4f9-206">LCase</span></span>](#lcase) | |
| [<span data-ttu-id="3f4f9-207">Vänster</span><span class="sxs-lookup"><span data-stu-id="3f4f9-207">Left</span></span>](#left) |[<span data-ttu-id="3f4f9-208">Len</span><span class="sxs-lookup"><span data-stu-id="3f4f9-208">Len</span></span>](#len) |[<span data-ttu-id="3f4f9-209">LTrim</span><span class="sxs-lookup"><span data-stu-id="3f4f9-209">LTrim</span></span>](#ltrim) |[<span data-ttu-id="3f4f9-210">Mid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-210">Mid</span></span>](#mid) | |
| [<span data-ttu-id="3f4f9-211">PadLeft</span><span class="sxs-lookup"><span data-stu-id="3f4f9-211">PadLeft</span></span>](#padleft) |[<span data-ttu-id="3f4f9-212">PadRight</span><span class="sxs-lookup"><span data-stu-id="3f4f9-212">PadRight</span></span>](#padright) |[<span data-ttu-id="3f4f9-213">PCase</span><span class="sxs-lookup"><span data-stu-id="3f4f9-213">PCase</span></span>](#pcase) |[<span data-ttu-id="3f4f9-214">Ersätt</span><span class="sxs-lookup"><span data-stu-id="3f4f9-214">Replace</span></span>](#replace) | |
| [<span data-ttu-id="3f4f9-215">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="3f4f9-215">ReplaceChars</span></span>](#replacechars) |[<span data-ttu-id="3f4f9-216">Höger</span><span class="sxs-lookup"><span data-stu-id="3f4f9-216">Right</span></span>](#right) |[<span data-ttu-id="3f4f9-217">RTrim</span><span class="sxs-lookup"><span data-stu-id="3f4f9-217">RTrim</span></span>](#rtrim) |[<span data-ttu-id="3f4f9-218">Rensa</span><span class="sxs-lookup"><span data-stu-id="3f4f9-218">Trim</span></span>](#trim) | |
| [<span data-ttu-id="3f4f9-219">UCase</span><span class="sxs-lookup"><span data-stu-id="3f4f9-219">UCase</span></span>](#ucase) |[<span data-ttu-id="3f4f9-220">Word</span><span class="sxs-lookup"><span data-stu-id="3f4f9-220">Word</span></span>](#word) | | | |

- - -
### <a name="bitand"></a><span data-ttu-id="3f4f9-221">BitAnd</span><span class="sxs-lookup"><span data-stu-id="3f4f9-221">BitAnd</span></span>
<span data-ttu-id="3f4f9-222">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-222">**Description:**</span></span>  
<span data-ttu-id="3f4f9-223">Hej BitAnd-funktion anger angivna bits på ett värde.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-223">hello BitAnd function sets specified bits on a value.</span></span>

<span data-ttu-id="3f4f9-224">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-224">**Syntax:**</span></span>  
`num BitAnd(num value1, num value2)`

* <span data-ttu-id="3f4f9-225">value1, value2: numeriska värden som ska vara AND'ed tillsammans</span><span class="sxs-lookup"><span data-stu-id="3f4f9-225">value1, value2: numeric values that should be AND’ed together</span></span>

<span data-ttu-id="3f4f9-226">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-226">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-227">Den här funktionen konverteras binär representation av båda parametrarna toohello och anger lite till:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-227">This function converts both parameters toohello binary representation and sets a bit to:</span></span>

* <span data-ttu-id="3f4f9-228">0 – om något eller båda av hello motsvarande bitar i *mask* och *flaggan* är 0</span><span class="sxs-lookup"><span data-stu-id="3f4f9-228">0 - if one or both of hello corresponding bits in *mask* and *flag* are 0</span></span>
* <span data-ttu-id="3f4f9-229">1 – om båda hello motsvarande bits är 1.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-229">1 - if both of hello corresponding bits are 1.</span></span>

<span data-ttu-id="3f4f9-230">Med andra ord returneras 0, utom när hello motsvarande bitarna i båda parametrarna är 1.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-230">In other words, it returns 0 in all cases except when hello corresponding bits of both parameters are 1.</span></span>

<span data-ttu-id="3f4f9-231">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-231">**Example:**</span></span>  
`BitAnd(&HF, &HF7)`  
<span data-ttu-id="3f4f9-232">Returnerar 7 eftersom hexadecimala ”F” och ”F7” utvärdera toothis värde.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-232">Returns 7 because hexadecimal "F" AND "F7" evaluate toothis value.</span></span>

- - -
### <a name="bitor"></a><span data-ttu-id="3f4f9-233">BitOr</span><span class="sxs-lookup"><span data-stu-id="3f4f9-233">BitOr</span></span>
<span data-ttu-id="3f4f9-234">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-234">**Description:**</span></span>  
<span data-ttu-id="3f4f9-235">Hej BITOR-funktion anger angivna bits på ett värde.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-235">hello BitOr function sets specified bits on a value.</span></span>

<span data-ttu-id="3f4f9-236">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-236">**Syntax:**</span></span>  
`num BitOr(num value1, num value2)`

* <span data-ttu-id="3f4f9-237">value1, value2: numeriska värden som ska vara sammansatta med or tillsammans</span><span class="sxs-lookup"><span data-stu-id="3f4f9-237">value1, value2: numeric values that should be OR’ed together</span></span>

<span data-ttu-id="3f4f9-238">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-238">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-239">Den här funktionen konverteras binär representation av båda parametrarna toohello och anger en bit too1 om något eller båda av hello motsvarande bitar i mask och flaggan är 1 och too0 om båda hello motsvarande bits är 0.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-239">This function converts both parameters toohello binary representation and sets a bit too1 if one or both of hello corresponding bits in mask and flag are 1, and too0 if both of hello corresponding bits are 0.</span></span> <span data-ttu-id="3f4f9-240">Med andra ord returnerar 1, utom där hello motsvarande bitarna av båda parametrarna är 0.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-240">In other words, it returns 1 in all cases except where hello corresponding bits of both parameters are 0.</span></span>

- - -
### <a name="cbool"></a><span data-ttu-id="3f4f9-241">CBool</span><span class="sxs-lookup"><span data-stu-id="3f4f9-241">CBool</span></span>
<span data-ttu-id="3f4f9-242">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-242">**Description:**</span></span>  
<span data-ttu-id="3f4f9-243">hello CBool funktionen returnerar ett booleskt värde baserat på hello utvärderas uttryck</span><span class="sxs-lookup"><span data-stu-id="3f4f9-243">hello CBool function returns a Boolean based on hello evaluated expression</span></span>

<span data-ttu-id="3f4f9-244">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-244">**Syntax:**</span></span>  
`bool CBool(exp Expression)`

<span data-ttu-id="3f4f9-245">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-245">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-246">Om hello uttrycket utvärderas tooa noll, och sedan CBool returnerar True, annars returneras False.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-246">If hello expression evaluates tooa nonzero value, then CBool returns True, else it returns False.</span></span>

<span data-ttu-id="3f4f9-247">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-247">**Example:**</span></span>  
`CBool([attrib1] = [attrib2])`  

<span data-ttu-id="3f4f9-248">Returnerar True om båda attribut har hello samma värde.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-248">Returns True if both attributes have hello same value.</span></span>

- - -
### <a name="cdate"></a><span data-ttu-id="3f4f9-249">CDate</span><span class="sxs-lookup"><span data-stu-id="3f4f9-249">CDate</span></span>
<span data-ttu-id="3f4f9-250">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-250">**Description:**</span></span>  
<span data-ttu-id="3f4f9-251">hello CDate funktionen returnerar UTC DateTime från en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-251">hello CDate function returns a UTC DateTime from a string.</span></span> <span data-ttu-id="3f4f9-252">DateTime är inte en inbyggd attributtyp synkroniserade men används av vissa funktioner.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-252">DateTime is not a native attribute type in Sync but is used by some functions.</span></span>

<span data-ttu-id="3f4f9-253">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-253">**Syntax:**</span></span>  
`dt CDate(str value)`

* <span data-ttu-id="3f4f9-254">Värde: En sträng med ett datum, tid och du kan också tidszon</span><span class="sxs-lookup"><span data-stu-id="3f4f9-254">Value: A string with a date, time, and optionally time zone</span></span>

<span data-ttu-id="3f4f9-255">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-255">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-256">hello returnerade strängen är alltid i UTC.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-256">hello returned string is always in UTC.</span></span>

<span data-ttu-id="3f4f9-257">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-257">**Example:**</span></span>  
`CDate([employeeStartTime])`  
<span data-ttu-id="3f4f9-258">Returnerar ett datetime-värde baserat på hello medarbetarens starttid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-258">Returns a DateTime based on hello employee’s start time</span></span>

`CDate("2013-01-10 4:00 PM -8")`  
<span data-ttu-id="3f4f9-259">Returnerar en DateTime som representerar ”2013-01-11 12:00:00”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-259">Returns a DateTime representing "2013-01-11 12:00 AM"</span></span>








- - -
### <a name="certextensionoids"></a><span data-ttu-id="3f4f9-260">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="3f4f9-260">CertExtensionOids</span></span>
<span data-ttu-id="3f4f9-261">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-261">**Description:**</span></span>  
<span data-ttu-id="3f4f9-262">Returnerar hello Oid-värden för alla kritiska hello-tillägg för ett certifikatobjekt.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-262">Returns hello Oid values of all hello critical extensions of a certificate object.</span></span>

<span data-ttu-id="3f4f9-263">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-263">**Syntax:**</span></span>  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-264">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-264">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-265">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-265">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certformat"></a><span data-ttu-id="3f4f9-266">CertFormat</span><span class="sxs-lookup"><span data-stu-id="3f4f9-266">CertFormat</span></span>
<span data-ttu-id="3f4f9-267">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-267">**Description:**</span></span>  
<span data-ttu-id="3f4f9-268">Returnerar hello namnet på hello format för den här X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-268">Returns hello name of hello format of this X.509v3 certificate.</span></span>

<span data-ttu-id="3f4f9-269">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-269">**Syntax:**</span></span>  
`str CertFormat(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-270">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-270">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-271">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-271">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certfriendlyname"></a><span data-ttu-id="3f4f9-272">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="3f4f9-272">CertFriendlyName</span></span>
<span data-ttu-id="3f4f9-273">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-273">**Description:**</span></span>  
<span data-ttu-id="3f4f9-274">Returnerar hello associerade alias för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-274">Returns hello associated alias for a certificate.</span></span>

<span data-ttu-id="3f4f9-275">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-275">**Syntax:**</span></span>  
`str CertFriendlyName(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-276">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-276">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-277">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-277">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certhashstring"></a><span data-ttu-id="3f4f9-278">CertHashString</span><span class="sxs-lookup"><span data-stu-id="3f4f9-278">CertHashString</span></span>
<span data-ttu-id="3f4f9-279">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-279">**Description:**</span></span>  
<span data-ttu-id="3f4f9-280">Returnerar hello SHA1-hash-värdet för hello X.509v3-certifikat som en hexadecimal sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-280">Returns hello SHA1 hash value for hello X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="3f4f9-281">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-281">**Syntax:**</span></span>  
`str CertHashString(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-282">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-282">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-283">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-283">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuer"></a><span data-ttu-id="3f4f9-284">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="3f4f9-284">CertIssuer</span></span>
<span data-ttu-id="3f4f9-285">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-285">**Description:**</span></span>  
<span data-ttu-id="3f4f9-286">Returnerar hello namnet på hello certifikatutfärdaren som utfärdade hello X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-286">Returns hello name of hello certificate authority that issued hello X.509v3 certificate.</span></span>

<span data-ttu-id="3f4f9-287">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-287">**Syntax:**</span></span>  
`str CertIssuer(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-288">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-288">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-289">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-289">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuerdn"></a><span data-ttu-id="3f4f9-290">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="3f4f9-290">CertIssuerDN</span></span>
<span data-ttu-id="3f4f9-291">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-291">**Description:**</span></span>  
<span data-ttu-id="3f4f9-292">Returnerar hello huvudnamnet på hello certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-292">Returns hello distinguished name of hello certificate issuer.</span></span>

<span data-ttu-id="3f4f9-293">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-293">**Syntax:**</span></span>  
`str CertIssuerDN(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-294">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-294">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-295">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-295">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissueroid"></a><span data-ttu-id="3f4f9-296">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-296">CertIssuerOid</span></span>
<span data-ttu-id="3f4f9-297">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-297">**Description:**</span></span>  
<span data-ttu-id="3f4f9-298">Returnerar hello Oid för hello certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-298">Returns hello Oid of hello certificate issuer.</span></span>

<span data-ttu-id="3f4f9-299">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-299">**Syntax:**</span></span>  
`str CertIssuerOid(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-300">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-300">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-301">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-301">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithm"></a><span data-ttu-id="3f4f9-302">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="3f4f9-302">CertKeyAlgorithm</span></span>
<span data-ttu-id="3f4f9-303">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-303">**Description:**</span></span>  
<span data-ttu-id="3f4f9-304">Returnerar information om hello nyckelalgoritm för den här X.509v3-certifikat som en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-304">Returns hello key algorithm information for this X.509v3 certificate as a string.</span></span>

<span data-ttu-id="3f4f9-305">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-305">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-306">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-306">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-307">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-307">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithmparams"></a><span data-ttu-id="3f4f9-308">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="3f4f9-308">CertKeyAlgorithmParams</span></span>
<span data-ttu-id="3f4f9-309">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-309">**Description:**</span></span>  
<span data-ttu-id="3f4f9-310">Returnerar hello nyckelalgoritm parametrar för hello X.509v3-certifikat som en hexadecimal sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-310">Returns hello key algorithm parameters for hello X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="3f4f9-311">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-311">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-312">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-312">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-313">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-313">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnameinfo"></a><span data-ttu-id="3f4f9-314">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="3f4f9-314">CertNameInfo</span></span>
<span data-ttu-id="3f4f9-315">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-315">**Description:**</span></span>  
<span data-ttu-id="3f4f9-316">Returnerar hello ämne och Utfärdarens namn från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-316">Returns hello subject and issuer names from a certificate.</span></span>

<span data-ttu-id="3f4f9-317">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-317">**Syntax:**</span></span>  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   <span data-ttu-id="3f4f9-318">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-318">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-319">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-319">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
*   <span data-ttu-id="3f4f9-320">X509NameType: hello X509NameType värde för hello ämnet.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-320">X509NameType: hello X509NameType value for hello subject.</span></span>
*   <span data-ttu-id="3f4f9-321">includesIssuerName: true tooinclude hello Utfärdarens namn; Annars, FALSKT.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-321">includesIssuerName: true tooinclude hello issuer name; otherwise, false.</span></span>

- - -
### <a name="certnotafter"></a><span data-ttu-id="3f4f9-322">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="3f4f9-322">CertNotAfter</span></span>
<span data-ttu-id="3f4f9-323">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-323">**Description:**</span></span>  
<span data-ttu-id="3f4f9-324">Returnerar hello datum i lokal tid som ett certifikat är inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-324">Returns hello date in local time after which a certificate is no longer valid.</span></span>

<span data-ttu-id="3f4f9-325">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-325">**Syntax:**</span></span>  
`dt CertNotAfter(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-326">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-326">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-327">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-327">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnotbefore"></a><span data-ttu-id="3f4f9-328">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="3f4f9-328">CertNotBefore</span></span>
<span data-ttu-id="3f4f9-329">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-329">**Description:**</span></span>  
<span data-ttu-id="3f4f9-330">Returnerar hello datum i lokal tid som ett certifikat börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-330">Returns hello date in local time on which a certificate becomes valid.</span></span>

<span data-ttu-id="3f4f9-331">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-331">**Syntax:**</span></span>  
`dt CertNotBefore(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-332">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-332">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-333">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-333">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyoid"></a><span data-ttu-id="3f4f9-334">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-334">CertPublicKeyOid</span></span>
<span data-ttu-id="3f4f9-335">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-335">**Description:**</span></span>  
<span data-ttu-id="3f4f9-336">Returnerar hello Oid för hello offentlig nyckel för hello X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-336">Returns hello Oid of hello public key for hello X.509v3 certificate.</span></span>

<span data-ttu-id="3f4f9-337">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-337">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-338">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-338">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-339">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-339">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyparametersoid"></a><span data-ttu-id="3f4f9-340">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-340">CertPublicKeyParametersOid</span></span>
<span data-ttu-id="3f4f9-341">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-341">**Description:**</span></span>  
<span data-ttu-id="3f4f9-342">Returnerar hello Oid för hello offentliga nyckelparametrar för hello X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-342">Returns hello Oid of hello public key parameters for hello X.509v3 certificate.</span></span>

<span data-ttu-id="3f4f9-343">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-343">**Syntax:**</span></span>  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-344">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-344">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-345">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-345">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certserialnumber"></a><span data-ttu-id="3f4f9-346">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="3f4f9-346">CertSerialNumber</span></span>
<span data-ttu-id="3f4f9-347">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-347">**Description:**</span></span>  
<span data-ttu-id="3f4f9-348">Returnerar hello serienumret för hello X.509v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-348">Returns hello serial number of hello X.509v3 certificate.</span></span>

<span data-ttu-id="3f4f9-349">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-349">**Syntax:**</span></span>  
`str CertSerialNumber(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-350">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-350">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-351">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-351">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsignaturealgorithmoid"></a><span data-ttu-id="3f4f9-352">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-352">CertSignatureAlgorithmOid</span></span>
<span data-ttu-id="3f4f9-353">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-353">**Description:**</span></span>  
<span data-ttu-id="3f4f9-354">Returnerar hello Oid för hello algoritmen används toocreate hello signaturen för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-354">Returns hello Oid of hello algorithm used toocreate hello signature of a certificate.</span></span>

<span data-ttu-id="3f4f9-355">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-355">**Syntax:**</span></span>  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-356">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-356">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-357">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-357">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubject"></a><span data-ttu-id="3f4f9-358">CertSubject</span><span class="sxs-lookup"><span data-stu-id="3f4f9-358">CertSubject</span></span>
<span data-ttu-id="3f4f9-359">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-359">**Description:**</span></span>  
<span data-ttu-id="3f4f9-360">Hämtar hello unika ämnesnamnet från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-360">Gets hello subject distinguished name from a certificate.</span></span>

<span data-ttu-id="3f4f9-361">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-361">**Syntax:**</span></span>  
`str CertSubject(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-362">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-362">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-363">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-363">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnamedn"></a><span data-ttu-id="3f4f9-364">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="3f4f9-364">CertSubjectNameDN</span></span>
<span data-ttu-id="3f4f9-365">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-365">**Description:**</span></span>  
<span data-ttu-id="3f4f9-366">Returnerar hello unika ämnesnamnet från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-366">Returns hello subject distinguished name from a certificate.</span></span>

<span data-ttu-id="3f4f9-367">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-367">**Syntax:**</span></span>  
`str CertSubjectNameDN(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-368">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-368">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-369">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-369">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnameoid"></a><span data-ttu-id="3f4f9-370">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-370">CertSubjectNameOid</span></span>
<span data-ttu-id="3f4f9-371">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-371">**Description:**</span></span>  
<span data-ttu-id="3f4f9-372">Returnerar hello Oid för hello ämnesnamnet från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-372">Returns hello Oid of hello subject name from a certificate.</span></span>

<span data-ttu-id="3f4f9-373">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-373">**Syntax:**</span></span>  
`str CertSubjectNameOid(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-374">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-374">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-375">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-375">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certthumbprint"></a><span data-ttu-id="3f4f9-376">certThumbprint</span><span class="sxs-lookup"><span data-stu-id="3f4f9-376">CertThumbprint</span></span>
<span data-ttu-id="3f4f9-377">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-377">**Description:**</span></span>  
<span data-ttu-id="3f4f9-378">Returnerar hello tumavtrycket för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-378">Returns hello thumbprint of a certificate.</span></span>

<span data-ttu-id="3f4f9-379">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-379">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-380">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-380">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-381">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-381">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certversion"></a><span data-ttu-id="3f4f9-382">CertVersion</span><span class="sxs-lookup"><span data-stu-id="3f4f9-382">CertVersion</span></span>
<span data-ttu-id="3f4f9-383">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-383">**Description:**</span></span>  
<span data-ttu-id="3f4f9-384">Returnerar hello X.509-Formatversion för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-384">Returns hello X.509 format version of a certificate.</span></span>

<span data-ttu-id="3f4f9-385">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-385">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-386">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-386">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-387">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-387">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="cguid"></a><span data-ttu-id="3f4f9-388">CGuid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-388">CGuid</span></span>
<span data-ttu-id="3f4f9-389">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-389">**Description:**</span></span>  
<span data-ttu-id="3f4f9-390">Hej CGuid funktionen konverterar hello strängrepresentation av en a binär representation tooits för GUID.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-390">hello CGuid function converts hello string representation of a GUID tooits binary representation.</span></span>

<span data-ttu-id="3f4f9-391">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-391">**Syntax:**</span></span>  
`bin CGuid(str GUID)`

* <span data-ttu-id="3f4f9-392">En sträng formaterad i det här mönstret: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx eller {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="3f4f9-392">A String formatted in this pattern: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

- - -
### <a name="contains"></a><span data-ttu-id="3f4f9-393">Contains</span><span class="sxs-lookup"><span data-stu-id="3f4f9-393">Contains</span></span>
<span data-ttu-id="3f4f9-394">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-394">**Description:**</span></span>  
<span data-ttu-id="3f4f9-395">hello innehåller funktionen returnerar en sträng i ett flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-395">hello Contains function finds a string inside a multi-valued attribute</span></span>

<span data-ttu-id="3f4f9-396">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-396">**Syntax:**</span></span>  
<span data-ttu-id="3f4f9-397">`num Contains (mvstring attribute, str search)`-skiftlägeskänsligt</span><span class="sxs-lookup"><span data-stu-id="3f4f9-397">`num Contains (mvstring attribute, str search)` - case-sensitive</span></span>  
`num Contains (mvstring attribute, str search, enum Casetype)`  
<span data-ttu-id="3f4f9-398">`num Contains (mvref attribute, str search)`-skiftlägeskänsligt</span><span class="sxs-lookup"><span data-stu-id="3f4f9-398">`num Contains (mvref attribute, str search)` - case-sensitive</span></span>

* <span data-ttu-id="3f4f9-399">attributet: hello flervärdesattribut toosearch.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-399">attribute: hello multi-valued attribute toosearch.</span></span>
* <span data-ttu-id="3f4f9-400">Sök: string toofind i hello-attribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-400">search: string toofind in hello attribute.</span></span>
* <span data-ttu-id="3f4f9-401">Casetype: CaseInsensitive- eller CaseSensitive.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-401">Casetype: CaseInsensitive or CaseSensitive.</span></span>

<span data-ttu-id="3f4f9-402">Returnerar index i hello flervärdesattribut där hello strängen hittades.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-402">Returns index in hello multi-valued attribute where hello string was found.</span></span> <span data-ttu-id="3f4f9-403">0 returneras om hello strängen inte hittas.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-403">0 is returned if hello string is not found.</span></span>

<span data-ttu-id="3f4f9-404">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-404">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-405">För flera värden strängattribut hello sökningen hitta delsträngar hello värden.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-405">For multi-valued string attributes, hello search finds substrings in hello values.</span></span>  
<span data-ttu-id="3f4f9-406">För referensattribut måste hello eftersökt sträng exakt matcha hello värdet toobe en matchning.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-406">For reference attributes, hello searched string must exactly match hello value toobe considered a match.</span></span>

<span data-ttu-id="3f4f9-407">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-407">**Example:**</span></span>  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
<span data-ttu-id="3f4f9-408">Om hello proxyAddresses attribut har en primär e-postadress (anges med versaler ”SMTP”:), returnera hello proxyAddress attribut, annars returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-408">If hello proxyAddresses attribute has a primary email address (indicated by uppercase "SMTP:"), then return hello proxyAddress attribute, else return an error.</span></span>

- - -
### <a name="convertfrombase64"></a><span data-ttu-id="3f4f9-409">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="3f4f9-409">ConvertFromBase64</span></span>
<span data-ttu-id="3f4f9-410">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-410">**Description:**</span></span>  
<span data-ttu-id="3f4f9-411">Hej ConvertFromBase64 funktionen konverterar hello angetts base64-kodad värdet tooa vanlig sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-411">hello ConvertFromBase64 function converts hello specified base64 encoded value tooa regular string.</span></span>

<span data-ttu-id="3f4f9-412">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-412">**Syntax:**</span></span>  
<span data-ttu-id="3f4f9-413">`str ConvertFromBase64(str source)`-förutsätter Unicode för kodning</span><span class="sxs-lookup"><span data-stu-id="3f4f9-413">`str ConvertFromBase64(str source)` - assumes Unicode for encoding</span></span>  
`str ConvertFromBase64(str source, enum Encoding)`

* <span data-ttu-id="3f4f9-414">källa: Base64-kodad sträng</span><span class="sxs-lookup"><span data-stu-id="3f4f9-414">source: Base64 encoded string</span></span>  
* <span data-ttu-id="3f4f9-415">Kodning: Unicode, ASCII, UTF8</span><span class="sxs-lookup"><span data-stu-id="3f4f9-415">Encoding: Unicode, ASCII, UTF8</span></span>

<span data-ttu-id="3f4f9-416">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-416">**Example**</span></span>  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

<span data-ttu-id="3f4f9-417">Båda exempel returnera ”*Hello world!*”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-417">Both examples return "*Hello world!*"</span></span>

- - -
### <a name="convertfromutf8hex"></a><span data-ttu-id="3f4f9-418">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="3f4f9-418">ConvertFromUTF8Hex</span></span>
<span data-ttu-id="3f4f9-419">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-419">**Description:**</span></span>  
<span data-ttu-id="3f4f9-420">Hej ConvertFromUTF8Hex funktionen konverterar hello angett Hex UTF8-kodade värdet tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-420">hello ConvertFromUTF8Hex function converts hello specified UTF8 Hex encoded value tooa string.</span></span>

<span data-ttu-id="3f4f9-421">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-421">**Syntax:**</span></span>  
`str ConvertFromUTF8Hex(str source)`

* <span data-ttu-id="3f4f9-422">källa: UTF8 2 byte kodade förekomster av textsträngen</span><span class="sxs-lookup"><span data-stu-id="3f4f9-422">source: UTF8 2-byte encoded sting</span></span>

<span data-ttu-id="3f4f9-423">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-423">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-424">hello skillnaden mellan den här funktionen och ConvertFromBase64([],UTF8) i resultatmängden hello är eget för hello DN-attribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-424">hello difference between this function and ConvertFromBase64([],UTF8) in that hello result is friendly for hello DN attribute.</span></span>  
<span data-ttu-id="3f4f9-425">Det här formatet används av Azure Active Directory som unikt namn.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-425">This format is used by Azure Active Directory as DN.</span></span>

<span data-ttu-id="3f4f9-426">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-426">**Example:**</span></span>  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
<span data-ttu-id="3f4f9-427">Returnerar ”*Hello world!*”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-427">Returns "*Hello world!*"</span></span>

- - -
### <a name="converttobase64"></a><span data-ttu-id="3f4f9-428">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="3f4f9-428">ConvertToBase64</span></span>
<span data-ttu-id="3f4f9-429">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-429">**Description:**</span></span>  
<span data-ttu-id="3f4f9-430">Hej ConvertToBase64 funktionen konverterar en sträng tooa Unicode base64-sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-430">hello ConvertToBase64 function converts a string tooa Unicode base64 string.</span></span>  
<span data-ttu-id="3f4f9-431">Konverterar hello-värdet för en matris med heltal tooits motsvarande strängrepresentation som kodats med Base64-siffror.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-431">Converts hello value of an array of integers tooits equivalent string representation that is encoded with base-64 digits.</span></span>

<span data-ttu-id="3f4f9-432">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-432">**Syntax:**</span></span>  
`str ConvertToBase64(str source)`

<span data-ttu-id="3f4f9-433">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-433">**Example:**</span></span>  
`ConvertToBase64("Hello world!")`  
<span data-ttu-id="3f4f9-434">Returnerar ”SABlAGwAbABvACAAdwBvAHIAbABkACEA”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-434">Returns "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span></span>

- - -
### <a name="converttoutf8hex"></a><span data-ttu-id="3f4f9-435">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="3f4f9-435">ConvertToUTF8Hex</span></span>
<span data-ttu-id="3f4f9-436">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-436">**Description:**</span></span>  
<span data-ttu-id="3f4f9-437">Hej ConvertToUTF8Hex funktionen konverterar en sträng tooa Hex UTF8-kodade värde.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-437">hello ConvertToUTF8Hex function converts a string tooa UTF8 Hex encoded value.</span></span>

<span data-ttu-id="3f4f9-438">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-438">**Syntax:**</span></span>  
`str ConvertToUTF8Hex(str source)`

<span data-ttu-id="3f4f9-439">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-439">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-440">hello utdataformatet för den här funktionen används av Azure Active Directory som DN attribute-format.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-440">hello output format of this function is used by Azure Active Directory as DN attribute format.</span></span>

<span data-ttu-id="3f4f9-441">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-441">**Example:**</span></span>  
`ConvertToUTF8Hex("Hello world!")`  
<span data-ttu-id="3f4f9-442">Returnerar 48656C6C6F20776F726C6421</span><span class="sxs-lookup"><span data-stu-id="3f4f9-442">Returns 48656C6C6F20776F726C6421</span></span>

- - -
### <a name="count"></a><span data-ttu-id="3f4f9-443">Antal</span><span class="sxs-lookup"><span data-stu-id="3f4f9-443">Count</span></span>
<span data-ttu-id="3f4f9-444">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-444">**Description:**</span></span>  
<span data-ttu-id="3f4f9-445">hello Count-funktionen returnerar hello antalet element i ett flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-445">hello Count function returns hello number of elements in a multi-valued attribute</span></span>

<span data-ttu-id="3f4f9-446">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-446">**Syntax:**</span></span>  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a><span data-ttu-id="3f4f9-447">CNum</span><span class="sxs-lookup"><span data-stu-id="3f4f9-447">CNum</span></span>
<span data-ttu-id="3f4f9-448">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-448">**Description:**</span></span>  
<span data-ttu-id="3f4f9-449">Hej CNum funktionen en sträng och returnerar en numerisk datatyp.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-449">hello CNum function takes a string and returns a numeric data type.</span></span>

<span data-ttu-id="3f4f9-450">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-450">**Syntax:**</span></span>  
`num CNum(str value)`

- - -
### <a name="cref"></a><span data-ttu-id="3f4f9-451">CRef</span><span class="sxs-lookup"><span data-stu-id="3f4f9-451">CRef</span></span>
<span data-ttu-id="3f4f9-452">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-452">**Description:**</span></span>  
<span data-ttu-id="3f4f9-453">Konverterar en sträng tooa referensattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-453">Converts a string tooa reference attribute</span></span>

<span data-ttu-id="3f4f9-454">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-454">**Syntax:**</span></span>  
`ref CRef(str value)`

<span data-ttu-id="3f4f9-455">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-455">**Example:**</span></span>  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a><span data-ttu-id="3f4f9-456">CStr</span><span class="sxs-lookup"><span data-stu-id="3f4f9-456">CStr</span></span>
<span data-ttu-id="3f4f9-457">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-457">**Description:**</span></span>  
<span data-ttu-id="3f4f9-458">hello CStr funktionen konverterar tooa strängdatatypen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-458">hello CStr function converts tooa string data type.</span></span>

<span data-ttu-id="3f4f9-459">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-459">**Syntax:**</span></span>  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* <span data-ttu-id="3f4f9-460">värde: kan vara ett numeriskt värde, referensattribut eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-460">value: Can be a numeric value, reference attribute, or Boolean.</span></span>

<span data-ttu-id="3f4f9-461">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-461">**Example:**</span></span>  
`CStr([dn])`  
<span data-ttu-id="3f4f9-462">Kan returnera ”cn = Johan, dc = contoso, dc = com”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-462">Could return "cn=Joe,dc=contoso,dc=com"</span></span>

- - -
### <a name="dateadd"></a><span data-ttu-id="3f4f9-463">DateAdd</span><span class="sxs-lookup"><span data-stu-id="3f4f9-463">DateAdd</span></span>
<span data-ttu-id="3f4f9-464">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-464">**Description:**</span></span>  
<span data-ttu-id="3f4f9-465">Returnerar ett datum som innehåller ett datum toowhich ett angivet tidsintervall har lagts till.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-465">Returns a Date containing a date toowhich a specified time interval has been added.</span></span>

<span data-ttu-id="3f4f9-466">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-466">**Syntax:**</span></span>  
`dt DateAdd(str interval, num value, dt date)`

* <span data-ttu-id="3f4f9-467">intervall: stränguttryck som är hello tidsintervall du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-467">interval: String expression that is hello interval of time you want tooadd.</span></span> <span data-ttu-id="3f4f9-468">hello sträng måste ha något av följande värden hello:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-468">hello string must have one of hello following values:</span></span>
  * <span data-ttu-id="3f4f9-469">åååå år</span><span class="sxs-lookup"><span data-stu-id="3f4f9-469">yyyy Year</span></span>
  * <span data-ttu-id="3f4f9-470">q kvartal</span><span class="sxs-lookup"><span data-stu-id="3f4f9-470">q Quarter</span></span>
  * <span data-ttu-id="3f4f9-471">m månad</span><span class="sxs-lookup"><span data-stu-id="3f4f9-471">m Month</span></span>
  * <span data-ttu-id="3f4f9-472">y dagen på året</span><span class="sxs-lookup"><span data-stu-id="3f4f9-472">y Day of year</span></span>
  * <span data-ttu-id="3f4f9-473">d dag</span><span class="sxs-lookup"><span data-stu-id="3f4f9-473">d Day</span></span>
  * <span data-ttu-id="3f4f9-474">w veckodag</span><span class="sxs-lookup"><span data-stu-id="3f4f9-474">w Weekday</span></span>
  * <span data-ttu-id="3f4f9-475">ww vecka</span><span class="sxs-lookup"><span data-stu-id="3f4f9-475">ww Week</span></span>
  * <span data-ttu-id="3f4f9-476">h timme</span><span class="sxs-lookup"><span data-stu-id="3f4f9-476">h Hour</span></span>
  * <span data-ttu-id="3f4f9-477">n minut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-477">n Minute</span></span>
  * <span data-ttu-id="3f4f9-478">s andra</span><span class="sxs-lookup"><span data-stu-id="3f4f9-478">s Second</span></span>
* <span data-ttu-id="3f4f9-479">värde: hello antalet enheter som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-479">value: hello number of units you want tooadd.</span></span> <span data-ttu-id="3f4f9-480">Det kan vara positivt (tooget datum i hello framtida) eller negativt (tooget datum i hello tidigare).</span><span class="sxs-lookup"><span data-stu-id="3f4f9-480">It can be positive (tooget dates in hello future) or negative (tooget dates in hello past).</span></span>
* <span data-ttu-id="3f4f9-481">datum: DateTime som representerar toowhich hello datumintervall har lagts till.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-481">date: DateTime representing date toowhich hello interval is added.</span></span>

<span data-ttu-id="3f4f9-482">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-482">**Example:**</span></span>  
`DateAdd("m", 3, CDate("2001-01-01"))`  
<span data-ttu-id="3f4f9-483">Lägger till tre månader och returnerar ett datetime-värde som representerar ”2001-04-01”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-483">Adds 3 months and returns a DateTime representing "2001-04-01".</span></span>

- - -
### <a name="datefromnum"></a><span data-ttu-id="3f4f9-484">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="3f4f9-484">DateFromNum</span></span>
<span data-ttu-id="3f4f9-485">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-485">**Description:**</span></span>  
<span data-ttu-id="3f4f9-486">Hej DateFromNum funktionen konverterar ett värde i Annonsens datum format tooa datum och tid.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-486">hello DateFromNum function converts a value in AD’s date format tooa DateTime type.</span></span>

<span data-ttu-id="3f4f9-487">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-487">**Syntax:**</span></span>  
`dt DateFromNum(num value)`

<span data-ttu-id="3f4f9-488">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-488">**Example:**</span></span>  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
<span data-ttu-id="3f4f9-489">Returnerar ett datetime-värde som representerar 2012-01-01 23:00:00</span><span class="sxs-lookup"><span data-stu-id="3f4f9-489">Returns a DateTime representing 2012-01-01 23:00:00</span></span>

- - -
### <a name="dncomponent"></a><span data-ttu-id="3f4f9-490">DNComponent</span><span class="sxs-lookup"><span data-stu-id="3f4f9-490">DNComponent</span></span>
<span data-ttu-id="3f4f9-491">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-491">**Description:**</span></span>  
<span data-ttu-id="3f4f9-492">Hej DNComponent funktionen returnerar hello värdet för en angiven DN-komponent från vänster.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-492">hello DNComponent function returns hello value of a specified DN component going from left.</span></span>

<span data-ttu-id="3f4f9-493">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-493">**Syntax:**</span></span>  
`str DNComponent(ref dn, num ComponentNumber)`

* <span data-ttu-id="3f4f9-494">DN: hello referens attributet toointerpret</span><span class="sxs-lookup"><span data-stu-id="3f4f9-494">dn: hello reference attribute toointerpret</span></span>
* <span data-ttu-id="3f4f9-495">ComponentNumber: hello-komponenten i hello DN tooreturn</span><span class="sxs-lookup"><span data-stu-id="3f4f9-495">ComponentNumber: hello component in hello DN tooreturn</span></span>

<span data-ttu-id="3f4f9-496">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-496">**Example:**</span></span>  
`DNComponent([dn],1)`  
<span data-ttu-id="3f4f9-497">Om dn är ”cn = Johan, ou =...”, returneras Joe</span><span class="sxs-lookup"><span data-stu-id="3f4f9-497">If dn is "cn=Joe,ou=…," it returns Joe</span></span>

- - -
### <a name="dncomponentrev"></a><span data-ttu-id="3f4f9-498">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="3f4f9-498">DNComponentRev</span></span>
<span data-ttu-id="3f4f9-499">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-499">**Description:**</span></span>  
<span data-ttu-id="3f4f9-500">Hej DNComponentRev funktionen returnerar hello värdet för en angiven DN-komponent från höger (hello end).</span><span class="sxs-lookup"><span data-stu-id="3f4f9-500">hello DNComponentRev function returns hello value of a specified DN component going from right (hello end).</span></span>

<span data-ttu-id="3f4f9-501">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-501">**Syntax:**</span></span>  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* <span data-ttu-id="3f4f9-502">DN: hello referens attributet toointerpret</span><span class="sxs-lookup"><span data-stu-id="3f4f9-502">dn: hello reference attribute toointerpret</span></span>
* <span data-ttu-id="3f4f9-503">ComponentNumber - hello-komponenten i hello DN tooreturn</span><span class="sxs-lookup"><span data-stu-id="3f4f9-503">ComponentNumber - hello component in hello DN tooreturn</span></span>
* <span data-ttu-id="3f4f9-504">Alternativ: DC – Ignorera alla komponenter med ”dc =”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-504">Options: DC – Ignore all components with "dc="</span></span>

<span data-ttu-id="3f4f9-505">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-505">**Example:**</span></span>  
<span data-ttu-id="3f4f9-506">Om dn är ”cn = Joe, ou = Atlanta, ou = GA, ou = US, dc = contoso, dc = com” sedan</span><span class="sxs-lookup"><span data-stu-id="3f4f9-506">If dn is "cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com" then</span></span>  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
<span data-ttu-id="3f4f9-507">Returnerar båda oss.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-507">Both return US.</span></span>

- - -
### <a name="error"></a><span data-ttu-id="3f4f9-508">Fel</span><span class="sxs-lookup"><span data-stu-id="3f4f9-508">Error</span></span>
<span data-ttu-id="3f4f9-509">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-509">**Description:**</span></span>  
<span data-ttu-id="3f4f9-510">hello fel funktion är används tooreturn ett anpassat fel.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-510">hello Error function is used tooreturn a custom error.</span></span>

<span data-ttu-id="3f4f9-511">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-511">**Syntax:**</span></span>  
`void Error(str ErrorMessage)`

<span data-ttu-id="3f4f9-512">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-512">**Example:**</span></span>  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
<span data-ttu-id="3f4f9-513">Om hello attributet accountName inte finns genereras ett fel på hello-objekt.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-513">If hello attribute accountName is not present, throw an error on hello object.</span></span>

- - -
### <a name="escapedncomponent"></a><span data-ttu-id="3f4f9-514">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="3f4f9-514">EscapeDNComponent</span></span>
<span data-ttu-id="3f4f9-515">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-515">**Description:**</span></span>  
<span data-ttu-id="3f4f9-516">Hej EscapeDNComponent funktionen tar en komponent i ett unikt namn och hoppas det så att den kan representeras i LDAP.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-516">hello EscapeDNComponent function takes one component of a DN and escapes it so it can be represented in LDAP.</span></span>

<span data-ttu-id="3f4f9-517">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-517">**Syntax:**</span></span>  
`str EscapeDNComponent(str value)`

<span data-ttu-id="3f4f9-518">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-518">**Example:**</span></span>  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
<span data-ttu-id="3f4f9-519">Säkerställer hello-objekt kan skapas i en LDAP-katalog även om hello displayName attributet innehåller tecken som måste hoppas i LDAP.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-519">Makes sure hello object can be created in an LDAP directory even if hello displayName attribute has characters that must be escaped in LDAP.</span></span>

- - -
### <a name="formatdatetime"></a><span data-ttu-id="3f4f9-520">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="3f4f9-520">FormatDateTime</span></span>
<span data-ttu-id="3f4f9-521">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-521">**Description:**</span></span>  
<span data-ttu-id="3f4f9-522">funktionen för hello FormatDateTime är används tooformat en DateTime tooa sträng med ett bestämt format</span><span class="sxs-lookup"><span data-stu-id="3f4f9-522">hello FormatDateTime function is used tooformat a DateTime tooa string with a specified format</span></span>

<span data-ttu-id="3f4f9-523">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-523">**Syntax:**</span></span>  
`str FormatDateTime(dt value, str format)`

* <span data-ttu-id="3f4f9-524">värde: ett värde i hello DateTime-format</span><span class="sxs-lookup"><span data-stu-id="3f4f9-524">value: a value in hello DateTime format</span></span>
* <span data-ttu-id="3f4f9-525">format: en sträng som representerar hello format tooconvert till.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-525">format: a string representing hello format tooconvert to.</span></span>

<span data-ttu-id="3f4f9-526">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-526">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-527">Hej möjliga värden för hello-format finns här: [User-defined datum/tid-format (Format-funktionen)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span><span class="sxs-lookup"><span data-stu-id="3f4f9-527">hello possible values for hello format can be found here: [User-Defined Date/Time Formats (Format Function)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span></span>

<span data-ttu-id="3f4f9-528">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-528">**Example:**</span></span>  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
<span data-ttu-id="3f4f9-529">Resulterar i ”2007-12-25”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-529">Results in "2007-12-25".</span></span>

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
<span data-ttu-id="3f4f9-530">Kan medföra ”20140905081453.0Z”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-530">Can result in "20140905081453.0Z"</span></span>

- - -
### <a name="guid"></a><span data-ttu-id="3f4f9-531">GUID</span><span class="sxs-lookup"><span data-stu-id="3f4f9-531">GUID</span></span>
<span data-ttu-id="3f4f9-532">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-532">**Description:**</span></span>  
<span data-ttu-id="3f4f9-533">hello funktionen GUID genererar en ny, slumpmässig GUID</span><span class="sxs-lookup"><span data-stu-id="3f4f9-533">hello function GUID generates a new random GUID</span></span>

<span data-ttu-id="3f4f9-534">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-534">**Syntax:**</span></span>  
`str GUID()`

- - -
### <a name="iif"></a><span data-ttu-id="3f4f9-535">IIF</span><span class="sxs-lookup"><span data-stu-id="3f4f9-535">IIF</span></span>
<span data-ttu-id="3f4f9-536">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-536">**Description:**</span></span>  
<span data-ttu-id="3f4f9-537">hello OOM returnerar ett av en uppsättning möjliga värden baserat på ett angivet villkor.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-537">hello IIF function returns one of a set of possible values based on a specified condition.</span></span>

<span data-ttu-id="3f4f9-538">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-538">**Syntax:**</span></span>  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* <span data-ttu-id="3f4f9-539">villkor: ett värde eller uttryck som kan utvärderas tootrue eller false.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-539">condition: any value or expression that can be evaluated tootrue or false.</span></span>
* <span data-ttu-id="3f4f9-540">värde_om_sant: om hello villkoret utvärderas tootrue, hello returneras värdet.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-540">valueIfTrue: If hello condition evaluates tootrue, hello returned value.</span></span>
* <span data-ttu-id="3f4f9-541">värde_om_falskt: om hello villkoret utvärderas toofalse, hello returneras värdet.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-541">valueIfFalse: If hello condition evaluates toofalse, hello returned value.</span></span>

<span data-ttu-id="3f4f9-542">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-542">**Example:**</span></span>  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 <span data-ttu-id="3f4f9-543">Om hello användare är en intern, returnerar hello-alias för en användare med ”t-” läggs toohello början av det, annars returnerar hello användarens alias som är.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-543">If hello user is an intern, returns hello alias of a user with "t-" added toohello beginning of it, else returns hello user’s alias as is.</span></span>

- - -
### <a name="instr"></a><span data-ttu-id="3f4f9-544">InStr</span><span class="sxs-lookup"><span data-stu-id="3f4f9-544">InStr</span></span>
<span data-ttu-id="3f4f9-545">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-545">**Description:**</span></span>  
<span data-ttu-id="3f4f9-546">hello funktionen InStr hittar hello första förekomsten av en understräng i en sträng</span><span class="sxs-lookup"><span data-stu-id="3f4f9-546">hello InStr function finds hello first occurrence of a substring in a string</span></span>

<span data-ttu-id="3f4f9-547">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-547">**Syntax:**</span></span>  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* <span data-ttu-id="3f4f9-548">stringcheck: string toobe genomsöks</span><span class="sxs-lookup"><span data-stu-id="3f4f9-548">stringcheck: string toobe searched</span></span>
* <span data-ttu-id="3f4f9-549">stringmatch: string toobe hittades</span><span class="sxs-lookup"><span data-stu-id="3f4f9-549">stringmatch: string toobe found</span></span>
* <span data-ttu-id="3f4f9-550">Starta: starta position toofind hello delsträngen</span><span class="sxs-lookup"><span data-stu-id="3f4f9-550">start: starting position toofind hello substring</span></span>
* <span data-ttu-id="3f4f9-551">Jämför: i vbTextCompare eller i vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="3f4f9-551">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="3f4f9-552">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-552">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-553">Returnerar hello positionen där delsträngen hello hittades eller 0 om inte hittas.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-553">Returns hello position where hello substring was found or 0 if not found.</span></span>

<span data-ttu-id="3f4f9-554">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-554">**Example:**</span></span>  
`InStr("hello quick brown fox","quick")`  
<span data-ttu-id="3f4f9-555">Evalues too5</span><span class="sxs-lookup"><span data-stu-id="3f4f9-555">Evalues too5</span></span>

`InStr("repEated","e",3,vbBinaryCompare)`  
<span data-ttu-id="3f4f9-556">Utvärderar too7</span><span class="sxs-lookup"><span data-stu-id="3f4f9-556">Evaluates too7</span></span>

- - -
### <a name="instrrev"></a><span data-ttu-id="3f4f9-557">InStrRev</span><span class="sxs-lookup"><span data-stu-id="3f4f9-557">InStrRev</span></span>
<span data-ttu-id="3f4f9-558">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-558">**Description:**</span></span>  
<span data-ttu-id="3f4f9-559">hello funktionen InStrRev hittar hello sista förekomsten av en understräng i en sträng</span><span class="sxs-lookup"><span data-stu-id="3f4f9-559">hello InStrRev function finds hello last occurrence of a substring in a string</span></span>

<span data-ttu-id="3f4f9-560">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-560">**Syntax:**</span></span>  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* <span data-ttu-id="3f4f9-561">stringcheck: string toobe genomsöks</span><span class="sxs-lookup"><span data-stu-id="3f4f9-561">stringcheck: string toobe searched</span></span>
* <span data-ttu-id="3f4f9-562">stringmatch: string toobe hittades</span><span class="sxs-lookup"><span data-stu-id="3f4f9-562">stringmatch: string toobe found</span></span>
* <span data-ttu-id="3f4f9-563">Starta: starta position toofind hello delsträngen</span><span class="sxs-lookup"><span data-stu-id="3f4f9-563">start: starting position toofind hello substring</span></span>
* <span data-ttu-id="3f4f9-564">Jämför: i vbTextCompare eller i vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="3f4f9-564">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="3f4f9-565">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-565">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-566">Returnerar hello positionen där delsträngen hello hittades eller 0 om inte hittas.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-566">Returns hello position where hello substring was found or 0 if not found.</span></span>

<span data-ttu-id="3f4f9-567">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-567">**Example:**</span></span>  
`InStrRev("abbcdbbbef","bb")`  
<span data-ttu-id="3f4f9-568">Returnerar 7</span><span class="sxs-lookup"><span data-stu-id="3f4f9-568">Returns 7</span></span>

- - -
### <a name="isbitset"></a><span data-ttu-id="3f4f9-569">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="3f4f9-569">IsBitSet</span></span>
<span data-ttu-id="3f4f9-570">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-570">**Description:**</span></span>  
<span data-ttu-id="3f4f9-571">hello funktionen IsBitSet tester om lite är eller inte</span><span class="sxs-lookup"><span data-stu-id="3f4f9-571">hello function IsBitSet Tests if a bit is set or not</span></span>

<span data-ttu-id="3f4f9-572">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-572">**Syntax:**</span></span>  
`bool IsBitSet(num value, num flag)`

* <span data-ttu-id="3f4f9-573">värde: ett numeriskt värde som är evaluated.flag: ett numeriskt värde som har hello bit toobe utvärderas</span><span class="sxs-lookup"><span data-stu-id="3f4f9-573">value: a numeric value that is evaluated.flag: a numeric value that has hello bit toobe evaluated</span></span>

<span data-ttu-id="3f4f9-574">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-574">**Example:**</span></span>  
`IsBitSet(&HF,4)`  
<span data-ttu-id="3f4f9-575">Returnerar SANT eftersom ”4”-biten är aktiverad i hello hexadecimalt värde ”F”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-575">Returns True because bit "4" is set in hello hexadecimal value "F"</span></span>

- - -
### <a name="isdate"></a><span data-ttu-id="3f4f9-576">IsDate</span><span class="sxs-lookup"><span data-stu-id="3f4f9-576">IsDate</span></span>
<span data-ttu-id="3f4f9-577">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-577">**Description:**</span></span>  
<span data-ttu-id="3f4f9-578">Om hello uttryck kan utvärderas som en DateTime-typ. sedan hello funktionen IsDate utvärderar tooTrue.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-578">If hello expression can be evaluates as a DateTime type, then hello IsDate function evaluates tooTrue.</span></span>

<span data-ttu-id="3f4f9-579">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-579">**Syntax:**</span></span>  
`bool IsDate(var Expression)`

<span data-ttu-id="3f4f9-580">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-580">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-581">Använda toodetermine om CDate() kan genomföras.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-581">Used toodetermine if CDate() can be successful.</span></span>

- - -
### <a name="iscert"></a><span data-ttu-id="3f4f9-582">IsCert</span><span class="sxs-lookup"><span data-stu-id="3f4f9-582">IsCert</span></span>
<span data-ttu-id="3f4f9-583">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-583">**Description:**</span></span>  
<span data-ttu-id="3f4f9-584">Returnerar true om hello rådata kan serialiseras till .NET X509Certificate2 certifikatobjekt.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-584">Returns true if hello raw data can be serialized into .NET X509Certificate2 certificate object.</span></span>

<span data-ttu-id="3f4f9-585">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-585">**Syntax:**</span></span>  
`bool CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="3f4f9-586">certificateRawData: Byte-matris representation av ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-586">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="3f4f9-587">hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-587">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
- - -
### <a name="isempty"></a><span data-ttu-id="3f4f9-588">IsEmpty</span><span class="sxs-lookup"><span data-stu-id="3f4f9-588">IsEmpty</span></span>
<span data-ttu-id="3f4f9-589">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-589">**Description:**</span></span>  
<span data-ttu-id="3f4f9-590">Om hello attribut finns i hello CS eller MV men utvärderar tooan tom sträng, utvärderar hello IsEmpty funktionen tooTrue.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-590">If hello attribute is present in hello CS or MV but evaluates tooan empty string, then hello IsEmpty function evaluates tooTrue.</span></span>

<span data-ttu-id="3f4f9-591">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-591">**Syntax:**</span></span>  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a><span data-ttu-id="3f4f9-592">IsGuid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-592">IsGuid</span></span>
<span data-ttu-id="3f4f9-593">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-593">**Description:**</span></span>  
<span data-ttu-id="3f4f9-594">Om hello strängen kan vara konverterade tooa GUID, utvärderas hello IsGuid funktionen tootrue.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-594">If hello string could be converted tooa GUID, then hello IsGuid function evaluated tootrue.</span></span>

<span data-ttu-id="3f4f9-595">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-595">**Syntax:**</span></span>  
`bool IsGuid(str GUID)`

<span data-ttu-id="3f4f9-596">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-596">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-597">Ett GUID som har definierats som en sträng som följer någon av dessa mönster: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx eller {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="3f4f9-597">A GUID is defined as a string following one of these patterns: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

<span data-ttu-id="3f4f9-598">Använda toodetermine om CGuid() kan genomföras.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-598">Used toodetermine if CGuid() can be successful.</span></span>

<span data-ttu-id="3f4f9-599">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-599">**Example:**</span></span>  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
<span data-ttu-id="3f4f9-600">Om hello StrAttribute har en GUID-format, returnera en a binär representation, annars returnerar Null.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-600">If hello StrAttribute has a GUID format, return a binary representation, otherwise return a Null.</span></span>

- - -
### <a name="isnull"></a><span data-ttu-id="3f4f9-601">IsNull</span><span class="sxs-lookup"><span data-stu-id="3f4f9-601">IsNull</span></span>
<span data-ttu-id="3f4f9-602">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-602">**Description:**</span></span>  
<span data-ttu-id="3f4f9-603">Om hello uttrycket utvärderas tooNull, returnerar hello IsNull funktionen SANT.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-603">If hello expression evaluates tooNull, then hello IsNull function returns true.</span></span>

<span data-ttu-id="3f4f9-604">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-604">**Syntax:**</span></span>  
`bool IsNull(var Expression)`

<span data-ttu-id="3f4f9-605">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-605">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-606">Ett null-värde uttrycks för ett attribut av hello hello-attributet saknas.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-606">For an attribute, a Null is expressed by hello absence of hello attribute.</span></span>

<span data-ttu-id="3f4f9-607">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-607">**Example:**</span></span>  
`IsNull([displayName])`  
<span data-ttu-id="3f4f9-608">Returnerar True om hello attributet inte finns i hello CS eller MV.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-608">Returns True if hello attribute is not present in hello CS or MV.</span></span>

- - -
### <a name="isnullorempty"></a><span data-ttu-id="3f4f9-609">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="3f4f9-609">IsNullOrEmpty</span></span>
<span data-ttu-id="3f4f9-610">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-610">**Description:**</span></span>  
<span data-ttu-id="3f4f9-611">Om hello uttryck är null eller en tom sträng, returnerar hello IsNullOrEmpty funktionen SANT.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-611">If hello expression is null or an empty string, then hello IsNullOrEmpty function returns true.</span></span>

<span data-ttu-id="3f4f9-612">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-612">**Syntax:**</span></span>  
`bool IsNullOrEmpty(var Expression)`

<span data-ttu-id="3f4f9-613">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-613">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-614">För ett attribut utvärderar detta tooTrue om hello-attribut saknas eller finns men är en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-614">For an attribute, this would evaluate tooTrue if hello attribute is absent or is present but is an empty string.</span></span>  
<span data-ttu-id="3f4f9-615">hello inversen till den här funktionen kallas IsPresent.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-615">hello inverse of this function is named IsPresent.</span></span>

<span data-ttu-id="3f4f9-616">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-616">**Example:**</span></span>  
`IsNullOrEmpty([displayName])`  
<span data-ttu-id="3f4f9-617">Returnerar True om hello attributet finns inte eller är en tom sträng i hello CS eller MV.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-617">Returns True if hello attribute is not present or is an empty string in hello CS or MV.</span></span>

- - -
### <a name="isnumeric"></a><span data-ttu-id="3f4f9-618">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="3f4f9-618">IsNumeric</span></span>
<span data-ttu-id="3f4f9-619">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-619">**Description:**</span></span>  
<span data-ttu-id="3f4f9-620">hello funktionen IsNumeric returnerar ett booleskt värde som anger om ett uttryck kan utvärderas som tal av typen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-620">hello IsNumeric function returns a Boolean value indicating whether an expression can be evaluated as a number type.</span></span>

<span data-ttu-id="3f4f9-621">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-621">**Syntax:**</span></span>  
`bool IsNumeric(var Expression)`

<span data-ttu-id="3f4f9-622">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-622">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-623">Använda toodetermine om CNum() kan vara lyckade tooparse hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-623">Used toodetermine if CNum() can be successful tooparse hello expression.</span></span>

- - -
### <a name="isstring"></a><span data-ttu-id="3f4f9-624">IsString</span><span class="sxs-lookup"><span data-stu-id="3f4f9-624">IsString</span></span>
<span data-ttu-id="3f4f9-625">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-625">**Description:**</span></span>  
<span data-ttu-id="3f4f9-626">Om hello uttrycket kan vara utvärderade tooa strängen typ, och sedan hello IsString evaluerar tooTrue.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-626">If hello expression can be evaluated tooa string type, then hello IsString function evaluates tooTrue.</span></span>

<span data-ttu-id="3f4f9-627">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-627">**Syntax:**</span></span>  
`bool IsString(var expression)`

<span data-ttu-id="3f4f9-628">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-628">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-629">Använda toodetermine om CStr() kan vara lyckade tooparse hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-629">Used toodetermine if CStr() can be successful tooparse hello expression.</span></span>

- - -
### <a name="ispresent"></a><span data-ttu-id="3f4f9-630">IsPresent</span><span class="sxs-lookup"><span data-stu-id="3f4f9-630">IsPresent</span></span>
<span data-ttu-id="3f4f9-631">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-631">**Description:**</span></span>  
<span data-ttu-id="3f4f9-632">Om hello uttrycket utvärderas tooa sträng som inte är Null och inte är tom, hello du IsPresent funktionen returnerar true.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-632">If hello expression evaluates tooa string that is not Null and is not empty, then hello IsPresent function returns true.</span></span>

<span data-ttu-id="3f4f9-633">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-633">**Syntax:**</span></span>  
`bool IsPresent(var expression)`

<span data-ttu-id="3f4f9-634">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-634">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-635">hello inversen till den här funktionen kallas IsNullOrEmpty.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-635">hello inverse of this function is named IsNullOrEmpty.</span></span>

<span data-ttu-id="3f4f9-636">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-636">**Example:**</span></span>  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a><span data-ttu-id="3f4f9-637">Objekt</span><span class="sxs-lookup"><span data-stu-id="3f4f9-637">Item</span></span>
<span data-ttu-id="3f4f9-638">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-638">**Description:**</span></span>  
<span data-ttu-id="3f4f9-639">hello funktionen Item returnerar ett objekt från ett med flera värden strängattribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-639">hello Item function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="3f4f9-640">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-640">**Syntax:**</span></span>  
`var Item(mvstr attribute, num index)`

* <span data-ttu-id="3f4f9-641">attributet: flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-641">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="3f4f9-642">index: indexet tooan objekt i flera värden hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-642">index: index tooan item in hello multi-valued string.</span></span>

<span data-ttu-id="3f4f9-643">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-643">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-644">hello funktionen Item används tillsammans med hello innehåller funktionen eftersom hello senare funktionen returnerar hello index tooan objektet i hello flervärdesattribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-644">hello Item function is useful together with hello Contains function since hello latter function returns hello index tooan item in hello multi-valued attribute.</span></span>

<span data-ttu-id="3f4f9-645">Genererar ett fel om index är utanför intervallet.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-645">Throws an error if index is out of bounds.</span></span>

<span data-ttu-id="3f4f9-646">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-646">**Example:**</span></span>  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
<span data-ttu-id="3f4f9-647">Returnerar hello primära e-postadress.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-647">Returns hello primary email address.</span></span>

- - -
### <a name="itemornull"></a><span data-ttu-id="3f4f9-648">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="3f4f9-648">ItemOrNull</span></span>
<span data-ttu-id="3f4f9-649">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-649">**Description:**</span></span>  
<span data-ttu-id="3f4f9-650">Hej ItemOrNull funktionen returnerar ett objekt från ett med flera värden strängattribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-650">hello ItemOrNull function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="3f4f9-651">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-651">**Syntax:**</span></span>  
`var ItemOrNull(mvstr attribute, num index)`

* <span data-ttu-id="3f4f9-652">attributet: flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-652">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="3f4f9-653">index: indexet tooan objekt i flera värden hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-653">index: index tooan item in hello multi-valued string.</span></span>

<span data-ttu-id="3f4f9-654">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-654">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-655">Hej ItemOrNull funktionen är användbart tillsammans med hello innehåller funktionen eftersom hello senare funktionen returnerar hello index tooan objektet i hello flervärdesattribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-655">hello ItemOrNull function is useful together with hello Contains function since hello latter function returns hello index tooan item in hello multi-valued attribute.</span></span>

<span data-ttu-id="3f4f9-656">Om index är utanför intervallet, returnerar du värdet Null.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-656">If index is out of bounds, then returns a Null value.</span></span>

- - -
### <a name="join"></a><span data-ttu-id="3f4f9-657">Slå ihop</span><span class="sxs-lookup"><span data-stu-id="3f4f9-657">Join</span></span>
<span data-ttu-id="3f4f9-658">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-658">**Description:**</span></span>  
<span data-ttu-id="3f4f9-659">hello koppling funktionen en sträng med flera värden och returnerar en enstaka sträng med angiven avgränsare infogas mellan varje element.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-659">hello Join function takes a multi-valued string and returns a single-valued string with specified separator inserted between each item.</span></span>

<span data-ttu-id="3f4f9-660">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-660">**Syntax:**</span></span>  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* <span data-ttu-id="3f4f9-661">attributet: flervärdesattribut som innehåller strängar toobe ansluten.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-661">attribute: Multi-valued attribute containing strings toobe joined.</span></span>
* <span data-ttu-id="3f4f9-662">en avgränsare: någon sträng, används tooseparate hello delsträngar i hello returnerade sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-662">delimiter: Any string, used tooseparate hello substrings in hello returned string.</span></span> <span data-ttu-id="3f4f9-663">Om det utelämnas används hello blanksteg (””) används.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-663">If omitted, hello space character (" ") is used.</span></span> <span data-ttu-id="3f4f9-664">Om en avgränsare för en tom sträng (””) eller något, alla objekt i listan hello sammanfogas med ingen avgränsare.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-664">If Delimiter is a zero-length string ("") or Nothing, all items in hello list are concatenated with no delimiters.</span></span>

<span data-ttu-id="3f4f9-665">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-665">**Remarks**</span></span>  
<span data-ttu-id="3f4f9-666">Det finns paritet mellan hello koppling och dela funktioner.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-666">There is parity between hello Join and Split functions.</span></span> <span data-ttu-id="3f4f9-667">hello funktionen Join tar en matris med strängar och slår ihop dem med hjälp av en avgränsare sträng, tooreturn en enskild textsträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-667">hello Join function takes an array of strings and joins them using a delimiter string, tooreturn a single string.</span></span> <span data-ttu-id="3f4f9-668">hello dela funktionen en sträng och skiljer den på hello avgränsaren, tooreturn en matris med strängar.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-668">hello Split function takes a string and separates it at hello delimiter, tooreturn an array of strings.</span></span> <span data-ttu-id="3f4f9-669">Viktigaste skillnaden är dock att kopplingen kan sammanfoga strängar med valfri avgränsare sträng, dela kan endast separata strängar som använder en avgränsare för en enstaka tecken.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-669">However, a key difference is that Join can concatenate strings with any delimiter string, Split can only separate strings using a single character delimiter.</span></span>

<span data-ttu-id="3f4f9-670">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-670">**Example:**</span></span>  
`Join([proxyAddresses],",")`  
<span data-ttu-id="3f4f9-671">Kan returnera ”:SMTP:john.doe@contoso.com,smtp:jd@contoso.com”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-671">Could return: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span></span>

- - -
### <a name="lcase"></a><span data-ttu-id="3f4f9-672">LCase</span><span class="sxs-lookup"><span data-stu-id="3f4f9-672">LCase</span></span>
<span data-ttu-id="3f4f9-673">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-673">**Description:**</span></span>  
<span data-ttu-id="3f4f9-674">hello funktionen LCase konverterar alla tecken i strängen toolower fall.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-674">hello LCase function converts all characters in a string toolower case.</span></span>

<span data-ttu-id="3f4f9-675">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-675">**Syntax:**</span></span>  
`str LCase(str value)`

<span data-ttu-id="3f4f9-676">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-676">**Example:**</span></span>  
`LCase("TeSt")`  
<span data-ttu-id="3f4f9-677">Returnerar ”test”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-677">Returns "test".</span></span>

- - -
### <a name="left"></a><span data-ttu-id="3f4f9-678">vänster</span><span class="sxs-lookup"><span data-stu-id="3f4f9-678">Left</span></span>
<span data-ttu-id="3f4f9-679">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-679">**Description:**</span></span>  
<span data-ttu-id="3f4f9-680">hello vänstra funktionen returnerar ett angivet antal tecken från hello till vänster i en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-680">hello Left function returns a specified number of characters from hello left of a string.</span></span>

<span data-ttu-id="3f4f9-681">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-681">**Syntax:**</span></span>  
`str Left(str string, num NumChars)`

* <span data-ttu-id="3f4f9-682">sträng: hello sträng tooreturn tecken från</span><span class="sxs-lookup"><span data-stu-id="3f4f9-682">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="3f4f9-683">NumChars: ett tal som identifierar hello antal tecken tooreturn från hello början (vänster) av sträng</span><span class="sxs-lookup"><span data-stu-id="3f4f9-683">NumChars: a number identifying hello number of characters tooreturn from hello beginning (left) of string</span></span>

<span data-ttu-id="3f4f9-684">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-684">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-685">En sträng som innehåller hello första numChars tecken i strängen:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-685">A string containing hello first numChars characters in string:</span></span>

* <span data-ttu-id="3f4f9-686">Om numChars = 0, returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-686">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="3f4f9-687">Om numChars < 0, returnerar Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-687">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="3f4f9-688">Om strängen är null returnera tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-688">If string is null, return empty string.</span></span>

<span data-ttu-id="3f4f9-689">Om strängen innehåller färre tecken än hello antal som anges i numChars, returneras en sträng identiska toostring (som är, som innehåller alla tecken i parameter 1).</span><span class="sxs-lookup"><span data-stu-id="3f4f9-689">If string contains fewer characters than hello number specified in numChars, a string identical toostring (that is, containing all characters in parameter 1) is returned.</span></span>

<span data-ttu-id="3f4f9-690">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-690">**Example:**</span></span>  
`Left("John Doe", 3)`  
<span data-ttu-id="3f4f9-691">Returnerar ”Joh”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-691">Returns "Joh".</span></span>

- - -
### <a name="len"></a><span data-ttu-id="3f4f9-692">Len</span><span class="sxs-lookup"><span data-stu-id="3f4f9-692">Len</span></span>
<span data-ttu-id="3f4f9-693">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-693">**Description:**</span></span>  
<span data-ttu-id="3f4f9-694">hello funktionen längd returnerar antalet tecken i en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-694">hello Len function returns number of characters in a string.</span></span>

<span data-ttu-id="3f4f9-695">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-695">**Syntax:**</span></span>  
`num Len(str value)`

<span data-ttu-id="3f4f9-696">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-696">**Example:**</span></span>  
`Len("John Doe")`  
<span data-ttu-id="3f4f9-697">Returnerar 8</span><span class="sxs-lookup"><span data-stu-id="3f4f9-697">Returns 8</span></span>

- - -
### <a name="ltrim"></a><span data-ttu-id="3f4f9-698">LTrim</span><span class="sxs-lookup"><span data-stu-id="3f4f9-698">LTrim</span></span>
<span data-ttu-id="3f4f9-699">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-699">**Description:**</span></span>  
<span data-ttu-id="3f4f9-700">hello LTrim funktionen tar bort inledande blanksteg från en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-700">hello LTrim function removes leading white spaces from a string.</span></span>

<span data-ttu-id="3f4f9-701">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-701">**Syntax:**</span></span>  
`str LTrim(str value)`

<span data-ttu-id="3f4f9-702">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-702">**Example:**</span></span>  
`LTrim(" Test ")`  
<span data-ttu-id="3f4f9-703">Returnerar ”Test”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-703">Returns "Test "</span></span>

- - -
### <a name="mid"></a><span data-ttu-id="3f4f9-704">Mid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-704">Mid</span></span>
<span data-ttu-id="3f4f9-705">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-705">**Description:**</span></span>  
<span data-ttu-id="3f4f9-706">Hej Mid funktionen returnerar ett angivet antal tecken från en angiven position i en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-706">hello Mid function returns a specified number of characters from a specified position in a string.</span></span>

<span data-ttu-id="3f4f9-707">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-707">**Syntax:**</span></span>  
`str Mid(str string, num start, num NumChars)`

* <span data-ttu-id="3f4f9-708">sträng: hello sträng tooreturn tecken från</span><span class="sxs-lookup"><span data-stu-id="3f4f9-708">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="3f4f9-709">Starta: ett tal som identifierar hello startposition i strängen tooreturn tecken från</span><span class="sxs-lookup"><span data-stu-id="3f4f9-709">start: a number identifying hello starting position in string tooreturn characters from</span></span>
* <span data-ttu-id="3f4f9-710">NumChars: ett tal som identifierar hello antalet tooreturn tecken från position i strängen</span><span class="sxs-lookup"><span data-stu-id="3f4f9-710">NumChars: a number identifying hello number of characters tooreturn from position in string</span></span>

<span data-ttu-id="3f4f9-711">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-711">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-712">Returnera numChars tecken med början från början position i strängen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-712">Return numChars characters starting from position start in string.</span></span>  
<span data-ttu-id="3f4f9-713">En sträng som innehåller numChars tecken från början position i strängen:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-713">A string containing numChars characters from position start in string:</span></span>

* <span data-ttu-id="3f4f9-714">Om numChars = 0, returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-714">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="3f4f9-715">Om numChars < 0, returnerar Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-715">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="3f4f9-716">Om start > Hej strängens längd, returnera Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-716">If start > hello length of string, return input string.</span></span>
* <span data-ttu-id="3f4f9-717">Om starta < = 0, returnera Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-717">If start <= 0, return input string.</span></span>
* <span data-ttu-id="3f4f9-718">Om strängen är null returnera tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-718">If string is null, return empty string.</span></span>

<span data-ttu-id="3f4f9-719">Om det inte numChar tecken returneras i strängen position, så många tecken som möjligt.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-719">If there are not numChar characters remaining in string from position start, as many characters as possible are returned.</span></span>

<span data-ttu-id="3f4f9-720">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-720">**Example:**</span></span>  
`Mid("John Doe", 3, 5)`  
<span data-ttu-id="3f4f9-721">Returnerar ”hn gör”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-721">Returns "hn Do".</span></span>

`Mid("John Doe", 6, 999)`  
<span data-ttu-id="3f4f9-722">Returnerar ”Berg”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-722">Returns "Doe"</span></span>

- - -
### <a name="now"></a><span data-ttu-id="3f4f9-723">Nu</span><span class="sxs-lookup"><span data-stu-id="3f4f9-723">Now</span></span>
<span data-ttu-id="3f4f9-724">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-724">**Description:**</span></span>  
<span data-ttu-id="3f4f9-725">hello nu returnerar funktionen DateTime anger hello aktuellt datum och tid, enligt tooyour datorns datum och klockslag.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-725">hello Now function returns a DateTime specifying hello current date and time, according tooyour computer's system date and time.</span></span>

<span data-ttu-id="3f4f9-726">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-726">**Syntax:**</span></span>  
`dt Now()`

- - -
### <a name="numfromdate"></a><span data-ttu-id="3f4f9-727">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="3f4f9-727">NumFromDate</span></span>
<span data-ttu-id="3f4f9-728">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-728">**Description:**</span></span>  
<span data-ttu-id="3f4f9-729">Hej NumFromDate funktionen returnerar ett datum i Annonsens datumformat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-729">hello NumFromDate function returns a date in AD’s date format.</span></span>

<span data-ttu-id="3f4f9-730">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-730">**Syntax:**</span></span>  
`num NumFromDate(dt value)`

<span data-ttu-id="3f4f9-731">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-731">**Example:**</span></span>  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
<span data-ttu-id="3f4f9-732">Returnerar 129699324000000000</span><span class="sxs-lookup"><span data-stu-id="3f4f9-732">Returns 129699324000000000</span></span>

- - -
### <a name="padleft"></a><span data-ttu-id="3f4f9-733">padLeft</span><span class="sxs-lookup"><span data-stu-id="3f4f9-733">PadLeft</span></span>
<span data-ttu-id="3f4f9-734">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-734">**Description:**</span></span>  
<span data-ttu-id="3f4f9-735">Hej PadLeft fungerar vänster Pad en sträng tooa som angetts med hjälp av angivna utfyllnadstecknet längd.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-735">hello PadLeft function left-pads a string tooa specified length using a provided padding character.</span></span>

<span data-ttu-id="3f4f9-736">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-736">**Syntax:**</span></span>  
`str PadLeft(str string, num length, str padCharacter)`

* <span data-ttu-id="3f4f9-737">sträng: hello sträng toopad.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-737">string: hello string toopad.</span></span>
* <span data-ttu-id="3f4f9-738">längd: ett heltal som representerar hello önskad strängens längd.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-738">length: An integer representing hello desired length of string.</span></span>
* <span data-ttu-id="3f4f9-739">padCharacter: en sträng som består av ett enskilt tecken toouse som hello pad tecken</span><span class="sxs-lookup"><span data-stu-id="3f4f9-739">padCharacter: A string consisting of a single character toouse as hello pad character</span></span>

<span data-ttu-id="3f4f9-740">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-740">**Remarks:**</span></span>

* <span data-ttu-id="3f4f9-741">Om hello längden på strängen är mindre än längden, är padCharacter upprepade gånger tillagda toohello början (vänster) av strängen förrän den har en lika lång toolength.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-741">If hello length of string is less than length, then padCharacter is repeatedly appended toohello beginning (left) of string until it has a length equal toolength.</span></span>
* <span data-ttu-id="3f4f9-742">padCharacter kan vara ett blanksteg, men den kan inte vara ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-742">PadCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="3f4f9-743">Om hello längden på strängen är lika tooor som är större än längden, returneras strängen oförändrat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-743">If hello length of string is equal tooor greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="3f4f9-744">Om strängen har en längd som är större än eller lika toolength, returneras en identisk toostring sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-744">If string has a length greater than or equal toolength, a string identical toostring is returned.</span></span>
* <span data-ttu-id="3f4f9-745">Om hello längden på strängen är mindre än längden, önskade en ny sträng av hello för längd returneras som innehåller strängen fylls ut med en padCharacter.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-745">If hello length of string is less than length, then a new string of hello desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="3f4f9-746">Om strängen är null, returneras hello en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-746">If string is null, hello function returns an empty string.</span></span>

<span data-ttu-id="3f4f9-747">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-747">**Example:**</span></span>  
`PadLeft("User", 10, "0")`  
<span data-ttu-id="3f4f9-748">Returnerar ”000000User”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-748">Returns "000000User".</span></span>

- - -
### <a name="padright"></a><span data-ttu-id="3f4f9-749">PadRight</span><span class="sxs-lookup"><span data-stu-id="3f4f9-749">PadRight</span></span>
<span data-ttu-id="3f4f9-750">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-750">**Description:**</span></span>  
<span data-ttu-id="3f4f9-751">Hej PadRight fungerar höger Pad en sträng tooa som angetts med hjälp av angivna utfyllnadstecknet längd.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-751">hello PadRight function right-pads a string tooa specified length using a provided padding character.</span></span>

<span data-ttu-id="3f4f9-752">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-752">**Syntax:**</span></span>  
`str PadRight(str string, num length, str padCharacter)`

* <span data-ttu-id="3f4f9-753">sträng: hello sträng toopad.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-753">string: hello string toopad.</span></span>
* <span data-ttu-id="3f4f9-754">längd: ett heltal som representerar hello önskad strängens längd.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-754">length: An integer representing hello desired length of string.</span></span>
* <span data-ttu-id="3f4f9-755">padCharacter: en sträng som består av ett enskilt tecken toouse som hello pad tecken</span><span class="sxs-lookup"><span data-stu-id="3f4f9-755">padCharacter: A string consisting of a single character toouse as hello pad character</span></span>

<span data-ttu-id="3f4f9-756">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-756">**Remarks:**</span></span>

* <span data-ttu-id="3f4f9-757">Om hello längden på strängen är mindre än längden, är padCharacter upprepade gånger tillagda toohello slutet (höger) av strängen förrän den har en lika lång toolength.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-757">If hello length of string is less than length, then padCharacter is repeatedly appended toohello end (right) of string until it has a length equal toolength.</span></span>
* <span data-ttu-id="3f4f9-758">padCharacter kan vara ett blanksteg, men den kan inte vara ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-758">padCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="3f4f9-759">Om hello längden på strängen är lika tooor som är större än längden, returneras strängen oförändrat.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-759">If hello length of string is equal tooor greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="3f4f9-760">Om strängen har en längd som är större än eller lika toolength, returneras en identisk toostring sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-760">If string has a length greater than or equal toolength, a string identical toostring is returned.</span></span>
* <span data-ttu-id="3f4f9-761">Om hello längden på strängen är mindre än längden, önskade en ny sträng av hello för längd returneras som innehåller strängen fylls ut med en padCharacter.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-761">If hello length of string is less than length, then a new string of hello desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="3f4f9-762">Om strängen är null, returneras hello en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-762">If string is null, hello function returns an empty string.</span></span>

<span data-ttu-id="3f4f9-763">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-763">**Example:**</span></span>  
`PadRight("User", 10, "0")`  
<span data-ttu-id="3f4f9-764">Returnerar ”User000000”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-764">Returns "User000000".</span></span>

- - -
### <a name="pcase"></a><span data-ttu-id="3f4f9-765">PCase</span><span class="sxs-lookup"><span data-stu-id="3f4f9-765">PCase</span></span>
<span data-ttu-id="3f4f9-766">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-766">**Description:**</span></span>  
<span data-ttu-id="3f4f9-767">Hej PCase funktionen konverterar hello första tecknet i varje blankstegsavgränsad ord i strängen tooupper fall och alla andra tecken konverteras toolower fallet.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-767">hello PCase function converts hello first character of each space delimited word in a string tooupper case, and all other characters are converted toolower case.</span></span>

<span data-ttu-id="3f4f9-768">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-768">**Syntax:**</span></span>  
`String PCase(string)`

<span data-ttu-id="3f4f9-769">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-769">**Remarks:**</span></span>

* <span data-ttu-id="3f4f9-770">Den här funktionen innehåller för närvarande inte rätt skiftläge tooconvert ett ord som är helt versaler, till exempel en förkortning.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-770">This function does not currently provide proper casing tooconvert a word that is entirely uppercase, such as an acronym.</span></span>

<span data-ttu-id="3f4f9-771">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-771">**Example:**</span></span>  
`PCase("TEsT")`  
<span data-ttu-id="3f4f9-772">Returnerar ”Test”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-772">Returns "Test".</span></span>

`PCase(LCase("TEST"))`  
<span data-ttu-id="3f4f9-773">Returnerar ”Test”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-773">Returns "Test"</span></span>

- - -
### <a name="randomnum"></a><span data-ttu-id="3f4f9-774">RandomNum</span><span class="sxs-lookup"><span data-stu-id="3f4f9-774">RandomNum</span></span>
<span data-ttu-id="3f4f9-775">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-775">**Description:**</span></span>  
<span data-ttu-id="3f4f9-776">Hej RandomNum funktionen returnerar ett slumptal mellan ett visst intervall.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-776">hello RandomNum function returns a random number between a specified interval.</span></span>

<span data-ttu-id="3f4f9-777">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-777">**Syntax:**</span></span>  
`num RandomNum(num start, num end)`

* <span data-ttu-id="3f4f9-778">Starta: antalet identifierande hello nedre gräns hello slumpmässigt värde toogenerate</span><span class="sxs-lookup"><span data-stu-id="3f4f9-778">start: a number identifying hello lower limit of hello random value toogenerate</span></span>
* <span data-ttu-id="3f4f9-779">End: ett tal identifierande hello övre gränsen för hello slumpmässigt värde toogenerate</span><span class="sxs-lookup"><span data-stu-id="3f4f9-779">end: a number identifying hello upper limit of hello random value toogenerate</span></span>

<span data-ttu-id="3f4f9-780">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-780">**Example:**</span></span>  
`Random(100,999)`  
<span data-ttu-id="3f4f9-781">Returnera 734.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-781">Can return 734.</span></span>

- - -
### <a name="removeduplicates"></a><span data-ttu-id="3f4f9-782">RemoveDuplicates</span><span class="sxs-lookup"><span data-stu-id="3f4f9-782">RemoveDuplicates</span></span>
<span data-ttu-id="3f4f9-783">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-783">**Description:**</span></span>  
<span data-ttu-id="3f4f9-784">Hej RemoveDuplicates funktionen en sträng med flera värden och kontrollera att varje värde är unikt.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-784">hello RemoveDuplicates function takes a multi-valued string and make sure each value is unique.</span></span>

<span data-ttu-id="3f4f9-785">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-785">**Syntax:**</span></span>  
`mvstr RemoveDuplicates(mvstr attribute)`

<span data-ttu-id="3f4f9-786">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-786">**Example:**</span></span>  
`RemoveDuplicates([proxyAddresses])`  
<span data-ttu-id="3f4f9-787">Returnerar ett språkoberoende proxyAddress attribut där alla dubblettvärden har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-787">Returns a sanitized proxyAddress attribute where all duplicate values have been removed.</span></span>

- - -
### <a name="replace"></a><span data-ttu-id="3f4f9-788">Ersätt</span><span class="sxs-lookup"><span data-stu-id="3f4f9-788">Replace</span></span>
<span data-ttu-id="3f4f9-789">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-789">**Description:**</span></span>  
<span data-ttu-id="3f4f9-790">hello Ersätt funktionen ersätter alla förekomster av en sträng tooanother sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-790">hello Replace function replaces all occurrences of a string tooanother string.</span></span>

<span data-ttu-id="3f4f9-791">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-791">**Syntax:**</span></span>  
`str Replace(str string, str OldValue, str NewValue)`

* <span data-ttu-id="3f4f9-792">sträng: en sträng tooreplace som värden i.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-792">string: A string tooreplace values in.</span></span>
* <span data-ttu-id="3f4f9-793">OldValue: hello sträng toosearch för och tooreplace.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-793">OldValue: hello string toosearch for and tooreplace.</span></span>
* <span data-ttu-id="3f4f9-794">NewValue: hello sträng tooreplace till.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-794">NewValue: hello string tooreplace to.</span></span>

<span data-ttu-id="3f4f9-795">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-795">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-796">hello funktionen identifierar hello följa särskilda monikrar:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-796">hello function recognizes hello following special monikers:</span></span>

* <span data-ttu-id="3f4f9-797">\n – ny rad</span><span class="sxs-lookup"><span data-stu-id="3f4f9-797">\n – New Line</span></span>
* <span data-ttu-id="3f4f9-798">\r – vagnretur</span><span class="sxs-lookup"><span data-stu-id="3f4f9-798">\r – Carriage Return</span></span>
* <span data-ttu-id="3f4f9-799">\t – fliken</span><span class="sxs-lookup"><span data-stu-id="3f4f9-799">\t – Tab</span></span>

<span data-ttu-id="3f4f9-800">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-800">**Example:**</span></span>  
`Replace([address],"\r\n",", ")`  
<span data-ttu-id="3f4f9-801">Ersätter CRLF med ett kommatecken och ett blanksteg och leda för ”en Microsoft sätt, Redmond, WA, USA”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-801">Replaces CRLF with a comma and space, and could lead too"One Microsoft Way, Redmond, WA, USA"</span></span>

- - -
### <a name="replacechars"></a><span data-ttu-id="3f4f9-802">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="3f4f9-802">ReplaceChars</span></span>
<span data-ttu-id="3f4f9-803">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-803">**Description:**</span></span>  
<span data-ttu-id="3f4f9-804">Hej ReplaceChars funktionen ersätter alla förekomster av tecken hittades i hello ReplacePattern sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-804">hello ReplaceChars function replaces all occurrences of characters found in hello ReplacePattern string.</span></span>

<span data-ttu-id="3f4f9-805">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-805">**Syntax:**</span></span>  
`str ReplaceChars(str string, str ReplacePattern)`

* <span data-ttu-id="3f4f9-806">sträng: en sträng tooreplace tecken.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-806">string: A string tooreplace characters in.</span></span>
* <span data-ttu-id="3f4f9-807">ReplacePattern: en sträng som innehåller en ordlista med tooreplace tecken.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-807">ReplacePattern: a string containing a dictionary with characters tooreplace.</span></span>

<span data-ttu-id="3f4f9-808">hello-formatet är {källa1}: {target1}, {källa2}: {target2}, {källan}, {targetN} där källan är hello tecken och mål för toofind hello sträng tooreplace med.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-808">hello format is {source1}:{target1},{source2}:{target2},{sourceN},{targetN} where source is hello character toofind and target hello string tooreplace with.</span></span>

<span data-ttu-id="3f4f9-809">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-809">**Remarks:**</span></span>

* <span data-ttu-id="3f4f9-810">hello funktionen tar för varje förekomst av definierade källor och ersätter dem med hello mål.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-810">hello function takes each occurrence of defined sources and replaces them with hello targets.</span></span>
* <span data-ttu-id="3f4f9-811">hello källan måste vara exakt ett tecken för (unicode).</span><span class="sxs-lookup"><span data-stu-id="3f4f9-811">hello source must be exactly one (unicode) character.</span></span>
* <span data-ttu-id="3f4f9-812">hello källa vara inte tomt eller längre än ett tecken (tolkningsfel).</span><span class="sxs-lookup"><span data-stu-id="3f4f9-812">hello source cannot be empty or longer than one character (parsing error).</span></span>
* <span data-ttu-id="3f4f9-813">hello mål kan ha flera tecken, till exempel ö:oe, β:ss.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-813">hello target can have multiple characters, for example ö:oe, β:ss.</span></span>
* <span data-ttu-id="3f4f9-814">hello mål kan vara tomt som anger att hello tecken ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-814">hello target can be empty indicating that hello character should be removed.</span></span>
* <span data-ttu-id="3f4f9-815">hello-källa är skiftlägeskänsligt och måste vara en exakt matchning.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-815">hello source is case-sensitive and must be an exact match.</span></span>
* <span data-ttu-id="3f4f9-816">hello, (semikolonavgränsad) och: (kolon) är reserverade tecken som inte kan ersättas med den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-816">hello , (comma) and : (colon) are reserved characters and cannot be replaced using this function.</span></span>
* <span data-ttu-id="3f4f9-817">Blanksteg och andra vit tecken i hello ReplacePattern strängen ignoreras.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-817">Spaces and other white characters in hello ReplacePattern string are ignored.</span></span>

<span data-ttu-id="3f4f9-818">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-818">**Example:**</span></span>  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
<span data-ttu-id="3f4f9-819">Returnerar Raksmorgas</span><span class="sxs-lookup"><span data-stu-id="3f4f9-819">Returns Raksmorgas</span></span>

`ReplaceChars("O’Neil",%ReplaceString%)`  
<span data-ttu-id="3f4f9-820">Returnerar ”ONeil”, hello markering är definierad toobe tas bort.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-820">Returns "ONeil", hello single tick is defined toobe removed.</span></span>

- - -
### <a name="right"></a><span data-ttu-id="3f4f9-821">Höger</span><span class="sxs-lookup"><span data-stu-id="3f4f9-821">Right</span></span>
<span data-ttu-id="3f4f9-822">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-822">**Description:**</span></span>  
<span data-ttu-id="3f4f9-823">hello rätt funktion returnerar ett angivet antal tecken från hello rätt (end) i en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-823">hello Right function returns a specified number of characters from hello right (end) of a string.</span></span>

<span data-ttu-id="3f4f9-824">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-824">**Syntax:**</span></span>  
`str Right(str string, num NumChars)`

* <span data-ttu-id="3f4f9-825">sträng: hello sträng tooreturn tecken från</span><span class="sxs-lookup"><span data-stu-id="3f4f9-825">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="3f4f9-826">NumChars: ett tal som identifierar hello antal tecken tooreturn från hello slutpunkt (höger) av sträng</span><span class="sxs-lookup"><span data-stu-id="3f4f9-826">NumChars: a number identifying hello number of characters tooreturn from hello end (right) of string</span></span>

<span data-ttu-id="3f4f9-827">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-827">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-828">NumChars tecken som ska returneras från hello senaste position i strängen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-828">NumChars characters are returned from hello last position of string.</span></span>

<span data-ttu-id="3f4f9-829">En sträng som innehåller hello senaste numChars tecken i strängen:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-829">A string containing hello last numChars characters in string:</span></span>

* <span data-ttu-id="3f4f9-830">Om numChars = 0, returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-830">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="3f4f9-831">Om numChars < 0, returnerar Indatasträngen.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-831">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="3f4f9-832">Om strängen är null returnera tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-832">If string is null, return empty string.</span></span>

<span data-ttu-id="3f4f9-833">Om strängen innehåller färre tecken än hello nummer anges i NumChars, returneras en identisk toostring sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-833">If string contains fewer characters than hello number specified in NumChars, a string identical toostring is returned.</span></span>

<span data-ttu-id="3f4f9-834">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-834">**Example:**</span></span>  
`Right("John Doe", 3)`  
<span data-ttu-id="3f4f9-835">Returnerar ”Berg”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-835">Returns "Doe".</span></span>

- - -
### <a name="rtrim"></a><span data-ttu-id="3f4f9-836">RTrim</span><span class="sxs-lookup"><span data-stu-id="3f4f9-836">RTrim</span></span>
<span data-ttu-id="3f4f9-837">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-837">**Description:**</span></span>  
<span data-ttu-id="3f4f9-838">hello RTrim funktionen tar bort avslutande blanksteg från en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-838">hello RTrim function removes trailing white spaces from a string.</span></span>

<span data-ttu-id="3f4f9-839">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-839">**Syntax:**</span></span>  
`str RTrim(str value)`

<span data-ttu-id="3f4f9-840">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-840">**Example:**</span></span>  
`RTrim(" Test ")`  
<span data-ttu-id="3f4f9-841">Returnerar ”Test”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-841">Returns " Test".</span></span>

- - -
### <a name="select"></a><span data-ttu-id="3f4f9-842">Välj</span><span class="sxs-lookup"><span data-stu-id="3f4f9-842">Select</span></span>
<span data-ttu-id="3f4f9-843">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-843">**Description:**</span></span>  
<span data-ttu-id="3f4f9-844">Processen för alla värden i en flervärdesfält attribut (eller utdata för ett uttryck) baserat på funktionen som anges.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-844">Process all values in a multi-valued attribute (or output of an expression) based on function specified.</span></span>

<span data-ttu-id="3f4f9-845">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-845">**Syntax:**</span></span>  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* <span data-ttu-id="3f4f9-846">objektet: representerar ett element i hello flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-846">item: Represents an element in hello multi-valued attribute</span></span>
* <span data-ttu-id="3f4f9-847">attributet: hello flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-847">attribute: hello multi-valued attribute</span></span>
* <span data-ttu-id="3f4f9-848">uttryck: ett uttryck som returnerar en mängd med värden</span><span class="sxs-lookup"><span data-stu-id="3f4f9-848">expression: an expression that returns a collection of values</span></span>
* <span data-ttu-id="3f4f9-849">villkor: en funktion som kan bearbeta ett objekt i hello attribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-849">condition: any function that can process an item in hello attribute</span></span>

<span data-ttu-id="3f4f9-850">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-850">**Examples:**</span></span>  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
<span data-ttu-id="3f4f9-851">Returnera alla hello-värden i hello flervärdesattribut Annantelefon när bindestreck (-) har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-851">Return all hello values in hello multi-valued attribute otherPhone after hyphens (-) have been removed.</span></span>

- - -
### <a name="split"></a><span data-ttu-id="3f4f9-852">Dela</span><span class="sxs-lookup"><span data-stu-id="3f4f9-852">Split</span></span>
<span data-ttu-id="3f4f9-853">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-853">**Description:**</span></span>  
<span data-ttu-id="3f4f9-854">hello dela funktionen en sträng som avgränsas med en avgränsare och gör det en sträng med flera värden.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-854">hello Split function takes a string separated with a delimiter and makes it a multi-valued string.</span></span>

<span data-ttu-id="3f4f9-855">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-855">**Syntax:**</span></span>  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* <span data-ttu-id="3f4f9-856">värde: sträng med en avgränsare tecken tooseparate hello.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-856">value: hello string with a delimiter character tooseparate.</span></span>
* <span data-ttu-id="3f4f9-857">en avgränsare: enkel tecken toobe används som hello avgränsare.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-857">delimiter: single character toobe used as hello delimiter.</span></span>
* <span data-ttu-id="3f4f9-858">gränsen: maximala antalet värden som kan returnera.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-858">limit: maximum number of values that can return.</span></span>

<span data-ttu-id="3f4f9-859">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-859">**Example:**</span></span>  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
<span data-ttu-id="3f4f9-860">Returnerar en sträng som har flera värden med 2 element användbart för hello proxyAddress attribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-860">Returns a multi-valued string with 2 elements useful for hello proxyAddress attribute.</span></span>

- - -
### <a name="stringfromguid"></a><span data-ttu-id="3f4f9-861">GUIDFromString</span><span class="sxs-lookup"><span data-stu-id="3f4f9-861">StringFromGuid</span></span>
<span data-ttu-id="3f4f9-862">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-862">**Description:**</span></span>  
<span data-ttu-id="3f4f9-863">hello GUIDFromString funktionen tar ett binärt GUID och konverterar den tooa sträng</span><span class="sxs-lookup"><span data-stu-id="3f4f9-863">hello StringFromGuid function takes a binary GUID and converts it tooa string</span></span>

<span data-ttu-id="3f4f9-864">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-864">**Syntax:**</span></span>  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a><span data-ttu-id="3f4f9-865">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="3f4f9-865">StringFromSid</span></span>
<span data-ttu-id="3f4f9-866">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-866">**Description:**</span></span>  
<span data-ttu-id="3f4f9-867">Hej StringFromSid funktionen konverterar en bytematris som innehåller en security identifier tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-867">hello StringFromSid function converts a byte array containing a security identifier tooa string.</span></span>

<span data-ttu-id="3f4f9-868">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-868">**Syntax:**</span></span>  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a><span data-ttu-id="3f4f9-869">Växel</span><span class="sxs-lookup"><span data-stu-id="3f4f9-869">Switch</span></span>
<span data-ttu-id="3f4f9-870">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-870">**Description:**</span></span>  
<span data-ttu-id="3f4f9-871">hello växelfunktionen är tooreturn används ett värde baserat på utvärderade villkoren.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-871">hello Switch function is used tooreturn a single value based on evaluated conditions.</span></span>

<span data-ttu-id="3f4f9-872">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-872">**Syntax:**</span></span>  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* <span data-ttu-id="3f4f9-873">uttryck: Variant-uttryck som du vill använda tooevaluate.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-873">expr: Variant expression you want tooevaluate.</span></span>
* <span data-ttu-id="3f4f9-874">värde: värdet toobe returneras om hello motsvarande uttryck är True.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-874">value: Value toobe returned if hello corresponding expression is True.</span></span>

<span data-ttu-id="3f4f9-875">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-875">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-876">hello växeln funktionsargument listan består av par med uttryck och värden.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-876">hello Switch function argument list consists of pairs of expressions and values.</span></span> <span data-ttu-id="3f4f9-877">hello uttryck utvärderas från vänster tooright och hello-värde som är associerade med hello första uttryck tooevaluate tooTrue returneras.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-877">hello expressions are evaluated from left tooright, and hello value associated with hello first expression tooevaluate tooTrue is returned.</span></span> <span data-ttu-id="3f4f9-878">Om hello delar inte är korrekt paras ihop, inträffar ett fel under körning.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-878">If hello parts aren't properly paired, a run-time error occurs.</span></span>

<span data-ttu-id="3f4f9-879">Om Uttr1 är SANT returnerar växeln value1.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-879">For example, if expr1 is True, Switch returns value1.</span></span> <span data-ttu-id="3f4f9-880">Om uttryck-1 är False, men uttryck-2 är sant, returnerar växeln värdet 2 och så vidare.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-880">If expr-1 is False, but expr-2 is True, Switch returns value-2, and so on.</span></span>

<span data-ttu-id="3f4f9-881">Växeln returnerar en ingenting om:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-881">Switch returns a Nothing if:</span></span>

* <span data-ttu-id="3f4f9-882">Ingen hälsningspaket uttryck är True.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-882">None of hello expressions are True.</span></span>
* <span data-ttu-id="3f4f9-883">hello första SANT uttrycket har ett motsvarande värde som är Null.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-883">hello first True expression has a corresponding value that is Null.</span></span>

<span data-ttu-id="3f4f9-884">Växeln utvärderar alla uttryck, även om den returnerar endast en av dem.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-884">Switch evaluates all expressions, even though it returns only one of them.</span></span> <span data-ttu-id="3f4f9-885">Därför bör du titta på för oönskade sidoeffekter.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-885">For this reason, you should watch for undesirable side effects.</span></span> <span data-ttu-id="3f4f9-886">Hello utvärderingen av ett uttryck som resulterar i en division med noll-fel, inträffar ett fel.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-886">For example, if hello evaluation of any expression results in a division by zero error, an error occurs.</span></span>

<span data-ttu-id="3f4f9-887">Värdet kan också vara hello fel funktion som returnerar en anpassad sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-887">Value can also be hello Error function, which would return a custom string.</span></span>

<span data-ttu-id="3f4f9-888">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-888">**Example:**</span></span>  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
<span data-ttu-id="3f4f9-889">Returnerar hello-språk som talas i vissa större orter, annars returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-889">Returns hello language spoken in some major cities, otherwise returns an Error.</span></span>

- - -
### <a name="trim"></a><span data-ttu-id="3f4f9-890">Rensa</span><span class="sxs-lookup"><span data-stu-id="3f4f9-890">Trim</span></span>
<span data-ttu-id="3f4f9-891">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-891">**Description:**</span></span>  
<span data-ttu-id="3f4f9-892">Rensa hello-funktionen tar bort inledande och avslutande blanksteg från en sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-892">hello Trim function removes leading and trailing white spaces from a string.</span></span>

<span data-ttu-id="3f4f9-893">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-893">**Syntax:**</span></span>  
`str Trim(str value)`  

<span data-ttu-id="3f4f9-894">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-894">**Example:**</span></span>  
`Trim(" Test ")`  
<span data-ttu-id="3f4f9-895">Returnerar ”Test”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-895">Returns "Test".</span></span>

`Trim([proxyAddresses])`  
<span data-ttu-id="3f4f9-896">Tar bort inledande och avslutande blanksteg för varje värde i hello proxyAddress attribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-896">Removes leading and trailing spaces for each value in hello proxyAddress attribute.</span></span>

- - -
### <a name="ucase"></a><span data-ttu-id="3f4f9-897">UCase</span><span class="sxs-lookup"><span data-stu-id="3f4f9-897">UCase</span></span>
<span data-ttu-id="3f4f9-898">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-898">**Description:**</span></span>  
<span data-ttu-id="3f4f9-899">hello funktionen UCase konverterar alla tecken i strängen tooupper fall.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-899">hello UCase function converts all characters in a string tooupper case.</span></span>

<span data-ttu-id="3f4f9-900">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-900">**Syntax:**</span></span>  
`str UCase(str string)`

<span data-ttu-id="3f4f9-901">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-901">**Example:**</span></span>  
`UCase("TeSt")`  
<span data-ttu-id="3f4f9-902">Returnerar ”TEST”.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-902">Returns "TEST".</span></span>

- - -
### <a name="where"></a><span data-ttu-id="3f4f9-903">där</span><span class="sxs-lookup"><span data-stu-id="3f4f9-903">Where</span></span>

<span data-ttu-id="3f4f9-904">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-904">**Description:**</span></span>  
<span data-ttu-id="3f4f9-905">Returnerar en delmängd av värden från ett med flera värden attribut (eller utdata för ett uttryck) som baserat på specifika villkor.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-905">Returns a subset of values from a multi-valued attribute (or output of an expression) based on specific condition.</span></span>

<span data-ttu-id="3f4f9-906">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-906">**Syntax:**</span></span>  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* <span data-ttu-id="3f4f9-907">objektet: representerar ett element i hello flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-907">item: Represents an element in hello multi-valued attribute</span></span>
* <span data-ttu-id="3f4f9-908">attributet: hello flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="3f4f9-908">attribute: hello multi-valued attribute</span></span>
* <span data-ttu-id="3f4f9-909">villkor: ett uttryck som kan utvärderas tootrue eller false</span><span class="sxs-lookup"><span data-stu-id="3f4f9-909">condition: any expression that can be evaluated tootrue or false</span></span>
* <span data-ttu-id="3f4f9-910">uttryck: ett uttryck som returnerar en mängd med värden</span><span class="sxs-lookup"><span data-stu-id="3f4f9-910">expression: an expression that returns a collection of values</span></span>

<span data-ttu-id="3f4f9-911">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-911">**Example:**</span></span>  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
<span data-ttu-id="3f4f9-912">Returvärden hello certifikat i hello flervärdesattribut userCertificate som inte har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-912">Return hello certificate values in hello multi-valued attribute userCertificate which aren’t expired.</span></span>

- - -
### <a name="with"></a><span data-ttu-id="3f4f9-913">med</span><span class="sxs-lookup"><span data-stu-id="3f4f9-913">With</span></span>
<span data-ttu-id="3f4f9-914">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-914">**Description:**</span></span>  
<span data-ttu-id="3f4f9-915">hello med funktionen ger ett sätt toosimplify ett komplext uttryck med hjälp av en variabel toorepresent ett deluttryck som visas en eller flera gånger i hello komplext uttryck.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-915">hello With function provides a way toosimplify a complex expression by using a variable toorepresent a subexpression which appears one or more times in hello complex expression.</span></span>

<span data-ttu-id="3f4f9-916">**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`</span><span class="sxs-lookup"><span data-stu-id="3f4f9-916">**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`</span></span>  
* <span data-ttu-id="3f4f9-917">variabel: representerar hello underuttryck.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-917">variable: Represents hello subexpression.</span></span>
* <span data-ttu-id="3f4f9-918">underuttryck: deluttryck som representeras av variabeln.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-918">subExpression: subexpression represented by variable.</span></span>
* <span data-ttu-id="3f4f9-919">complexExpression: ett komplext uttryck.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-919">complexExpression: A complex expression.</span></span>

<span data-ttu-id="3f4f9-920">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-920">**Example:**</span></span>  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
<span data-ttu-id="3f4f9-921">Har samma funktioner som:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-921">Is functionally equivalent to:</span></span>  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
<span data-ttu-id="3f4f9-922">Som returnerar endast återstående certifikat värden i hello userCertificate attribut.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-922">Which returns only unexpired certificate values in hello userCertificate attribute.</span></span>


- - -
### <a name="word"></a><span data-ttu-id="3f4f9-923">Word</span><span class="sxs-lookup"><span data-stu-id="3f4f9-923">Word</span></span>
<span data-ttu-id="3f4f9-924">**Beskrivning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-924">**Description:**</span></span>  
<span data-ttu-id="3f4f9-925">hello funktion returnerar ett ord som ingår i en sträng, baserat på parametrarna som beskriver hello avgränsare toouse och hello word nummer tooreturn.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-925">hello Word function returns a word contained within a string, based on parameters describing hello delimiters toouse and hello word number tooreturn.</span></span>

<span data-ttu-id="3f4f9-926">**Syntax:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-926">**Syntax:**</span></span>  
`str Word(str string, num WordNumber, str delimiters)`

* <span data-ttu-id="3f4f9-927">sträng: hello sträng tooreturn ett ord från.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-927">string: hello string tooreturn a word from.</span></span>
* <span data-ttu-id="3f4f9-928">WordNumber: ett tal som identifierar vilka word-numret ska returnera.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-928">WordNumber: a number identifying which word number should return.</span></span>
* <span data-ttu-id="3f4f9-929">avgränsare: en sträng som representerar hello delimiter(s) som ska använda tooidentify ord</span><span class="sxs-lookup"><span data-stu-id="3f4f9-929">delimiters: a string representing hello delimiter(s) that should be used tooidentify words</span></span>

<span data-ttu-id="3f4f9-930">**Anmärkning:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-930">**Remarks:**</span></span>  
<span data-ttu-id="3f4f9-931">Varje sträng med tecken i strängen avgränsade med hello hello tecken i avgränsare identifieras som ord:</span><span class="sxs-lookup"><span data-stu-id="3f4f9-931">Each string of characters in string separated by hello one of hello characters in delimiters are identified as words:</span></span>

* <span data-ttu-id="3f4f9-932">Om number < 1, returnerar tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-932">If number < 1, returns empty string.</span></span>
* <span data-ttu-id="3f4f9-933">Om strängen är null returnerar tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-933">If string is null, returns empty string.</span></span>

<span data-ttu-id="3f4f9-934">Om strängen innehåller mindre än antalet ord eller strängen innehåller inte några ord som identifieras av avgränsare, returneras en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="3f4f9-934">If string contains less than number words, or string does not contain any words identified by delimiters, an empty string is returned.</span></span>

<span data-ttu-id="3f4f9-935">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3f4f9-935">**Example:**</span></span>  
`Word("hello quick brown fox",3," ")`  
<span data-ttu-id="3f4f9-936">Returnerar ”Jansson”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-936">Returns "brown"</span></span>

`Word("This,string!has&many separators",3,",!&#")`  
<span data-ttu-id="3f4f9-937">Returnerar ”har”</span><span class="sxs-lookup"><span data-stu-id="3f4f9-937">Would return "has"</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f4f9-938">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3f4f9-938">Additional Resources</span></span>
* [<span data-ttu-id="3f4f9-939">Förstå uttryck för deklarativ etablering</span><span class="sxs-lookup"><span data-stu-id="3f4f9-939">Understanding Declarative Provisioning Expressions</span></span>](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [<span data-ttu-id="3f4f9-940">Azure AD Connect Sync: Anpassa synkroniseringsalternativ</span><span class="sxs-lookup"><span data-stu-id="3f4f9-940">Azure AD Connect Sync: Customizing Synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="3f4f9-941">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3f4f9-941">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
