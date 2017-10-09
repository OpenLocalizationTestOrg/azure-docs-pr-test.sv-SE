---
title: 'aaaAzure API: er Mobile Engagement Web SDK | Microsoft Docs'
description: "Hej senaste uppdateringarna och procedurer för hello Web SDK för Azure Mobile Engagement"
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
ms.openlocfilehash: ec1261d6ad573b8c3ad6d5f616ab7bbe560d6fe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-mobile-engagement-api-in-a-web-application"></a><span data-ttu-id="123d4-103">Använd hello Azure Mobile Engagement API i ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="123d4-103">Use hello Azure Mobile Engagement API in a web application</span></span>
<span data-ttu-id="123d4-104">Det här dokumentet är ett tillägg toohello dokument som anger hur för[Mobile Engagement ska integreras i ett webbprogram](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="123d4-104">This document is an addition toohello document that tells you how too[integrate Mobile Engagement in a web application](mobile-engagement-web-integrate-engagement.md).</span></span> <span data-ttu-id="123d4-105">Det ger detaljerad information om hur toouse hello Azure Mobile Engagement API tooreport programstatistik.</span><span class="sxs-lookup"><span data-stu-id="123d4-105">It provides in-depth details about how toouse hello Azure Mobile Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="123d4-106">hello Mobile Engagement API tillhandahålls av hello `engagement.agent` objekt.</span><span class="sxs-lookup"><span data-stu-id="123d4-106">hello Mobile Engagement API is provided by hello `engagement.agent` object.</span></span> <span data-ttu-id="123d4-107">hello Azure Mobile Engagement Web SDK alias är standard `engagement`.</span><span class="sxs-lookup"><span data-stu-id="123d4-107">hello default Azure Mobile Engagement Web SDK alias is `engagement`.</span></span> <span data-ttu-id="123d4-108">Du kan ändra detta alias från hello SDK-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="123d4-108">You can redefine this alias from hello SDK configuration.</span></span>

## <a name="mobile-engagement-concepts"></a><span data-ttu-id="123d4-109">Koncept i Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="123d4-109">Mobile Engagement concepts</span></span>
<span data-ttu-id="123d4-110">hello följande delar förfina vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md) för hello webbplattform.</span><span class="sxs-lookup"><span data-stu-id="123d4-110">hello following parts refine common [Mobile Engagement concepts](mobile-engagement-concepts.md) for hello web platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="123d4-111">`Session` och `Activity`</span><span class="sxs-lookup"><span data-stu-id="123d4-111">`Session` and `Activity`</span></span>
<span data-ttu-id="123d4-112">Om hello användaren är inaktiv under mer än ett par sekunder mellan två aktiviteter, är hello användarens sekvensen av aktiviteter uppdelad i två olika sessioner.</span><span class="sxs-lookup"><span data-stu-id="123d4-112">If hello user stays idle for more than a few seconds between two activities, hello user's sequence of activities is split into two distinct sessions.</span></span> <span data-ttu-id="123d4-113">Dessa några sekunder kallas hello-sessions Time-out.</span><span class="sxs-lookup"><span data-stu-id="123d4-113">These few seconds are called hello session timeout.</span></span>

<span data-ttu-id="123d4-114">Om ditt webbprogram inte deklarera hello slutet av användaraktiviteter ensamt (genom att anropa hello `engagement.agent.endActivity` funktionen), hello Mobile Engagement server upphör automatiskt hello användarsession inom tre minuter efter hello appen på sidan är stängd.</span><span class="sxs-lookup"><span data-stu-id="123d4-114">If your web application doesn't declare hello end of user activities by itself (by calling hello `engagement.agent.endActivity` function), hello Mobile Engagement server automatically expires hello user session within three minutes after hello application page is closed.</span></span> <span data-ttu-id="123d4-115">Detta kallas hello server sessions Time-out.</span><span class="sxs-lookup"><span data-stu-id="123d4-115">This is called hello server session timeout.</span></span>

### `Crash`
<span data-ttu-id="123d4-116">Automatisk rapporter om undantagsfel utan felhantering JavaScript skapas inte som standard.</span><span class="sxs-lookup"><span data-stu-id="123d4-116">Automated reports of uncaught JavaScript exceptions are not created by default.</span></span> <span data-ttu-id="123d4-117">Dock kan du rapportera krascher manuellt med hjälp av hello `sendCrash` fungera (se avsnittet hello reporting krascher).</span><span class="sxs-lookup"><span data-stu-id="123d4-117">However, you can report crashes manually by using hello `sendCrash` function (see hello section on reporting crashes).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="123d4-118">Rapportering</span><span class="sxs-lookup"><span data-stu-id="123d4-118">Reporting activities</span></span>
<span data-ttu-id="123d4-119">Rapportering av användaraktivitet inkluderar när en användare startar en ny aktivitet och när hello användaren slutar hello aktuella aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="123d4-119">Reporting on user activity includes when a user starts a new activity, and when hello user ends hello current activity.</span></span>

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="123d4-120">Användaren startar en ny aktivitet</span><span class="sxs-lookup"><span data-stu-id="123d4-120">User starts a new activity</span></span>
    engagement.agent.startActivity("MyUserActivity");

<span data-ttu-id="123d4-121">Du behöver toocall `startActivity()` varje gång användaraktivitet ändras.</span><span class="sxs-lookup"><span data-stu-id="123d4-121">You need toocall `startActivity()` each time user activity changes.</span></span> <span data-ttu-id="123d4-122">hello första anropet toothis funktionen startar en ny session.</span><span class="sxs-lookup"><span data-stu-id="123d4-122">hello first call toothis function starts a new user session.</span></span>

### <a name="user-ends-hello-current-activity"></a><span data-ttu-id="123d4-123">Användaren slutar hello aktuell aktivitet</span><span class="sxs-lookup"><span data-stu-id="123d4-123">User ends hello current activity</span></span>
    engagement.agent.endActivity();

<span data-ttu-id="123d4-124">Du behöver toocall `endActivity()` minst en gång när hello användaren är klar deras senaste aktivitet.</span><span class="sxs-lookup"><span data-stu-id="123d4-124">You need toocall `endActivity()` at least once when hello user finishes their last activity.</span></span> <span data-ttu-id="123d4-125">Det informerar hello Mobile Engagement Web SDK hello användare är för närvarande inaktiv, och att hello användarsession måste toobe stängs när hello sessionstidsgränsen upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="123d4-125">This informs hello Mobile Engagement Web SDK that hello user is currently idle, and that hello user session needs toobe closed after hello session timeout expires.</span></span> <span data-ttu-id="123d4-126">Om du anropar `startActivity()` innan hello sessionstidsgränsen upphör att gälla hello sessionen är helt enkelt återupptas.</span><span class="sxs-lookup"><span data-stu-id="123d4-126">If you call `startActivity()` before hello session timeout expires, hello session is simply resumed.</span></span>

<span data-ttu-id="123d4-127">Eftersom det finns ingen tillförlitlig anrop för när hello navigator fönstret stängs, är det ofta svårt eller omöjligt toocatch hello slutet av användaraktiviteter inuti en webbmiljö.</span><span class="sxs-lookup"><span data-stu-id="123d4-127">Because there's no reliable call for when hello navigator window is closed, it's often difficult or impossible toocatch hello end of user activities inside a web environment.</span></span> <span data-ttu-id="123d4-128">Som är varför hello Mobile Engagement server upphör automatiskt hello användarsession inom tre minuter efter hello appen på sidan är stängd.</span><span class="sxs-lookup"><span data-stu-id="123d4-128">That's why hello Mobile Engagement server automatically expires hello user session within three minutes after hello application page is closed.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="123d4-129">Rapporteringshändelser</span><span class="sxs-lookup"><span data-stu-id="123d4-129">Reporting events</span></span>
<span data-ttu-id="123d4-130">Rapportering av händelser som täcker Sessionshändelser och fristående händelser.</span><span class="sxs-lookup"><span data-stu-id="123d4-130">Reporting on events covers session events and standalone events.</span></span>

### <a name="session-events"></a><span data-ttu-id="123d4-131">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="123d4-131">Session events</span></span>
<span data-ttu-id="123d4-132">Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under hello användarens session.</span><span class="sxs-lookup"><span data-stu-id="123d4-132">Session events usually are used tooreport hello actions performed by a user during hello user's session.</span></span>

<span data-ttu-id="123d4-133">**Exempel utan extra data:**</span><span class="sxs-lookup"><span data-stu-id="123d4-133">**Example without extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

<span data-ttu-id="123d4-134">**Exempel med extra data:**</span><span class="sxs-lookup"><span data-stu-id="123d4-134">**Example with extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a><span data-ttu-id="123d4-135">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="123d4-135">Standalone events</span></span>
<span data-ttu-id="123d4-136">Till skillnad från Sessionshändelser, kan det ske fristående händelser utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="123d4-136">Unlike session events, standalone events can occur outside hello context of a session.</span></span>

<span data-ttu-id="123d4-137">För att använda ``engagement.agent.sendEvent`` i stället för ``engagement.agent.sendSessionEvent``.</span><span class="sxs-lookup"><span data-stu-id="123d4-137">For that, use ``engagement.agent.sendEvent`` instead of ``engagement.agent.sendSessionEvent``.</span></span>

## <a name="reporting-errors"></a><span data-ttu-id="123d4-138">Rapporterat fel</span><span class="sxs-lookup"><span data-stu-id="123d4-138">Reporting errors</span></span>
<span data-ttu-id="123d4-139">Rapportering om felen omfattar session fel och fristående fel.</span><span class="sxs-lookup"><span data-stu-id="123d4-139">Reporting on errors covers session errors and standalone errors.</span></span>

### <a name="session-errors"></a><span data-ttu-id="123d4-140">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="123d4-140">Session errors</span></span>
<span data-ttu-id="123d4-141">Sessionen felen är oftast används tooreport hello fel som kan påverkar hello användaren under hello användarsession.</span><span class="sxs-lookup"><span data-stu-id="123d4-141">Session errors usually are used tooreport hello errors that have an impact on hello user during hello user's session.</span></span>

<span data-ttu-id="123d4-142">**Exempel utan extra data:**</span><span class="sxs-lookup"><span data-stu-id="123d4-142">**Example without extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

<span data-ttu-id="123d4-143">**Exempel med extra data:**</span><span class="sxs-lookup"><span data-stu-id="123d4-143">**Example with extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="123d4-144">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="123d4-144">Standalone errors</span></span>
<span data-ttu-id="123d4-145">Till skillnad från sessionen fel inträffa fristående fel utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="123d4-145">Unlike session errors, standalone errors can occur outside hello context of a session.</span></span>

<span data-ttu-id="123d4-146">För att använda `engagement.agent.sendError` i stället för `engagement.agent.sendSessionError`.</span><span class="sxs-lookup"><span data-stu-id="123d4-146">For that, use `engagement.agent.sendError` instead of `engagement.agent.sendSessionError`.</span></span>

## <a name="reporting-jobs"></a><span data-ttu-id="123d4-147">Rapportering av jobb</span><span class="sxs-lookup"><span data-stu-id="123d4-147">Reporting jobs</span></span>
<span data-ttu-id="123d4-148">Rapportering av jobb omfattar rapportera fel och händelser som inträffar när ett jobb och rapportering av krascher.</span><span class="sxs-lookup"><span data-stu-id="123d4-148">Reporting on jobs covers reporting errors and events that occur during a job, and reporting crashes.</span></span>

<span data-ttu-id="123d4-149">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="123d4-149">**Example:**</span></span>

<span data-ttu-id="123d4-150">Om du vill toomonitor en begäran om AJAX använder hello följande:</span><span class="sxs-lookup"><span data-stu-id="123d4-150">If you want toomonitor an AJAX request, you'd use hello following:</span></span>

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

### <a name="reporting-errors-during-a-job"></a><span data-ttu-id="123d4-151">Rapporterat fel under ett jobb</span><span class="sxs-lookup"><span data-stu-id="123d4-151">Reporting errors during a job</span></span>
<span data-ttu-id="123d4-152">Fel kan vara relaterade tooa köra jobb i stället för toohello aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="123d4-152">Errors can be related tooa running job instead of toohello current user session.</span></span>

<span data-ttu-id="123d4-153">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="123d4-153">**Example:**</span></span>

<span data-ttu-id="123d4-154">Om du vill tooreport ett fel om en begäran om AJAX misslyckas:</span><span class="sxs-lookup"><span data-stu-id="123d4-154">If you want tooreport an error if an AJAX request fails:</span></span>

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

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="123d4-155">Rapporteringshändelser under ett jobb</span><span class="sxs-lookup"><span data-stu-id="123d4-155">Reporting events during a job</span></span>
<span data-ttu-id="123d4-156">Händelser kan vara relaterade tooa köra jobb i stället för toohello aktuella användarsessionen, tack vare toohello `engagement.agent.sendJobEvent` funktion.</span><span class="sxs-lookup"><span data-stu-id="123d4-156">Events can be related tooa running job instead of toohello current user session, thanks toohello `engagement.agent.sendJobEvent` function.</span></span>

<span data-ttu-id="123d4-157">Den här funktionen fungerar på samma sätt som `engagement.agent.sendJobError`.</span><span class="sxs-lookup"><span data-stu-id="123d4-157">This function works exactly like `engagement.agent.sendJobError`.</span></span>

### <a name="reporting-crashes"></a><span data-ttu-id="123d4-158">Rapportering krascher</span><span class="sxs-lookup"><span data-stu-id="123d4-158">Reporting crashes</span></span>
<span data-ttu-id="123d4-159">Använd hello `sendCrash` funktionen tooreport kraschar manuellt.</span><span class="sxs-lookup"><span data-stu-id="123d4-159">Use hello `sendCrash` function tooreport crashes manually.</span></span>

<span data-ttu-id="123d4-160">Hej `crashid` argumentet är en sträng som identifierar hello typ av krascher.</span><span class="sxs-lookup"><span data-stu-id="123d4-160">hello `crashid` argument is a string that identifies hello type of crash.</span></span>
<span data-ttu-id="123d4-161">Hej `crash` argumentet är vanligtvis hello stackspårning hello havererade som en sträng.</span><span class="sxs-lookup"><span data-stu-id="123d4-161">hello `crash` argument usually is hello stack trace of hello crash as a string.</span></span>

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a><span data-ttu-id="123d4-162">Extra parametrar</span><span class="sxs-lookup"><span data-stu-id="123d4-162">Extra parameters</span></span>
<span data-ttu-id="123d4-163">Du kan koppla godtyckliga data tooan händelse, fel, aktivitet eller jobb.</span><span class="sxs-lookup"><span data-stu-id="123d4-163">You can attach arbitrary data tooan event, error, activity, or job.</span></span>

<span data-ttu-id="123d4-164">hello data kan vara ett JSON-objekt (men inte en matris eller primitiv typ).</span><span class="sxs-lookup"><span data-stu-id="123d4-164">hello data can be any JSON object (but not an array or primitive type).</span></span>

<span data-ttu-id="123d4-165">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="123d4-165">**Example:**</span></span>

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="123d4-166">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="123d4-166">Limits</span></span>
<span data-ttu-id="123d4-167">Gränser som gäller tooextra parametrar har hello områden i reguljära uttryck för nycklar, värdetyper och storlek.</span><span class="sxs-lookup"><span data-stu-id="123d4-167">Limits that apply tooextra parameters are in hello areas of regular expressions for keys, value types, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="123d4-168">Nycklar</span><span class="sxs-lookup"><span data-stu-id="123d4-168">Keys</span></span>
<span data-ttu-id="123d4-169">Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="123d4-169">Each key in hello object must match hello following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="123d4-170">Detta innebär att nycklar måste börja med minst en bokstav följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="123d4-170">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="values"></a><span data-ttu-id="123d4-171">Värden</span><span class="sxs-lookup"><span data-stu-id="123d4-171">Values</span></span>
<span data-ttu-id="123d4-172">Värden är begränsad toostring, antal och typer av boolesk.</span><span class="sxs-lookup"><span data-stu-id="123d4-172">Values are limited toostring, number, and Boolean types.</span></span>

#### <a name="size"></a><span data-ttu-id="123d4-173">Storlek</span><span class="sxs-lookup"><span data-stu-id="123d4-173">Size</span></span>
<span data-ttu-id="123d4-174">Tillägg är begränsad too1, 024 tecken per anrop (efter hello Mobile Engagement Web SDK kodar den i JSON).</span><span class="sxs-lookup"><span data-stu-id="123d4-174">Extras are limited too1,024 characters per call (after hello Mobile Engagement Web SDK encodes it in JSON).</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="123d4-175">Rapportering programinformation</span><span class="sxs-lookup"><span data-stu-id="123d4-175">Reporting application information</span></span>
<span data-ttu-id="123d4-176">Du kan manuellt rapportera spåra information (eller programspecifik information) med hjälp av hello `sendAppInfo()` funktion.</span><span class="sxs-lookup"><span data-stu-id="123d4-176">You can manually report tracking information (or any other application-specific information) by using hello `sendAppInfo()` function.</span></span>

<span data-ttu-id="123d4-177">Observera att denna information kan skickas inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="123d4-177">Note that this information can be sent incrementally.</span></span> <span data-ttu-id="123d4-178">Endast hello senaste värdet för en viss nyckel sparas för en specifik enhet.</span><span class="sxs-lookup"><span data-stu-id="123d4-178">Only hello latest value for a specific key will be kept for a specific device.</span></span>

<span data-ttu-id="123d4-179">Du kan använda en JSON-objekt tooabstract programinformation som händelsen tillägg.</span><span class="sxs-lookup"><span data-stu-id="123d4-179">Like event extras, you can use any JSON object tooabstract application information.</span></span> <span data-ttu-id="123d4-180">Observera att matriser eller underordnade objekt behandlas som flat strängar (med JSON-serialisering).</span><span class="sxs-lookup"><span data-stu-id="123d4-180">Note that arrays or sub-objects are treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="123d4-181">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="123d4-181">**Example:**</span></span>

<span data-ttu-id="123d4-182">Här följer ett exempel på koden för att skicka hello användarens kön och födelsedatum:</span><span class="sxs-lookup"><span data-stu-id="123d4-182">Here is a code sample for sending hello user's gender and birth date:</span></span>

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a><span data-ttu-id="123d4-183">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="123d4-183">Limits</span></span>
<span data-ttu-id="123d4-184">Gränser som gäller tooapplication information finns i hello områden i reguljära uttryck för nycklar och storlek.</span><span class="sxs-lookup"><span data-stu-id="123d4-184">Limits that apply tooapplication information are in hello areas of regular expressions for keys, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="123d4-185">Nycklar</span><span class="sxs-lookup"><span data-stu-id="123d4-185">Keys</span></span>
<span data-ttu-id="123d4-186">Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="123d4-186">Each key in hello object must match hello following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="123d4-187">Detta innebär att nycklar måste börja med minst en bokstav följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="123d4-187">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="123d4-188">Storlek</span><span class="sxs-lookup"><span data-stu-id="123d4-188">Size</span></span>
<span data-ttu-id="123d4-189">Information om programmet är begränsad too1, 024 tecken per anrop (efter hello Mobile Engagement Web SDK kodar den i JSON).</span><span class="sxs-lookup"><span data-stu-id="123d4-189">Application information is limited too1,024 characters per call (after hello Mobile Engagement Web SDK encodes it in JSON).</span></span>

<span data-ttu-id="123d4-190">I föregående exempel hello, skickas hello JSON toohello servern är 44 tecken:</span><span class="sxs-lookup"><span data-stu-id="123d4-190">In hello preceding example, hello JSON sent toohello server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
