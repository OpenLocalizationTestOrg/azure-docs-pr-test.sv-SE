---
title: "Hur du använder Engagement API på Windows Phone Silverlight"
description: "Hur du använder Engagement API på Windows Phone Silverlight"
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
ms.openlocfilehash: ec8b6c13ea052c8063dfde4321cdd286ab6cb817
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a><span data-ttu-id="b88de-103">Hur du använder Engagement API på Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="b88de-103">How to Use the Engagement API on Windows Phone Silverlight</span></span>
<span data-ttu-id="b88de-104">Det här dokumentet är ett tillägg till dokumentet [hur du integrerar Mobile Engagement i din app för Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="b88de-104">This document is an add-on to the document [How to integrate Mobile Engagement in your Windows Phone Silverlight app](mobile-engagement-windows-phone-integrate-engagement.md).</span></span> <span data-ttu-id="b88de-105">Det ger i djup information om hur du använder Engagement API för att rapportera programmet-statistik.</span><span class="sxs-lookup"><span data-stu-id="b88de-105">It provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="b88de-106">Om du bara vill Engagement att rapportera programmets sessioner, aktiviteter, krascher och teknisk information, så det enklaste sättet att se alla dina `PhoneApplicationPage` underordnade klasser ärver från den `EngagementPage` klass.</span><span class="sxs-lookup"><span data-stu-id="b88de-106">If you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your `PhoneApplicationPage` sub-classes inherit from the `EngagementPage` class.</span></span>

<span data-ttu-id="b88de-107">Om du vill göra mer, till exempel om du behöver rapportera programmet specifika händelser, fel och jobb, eller om du vill rapportera ditt programs aktiviteter i ett annat sätt än den som implementerats i den `EngagementPage` klasser, måste du använda Engagement-API.</span><span class="sxs-lookup"><span data-stu-id="b88de-107">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementPage` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="b88de-108">Engagement-API som tillhandahålls av den `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="b88de-108">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="b88de-109">Du har åtkomst till de här metoderna via `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="b88de-109">You can access to those methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="b88de-110">Även om modulen agent inte har initierats, varje anrop till API: et skjuts upp och köras igen när agenten är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="b88de-110">Even if the agent module has not been initialized, each call to the API is deferred, and will be executed again when the agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="b88de-111">Koncept i engagement</span><span class="sxs-lookup"><span data-stu-id="b88de-111">Engagement concepts</span></span>
<span data-ttu-id="b88de-112">Följande delar förfina den koncept i Mobile Engagement för Windows Phone-plattformen.</span><span class="sxs-lookup"><span data-stu-id="b88de-112">The following parts refine the Mobile Engagement Concepts for the Windows Phone platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="b88de-113">`Session` och `Activity`</span><span class="sxs-lookup"><span data-stu-id="b88de-113">`Session` and `Activity`</span></span>
<span data-ttu-id="b88de-114">En *aktiviteten* beror vanligtvis på en sida av programmet, det vill säga den *aktiviteten* när sidan visas och stoppas när sidan stängs: så är fallet när Engagement-SDK är integrerad med hjälp av den `EngagementPage` klass.</span><span class="sxs-lookup"><span data-stu-id="b88de-114">An *activity* is usually associated with one page of the application, that is to say the *activity* starts when the page is displayed and stops when the page is closed: this is the case when the Engagement SDK is integrated by using the `EngagementPage` class.</span></span>

<span data-ttu-id="b88de-115">Men *aktiviteter* kan också kontrolleras manuellt med hjälp av Engagement-API.</span><span class="sxs-lookup"><span data-stu-id="b88de-115">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="b88de-116">Detta gör för att dela en viss sida i flera sub delar för att få mer information om användning av den här sidan (till exempel och hur ofta kända och hur länge dialogrutor används i den här sidan).</span><span class="sxs-lookup"><span data-stu-id="b88de-116">This allows to split a given page in several sub parts to get more details about the usage of this page (for example to known how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="b88de-117">Rapportering</span><span class="sxs-lookup"><span data-stu-id="b88de-117">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="b88de-118">Användaren startar en ny aktivitet</span><span class="sxs-lookup"><span data-stu-id="b88de-118">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-119">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-119">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="b88de-120">Du måste anropa `StartActivity()` varje gång aktiviteten användarändringar.</span><span class="sxs-lookup"><span data-stu-id="b88de-120">You need to call `StartActivity()` each time the user activity changes.</span></span> <span data-ttu-id="b88de-121">Det första anropet till funktionen startar en ny session.</span><span class="sxs-lookup"><span data-stu-id="b88de-121">The first call to this function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b88de-122">SDK anropa metoden EndActivity automatiskt när programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="b88de-122">The SDK automatically call the EndActivity method when the application is closed.</span></span> <span data-ttu-id="b88de-123">Därför rekommenderas att anropa metoden StartActivity när aktiviteten för användaren och till aldrig anropa metoden EndActivity eftersom den här metoden anropas tvingar den aktuella sessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="b88de-123">Thus, it is HIGHLY recommended to call the StartActivity method whenever the activity of the user change, and to NEVER call the EndActivity method, since calling this method forces the current session to be ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="b88de-124">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-124">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="b88de-125">Användaren avslutar sin aktuella aktiviteten</span><span class="sxs-lookup"><span data-stu-id="b88de-125">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-126">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-126">Reference</span></span>
            void EndActivity()

<span data-ttu-id="b88de-127">Du måste anropa `EndActivity()` minst en gång när användaren är klar användarens sista aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b88de-127">You need to call `EndActivity()` at least once when the user finishes his last activity.</span></span> <span data-ttu-id="b88de-128">Det informerar Engagement SDK att användaren är för närvarande inaktiv och användarsessionen måste stängas när tidsgränsen för sessionen upphör att gälla (om du anropar `StartActivity()` innan tidsgränsen för sessionen upphör sessionen bara fortsätter).</span><span class="sxs-lookup"><span data-stu-id="b88de-128">This informs the Engagement SDK that the user is currently idle, and that the user session need to be closed once the session timeout will expire (if you call `StartActivity()` before the session timeout expires, the session is simply continued).</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-129">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="b88de-130">Rapportering av jobb</span><span class="sxs-lookup"><span data-stu-id="b88de-130">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="b88de-131">Starta ett jobb</span><span class="sxs-lookup"><span data-stu-id="b88de-131">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-132">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-132">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="b88de-133">Du kan använda jobbet för att spåra vissa aktiviteter under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="b88de-133">You can use the job to track certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-134">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-134">Example</span></span>
            // An upload begins...

            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="b88de-135">Avsluta ett jobb</span><span class="sxs-lookup"><span data-stu-id="b88de-135">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-136">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-136">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="b88de-137">När en aktivitet som spåras av ett jobb har avslutats, bör du anropa metoden EndJob för det här jobbet genom att ange namnet på det jobb.</span><span class="sxs-lookup"><span data-stu-id="b88de-137">As soon as a task tracked by a job has been terminated, you should call the EndJob method for this job, by supplying the job name.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-138">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-138">Example</span></span>
            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="b88de-139">Rapporteringshändelser</span><span class="sxs-lookup"><span data-stu-id="b88de-139">Reporting Events</span></span>
<span data-ttu-id="b88de-140">Det finns tre typer av händelser:</span><span class="sxs-lookup"><span data-stu-id="b88de-140">There is three types of events :</span></span>

* <span data-ttu-id="b88de-141">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="b88de-141">Standalone events</span></span>
* <span data-ttu-id="b88de-142">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="b88de-142">Session events</span></span>
* <span data-ttu-id="b88de-143">Jobbhändelser</span><span class="sxs-lookup"><span data-stu-id="b88de-143">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="b88de-144">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="b88de-144">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-145">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-145">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="b88de-146">Fristående händelser kan inträffa för en session.</span><span class="sxs-lookup"><span data-stu-id="b88de-146">Standalone events can occur outside of the context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-147">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-147">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="b88de-148">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="b88de-148">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-149">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-149">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="b88de-150">Sessionshändelser används vanligtvis för att rapportera åtgärder som utförs av en användare under sin session.</span><span class="sxs-lookup"><span data-stu-id="b88de-150">Session events are usually used to report the actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-151">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-151">Example</span></span>
<span data-ttu-id="b88de-152">**Utan data:**</span><span class="sxs-lookup"><span data-stu-id="b88de-152">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="b88de-153">**Med data:**</span><span class="sxs-lookup"><span data-stu-id="b88de-153">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="b88de-154">Jobbhändelser</span><span class="sxs-lookup"><span data-stu-id="b88de-154">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-155">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-155">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="b88de-156">Jobbhändelser används vanligtvis för att rapportera åtgärder som utförs av en användare när ett jobb.</span><span class="sxs-lookup"><span data-stu-id="b88de-156">Job events are usually used to report the actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-157">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-157">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="b88de-158">Rapporterat fel</span><span class="sxs-lookup"><span data-stu-id="b88de-158">Reporting Errors</span></span>
<span data-ttu-id="b88de-159">Det finns tre typer av fel:</span><span class="sxs-lookup"><span data-stu-id="b88de-159">There is three types of errors :</span></span>

* <span data-ttu-id="b88de-160">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="b88de-160">Standalone errors</span></span>
* <span data-ttu-id="b88de-161">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="b88de-161">Session errors</span></span>
* <span data-ttu-id="b88de-162">Jobbfel</span><span class="sxs-lookup"><span data-stu-id="b88de-162">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="b88de-163">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="b88de-163">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-164">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-164">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="b88de-165">Strider mot session fel inträffa fristående fel utanför ramen för en session.</span><span class="sxs-lookup"><span data-stu-id="b88de-165">Contrary to session errors, standalone errors can occur outside of the context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-166">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-166">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="b88de-167">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="b88de-167">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-168">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-168">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="b88de-169">Sessionen fel används vanligtvis för att rapportera fel som påverkar användaren under sin session.</span><span class="sxs-lookup"><span data-stu-id="b88de-169">Session errors are usually used to report the errors impacting the user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-170">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-170">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="b88de-171">Jobbfel</span><span class="sxs-lookup"><span data-stu-id="b88de-171">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-172">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-172">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="b88de-173">Fel kan vara relaterad till ett jobb som körs i stället för som rör den aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="b88de-173">Errors can be related to a running job instead of being related to the current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-174">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-174">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="b88de-175">Rapportering krascher</span><span class="sxs-lookup"><span data-stu-id="b88de-175">Reporting Crashes</span></span>
<span data-ttu-id="b88de-176">Agenten erbjuder två metoder för att hantera kraschar.</span><span class="sxs-lookup"><span data-stu-id="b88de-176">The agent provides two methods to deal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="b88de-177">Skicka ett undantag</span><span class="sxs-lookup"><span data-stu-id="b88de-177">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-178">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-178">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="b88de-179">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-179">Example</span></span>
<span data-ttu-id="b88de-180">Du kan skicka ett undantag när som helst genom att anropa:</span><span class="sxs-lookup"><span data-stu-id="b88de-180">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="b88de-181">Du kan också använda en valfri parameter för att avsluta sessionen engagement samtidigt än att skicka kraschen.</span><span class="sxs-lookup"><span data-stu-id="b88de-181">You can also use an optional parameter to terminate the engagement session at the same time than sending the crash.</span></span> <span data-ttu-id="b88de-182">Det gör du genom att anropa:</span><span class="sxs-lookup"><span data-stu-id="b88de-182">To do so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="b88de-183">Om du gör det stängs sessionen och jobb när du skickar kraschen.</span><span class="sxs-lookup"><span data-stu-id="b88de-183">If you do that, the session and jobs will be closed just after sending the crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="b88de-184">Skicka ett ohanterat undantag</span><span class="sxs-lookup"><span data-stu-id="b88de-184">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="b88de-185">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-185">Reference</span></span>
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

<span data-ttu-id="b88de-186">Engagement tillhandahåller också en metod för att skicka ett ohanterat undantag.</span><span class="sxs-lookup"><span data-stu-id="b88de-186">Engagement also provides a method to send unhandled exceptions.</span></span> <span data-ttu-id="b88de-187">Detta är särskilt användbar när den används i händelsehanteraren silverlight UnhandledException.</span><span class="sxs-lookup"><span data-stu-id="b88de-187">This is especially useful when used inside the silverlight UnhandledException event handler.</span></span>

<span data-ttu-id="b88de-188">Den här metoden kommer **alltid** avslutas sessionen för engagement och jobb efter anropas.</span><span class="sxs-lookup"><span data-stu-id="b88de-188">This method will **ALWAYS** terminate the engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="b88de-189">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-189">Example</span></span>
<span data-ttu-id="b88de-190">Du kan använda den för att implementera din egen UnhandledException hanterare (särskilt om du har inaktiverat automatisk kraschen reporting-funktionen i Engagement).</span><span class="sxs-lookup"><span data-stu-id="b88de-190">You can use it to implement your own UnhandledException handler (especially if you have disabled the automatic crash reporting feature of Engagement).</span></span> <span data-ttu-id="b88de-191">Till exempel i den `Application_UnhandledException` metod för den `App.xaml.cs` filen:</span><span class="sxs-lookup"><span data-stu-id="b88de-191">For example, in the `Application_UnhandledException` method of the `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a><span data-ttu-id="b88de-192">OnActivated</span><span class="sxs-lookup"><span data-stu-id="b88de-192">OnActivated</span></span>
### <a name="reference"></a><span data-ttu-id="b88de-193">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-193">Reference</span></span>
            void OnActivated(ActivatedEventArgs e)

<span data-ttu-id="b88de-194">När användaren navigerar framåt, från ett program när inaktiverad händelsen utlöses försöker operativsystemet placera programmet i en vilande tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b88de-194">When the user navigates forward, away from an application, after the Deactivated event is raised, the operating system will attempt to put the application into a dormant state.</span></span> <span data-ttu-id="b88de-195">Programmet är tombstone-borttagning.</span><span class="sxs-lookup"><span data-stu-id="b88de-195">Then, the application is Tombstoning.</span></span> <span data-ttu-id="b88de-196">I den här processen avslutas ett program, men vissa data om tillståndet för programmet och enskilda sidor i programmet bevaras.</span><span class="sxs-lookup"><span data-stu-id="b88de-196">In this process an application is terminated but some data about the state of the application and the individual pages within the application is preserved.</span></span>

<span data-ttu-id="b88de-197">Du måste infoga `EngagementAgent.Instance.OnActivated(e)` i den `Application_Activated` metoden från filen App.xaml.cs att återställa Engagement agenten när programmet har tombstone.</span><span class="sxs-lookup"><span data-stu-id="b88de-197">You have to insert `EngagementAgent.Instance.OnActivated(e)` in the `Application_Activated` method from the App.xaml.cs file to reset the Engagement Agent when the application has been Tombstoned.</span></span>

### <a name="example"></a><span data-ttu-id="b88de-198">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-198">Example</span></span>
            // Inside your App.xaml.cs file

            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a><span data-ttu-id="b88de-199">Enhets-Id</span><span class="sxs-lookup"><span data-stu-id="b88de-199">Device Id</span></span>
            String GetDeviceId()

<span data-ttu-id="b88de-200">Du kan hämta engagement enhets-id genom att anropa den här metoden.</span><span class="sxs-lookup"><span data-stu-id="b88de-200">You can get the engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="b88de-201">Tillägg parametrar</span><span class="sxs-lookup"><span data-stu-id="b88de-201">Extras parameters</span></span>
<span data-ttu-id="b88de-202">Diverse uppgifter kan kopplas till en händelse, ett fel, en aktivitet eller ett jobb.</span><span class="sxs-lookup"><span data-stu-id="b88de-202">Arbitrary data can be attached to an event, an error, an activity or a job.</span></span> <span data-ttu-id="b88de-203">Dessa data strukturerade med hjälp av en ordlista.</span><span class="sxs-lookup"><span data-stu-id="b88de-203">These data can be structured using a dictionary.</span></span> <span data-ttu-id="b88de-204">Nycklar och värden som kan vara av valfri typ.</span><span class="sxs-lookup"><span data-stu-id="b88de-204">Keys and values can be of any type.</span></span>

<span data-ttu-id="b88de-205">Extra data serialiseras så om du vill infoga en egen typ i tillägg du vill lägga till ett datakontrakt för den här typen.</span><span class="sxs-lookup"><span data-stu-id="b88de-205">Extras data are serialized so if you want to insert your own type in extras you have to add a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="b88de-206">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-206">Example</span></span>
<span data-ttu-id="b88de-207">Vi skapar en ny klass ”Person”.</span><span class="sxs-lookup"><span data-stu-id="b88de-207">We create a new class "Person".</span></span>

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

<span data-ttu-id="b88de-208">Sedan lägger vi till en `Person` instans till en extra.</span><span class="sxs-lookup"><span data-stu-id="b88de-208">Then, we will add a `Person` instance to an extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="b88de-209">Om du placerar andra typer av objekt, kontrollera att deras toString ()-metoden implementeras för att returnera en mänsklig läsbar sträng.</span><span class="sxs-lookup"><span data-stu-id="b88de-209">If you put other types of objects, make sure their ToString() method is implemented to return a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="b88de-210">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="b88de-210">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="b88de-211">Nycklar</span><span class="sxs-lookup"><span data-stu-id="b88de-211">Keys</span></span>
<span data-ttu-id="b88de-212">Varje nyckel i objektet måste matcha följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="b88de-212">Each key in the object must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="b88de-213">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="b88de-213">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="b88de-214">Storlek</span><span class="sxs-lookup"><span data-stu-id="b88de-214">Size</span></span>
<span data-ttu-id="b88de-215">Tillägg är begränsade till **1024** tecken per anrop.</span><span class="sxs-lookup"><span data-stu-id="b88de-215">Extras are limited to **1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="b88de-216">Rapportering programinformation</span><span class="sxs-lookup"><span data-stu-id="b88de-216">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="b88de-217">Referens</span><span class="sxs-lookup"><span data-stu-id="b88de-217">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="b88de-218">Du kan manuellt rapportera spåra information (eller andra program specifik information) med hjälp av SendAppInfo() funktion.</span><span class="sxs-lookup"><span data-stu-id="b88de-218">You can manually report tracking information (or any other application specific information) using the SendAppInfo() function.</span></span>

<span data-ttu-id="b88de-219">Observera att denna information kan skickas inkrementellt: endast det senaste värdet för en viss nyckel sparas för en viss enhet.</span><span class="sxs-lookup"><span data-stu-id="b88de-219">Note that these information can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="b88de-220">Använda en ordlista som händelsen tillägg\<objekt, objektet\> bifoga information.</span><span class="sxs-lookup"><span data-stu-id="b88de-220">Like event extras, use a Dictionary\<object, object\> to attach informations.</span></span>

### <a name="example"></a><span data-ttu-id="b88de-221">Exempel</span><span class="sxs-lookup"><span data-stu-id="b88de-221">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="b88de-222">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="b88de-222">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="b88de-223">Nycklar</span><span class="sxs-lookup"><span data-stu-id="b88de-223">Keys</span></span>
<span data-ttu-id="b88de-224">Varje nyckel i objektet måste matcha följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="b88de-224">Each key in the object must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="b88de-225">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="b88de-225">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="b88de-226">Storlek</span><span class="sxs-lookup"><span data-stu-id="b88de-226">Size</span></span>
<span data-ttu-id="b88de-227">Information om programmet är begränsade till **1024** tecken per anrop.</span><span class="sxs-lookup"><span data-stu-id="b88de-227">Application information are limited to **1024** characters per call.</span></span>

<span data-ttu-id="b88de-228">I exemplet ovan är JSON som skickas till servern 44 tecken:</span><span class="sxs-lookup"><span data-stu-id="b88de-228">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a><span data-ttu-id="b88de-229">Loggning</span><span class="sxs-lookup"><span data-stu-id="b88de-229">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="b88de-230">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="b88de-230">Enable Logging</span></span>
<span data-ttu-id="b88de-231">SDK kan konfigureras för att skapa test loggar i IDE-konsolen.</span><span class="sxs-lookup"><span data-stu-id="b88de-231">The SDK can be configured to produce test logs in the IDE console.</span></span>
<span data-ttu-id="b88de-232">Dessa loggar är inte aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="b88de-232">These logs are not activated by default.</span></span> <span data-ttu-id="b88de-233">Anpassa detta genom att uppdatera egenskapen `EngagementAgent.Instance.TestLogEnabled` till ett värde som är tillgängliga från den `EngagementTestLogLevel` uppräkning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="b88de-233">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
