---
title: "Hur du använder Engagement API på Android"
description: "Senaste Android SDK - hur du använder Engagement API på Android"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 09b62659-82ae-4a55-8784-fca0b6b22eaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: d353cd2fe47c54a0282cc5bb1b22b4a56e0cd82c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-android"></a><span data-ttu-id="456d4-103">Hur du använder Engagement API på Android</span><span class="sxs-lookup"><span data-stu-id="456d4-103">How to Use the Engagement API on Android</span></span>
<span data-ttu-id="456d4-104">Det här dokumentet är ett tillägg till dokumentet [Reporting avancerade alternativ för Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="456d4-104">This document is an add-on to the document [Advanced Reporting options for Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span></span> <span data-ttu-id="456d4-105">Det ger i djup information om hur du använder Engagement API för att rapportera programmet-statistik.</span><span class="sxs-lookup"><span data-stu-id="456d4-105">It provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="456d4-106">Kom ihåg att om du bara vill Engagement att rapportera programmets sessioner, aktiviteter, krascher och teknisk information sedan det enklaste sättet är att se alla dina `Activity` underordnade klasser ärver från motsvarande `EngagementActivity` klass.</span><span class="sxs-lookup"><span data-stu-id="456d4-106">Keep in mind that if you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your `Activity` sub-classes inherit from the corresponding `EngagementActivity` class.</span></span>

<span data-ttu-id="456d4-107">Om du vill göra mer, till exempel om du behöver rapportera programmet specifika händelser, fel och jobb, eller om du vill rapportera ditt programs aktiviteter i ett annat sätt än den som implementerats i den `EngagementActivity` klasser, måste du använda Engagement-API.</span><span class="sxs-lookup"><span data-stu-id="456d4-107">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementActivity` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="456d4-108">Engagement-API som tillhandahålls av den `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="456d4-108">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="456d4-109">En instans av den här klassen kan hämtas genom att anropa den `EngagementAgent.getInstance(Context)` statisk metod (Observera att den `EngagementAgent` objektet som returnerades är en singleton).</span><span class="sxs-lookup"><span data-stu-id="456d4-109">An instance of this class can be retrieved by calling the `EngagementAgent.getInstance(Context)` static method (note that the `EngagementAgent` object returned is a singleton).</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="456d4-110">Koncept i engagement</span><span class="sxs-lookup"><span data-stu-id="456d4-110">Engagement concepts</span></span>
<span data-ttu-id="456d4-111">Följande delar förfina vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md), för Android-plattformen.</span><span class="sxs-lookup"><span data-stu-id="456d4-111">The following parts refine the common [Mobile Engagement Concepts](mobile-engagement-concepts.md), for the Android platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="456d4-112">`Session` och `Activity`</span><span class="sxs-lookup"><span data-stu-id="456d4-112">`Session` and `Activity`</span></span>
<span data-ttu-id="456d4-113">Om användaren är fler än några få sekunder inaktiv mellan två *aktiviteter*, sedan hans sekvens av *aktiviteter* är uppdelad i två olika *sessioner*.</span><span class="sxs-lookup"><span data-stu-id="456d4-113">If the user stays more than a few seconds idle between two *activities*, then his sequence of *activities* is split in two distinct *sessions*.</span></span> <span data-ttu-id="456d4-114">Dessa några sekunder kallas ”sessionstidsgränsen”.</span><span class="sxs-lookup"><span data-stu-id="456d4-114">These few seconds are called the "session timeout".</span></span>

<span data-ttu-id="456d4-115">En *aktiviteten* är ofta kopplade till en skärm för programmet, det vill säga den *aktiviteten* när skärmen visas och stoppas när skärmen stängs: så är fallet när Engagement-SDK är integrerad med hjälp av den `EngagementActivity` klasser.</span><span class="sxs-lookup"><span data-stu-id="456d4-115">An *activity* is usually associated with one screen of the application, that is to say the *activity* starts when the screen is displayed and stops when the screen is closed: this is the case when the Engagement SDK is integrated by using the `EngagementActivity` classes.</span></span>

<span data-ttu-id="456d4-116">Men *aktiviteter* kan också kontrolleras manuellt med hjälp av Engagement-API.</span><span class="sxs-lookup"><span data-stu-id="456d4-116">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="456d4-117">Detta gör för att dela en viss skärm i flera sub delar för att få mer information om användning av den här skärmen (till exempel hur ofta kända och hur länge dialogrutor används i den här skärmen).</span><span class="sxs-lookup"><span data-stu-id="456d4-117">This allows to split a given screen in several sub parts to get more details about the usage of this screen (for example to known how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="456d4-118">Rapportering</span><span class="sxs-lookup"><span data-stu-id="456d4-118">Reporting Activities</span></span>
> [!IMPORTANT]
> <span data-ttu-id="456d4-119">Du behöver inte rapporten aktiviteter som beskrivs i det här avsnittet om du använder den `EngagementActivity` klassen och dess varianter enligt beskrivningen i hur du integrerar Engagement för Android-dokument.</span><span class="sxs-lookup"><span data-stu-id="456d4-119">You don't need to report activities like described in this section if you are using the `EngagementActivity` class and its variants as explained in the How to Integrate Engagement on Android document.</span></span>
> 
> 

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="456d4-120">Användaren startar en ny aktivitet</span><span class="sxs-lookup"><span data-stu-id="456d4-120">User starts a new Activity</span></span>
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

<span data-ttu-id="456d4-121">Du måste anropa `startActivity()` varje gång aktiviteten användarändringar.</span><span class="sxs-lookup"><span data-stu-id="456d4-121">You need to call `startActivity()` each time the user activity changes.</span></span> <span data-ttu-id="456d4-122">Det första anropet till funktionen startar en ny session.</span><span class="sxs-lookup"><span data-stu-id="456d4-122">The first call to this function starts a new user session.</span></span>

<span data-ttu-id="456d4-123">Den bästa platsen för att anropa den här funktionen finns på varje aktivitet `onResume` återanrop.</span><span class="sxs-lookup"><span data-stu-id="456d4-123">The best place to call this function is on each activity `onResume` callback.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="456d4-124">Användaren avslutar sin aktuella aktiviteten</span><span class="sxs-lookup"><span data-stu-id="456d4-124">User ends his current Activity</span></span>
            EngagementAgent.getInstance(this).endActivity();

<span data-ttu-id="456d4-125">Du måste anropa `endActivity()` minst en gång när användaren är klar användarens sista aktivitet.</span><span class="sxs-lookup"><span data-stu-id="456d4-125">You need to call `endActivity()` at least once when the user finishes his last activity.</span></span> <span data-ttu-id="456d4-126">Det informerar Engagement SDK att användaren är för närvarande inaktiv och användarsessionen måste stängas när tidsgränsen för sessionen upphör att gälla (om du anropar `startActivity()` innan tidsgränsen för sessionen upphör sessionen bara återupptas).</span><span class="sxs-lookup"><span data-stu-id="456d4-126">This informs the Engagement SDK that the user is currently idle, and that the user session need to be closed once the session timeout will expire (if you call `startActivity()` before the session timeout expires, the session is simply resumed).</span></span>

<span data-ttu-id="456d4-127">Den bästa platsen för att anropa den här funktionen finns på varje aktivitet `onPause` återanrop.</span><span class="sxs-lookup"><span data-stu-id="456d4-127">The best place to call this function is on each activity `onPause` callback.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="456d4-128">Rapporteringshändelser</span><span class="sxs-lookup"><span data-stu-id="456d4-128">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="456d4-129">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="456d4-129">Session events</span></span>
<span data-ttu-id="456d4-130">Sessionshändelser används vanligtvis för att rapportera åtgärder som utförs av en användare under sin session.</span><span class="sxs-lookup"><span data-stu-id="456d4-130">Session events are usually used to report the actions performed by a user during his session.</span></span>

<span data-ttu-id="456d4-131">**Exempel utan extra data:**</span><span class="sxs-lookup"><span data-stu-id="456d4-131">**Example without extra data:**</span></span>

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

<span data-ttu-id="456d4-132">**Exempel med extra data:**</span><span class="sxs-lookup"><span data-stu-id="456d4-132">**Example with extra data:**</span></span>

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a><span data-ttu-id="456d4-133">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="456d4-133">Standalone Events</span></span>
<span data-ttu-id="456d4-134">Strider mot Sessionshändelser, kan det ske fristående händelser utanför ramen för en session.</span><span class="sxs-lookup"><span data-stu-id="456d4-134">Contrary to session events, standalone events can occur outside of the context of a session.</span></span>

<span data-ttu-id="456d4-135">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="456d4-135">**Example:**</span></span>

<span data-ttu-id="456d4-136">Anta att du vill rapporten händelser som inträffar när en broadcast mottagare utlöses:</span><span class="sxs-lookup"><span data-stu-id="456d4-136">Suppose you want to report events occurring when a broadcast receiver is triggered:</span></span>

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a><span data-ttu-id="456d4-137">Rapporterat fel</span><span class="sxs-lookup"><span data-stu-id="456d4-137">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="456d4-138">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="456d4-138">Session errors</span></span>
<span data-ttu-id="456d4-139">Sessionen fel används vanligtvis för att rapportera fel som påverkar användaren under sin session.</span><span class="sxs-lookup"><span data-stu-id="456d4-139">Session errors are usually used to report the errors impacting the user during his session.</span></span>

<span data-ttu-id="456d4-140">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="456d4-140">**Example:**</span></span>

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a><span data-ttu-id="456d4-141">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="456d4-141">Standalone errors</span></span>
<span data-ttu-id="456d4-142">Strider mot session fel inträffa fristående fel utanför ramen för en session.</span><span class="sxs-lookup"><span data-stu-id="456d4-142">Contrary to session errors, standalone errors can occur outside of the context of a session.</span></span>

<span data-ttu-id="456d4-143">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="456d4-143">**Example:**</span></span>

<span data-ttu-id="456d4-144">I följande exempel visas hur du rapporterar ett fel när minnet blir låg på telefonen medan programmet processen körs.</span><span class="sxs-lookup"><span data-stu-id="456d4-144">The following example shows how to report an error whenever the memory becomes low on the phone while your application process is running.</span></span>

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a><span data-ttu-id="456d4-145">Rapportering av jobb</span><span class="sxs-lookup"><span data-stu-id="456d4-145">Reporting Jobs</span></span>
### <a name="example"></a><span data-ttu-id="456d4-146">Exempel</span><span class="sxs-lookup"><span data-stu-id="456d4-146">Example</span></span>
<span data-ttu-id="456d4-147">Anta att du vill rapportera varaktighet logga in:</span><span class="sxs-lookup"><span data-stu-id="456d4-147">Suppose you want to report the duration of your login process:</span></span>

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a><span data-ttu-id="456d4-148">Rapportera fel under ett jobb</span><span class="sxs-lookup"><span data-stu-id="456d4-148">Report Errors during a Job</span></span>
<span data-ttu-id="456d4-149">Fel kan vara relaterad till ett jobb som körs i stället för som rör den aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="456d4-149">Errors can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="456d4-150">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="456d4-150">**Example:**</span></span>

<span data-ttu-id="456d4-151">Anta att du vill rapportera ett fel under du logga in processen:</span><span class="sxs-lookup"><span data-stu-id="456d4-151">Suppose you want to report an error during you login process:</span></span>

<span data-ttu-id="456d4-152">[...] offentliga void inloggning (kontexten kontext,...) {</span><span class="sxs-lookup"><span data-stu-id="456d4-152">[...] public void signIn(Context context, ...) {</span></span>

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="456d4-153">Rapporteringshändelser under ett jobb</span><span class="sxs-lookup"><span data-stu-id="456d4-153">Reporting Events during a job</span></span>
<span data-ttu-id="456d4-154">Händelser kan vara relaterad till ett jobb som körs i stället för som rör den aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="456d4-154">Events can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="456d4-155">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="456d4-155">**Example:**</span></span>

<span data-ttu-id="456d4-156">Anta att vi har ett sociala nätverk och vi använder ett jobb att rapportera den totala tiden under vilken användaren är ansluten till servern.</span><span class="sxs-lookup"><span data-stu-id="456d4-156">Suppose we have a social network, and we use a job to report the total time during which the user is connected to the server.</span></span> <span data-ttu-id="456d4-157">Användaren kan vara ansluten i bakgrunden även när han använder ett annat program eller när telefonen är i viloläge, så det finns ingen session.</span><span class="sxs-lookup"><span data-stu-id="456d4-157">The user can stay connected in background even when he's using another application or when the phone is sleeping, so there is no session.</span></span>

<span data-ttu-id="456d4-158">Användaren kan ta emot meddelanden från sina vänner, detta är en jobbhändelse.</span><span class="sxs-lookup"><span data-stu-id="456d4-158">The user can receive messages from his friends, this is a job event.</span></span>

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

## <a name="extra-parameters"></a><span data-ttu-id="456d4-159">Extra parametrar</span><span class="sxs-lookup"><span data-stu-id="456d4-159">Extra parameters</span></span>
<span data-ttu-id="456d4-160">Diverse uppgifter kan kopplas till händelser, fel, aktiviteter och jobb.</span><span class="sxs-lookup"><span data-stu-id="456d4-160">Arbitrary data can be attached to events, errors, activities and jobs.</span></span>

<span data-ttu-id="456d4-161">Dessa data kan vara strukturerad, används Androids paket klass (faktiskt, den fungerar som extra parametrar i Android avsikter).</span><span class="sxs-lookup"><span data-stu-id="456d4-161">This data can be structured, it uses Android's Bundle class (actually, it works like extra parameters in Android Intents).</span></span> <span data-ttu-id="456d4-162">Observera att ett paket kan innehålla matriser eller ett annat paket instanser.</span><span class="sxs-lookup"><span data-stu-id="456d4-162">Note that a Bundle can contain arrays or another Bundle instances.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="456d4-163">Om du lägger till i parcelable eller serializable parametrar, kontrollerar du att deras `toString()` metoden implementeras för att returnera en läsbar sträng.</span><span class="sxs-lookup"><span data-stu-id="456d4-163">If you put in parcelable or serializable parameters, make sure their `toString()` method is implemented to return a human-readable string.</span></span> <span data-ttu-id="456d4-164">Serialiserbara klasser som innehåller icke tillfälligt fält som inte kan serialiseras gör Android kraschar när du anropar`bundle.putSerializable("key",value);`</span><span class="sxs-lookup"><span data-stu-id="456d4-164">Serializable classes that contain non transient fields that are not serializable will make Android crash when you will call `bundle.putSerializable("key",value);`</span></span>
> 
> [!WARNING]
> <span data-ttu-id="456d4-165">Glesa matriser i extra parametrar stöds inte, det vill säga den kommer inte att serialisera som en matris.</span><span class="sxs-lookup"><span data-stu-id="456d4-165">Sparse arrays in extra parameters are not supported, that is, it won't be serialized as an array.</span></span> <span data-ttu-id="456d4-166">Du måste konvertera dem till standard matriser innan den används i extra parametrar.</span><span class="sxs-lookup"><span data-stu-id="456d4-166">You should convert them into standard arrays before using it in extra parameters.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="456d4-167">Exempel</span><span class="sxs-lookup"><span data-stu-id="456d4-167">Example</span></span>
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="456d4-168">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="456d4-168">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="456d4-169">Nycklar</span><span class="sxs-lookup"><span data-stu-id="456d4-169">Keys</span></span>
<span data-ttu-id="456d4-170">Varje nyckel i den `Bundle` måste överensstämma med följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="456d4-170">Each key in the `Bundle` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="456d4-171">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="456d4-171">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="456d4-172">Storlek</span><span class="sxs-lookup"><span data-stu-id="456d4-172">Size</span></span>
<span data-ttu-id="456d4-173">Tillägg är begränsade till **1024** tecken per anrop (efter kodning i JSON av tjänsten Engagement).</span><span class="sxs-lookup"><span data-stu-id="456d4-173">Extras are limited to **1024** characters per call (once encoded in JSON by the Engagement service).</span></span>

<span data-ttu-id="456d4-174">I exemplet ovan är JSON som skickas till servern 58 tecken:</span><span class="sxs-lookup"><span data-stu-id="456d4-174">In the previous example, the JSON sent to the server is 58 characters long:</span></span>

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="456d4-175">Rapportering programinformation</span><span class="sxs-lookup"><span data-stu-id="456d4-175">Reporting Application Information</span></span>
<span data-ttu-id="456d4-176">Du kan manuellt rapportera spåra information (eller andra program specifik information) med hjälp av den `sendAppInfo()` funktion.</span><span class="sxs-lookup"><span data-stu-id="456d4-176">You can manually report tracking information (or any other application specific information) using the `sendAppInfo()` function.</span></span>

<span data-ttu-id="456d4-177">Observera att denna information kan skickas inkrementellt: endast det senaste värdet för en viss nyckel sparas för en viss enhet.</span><span class="sxs-lookup"><span data-stu-id="456d4-177">Note that these information can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="456d4-178">Som händelsen tillägg används paket klassen för att abstrahera information om programmet, Observera att matriser eller underordnade paket kommer att behandlas som flat strängar (med JSON-serialisering).</span><span class="sxs-lookup"><span data-stu-id="456d4-178">Like event extras, the Bundle class is used to abstract application information, note that arrays or sub-bundles will be treated as flat strings (using JSON serialization).</span></span>

### <a name="example"></a><span data-ttu-id="456d4-179">Exempel</span><span class="sxs-lookup"><span data-stu-id="456d4-179">Example</span></span>
<span data-ttu-id="456d4-180">Här är ett kodexempel för att skicka användaren kön och födelsedatum:</span><span class="sxs-lookup"><span data-stu-id="456d4-180">Here is a code sample to send user gender and birthdate:</span></span>

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="456d4-181">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="456d4-181">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="456d4-182">Nycklar</span><span class="sxs-lookup"><span data-stu-id="456d4-182">Keys</span></span>
<span data-ttu-id="456d4-183">Varje nyckel i den `Bundle` måste överensstämma med följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="456d4-183">Each key in the `Bundle` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="456d4-184">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="456d4-184">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="456d4-185">Storlek</span><span class="sxs-lookup"><span data-stu-id="456d4-185">Size</span></span>
<span data-ttu-id="456d4-186">Information om programmet är begränsade till **1024** tecken per anrop (efter kodning i JSON av tjänsten Engagement).</span><span class="sxs-lookup"><span data-stu-id="456d4-186">Application information are limited to **1024** characters per call (once encoded in JSON by the Engagement service).</span></span>

<span data-ttu-id="456d4-187">I exemplet ovan är JSON som skickas till servern 44 tecken:</span><span class="sxs-lookup"><span data-stu-id="456d4-187">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

            {"expiration":"2016-12-07","status":"premium"}
