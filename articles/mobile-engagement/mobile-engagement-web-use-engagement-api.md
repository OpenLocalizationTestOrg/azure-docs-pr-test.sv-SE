---
title: 'Azure Mobile Engagement Web API: er SDK | Microsoft Docs'
description: "De senaste uppdateringarna och procedurer för webbtjänst-SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8a87d5ac-d8b7-4a0d-bdee-414dbcc561b2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: 54c22ce6a03e382b1bbde102bccc97deec249b30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a><span data-ttu-id="4ea46-103">Använd Azure Mobile Engagement-API: et i ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="4ea46-103">Use the Azure Mobile Engagement API in a web application</span></span>
<span data-ttu-id="4ea46-104">Det här dokumentet är ett tillägg till det dokument som får du veta hur till [Mobile Engagement ska integreras i ett webbprogram](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="4ea46-104">This document is an addition to the document that tells you how to [integrate Mobile Engagement in a web application](mobile-engagement-web-integrate-engagement.md).</span></span> <span data-ttu-id="4ea46-105">Det ger detaljerad information om hur du använder Azure Mobile Engagement-API: et för att rapportera programmet-statistik.</span><span class="sxs-lookup"><span data-stu-id="4ea46-105">It provides in-depth details about how to use the Azure Mobile Engagement API to report your application statistics.</span></span>

<span data-ttu-id="4ea46-106">Mobile Engagement-API som tillhandahålls av den `engagement.agent` objekt.</span><span class="sxs-lookup"><span data-stu-id="4ea46-106">The Mobile Engagement API is provided by the `engagement.agent` object.</span></span> <span data-ttu-id="4ea46-107">Standard Azure Mobile Engagement Web SDK alias är `engagement`.</span><span class="sxs-lookup"><span data-stu-id="4ea46-107">The default Azure Mobile Engagement Web SDK alias is `engagement`.</span></span> <span data-ttu-id="4ea46-108">Du kan ändra detta alias från SDK-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="4ea46-108">You can redefine this alias from the SDK configuration.</span></span>

## <a name="mobile-engagement-concepts"></a><span data-ttu-id="4ea46-109">Koncept i Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="4ea46-109">Mobile Engagement concepts</span></span>
<span data-ttu-id="4ea46-110">Följande delar förfina vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md) för webbplattformen.</span><span class="sxs-lookup"><span data-stu-id="4ea46-110">The following parts refine common [Mobile Engagement concepts](mobile-engagement-concepts.md) for the web platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="4ea46-111">`Session` och `Activity`</span><span class="sxs-lookup"><span data-stu-id="4ea46-111">`Session` and `Activity`</span></span>
<span data-ttu-id="4ea46-112">Om användaren är inaktiv i mer än ett par sekunder mellan två aktiviteter, är användarens sekvensen av aktiviteter uppdelad i två olika sessioner.</span><span class="sxs-lookup"><span data-stu-id="4ea46-112">If the user stays idle for more than a few seconds between two activities, the user's sequence of activities is split into two distinct sessions.</span></span> <span data-ttu-id="4ea46-113">Dessa några sekunder kallas tidsgränsen för sessionen.</span><span class="sxs-lookup"><span data-stu-id="4ea46-113">These few seconds are called the session timeout.</span></span>

<span data-ttu-id="4ea46-114">Om ditt webbprogram inte deklarera slutet av användaraktiviteter ensamt (genom att anropa den `engagement.agent.endActivity` funktionen), Mobile Engagement-servern automatiskt upphör att gälla inom tre minuter efter appen på sidan stängs sessionen.</span><span class="sxs-lookup"><span data-stu-id="4ea46-114">If your web application doesn't declare the end of user activities by itself (by calling the `engagement.agent.endActivity` function), the Mobile Engagement server automatically expires the user session within three minutes after the application page is closed.</span></span> <span data-ttu-id="4ea46-115">Detta kallas för sessionstidsgränsen server.</span><span class="sxs-lookup"><span data-stu-id="4ea46-115">This is called the server session timeout.</span></span>

### `Crash`
<span data-ttu-id="4ea46-116">Automatisk rapporter om undantagsfel utan felhantering JavaScript skapas inte som standard.</span><span class="sxs-lookup"><span data-stu-id="4ea46-116">Automated reports of uncaught JavaScript exceptions are not created by default.</span></span> <span data-ttu-id="4ea46-117">Dock kan du rapportera krascher manuellt med hjälp av den `sendCrash` fungera (se avsnittet reporting krascher).</span><span class="sxs-lookup"><span data-stu-id="4ea46-117">However, you can report crashes manually by using the `sendCrash` function (see the section on reporting crashes).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="4ea46-118">Rapportering</span><span class="sxs-lookup"><span data-stu-id="4ea46-118">Reporting activities</span></span>
<span data-ttu-id="4ea46-119">Rapportering av användaraktivitet inkluderar när en användare startar en ny aktivitet och när användaren slutar den aktuella aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="4ea46-119">Reporting on user activity includes when a user starts a new activity, and when the user ends the current activity.</span></span>

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="4ea46-120">Användaren startar en ny aktivitet</span><span class="sxs-lookup"><span data-stu-id="4ea46-120">User starts a new activity</span></span>
    engagement.agent.startActivity("MyUserActivity");

<span data-ttu-id="4ea46-121">Du måste anropa `startActivity()` varje gång användaraktivitet ändras.</span><span class="sxs-lookup"><span data-stu-id="4ea46-121">You need to call `startActivity()` each time user activity changes.</span></span> <span data-ttu-id="4ea46-122">Det första anropet till funktionen startar en ny session.</span><span class="sxs-lookup"><span data-stu-id="4ea46-122">The first call to this function starts a new user session.</span></span>

### <a name="user-ends-the-current-activity"></a><span data-ttu-id="4ea46-123">Användaren slutar den aktuella aktiviteten</span><span class="sxs-lookup"><span data-stu-id="4ea46-123">User ends the current activity</span></span>
    engagement.agent.endActivity();

<span data-ttu-id="4ea46-124">Du måste anropa `endActivity()` minst en gång när användaren är klar deras senaste aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4ea46-124">You need to call `endActivity()` at least once when the user finishes their last activity.</span></span> <span data-ttu-id="4ea46-125">Det informerar Mobile Engagement Web SDK att användaren är för närvarande inaktiv och att användarsessionen måste stängas när tidsgränsen för sessionen upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="4ea46-125">This informs the Mobile Engagement Web SDK that the user is currently idle, and that the user session needs to be closed after the session timeout expires.</span></span> <span data-ttu-id="4ea46-126">Om du anropar `startActivity()` innan tidsgränsen för sessionen upphör sessionen bara återupptas.</span><span class="sxs-lookup"><span data-stu-id="4ea46-126">If you call `startActivity()` before the session timeout expires, the session is simply resumed.</span></span>

<span data-ttu-id="4ea46-127">Eftersom det finns ingen tillförlitlig anrop för när navigator fönstret stängs, är det ofta svårt eller omöjligt att fånga slutet av användaraktiviteter inuti en webbmiljö.</span><span class="sxs-lookup"><span data-stu-id="4ea46-127">Because there's no reliable call for when the navigator window is closed, it's often difficult or impossible to catch the end of user activities inside a web environment.</span></span> <span data-ttu-id="4ea46-128">Det är därför Mobile Engagement-servern automatiskt upphör att gälla inom tre minuter efter appen på sidan stängs sessionen.</span><span class="sxs-lookup"><span data-stu-id="4ea46-128">That's why the Mobile Engagement server automatically expires the user session within three minutes after the application page is closed.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="4ea46-129">Rapporteringshändelser</span><span class="sxs-lookup"><span data-stu-id="4ea46-129">Reporting events</span></span>
<span data-ttu-id="4ea46-130">Rapportering av händelser som täcker Sessionshändelser och fristående händelser.</span><span class="sxs-lookup"><span data-stu-id="4ea46-130">Reporting on events covers session events and standalone events.</span></span>

### <a name="session-events"></a><span data-ttu-id="4ea46-131">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="4ea46-131">Session events</span></span>
<span data-ttu-id="4ea46-132">Sessionshändelser används normalt att rapportera åtgärder som utförs av en användare under användarens session.</span><span class="sxs-lookup"><span data-stu-id="4ea46-132">Session events usually are used to report the actions performed by a user during the user's session.</span></span>

<span data-ttu-id="4ea46-133">**Exempel utan extra data:**</span><span class="sxs-lookup"><span data-stu-id="4ea46-133">**Example without extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

<span data-ttu-id="4ea46-134">**Exempel med extra data:**</span><span class="sxs-lookup"><span data-stu-id="4ea46-134">**Example with extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a><span data-ttu-id="4ea46-135">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="4ea46-135">Standalone events</span></span>
<span data-ttu-id="4ea46-136">Till skillnad från Sessionshändelser, kan det ske fristående händelser utanför en session.</span><span class="sxs-lookup"><span data-stu-id="4ea46-136">Unlike session events, standalone events can occur outside the context of a session.</span></span>

<span data-ttu-id="4ea46-137">För att använda ``engagement.agent.sendEvent`` i stället för ``engagement.agent.sendSessionEvent``.</span><span class="sxs-lookup"><span data-stu-id="4ea46-137">For that, use ``engagement.agent.sendEvent`` instead of ``engagement.agent.sendSessionEvent``.</span></span>

## <a name="reporting-errors"></a><span data-ttu-id="4ea46-138">Rapporterat fel</span><span class="sxs-lookup"><span data-stu-id="4ea46-138">Reporting errors</span></span>
<span data-ttu-id="4ea46-139">Rapportering om felen omfattar session fel och fristående fel.</span><span class="sxs-lookup"><span data-stu-id="4ea46-139">Reporting on errors covers session errors and standalone errors.</span></span>

### <a name="session-errors"></a><span data-ttu-id="4ea46-140">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="4ea46-140">Session errors</span></span>
<span data-ttu-id="4ea46-141">Sessionen fel används oftast för att rapportera fel som kan påverkar användaren under användarens session.</span><span class="sxs-lookup"><span data-stu-id="4ea46-141">Session errors usually are used to report the errors that have an impact on the user during the user's session.</span></span>

<span data-ttu-id="4ea46-142">**Exempel utan extra data:**</span><span class="sxs-lookup"><span data-stu-id="4ea46-142">**Example without extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

<span data-ttu-id="4ea46-143">**Exempel med extra data:**</span><span class="sxs-lookup"><span data-stu-id="4ea46-143">**Example with extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="4ea46-144">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="4ea46-144">Standalone errors</span></span>
<span data-ttu-id="4ea46-145">Till skillnad från sessionen fel inträffa fristående fel utanför en session.</span><span class="sxs-lookup"><span data-stu-id="4ea46-145">Unlike session errors, standalone errors can occur outside the context of a session.</span></span>

<span data-ttu-id="4ea46-146">För att använda `engagement.agent.sendError` i stället för `engagement.agent.sendSessionError`.</span><span class="sxs-lookup"><span data-stu-id="4ea46-146">For that, use `engagement.agent.sendError` instead of `engagement.agent.sendSessionError`.</span></span>

## <a name="reporting-jobs"></a><span data-ttu-id="4ea46-147">Rapportering av jobb</span><span class="sxs-lookup"><span data-stu-id="4ea46-147">Reporting jobs</span></span>
<span data-ttu-id="4ea46-148">Rapportering av jobb omfattar rapportera fel och händelser som inträffar när ett jobb och rapportering av krascher.</span><span class="sxs-lookup"><span data-stu-id="4ea46-148">Reporting on jobs covers reporting errors and events that occur during a job, and reporting crashes.</span></span>

<span data-ttu-id="4ea46-149">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="4ea46-149">**Example:**</span></span>

<span data-ttu-id="4ea46-150">Om du vill övervaka en AJAX-begäran, använder du följande:</span><span class="sxs-lookup"><span data-stu-id="4ea46-150">If you want to monitor an AJAX request, you'd use the following:</span></span>

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a><span data-ttu-id="4ea46-151">Rapporterat fel under ett jobb</span><span class="sxs-lookup"><span data-stu-id="4ea46-151">Reporting errors during a job</span></span>
<span data-ttu-id="4ea46-152">Fel kan vara relaterad till ett jobb som körs i stället för att den aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="4ea46-152">Errors can be related to a running job instead of to the current user session.</span></span>

<span data-ttu-id="4ea46-153">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="4ea46-153">**Example:**</span></span>

<span data-ttu-id="4ea46-154">Om du vill rapportera ett fel om en begäran om AJAX misslyckas:</span><span class="sxs-lookup"><span data-stu-id="4ea46-154">If you want to report an error if an AJAX request fails:</span></span>

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="4ea46-155">Rapporteringshändelser under ett jobb</span><span class="sxs-lookup"><span data-stu-id="4ea46-155">Reporting events during a job</span></span>
<span data-ttu-id="4ea46-156">Händelser kan vara relaterad till ett jobb som körs i stället för att den aktuella användarsessionen tack till den `engagement.agent.sendJobEvent` funktion.</span><span class="sxs-lookup"><span data-stu-id="4ea46-156">Events can be related to a running job instead of to the current user session, thanks to the `engagement.agent.sendJobEvent` function.</span></span>

<span data-ttu-id="4ea46-157">Den här funktionen fungerar på samma sätt som `engagement.agent.sendJobError`.</span><span class="sxs-lookup"><span data-stu-id="4ea46-157">This function works exactly like `engagement.agent.sendJobError`.</span></span>

### <a name="reporting-crashes"></a><span data-ttu-id="4ea46-158">Rapportering krascher</span><span class="sxs-lookup"><span data-stu-id="4ea46-158">Reporting crashes</span></span>
<span data-ttu-id="4ea46-159">Använd den `sendCrash` funktionen rapporten kraschar manuellt.</span><span class="sxs-lookup"><span data-stu-id="4ea46-159">Use the `sendCrash` function to report crashes manually.</span></span>

<span data-ttu-id="4ea46-160">Den `crashid` argumentet är en sträng som identifierar typ av krascher.</span><span class="sxs-lookup"><span data-stu-id="4ea46-160">The `crashid` argument is a string that identifies the type of crash.</span></span>
<span data-ttu-id="4ea46-161">Den `crash` argumentet är vanligtvis stackspårning av krascher som en sträng.</span><span class="sxs-lookup"><span data-stu-id="4ea46-161">The `crash` argument usually is the stack trace of the crash as a string.</span></span>

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a><span data-ttu-id="4ea46-162">Extra parametrar</span><span class="sxs-lookup"><span data-stu-id="4ea46-162">Extra parameters</span></span>
<span data-ttu-id="4ea46-163">Du kan koppla godtyckliga data till en händelse, fel, aktiviteter eller jobb.</span><span class="sxs-lookup"><span data-stu-id="4ea46-163">You can attach arbitrary data to an event, error, activity, or job.</span></span>

<span data-ttu-id="4ea46-164">Data kan vara ett JSON-objekt (men inte en matris eller primitiv typ).</span><span class="sxs-lookup"><span data-stu-id="4ea46-164">The data can be any JSON object (but not an array or primitive type).</span></span>

<span data-ttu-id="4ea46-165">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="4ea46-165">**Example:**</span></span>

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="4ea46-166">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="4ea46-166">Limits</span></span>
<span data-ttu-id="4ea46-167">Gränser som gäller för extra parametrar har områden i reguljära uttryck för nycklar, värdetyper och storlek.</span><span class="sxs-lookup"><span data-stu-id="4ea46-167">Limits that apply to extra parameters are in the areas of regular expressions for keys, value types, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="4ea46-168">Nycklar</span><span class="sxs-lookup"><span data-stu-id="4ea46-168">Keys</span></span>
<span data-ttu-id="4ea46-169">Varje nyckel i objektet måste matcha följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="4ea46-169">Each key in the object must match the following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="4ea46-170">Detta innebär att nycklar måste börja med minst en bokstav följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="4ea46-170">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="values"></a><span data-ttu-id="4ea46-171">Värden</span><span class="sxs-lookup"><span data-stu-id="4ea46-171">Values</span></span>
<span data-ttu-id="4ea46-172">Värden är begränsade till sträng, siffra och booleskt typer.</span><span class="sxs-lookup"><span data-stu-id="4ea46-172">Values are limited to string, number, and Boolean types.</span></span>

#### <a name="size"></a><span data-ttu-id="4ea46-173">Storlek</span><span class="sxs-lookup"><span data-stu-id="4ea46-173">Size</span></span>
<span data-ttu-id="4ea46-174">Tillägg är begränsade till 1 024 tecken per anrop (efter Mobile Engagement Web SDK kodar den i JSON).</span><span class="sxs-lookup"><span data-stu-id="4ea46-174">Extras are limited to 1,024 characters per call (after the Mobile Engagement Web SDK encodes it in JSON).</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="4ea46-175">Rapportering programinformation</span><span class="sxs-lookup"><span data-stu-id="4ea46-175">Reporting application information</span></span>
<span data-ttu-id="4ea46-176">Du kan manuellt rapportera spåra information (eller programspecifik information) med hjälp av den `sendAppInfo()` funktion.</span><span class="sxs-lookup"><span data-stu-id="4ea46-176">You can manually report tracking information (or any other application-specific information) by using the `sendAppInfo()` function.</span></span>

<span data-ttu-id="4ea46-177">Observera att denna information kan skickas inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="4ea46-177">Note that this information can be sent incrementally.</span></span> <span data-ttu-id="4ea46-178">Det senaste värdet för en viss nyckel sparas för en specifik enhet.</span><span class="sxs-lookup"><span data-stu-id="4ea46-178">Only the latest value for a specific key will be kept for a specific device.</span></span>

<span data-ttu-id="4ea46-179">Du kan använda en JSON-objekt för att abstrahera information om programmet som händelsen tillägg.</span><span class="sxs-lookup"><span data-stu-id="4ea46-179">Like event extras, you can use any JSON object to abstract application information.</span></span> <span data-ttu-id="4ea46-180">Observera att matriser eller underordnade objekt behandlas som flat strängar (med JSON-serialisering).</span><span class="sxs-lookup"><span data-stu-id="4ea46-180">Note that arrays or sub-objects are treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="4ea46-181">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="4ea46-181">**Example:**</span></span>

<span data-ttu-id="4ea46-182">Här följer ett exempel på koden för att skicka användarens kön och födelsedatum:</span><span class="sxs-lookup"><span data-stu-id="4ea46-182">Here is a code sample for sending the user's gender and birth date:</span></span>

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a><span data-ttu-id="4ea46-183">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="4ea46-183">Limits</span></span>
<span data-ttu-id="4ea46-184">Gränserna som gäller för information om programmet är i områden som reguljära uttryck för nycklar och storlek.</span><span class="sxs-lookup"><span data-stu-id="4ea46-184">Limits that apply to application information are in the areas of regular expressions for keys, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="4ea46-185">Nycklar</span><span class="sxs-lookup"><span data-stu-id="4ea46-185">Keys</span></span>
<span data-ttu-id="4ea46-186">Varje nyckel i objektet måste matcha följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="4ea46-186">Each key in the object must match the following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="4ea46-187">Detta innebär att nycklar måste börja med minst en bokstav följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="4ea46-187">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="4ea46-188">Storlek</span><span class="sxs-lookup"><span data-stu-id="4ea46-188">Size</span></span>
<span data-ttu-id="4ea46-189">Information om programmet är begränsat till 1 024 tecken per anrop (efter Mobile Engagement Web SDK kodar den i JSON).</span><span class="sxs-lookup"><span data-stu-id="4ea46-189">Application information is limited to 1,024 characters per call (after the Mobile Engagement Web SDK encodes it in JSON).</span></span>

<span data-ttu-id="4ea46-190">I föregående exempel är JSON som skickas till servern 44 tecken:</span><span class="sxs-lookup"><span data-stu-id="4ea46-190">In the preceding example, the JSON sent to the server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
