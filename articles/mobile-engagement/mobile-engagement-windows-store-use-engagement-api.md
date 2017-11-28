---
title: "aaaHow tooUse hello Engagement API på Windows Universal"
description: "Hur tooUse hello Engagement API på Windows Universal"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: bb501fca-9cfe-4495-81df-b5efd6e0137b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0256b839c28e4ef6c530106408d744038fa711ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-universal"></a><span data-ttu-id="86e3c-103">Hur tooUse hello Engagement API på Windows Universal</span><span class="sxs-lookup"><span data-stu-id="86e3c-103">How tooUse hello Engagement API on Windows Universal</span></span>
<span data-ttu-id="86e3c-104">Det här dokumentet är tillägg toohello [hur tooIntegrate Engagement för universella Windows-](mobile-engagement-windows-store-integrate-engagement.md): ger i djup information om hur hello toouse Engagement API tooreport programstatistik.</span><span class="sxs-lookup"><span data-stu-id="86e3c-104">This document is an add-on toohello document [How tooIntegrate Engagement on Windows Universal](mobile-engagement-windows-store-integrate-engagement.md): it provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="86e3c-105">Tänk på att om du bara vill Engagement tooreport programmets sessioner, aktiviteter, krascher och teknisk information sedan hello enklaste sättet är toomake alla dina `Page` underordnade klasser ärver från hello `EngagementPage` klass.</span><span class="sxs-lookup"><span data-stu-id="86e3c-105">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `Page` sub-classes inherit from hello `EngagementPage` class.</span></span>

<span data-ttu-id="86e3c-106">Om du vill toodo mer, till exempel om du behöver tooreport programmet specifika händelser, fel och jobb, eller om du har tooreport ditt programs aktiviteter i ett annat sätt än hello en implementeras i hello `EngagementPage` klasser måste toouse hello Engagement API.</span><span class="sxs-lookup"><span data-stu-id="86e3c-106">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementPage` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="86e3c-107">Hej Engagement API tillhandahålls av hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="86e3c-107">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="86e3c-108">Du kan komma åt toothose metoder via `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="86e3c-108">You can access toothose methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="86e3c-109">Även om hello agent modulen inte har initierats, varje anrop toohello API skjuts upp och köras igen när hello agent är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="86e3c-109">Even if hello agent module has not been initialized, each call toohello API is deferred, and will be executed again when hello agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="86e3c-110">Koncept i engagement</span><span class="sxs-lookup"><span data-stu-id="86e3c-110">Engagement concepts</span></span>
<span data-ttu-id="86e3c-111">hello följande delar förfina hello vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md) för hello universella Windows-plattformen.</span><span class="sxs-lookup"><span data-stu-id="86e3c-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for hello Windows Universal platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="86e3c-112">`Session` och `Activity`</span><span class="sxs-lookup"><span data-stu-id="86e3c-112">`Session` and `Activity`</span></span>
<span data-ttu-id="86e3c-113">En *aktiviteten* är ofta kopplade till en sida på hello-program som är toosay hello *aktiviteten* när hello sidan visas och stoppas när hello sidan stängs: hello fall när hello Engagement SDK är integrerad med hjälp av hello `EngagementPage` klass.</span><span class="sxs-lookup"><span data-stu-id="86e3c-113">An *activity* is usually associated with one page of hello application, that is toosay hello *activity* starts when hello page is displayed and stops when hello page is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementPage` class.</span></span>

<span data-ttu-id="86e3c-114">Men *aktiviteter* kan också kontrolleras manuellt med hjälp av hello Engagement API.</span><span class="sxs-lookup"><span data-stu-id="86e3c-114">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="86e3c-115">Detta gör att du toosplit som en viss sida i flera sub delar tooget mer information om hello användning av den här sidan (till exempel hur ofta tooknow och hur länge dialogrutor används i den här sidan).</span><span class="sxs-lookup"><span data-stu-id="86e3c-115">This allows you toosplit a given page in several sub parts tooget more details about hello usage of this page (for example tooknow how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="86e3c-116">Rapportering</span><span class="sxs-lookup"><span data-stu-id="86e3c-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="86e3c-117">Användaren startar en ny aktivitet</span><span class="sxs-lookup"><span data-stu-id="86e3c-117">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-118">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-118">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="86e3c-119">Du behöver toocall `StartActivity()` varje gång hello användaraktivitet ändras.</span><span class="sxs-lookup"><span data-stu-id="86e3c-119">You need toocall `StartActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="86e3c-120">hello första anropet toothis funktionen startar en ny session.</span><span class="sxs-lookup"><span data-stu-id="86e3c-120">hello first call toothis function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86e3c-121">hello SDK anropar automatiskt hello EndActivity metod när hello programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="86e3c-121">hello SDK automatically calls hello EndActivity method when hello application is closed.</span></span> <span data-ttu-id="86e3c-122">Därför rekommenderas toocall hello StartActivity metod när hello aktiviteten för hello användare ändrar och tooNEVER anrop hello EndActivity metod, eftersom den här metoden anropas tvingar hello aktuell session toobe avslutades.</span><span class="sxs-lookup"><span data-stu-id="86e3c-122">Thus, it is HIGHLY recommended toocall hello StartActivity method whenever hello activity of hello user changes, and tooNEVER call hello EndActivity method, since calling this method forces hello current session toobe ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="86e3c-123">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-123">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="86e3c-124">Användaren avslutar sin aktuella aktiviteten</span><span class="sxs-lookup"><span data-stu-id="86e3c-124">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-125">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-125">Reference</span></span>
            void EndActivity()

<span data-ttu-id="86e3c-126">Detta avslutar hello aktivitet och hello-sessionen.</span><span class="sxs-lookup"><span data-stu-id="86e3c-126">This ends hello activity and hello session.</span></span> <span data-ttu-id="86e3c-127">Du bör inte anropa den här metoden om du inte verkligen vet vad du gör.</span><span class="sxs-lookup"><span data-stu-id="86e3c-127">You should not call this method unless you really know what you're doing.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-128">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-128">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="86e3c-129">Rapportering av jobb</span><span class="sxs-lookup"><span data-stu-id="86e3c-129">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="86e3c-130">Starta ett jobb</span><span class="sxs-lookup"><span data-stu-id="86e3c-130">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-131">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-131">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="86e3c-132">Du kan använda hello jobbet tootrack vissa aktiviteter under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="86e3c-132">You can use hello job tootrack certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-133">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-133">Example</span></span>
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="86e3c-134">Avsluta ett jobb</span><span class="sxs-lookup"><span data-stu-id="86e3c-134">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-135">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-135">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="86e3c-136">Som en aktivitet som spåras av ett jobb har avslutats, bör du anropa hello EndJob metod för det här jobbet genom att ange hello jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="86e3c-136">As soon as a task tracked by a job has been terminated, you should call hello EndJob method for this job, by supplying hello job name.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-137">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-137">Example</span></span>
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="86e3c-138">Rapporteringshändelser</span><span class="sxs-lookup"><span data-stu-id="86e3c-138">Reporting Events</span></span>
<span data-ttu-id="86e3c-139">Det finns tre typer av händelser:</span><span class="sxs-lookup"><span data-stu-id="86e3c-139">There is three types of events :</span></span>

* <span data-ttu-id="86e3c-140">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="86e3c-140">Standalone events</span></span>
* <span data-ttu-id="86e3c-141">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="86e3c-141">Session events</span></span>
* <span data-ttu-id="86e3c-142">Jobbhändelser</span><span class="sxs-lookup"><span data-stu-id="86e3c-142">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="86e3c-143">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="86e3c-143">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-144">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-144">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="86e3c-145">Fristående händelser kan inträffa utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="86e3c-145">Standalone events can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-146">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-146">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="86e3c-147">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="86e3c-147">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-148">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-148">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="86e3c-149">Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under sin session.</span><span class="sxs-lookup"><span data-stu-id="86e3c-149">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-150">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-150">Example</span></span>
<span data-ttu-id="86e3c-151">**Utan data:**</span><span class="sxs-lookup"><span data-stu-id="86e3c-151">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="86e3c-152">**Med data:**</span><span class="sxs-lookup"><span data-stu-id="86e3c-152">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="86e3c-153">Jobbhändelser</span><span class="sxs-lookup"><span data-stu-id="86e3c-153">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-154">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-154">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="86e3c-155">Jobbhändelser är oftast används tooreport hello åtgärder som utförs av en användare under ett jobb.</span><span class="sxs-lookup"><span data-stu-id="86e3c-155">Job events are usually used tooreport hello actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-156">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-156">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="86e3c-157">Rapporterat fel</span><span class="sxs-lookup"><span data-stu-id="86e3c-157">Reporting Errors</span></span>
<span data-ttu-id="86e3c-158">Det finns tre typer av fel:</span><span class="sxs-lookup"><span data-stu-id="86e3c-158">There are three types of errors :</span></span>

* <span data-ttu-id="86e3c-159">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="86e3c-159">Standalone errors</span></span>
* <span data-ttu-id="86e3c-160">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="86e3c-160">Session errors</span></span>
* <span data-ttu-id="86e3c-161">Jobbfel</span><span class="sxs-lookup"><span data-stu-id="86e3c-161">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="86e3c-162">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="86e3c-162">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-163">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-163">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="86e3c-164">Motstridiga toosession fel, fristående kan det uppstå fel utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="86e3c-164">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-165">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-165">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="86e3c-166">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="86e3c-166">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-167">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-167">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="86e3c-168">Sessionen felen är oftast används tooreport hello fel påverkar hello användaren under sin session.</span><span class="sxs-lookup"><span data-stu-id="86e3c-168">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-169">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-169">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="86e3c-170">Jobbfel</span><span class="sxs-lookup"><span data-stu-id="86e3c-170">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-171">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-171">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="86e3c-172">Fel kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="86e3c-172">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-173">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-173">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="86e3c-174">Rapportering krascher</span><span class="sxs-lookup"><span data-stu-id="86e3c-174">Reporting Crashes</span></span>
<span data-ttu-id="86e3c-175">hello agent ger två metoder toodeal kraschar.</span><span class="sxs-lookup"><span data-stu-id="86e3c-175">hello agent provides two methods toodeal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="86e3c-176">Skicka ett undantag</span><span class="sxs-lookup"><span data-stu-id="86e3c-176">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-177">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-177">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="86e3c-178">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-178">Example</span></span>
<span data-ttu-id="86e3c-179">Du kan skicka ett undantag när som helst genom att anropa:</span><span class="sxs-lookup"><span data-stu-id="86e3c-179">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="86e3c-180">Du kan också använda en valfri parameter tooterminate hello engagement session på hello samtidigt än att skicka hello krascher.</span><span class="sxs-lookup"><span data-stu-id="86e3c-180">You can also use an optional parameter tooterminate hello engagement session at hello same time than sending hello crash.</span></span> <span data-ttu-id="86e3c-181">toodo anropa så:</span><span class="sxs-lookup"><span data-stu-id="86e3c-181">toodo so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="86e3c-182">Om du gör det stängs jobb och hello sessionen när du skickar hello krascher.</span><span class="sxs-lookup"><span data-stu-id="86e3c-182">If you do that, hello session and jobs will be closed just after sending hello crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="86e3c-183">Skicka ett ohanterat undantag</span><span class="sxs-lookup"><span data-stu-id="86e3c-183">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="86e3c-184">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-184">Reference</span></span>
            void SendCrash(Exception e)

<span data-ttu-id="86e3c-185">Engagement innehåller också en metod toosend ohanterat undantag om du har **inaktiverad** Engagement automatisk **krasch** reporting.</span><span class="sxs-lookup"><span data-stu-id="86e3c-185">Engagement also provides a method toosend unhandled exceptions if you have **DISABLED** Engagement automatic **crash** reporting.</span></span> <span data-ttu-id="86e3c-186">Detta är särskilt användbar när den används i händelsehanteraren för hello program UnhandledException.</span><span class="sxs-lookup"><span data-stu-id="86e3c-186">This is especially useful when used inside hello application UnhandledException event handler.</span></span>

<span data-ttu-id="86e3c-187">Den här metoden kommer **alltid** avslutas hello engagement sessionen och jobb efter anropas.</span><span class="sxs-lookup"><span data-stu-id="86e3c-187">This method will **ALWAYS** terminate hello engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="86e3c-188">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-188">Example</span></span>
<span data-ttu-id="86e3c-189">Du kan använda den tooimplement egna UnhandledExceptionEventArgs hanterare.</span><span class="sxs-lookup"><span data-stu-id="86e3c-189">You can use it tooimplement your own UnhandledExceptionEventArgs handler.</span></span> <span data-ttu-id="86e3c-190">Lägg exempelvis till hello `Current_UnhandledException` metod för hello `App.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="86e3c-190">For example, add hello `Current_UnhandledException` method of hello `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

<span data-ttu-id="86e3c-191">Lägg till i App.xaml.cs i ”gemensamma App() {}”:</span><span class="sxs-lookup"><span data-stu-id="86e3c-191">In App.xaml.cs in "Public App(){}" add:</span></span>

            Application.Current.UnhandledException += Current_UnhandledException;

## <a name="device-id"></a><span data-ttu-id="86e3c-192">Enhets-Id</span><span class="sxs-lookup"><span data-stu-id="86e3c-192">Device Id</span></span>
            String EngagementAgent.Instance.GetDeviceId()

<span data-ttu-id="86e3c-193">Du kan hämta hello engagement enhets-id genom att anropa den här metoden.</span><span class="sxs-lookup"><span data-stu-id="86e3c-193">You can get hello engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="86e3c-194">Tillägg parametrar</span><span class="sxs-lookup"><span data-stu-id="86e3c-194">Extras parameters</span></span>
<span data-ttu-id="86e3c-195">Godtycklig data kan vara anslutna tooan händelse, ett fel, en aktivitet eller ett jobb.</span><span class="sxs-lookup"><span data-stu-id="86e3c-195">Arbitrary data can be attached tooan event, an error, an activity or a job.</span></span> <span data-ttu-id="86e3c-196">Dessa data strukturerade med hjälp av en ordlista.</span><span class="sxs-lookup"><span data-stu-id="86e3c-196">These data can be structured using a dictionary.</span></span> <span data-ttu-id="86e3c-197">Nycklar och värden som kan vara av valfri typ.</span><span class="sxs-lookup"><span data-stu-id="86e3c-197">Keys and values can be of any type.</span></span>

<span data-ttu-id="86e3c-198">Extra data serialiseras så om du vill tooinsert din egen typ i tillägg du tooadd ett datakontrakt för den här typen.</span><span class="sxs-lookup"><span data-stu-id="86e3c-198">Extras data are serialized so if you want tooinsert your own type in extras you have tooadd a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="86e3c-199">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-199">Example</span></span>
<span data-ttu-id="86e3c-200">Vi skapar en ny klass ”Person”.</span><span class="sxs-lookup"><span data-stu-id="86e3c-200">We create a new class "Person".</span></span>

            using System.Runtime.Serialization;

            namespace Microsoft.Azure.Engagement
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

<span data-ttu-id="86e3c-201">Sedan lägger vi till en `Person` instans tooan extra.</span><span class="sxs-lookup"><span data-stu-id="86e3c-201">Then, we will add a `Person` instance tooan extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="86e3c-202">Om du placerar andra typer av objekt, kontrollera att deras toString ()-metoden är implementerad tooreturn en mänsklig läsbar sträng.</span><span class="sxs-lookup"><span data-stu-id="86e3c-202">If you put other types of objects, make sure their ToString() method is implemented tooreturn a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="86e3c-203">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="86e3c-203">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="86e3c-204">Nycklar</span><span class="sxs-lookup"><span data-stu-id="86e3c-204">Keys</span></span>
<span data-ttu-id="86e3c-205">Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="86e3c-205">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="86e3c-206">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="86e3c-206">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="86e3c-207">Storlek</span><span class="sxs-lookup"><span data-stu-id="86e3c-207">Size</span></span>
<span data-ttu-id="86e3c-208">Tillägg är begränsad för**1024** tecken per anrop.</span><span class="sxs-lookup"><span data-stu-id="86e3c-208">Extras are limited too**1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="86e3c-209">Rapportering programinformation</span><span class="sxs-lookup"><span data-stu-id="86e3c-209">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="86e3c-210">Referens</span><span class="sxs-lookup"><span data-stu-id="86e3c-210">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="86e3c-211">Du kan manuellt rapportera spåra information (eller andra program specifik information) med hello SendAppInfo()-funktionen.</span><span class="sxs-lookup"><span data-stu-id="86e3c-211">You can manually report tracking information (or any other application specific information) using hello SendAppInfo() function.</span></span>

<span data-ttu-id="86e3c-212">Observera att dessa data kan skickas inkrementellt: endast hello senaste värdet för en viss nyckel sparas för en viss enhet.</span><span class="sxs-lookup"><span data-stu-id="86e3c-212">Note that this data can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="86e3c-213">Använda en ordlista som händelsen tillägg\<objekt, objektet\> tooattach data.</span><span class="sxs-lookup"><span data-stu-id="86e3c-213">Like event extras, use a Dictionary\<object, object\> tooattach data.</span></span>

### <a name="example"></a><span data-ttu-id="86e3c-214">Exempel</span><span class="sxs-lookup"><span data-stu-id="86e3c-214">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="86e3c-215">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="86e3c-215">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="86e3c-216">Nycklar</span><span class="sxs-lookup"><span data-stu-id="86e3c-216">Keys</span></span>
<span data-ttu-id="86e3c-217">Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="86e3c-217">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="86e3c-218">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="86e3c-218">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="86e3c-219">Storlek</span><span class="sxs-lookup"><span data-stu-id="86e3c-219">Size</span></span>
<span data-ttu-id="86e3c-220">Information om programmet är begränsad för**1024** tecken per anrop.</span><span class="sxs-lookup"><span data-stu-id="86e3c-220">Application information is limited too**1024** characters per call.</span></span>

<span data-ttu-id="86e3c-221">I hello är 44 tecken i föregående exempel hello JSON skickas toohello server:</span><span class="sxs-lookup"><span data-stu-id="86e3c-221">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"birthdate":"1983-12-07","gender":"female"}

## <a name="logging"></a><span data-ttu-id="86e3c-222">Loggning</span><span class="sxs-lookup"><span data-stu-id="86e3c-222">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="86e3c-223">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="86e3c-223">Enable Logging</span></span>
<span data-ttu-id="86e3c-224">hello SDK kan vara konfigurerade tooproduce test loggar i hello IDE-konsolen.</span><span class="sxs-lookup"><span data-stu-id="86e3c-224">hello SDK can be configured tooproduce test logs in hello IDE console.</span></span>
<span data-ttu-id="86e3c-225">Dessa loggar är inte aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="86e3c-225">These logs are not activated by default.</span></span> <span data-ttu-id="86e3c-226">toocustomize, uppdatera hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värde som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="86e3c-226">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

