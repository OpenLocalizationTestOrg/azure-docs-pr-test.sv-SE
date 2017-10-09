---
title: "aaaHow tooUse hello Engagement API på Windows Phone Silverlight"
description: "Hur tooUse hello Engagement API på Windows Phone Silverlight"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ae2ba2e8-f75b-4dee-a164-a7dd65d35a23
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1e84be95cc910be7f1227b4ae60eb483a1939284
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-phone-silverlight"></a><span data-ttu-id="abf5c-103">Hur tooUse hello Engagement API på Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="abf5c-103">How tooUse hello Engagement API on Windows Phone Silverlight</span></span>
<span data-ttu-id="abf5c-104">Det här dokumentet är tillägg toohello [hur toointegrate Mobile Engagement i din app för Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="abf5c-104">This document is an add-on toohello document [How toointegrate Mobile Engagement in your Windows Phone Silverlight app](mobile-engagement-windows-phone-integrate-engagement.md).</span></span> <span data-ttu-id="abf5c-105">Det ger i djup information om hur hello toouse Engagement API tooreport programstatistik.</span><span class="sxs-lookup"><span data-stu-id="abf5c-105">It provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="abf5c-106">Om du bara vill Engagement tooreport programmets sessioner, aktiviteter, krascher och teknisk information, så hello enklaste vägen toomake alla dina `PhoneApplicationPage` underordnade klasser ärver från hello `EngagementPage` klass.</span><span class="sxs-lookup"><span data-stu-id="abf5c-106">If you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `PhoneApplicationPage` sub-classes inherit from hello `EngagementPage` class.</span></span>

<span data-ttu-id="abf5c-107">Om du vill toodo mer, till exempel om du behöver tooreport programmet specifika händelser, fel och jobb, eller om du har tooreport ditt programs aktiviteter i ett annat sätt än hello en implementeras i hello `EngagementPage` klasser måste toouse hello Engagement API.</span><span class="sxs-lookup"><span data-stu-id="abf5c-107">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementPage` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="abf5c-108">Hej Engagement API tillhandahålls av hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="abf5c-108">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="abf5c-109">Du kan komma åt toothose metoder via `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="abf5c-109">You can access toothose methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="abf5c-110">Även om hello agent modulen inte har initierats, varje anrop toohello API skjuts upp och köras igen när hello agent är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="abf5c-110">Even if hello agent module has not been initialized, each call toohello API is deferred, and will be executed again when hello agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="abf5c-111">Koncept i engagement</span><span class="sxs-lookup"><span data-stu-id="abf5c-111">Engagement concepts</span></span>
<span data-ttu-id="abf5c-112">hello följande delar förfina hello Mobile Engagement-koncept för hello Windows Phone-plattformen.</span><span class="sxs-lookup"><span data-stu-id="abf5c-112">hello following parts refine hello Mobile Engagement Concepts for hello Windows Phone platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="abf5c-113">`Session` och `Activity`</span><span class="sxs-lookup"><span data-stu-id="abf5c-113">`Session` and `Activity`</span></span>
<span data-ttu-id="abf5c-114">En *aktiviteten* är ofta kopplade till en sida på hello-program som är toosay hello *aktiviteten* när hello sidan visas och stoppas när hello sidan stängs: hello fall när hello Engagement SDK är integrerad med hjälp av hello `EngagementPage` klass.</span><span class="sxs-lookup"><span data-stu-id="abf5c-114">An *activity* is usually associated with one page of hello application, that is toosay hello *activity* starts when hello page is displayed and stops when hello page is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementPage` class.</span></span>

<span data-ttu-id="abf5c-115">Men *aktiviteter* kan också kontrolleras manuellt med hjälp av hello Engagement API.</span><span class="sxs-lookup"><span data-stu-id="abf5c-115">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="abf5c-116">Detta gör toosplit en viss sida i flera sub delar tooget mer information om hello användning av den här sidan (till exempel hur ofta tooknown och hur länge dialogrutor används i den här sidan).</span><span class="sxs-lookup"><span data-stu-id="abf5c-116">This allows toosplit a given page in several sub parts tooget more details about hello usage of this page (for example tooknown how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="abf5c-117">Rapportering</span><span class="sxs-lookup"><span data-stu-id="abf5c-117">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="abf5c-118">Användaren startar en ny aktivitet</span><span class="sxs-lookup"><span data-stu-id="abf5c-118">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-119">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-119">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="abf5c-120">Du behöver toocall `StartActivity()` varje gång hello användaraktivitet ändras.</span><span class="sxs-lookup"><span data-stu-id="abf5c-120">You need toocall `StartActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="abf5c-121">hello första anropet toothis funktionen startar en ny session.</span><span class="sxs-lookup"><span data-stu-id="abf5c-121">hello first call toothis function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="abf5c-122">hello SDK anropa hello EndActivity metoden automatiskt när hello programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="abf5c-122">hello SDK automatically call hello EndActivity method when hello application is closed.</span></span> <span data-ttu-id="abf5c-123">Därför rekommenderas toocall hello StartActivity metod när hello verksamhet hello användare ändra och tooNEVER anrop hello EndActivity metod, eftersom den här metoden anropas tvingar hello aktuell session toobe avslutades.</span><span class="sxs-lookup"><span data-stu-id="abf5c-123">Thus, it is HIGHLY recommended toocall hello StartActivity method whenever hello activity of hello user change, and tooNEVER call hello EndActivity method, since calling this method forces hello current session toobe ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="abf5c-124">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-124">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="abf5c-125">Användaren avslutar sin aktuella aktiviteten</span><span class="sxs-lookup"><span data-stu-id="abf5c-125">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-126">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-126">Reference</span></span>
            void EndActivity()

<span data-ttu-id="abf5c-127">Du behöver toocall `EndActivity()` minst en gång när hello användaren är klar användarens sista aktivitet.</span><span class="sxs-lookup"><span data-stu-id="abf5c-127">You need toocall `EndActivity()` at least once when hello user finishes his last activity.</span></span> <span data-ttu-id="abf5c-128">Det informerar hello Engagement SDK att hello användare är för närvarande inaktiv och att hello användarsession måste toobe stängd när hello sessionstidsgränsen upphör att gälla (om du anropar `StartActivity()` innan hello sessionstidsgränsen upphör att gälla hello session bara fortsätter).</span><span class="sxs-lookup"><span data-stu-id="abf5c-128">This informs hello Engagement SDK that hello user is currently idle, and that hello user session need toobe closed once hello session timeout will expire (if you call `StartActivity()` before hello session timeout expires, hello session is simply continued).</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-129">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="abf5c-130">Rapportering av jobb</span><span class="sxs-lookup"><span data-stu-id="abf5c-130">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="abf5c-131">Starta ett jobb</span><span class="sxs-lookup"><span data-stu-id="abf5c-131">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-132">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-132">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="abf5c-133">Du kan använda hello jobbet tootrack vissa aktiviteter under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="abf5c-133">You can use hello job tootrack certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-134">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-134">Example</span></span>
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="abf5c-135">Avsluta ett jobb</span><span class="sxs-lookup"><span data-stu-id="abf5c-135">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-136">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-136">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="abf5c-137">Som en aktivitet som spåras av ett jobb har avslutats, bör du anropa hello EndJob metod för det här jobbet genom att ange hello jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="abf5c-137">As soon as a task tracked by a job has been terminated, you should call hello EndJob method for this job, by supplying hello job name.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-138">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-138">Example</span></span>
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="abf5c-139">Rapporteringshändelser</span><span class="sxs-lookup"><span data-stu-id="abf5c-139">Reporting Events</span></span>
<span data-ttu-id="abf5c-140">Det finns tre typer av händelser:</span><span class="sxs-lookup"><span data-stu-id="abf5c-140">There is three types of events :</span></span>

* <span data-ttu-id="abf5c-141">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="abf5c-141">Standalone events</span></span>
* <span data-ttu-id="abf5c-142">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="abf5c-142">Session events</span></span>
* <span data-ttu-id="abf5c-143">Jobbhändelser</span><span class="sxs-lookup"><span data-stu-id="abf5c-143">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="abf5c-144">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="abf5c-144">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-145">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-145">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="abf5c-146">Fristående händelser kan inträffa utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="abf5c-146">Standalone events can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-147">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-147">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="abf5c-148">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="abf5c-148">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-149">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-149">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="abf5c-150">Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under sin session.</span><span class="sxs-lookup"><span data-stu-id="abf5c-150">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-151">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-151">Example</span></span>
<span data-ttu-id="abf5c-152">**Utan data:**</span><span class="sxs-lookup"><span data-stu-id="abf5c-152">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="abf5c-153">**Med data:**</span><span class="sxs-lookup"><span data-stu-id="abf5c-153">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="abf5c-154">Jobbhändelser</span><span class="sxs-lookup"><span data-stu-id="abf5c-154">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-155">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-155">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="abf5c-156">Jobbhändelser är oftast används tooreport hello åtgärder som utförs av en användare under ett jobb.</span><span class="sxs-lookup"><span data-stu-id="abf5c-156">Job events are usually used tooreport hello actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-157">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-157">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="abf5c-158">Rapporterat fel</span><span class="sxs-lookup"><span data-stu-id="abf5c-158">Reporting Errors</span></span>
<span data-ttu-id="abf5c-159">Det finns tre typer av fel:</span><span class="sxs-lookup"><span data-stu-id="abf5c-159">There is three types of errors :</span></span>

* <span data-ttu-id="abf5c-160">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="abf5c-160">Standalone errors</span></span>
* <span data-ttu-id="abf5c-161">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="abf5c-161">Session errors</span></span>
* <span data-ttu-id="abf5c-162">Jobbfel</span><span class="sxs-lookup"><span data-stu-id="abf5c-162">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="abf5c-163">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="abf5c-163">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-164">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-164">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="abf5c-165">Motstridiga toosession fel, fristående kan det uppstå fel utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="abf5c-165">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-166">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-166">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="abf5c-167">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="abf5c-167">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-168">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-168">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="abf5c-169">Sessionen felen är oftast används tooreport hello fel påverkar hello användaren under sin session.</span><span class="sxs-lookup"><span data-stu-id="abf5c-169">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-170">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-170">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="abf5c-171">Jobbfel</span><span class="sxs-lookup"><span data-stu-id="abf5c-171">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-172">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-172">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="abf5c-173">Fel kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="abf5c-173">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-174">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-174">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="abf5c-175">Rapportering krascher</span><span class="sxs-lookup"><span data-stu-id="abf5c-175">Reporting Crashes</span></span>
<span data-ttu-id="abf5c-176">hello agent ger två metoder toodeal kraschar.</span><span class="sxs-lookup"><span data-stu-id="abf5c-176">hello agent provides two methods toodeal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="abf5c-177">Skicka ett undantag</span><span class="sxs-lookup"><span data-stu-id="abf5c-177">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-178">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-178">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="abf5c-179">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-179">Example</span></span>
<span data-ttu-id="abf5c-180">Du kan skicka ett undantag när som helst genom att anropa:</span><span class="sxs-lookup"><span data-stu-id="abf5c-180">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="abf5c-181">Du kan också använda en valfri parameter tooterminate hello engagement session på hello samtidigt än att skicka hello krascher.</span><span class="sxs-lookup"><span data-stu-id="abf5c-181">You can also use an optional parameter tooterminate hello engagement session at hello same time than sending hello crash.</span></span> <span data-ttu-id="abf5c-182">toodo anropa så:</span><span class="sxs-lookup"><span data-stu-id="abf5c-182">toodo so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="abf5c-183">Om du gör det stängs jobb och hello sessionen när du skickar hello krascher.</span><span class="sxs-lookup"><span data-stu-id="abf5c-183">If you do that, hello session and jobs will be closed just after sending hello crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="abf5c-184">Skicka ett ohanterat undantag</span><span class="sxs-lookup"><span data-stu-id="abf5c-184">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="abf5c-185">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-185">Reference</span></span>
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

<span data-ttu-id="abf5c-186">Engagement tillhandahåller också en metod toosend ohanterat undantag.</span><span class="sxs-lookup"><span data-stu-id="abf5c-186">Engagement also provides a method toosend unhandled exceptions.</span></span> <span data-ttu-id="abf5c-187">Detta är särskilt användbar när den används i hello silverlight UnhandledException händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="abf5c-187">This is especially useful when used inside hello silverlight UnhandledException event handler.</span></span>

<span data-ttu-id="abf5c-188">Den här metoden kommer **alltid** avslutas hello engagement sessionen och jobb efter anropas.</span><span class="sxs-lookup"><span data-stu-id="abf5c-188">This method will **ALWAYS** terminate hello engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="abf5c-189">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-189">Example</span></span>
<span data-ttu-id="abf5c-190">Du kan använda den tooimplement egna UnhandledException hanterare (särskilt om du har inaktiverat hello automatisk krascher reporting-funktionen i Engagement).</span><span class="sxs-lookup"><span data-stu-id="abf5c-190">You can use it tooimplement your own UnhandledException handler (especially if you have disabled hello automatic crash reporting feature of Engagement).</span></span> <span data-ttu-id="abf5c-191">Till exempel i hello `Application_UnhandledException` metod för hello `App.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="abf5c-191">For example, in hello `Application_UnhandledException` method of hello `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a><span data-ttu-id="abf5c-192">OnActivated</span><span class="sxs-lookup"><span data-stu-id="abf5c-192">OnActivated</span></span>
### <a name="reference"></a><span data-ttu-id="abf5c-193">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-193">Reference</span></span>
            void OnActivated(ActivatedEventArgs e)

<span data-ttu-id="abf5c-194">När hello användaren går framåt, från ett program när hello inaktiverad händelse utlöses försöker hello operativsystemet tooput hello programmet till en vilande tillstånd.</span><span class="sxs-lookup"><span data-stu-id="abf5c-194">When hello user navigates forward, away from an application, after hello Deactivated event is raised, hello operating system will attempt tooput hello application into a dormant state.</span></span> <span data-ttu-id="abf5c-195">Programmet hello är tombstone-borttagning.</span><span class="sxs-lookup"><span data-stu-id="abf5c-195">Then, hello application is Tombstoning.</span></span> <span data-ttu-id="abf5c-196">I den här processen avslutas ett program, men vissa data om hello hello program och hello enskilda sidor i programmet hello bevaras.</span><span class="sxs-lookup"><span data-stu-id="abf5c-196">In this process an application is terminated but some data about hello state of hello application and hello individual pages within hello application is preserved.</span></span>

<span data-ttu-id="abf5c-197">Du har tooinsert `EngagementAgent.Instance.OnActivated(e)` i hello `Application_Activated` metod från hello App.xaml.cs filen tooreset hello Engagement Agent när programmet hello har tombstone.</span><span class="sxs-lookup"><span data-stu-id="abf5c-197">You have tooinsert `EngagementAgent.Instance.OnActivated(e)` in hello `Application_Activated` method from hello App.xaml.cs file tooreset hello Engagement Agent when hello application has been Tombstoned.</span></span>

### <a name="example"></a><span data-ttu-id="abf5c-198">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-198">Example</span></span>
            // Inside your App.xaml.cs file

            // Code tooexecute when hello application is activated (brought tooforeground)
            // This code will not execute when hello application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a><span data-ttu-id="abf5c-199">Enhets-Id</span><span class="sxs-lookup"><span data-stu-id="abf5c-199">Device Id</span></span>
            String GetDeviceId()

<span data-ttu-id="abf5c-200">Du kan hämta hello engagement enhets-id genom att anropa den här metoden.</span><span class="sxs-lookup"><span data-stu-id="abf5c-200">You can get hello engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="abf5c-201">Tillägg parametrar</span><span class="sxs-lookup"><span data-stu-id="abf5c-201">Extras parameters</span></span>
<span data-ttu-id="abf5c-202">Godtycklig data kan vara anslutna tooan händelse, ett fel, en aktivitet eller ett jobb.</span><span class="sxs-lookup"><span data-stu-id="abf5c-202">Arbitrary data can be attached tooan event, an error, an activity or a job.</span></span> <span data-ttu-id="abf5c-203">Dessa data strukturerade med hjälp av en ordlista.</span><span class="sxs-lookup"><span data-stu-id="abf5c-203">These data can be structured using a dictionary.</span></span> <span data-ttu-id="abf5c-204">Nycklar och värden som kan vara av valfri typ.</span><span class="sxs-lookup"><span data-stu-id="abf5c-204">Keys and values can be of any type.</span></span>

<span data-ttu-id="abf5c-205">Extra data serialiseras så om du vill tooinsert din egen typ i tillägg du tooadd ett datakontrakt för den här typen.</span><span class="sxs-lookup"><span data-stu-id="abf5c-205">Extras data are serialized so if you want tooinsert your own type in extras you have tooadd a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="abf5c-206">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-206">Example</span></span>
<span data-ttu-id="abf5c-207">Vi skapar en ny klass ”Person”.</span><span class="sxs-lookup"><span data-stu-id="abf5c-207">We create a new class "Person".</span></span>

            using System.Runtime.Serialization;

            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }

                // Properties

                [DataMember]
                public int Age
                {
                  get;
                  set;
                }

                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

<span data-ttu-id="abf5c-208">Sedan lägger vi till en `Person` instans tooan extra.</span><span class="sxs-lookup"><span data-stu-id="abf5c-208">Then, we will add a `Person` instance tooan extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="abf5c-209">Om du placerar andra typer av objekt, kontrollera att deras toString ()-metoden är implementerad tooreturn en mänsklig läsbar sträng.</span><span class="sxs-lookup"><span data-stu-id="abf5c-209">If you put other types of objects, make sure their ToString() method is implemented tooreturn a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="abf5c-210">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="abf5c-210">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="abf5c-211">Nycklar</span><span class="sxs-lookup"><span data-stu-id="abf5c-211">Keys</span></span>
<span data-ttu-id="abf5c-212">Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="abf5c-212">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="abf5c-213">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="abf5c-213">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="abf5c-214">Storlek</span><span class="sxs-lookup"><span data-stu-id="abf5c-214">Size</span></span>
<span data-ttu-id="abf5c-215">Tillägg är begränsad för**1024** tecken per anrop.</span><span class="sxs-lookup"><span data-stu-id="abf5c-215">Extras are limited too**1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="abf5c-216">Rapportering programinformation</span><span class="sxs-lookup"><span data-stu-id="abf5c-216">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="abf5c-217">Referens</span><span class="sxs-lookup"><span data-stu-id="abf5c-217">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="abf5c-218">Du kan manuellt rapportera spåra information (eller andra program specifik information) med hello SendAppInfo()-funktionen.</span><span class="sxs-lookup"><span data-stu-id="abf5c-218">You can manually report tracking information (or any other application specific information) using hello SendAppInfo() function.</span></span>

<span data-ttu-id="abf5c-219">Observera att denna information kan skickas inkrementellt: endast hello senaste värdet för en viss nyckel sparas för en viss enhet.</span><span class="sxs-lookup"><span data-stu-id="abf5c-219">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="abf5c-220">Använda en ordlista som händelsen tillägg\<objekt, objektet\> tooattach information.</span><span class="sxs-lookup"><span data-stu-id="abf5c-220">Like event extras, use a Dictionary\<object, object\> tooattach informations.</span></span>

### <a name="example"></a><span data-ttu-id="abf5c-221">Exempel</span><span class="sxs-lookup"><span data-stu-id="abf5c-221">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="abf5c-222">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="abf5c-222">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="abf5c-223">Nycklar</span><span class="sxs-lookup"><span data-stu-id="abf5c-223">Keys</span></span>
<span data-ttu-id="abf5c-224">Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="abf5c-224">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="abf5c-225">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="abf5c-225">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="abf5c-226">Storlek</span><span class="sxs-lookup"><span data-stu-id="abf5c-226">Size</span></span>
<span data-ttu-id="abf5c-227">Information om programmet är begränsad för**1024** tecken per anrop.</span><span class="sxs-lookup"><span data-stu-id="abf5c-227">Application information are limited too**1024** characters per call.</span></span>

<span data-ttu-id="abf5c-228">I hello är 44 tecken i föregående exempel hello JSON skickas toohello server:</span><span class="sxs-lookup"><span data-stu-id="abf5c-228">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a><span data-ttu-id="abf5c-229">Loggning</span><span class="sxs-lookup"><span data-stu-id="abf5c-229">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="abf5c-230">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="abf5c-230">Enable Logging</span></span>
<span data-ttu-id="abf5c-231">hello SDK kan vara konfigurerade tooproduce test loggar i hello IDE-konsolen.</span><span class="sxs-lookup"><span data-stu-id="abf5c-231">hello SDK can be configured tooproduce test logs in hello IDE console.</span></span>
<span data-ttu-id="abf5c-232">Dessa loggar är inte aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="abf5c-232">These logs are not activated by default.</span></span> <span data-ttu-id="abf5c-233">toocustomize, uppdatera hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värde som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="abf5c-233">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
