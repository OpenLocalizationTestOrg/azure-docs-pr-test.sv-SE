---
title: "aaaHow tooUse hello Engagement API på Android"
description: "Senaste Android SDK - hur tooUse hello Engagement API på Android"
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
ms.openlocfilehash: e0b2d484616c0c7874e77c5283d94c3063949ed2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-android"></a><span data-ttu-id="5b3c4-103">Hur tooUse hello Engagement API på Android</span><span class="sxs-lookup"><span data-stu-id="5b3c4-103">How tooUse hello Engagement API on Android</span></span>
<span data-ttu-id="5b3c4-104">Det här dokumentet är tillägg toohello [Reporting avancerade alternativ för Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-104">This document is an add-on toohello document [Advanced Reporting options for Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span></span> <span data-ttu-id="5b3c4-105">Det ger i djup information om hur hello toouse Engagement API tooreport programstatistik.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-105">It provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="5b3c4-106">Tänk på att om du bara vill Engagement tooreport programmets sessioner, aktiviteter, krascher och teknisk information sedan hello enklaste sättet är toomake alla dina `Activity` underordnade klasser ärver från hello motsvarande `EngagementActivity` klass.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-106">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `Activity` sub-classes inherit from hello corresponding `EngagementActivity` class.</span></span>

<span data-ttu-id="5b3c4-107">Om du vill toodo mer, till exempel om du behöver tooreport programmet specifika händelser, fel och jobb, eller om du har tooreport ditt programs aktiviteter i ett annat sätt än hello en implementeras i hello `EngagementActivity` klasser måste toouse hello Engagement API.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-107">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementActivity` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="5b3c4-108">Hej Engagement API tillhandahålls av hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-108">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="5b3c4-109">En instans av den här klassen kan hämtas genom att anropa hello `EngagementAgent.getInstance(Context)` statisk metod (Observera att hello `EngagementAgent` objektet som returnerades är en singleton).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-109">An instance of this class can be retrieved by calling hello `EngagementAgent.getInstance(Context)` static method (note that hello `EngagementAgent` object returned is a singleton).</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="5b3c4-110">Koncept i engagement</span><span class="sxs-lookup"><span data-stu-id="5b3c4-110">Engagement concepts</span></span>
<span data-ttu-id="5b3c4-111">hello följande delar förfina hello vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md), för hello Android-plattformen.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md), for hello Android platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="5b3c4-112">`Session` och `Activity`</span><span class="sxs-lookup"><span data-stu-id="5b3c4-112">`Session` and `Activity`</span></span>
<span data-ttu-id="5b3c4-113">Om hello användaren är fler än några få sekunder inaktiv mellan två *aktiviteter*, sedan hans sekvens av *aktiviteter* är uppdelad i två olika *sessioner*.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-113">If hello user stays more than a few seconds idle between two *activities*, then his sequence of *activities* is split in two distinct *sessions*.</span></span> <span data-ttu-id="5b3c4-114">Dessa några sekunder kallas hello ”sessionstidsgränsen”.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-114">These few seconds are called hello "session timeout".</span></span>

<span data-ttu-id="5b3c4-115">En *aktiviteten* är ofta kopplade till en skärm på hello-program som är toosay hello *aktiviteten* när hello-skärmen visas och stoppas när hello-skärmen stängs: Detta är hello fall när Hej Engagement SDK är integrerad med hjälp av hello `EngagementActivity` klasser.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-115">An *activity* is usually associated with one screen of hello application, that is toosay hello *activity* starts when hello screen is displayed and stops when hello screen is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementActivity` classes.</span></span>

<span data-ttu-id="5b3c4-116">Men *aktiviteter* kan också kontrolleras manuellt med hjälp av hello Engagement API.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-116">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="5b3c4-117">Detta gör toosplit en viss skärm i flera sub delar tooget mer information om hello användning av den här skärmen (till exempel hur ofta tooknown och hur länge dialogrutor används i den här skärmen).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-117">This allows toosplit a given screen in several sub parts tooget more details about hello usage of this screen (for example tooknown how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="5b3c4-118">Rapportering</span><span class="sxs-lookup"><span data-stu-id="5b3c4-118">Reporting Activities</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5b3c4-119">Du behöver inte tooreport aktiviteter som beskrivs i det här avsnittet om du använder hello `EngagementActivity` klassen och dess varianter enligt beskrivningen i hello hur tooIntegrate Engagement för Android-dokument.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-119">You don't need tooreport activities like described in this section if you are using hello `EngagementActivity` class and its variants as explained in hello How tooIntegrate Engagement on Android document.</span></span>
> 
> 

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="5b3c4-120">Användaren startar en ny aktivitet</span><span class="sxs-lookup"><span data-stu-id="5b3c4-120">User starts a new Activity</span></span>
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing hello current activity is required for Reach toodisplay in-app notifications, passing null will postpone such announcements and polls.

<span data-ttu-id="5b3c4-121">Du behöver toocall `startActivity()` varje gång hello användaraktivitet ändras.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-121">You need toocall `startActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="5b3c4-122">hello första anropet toothis funktionen startar en ny session.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-122">hello first call toothis function starts a new user session.</span></span>

<span data-ttu-id="5b3c4-123">Hej bästa plats toocall den här funktionen finns på varje aktivitet `onResume` återanrop.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-123">hello best place toocall this function is on each activity `onResume` callback.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="5b3c4-124">Användaren avslutar sin aktuella aktiviteten</span><span class="sxs-lookup"><span data-stu-id="5b3c4-124">User ends his current Activity</span></span>
            EngagementAgent.getInstance(this).endActivity();

<span data-ttu-id="5b3c4-125">Du behöver toocall `endActivity()` minst en gång när hello användaren är klar användarens sista aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-125">You need toocall `endActivity()` at least once when hello user finishes his last activity.</span></span> <span data-ttu-id="5b3c4-126">Det informerar hello Engagement SDK att hello användare är för närvarande inaktiv och att hello användarsession måste toobe stängd när hello sessionstidsgränsen upphör att gälla (om du anropar `startActivity()` innan hello sessionstidsgränsen upphör att gälla hello-sessionen återupptas bara).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-126">This informs hello Engagement SDK that hello user is currently idle, and that hello user session need toobe closed once hello session timeout will expire (if you call `startActivity()` before hello session timeout expires, hello session is simply resumed).</span></span>

<span data-ttu-id="5b3c4-127">Hej bästa plats toocall den här funktionen finns på varje aktivitet `onPause` återanrop.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-127">hello best place toocall this function is on each activity `onPause` callback.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="5b3c4-128">Rapporteringshändelser</span><span class="sxs-lookup"><span data-stu-id="5b3c4-128">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="5b3c4-129">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="5b3c4-129">Session events</span></span>
<span data-ttu-id="5b3c4-130">Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under sin session.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-130">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

<span data-ttu-id="5b3c4-131">**Exempel utan extra data:**</span><span class="sxs-lookup"><span data-stu-id="5b3c4-131">**Example without extra data:**</span></span>

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

<span data-ttu-id="5b3c4-132">**Exempel med extra data:**</span><span class="sxs-lookup"><span data-stu-id="5b3c4-132">**Example with extra data:**</span></span>

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

### <a name="standalone-events"></a><span data-ttu-id="5b3c4-133">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="5b3c4-133">Standalone Events</span></span>
<span data-ttu-id="5b3c4-134">Motstridiga toosession händelser, fristående händelser kan inträffa utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-134">Contrary toosession events, standalone events can occur outside of hello context of a session.</span></span>

<span data-ttu-id="5b3c4-135">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="5b3c4-135">**Example:**</span></span>

<span data-ttu-id="5b3c4-136">Anta att du vill tooreport händelser som inträffar när en broadcast mottagare utlöses:</span><span class="sxs-lookup"><span data-stu-id="5b3c4-136">Suppose you want tooreport events occurring when a broadcast receiver is triggered:</span></span>

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a><span data-ttu-id="5b3c4-137">Rapporterat fel</span><span class="sxs-lookup"><span data-stu-id="5b3c4-137">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="5b3c4-138">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="5b3c4-138">Session errors</span></span>
<span data-ttu-id="5b3c4-139">Sessionen felen är oftast används tooreport hello fel påverkar hello användaren under sin session.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-139">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

<span data-ttu-id="5b3c4-140">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="5b3c4-140">**Example:**</span></span>

            /** hello user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* hello user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a><span data-ttu-id="5b3c4-141">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="5b3c4-141">Standalone errors</span></span>
<span data-ttu-id="5b3c4-142">Motstridiga toosession fel, fristående kan det uppstå fel utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-142">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

<span data-ttu-id="5b3c4-143">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="5b3c4-143">**Example:**</span></span>

<span data-ttu-id="5b3c4-144">hello följande exempel visas hur tooreport ett fel när hello minne blir låg på hello telefon när programmet processen körs.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-144">hello following example shows how tooreport an error whenever hello memory becomes low on hello phone while your application process is running.</span></span>

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a><span data-ttu-id="5b3c4-145">Rapportering av jobb</span><span class="sxs-lookup"><span data-stu-id="5b3c4-145">Reporting Jobs</span></span>
### <a name="example"></a><span data-ttu-id="5b3c4-146">Exempel</span><span class="sxs-lookup"><span data-stu-id="5b3c4-146">Example</span></span>
<span data-ttu-id="5b3c4-147">Anta att du vill tooreport hello varaktighet logga in:</span><span class="sxs-lookup"><span data-stu-id="5b3c4-147">Suppose you want tooreport hello duration of your login process:</span></span>

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context toocall hello Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a><span data-ttu-id="5b3c4-148">Rapportera fel under ett jobb</span><span class="sxs-lookup"><span data-stu-id="5b3c4-148">Report Errors during a Job</span></span>
<span data-ttu-id="5b3c4-149">Fel kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-149">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="5b3c4-150">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="5b3c4-150">**Example:**</span></span>

<span data-ttu-id="5b3c4-151">Anta att du vill tooreport ett fel under du logga in processen:</span><span class="sxs-lookup"><span data-stu-id="5b3c4-151">Suppose you want tooreport an error during you login process:</span></span>

<span data-ttu-id="5b3c4-152">[...] offentliga void inloggning (kontexten kontext,...) {</span><span class="sxs-lookup"><span data-stu-id="5b3c4-152">[...] public void signIn(Context context, ...) {</span></span>

              /* We need an Android context toocall hello Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try toosign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report hello error tooEngagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="5b3c4-153">Rapporteringshändelser under ett jobb</span><span class="sxs-lookup"><span data-stu-id="5b3c4-153">Reporting Events during a job</span></span>
<span data-ttu-id="5b3c4-154">Händelser kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-154">Events can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="5b3c4-155">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="5b3c4-155">**Example:**</span></span>

<span data-ttu-id="5b3c4-156">Anta att vi har ett sociala nätverk och vi använder en jobbet tooreport hello total tid under vilken hello användare är anslutna toohello server.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-156">Suppose we have a social network, and we use a job tooreport hello total time during which hello user is connected toohello server.</span></span> <span data-ttu-id="5b3c4-157">hello användaren kan vara ansluten i bakgrunden även när han använder ett annat program eller hello telefon är i viloläge, så det finns ingen session.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-157">hello user can stay connected in background even when he's using another application or when hello phone is sleeping, so there is no session.</span></span>

<span data-ttu-id="5b3c4-158">hello användare kan ta emot meddelanden från sina vänner, detta är en jobbhändelse.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-158">hello user can receive messages from his friends, this is a job event.</span></span>

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

## <a name="extra-parameters"></a><span data-ttu-id="5b3c4-159">Extra parametrar</span><span class="sxs-lookup"><span data-stu-id="5b3c4-159">Extra parameters</span></span>
<span data-ttu-id="5b3c4-160">Godtycklig data kan vara anslutna tooevents, fel, aktiviteter och jobb.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-160">Arbitrary data can be attached tooevents, errors, activities and jobs.</span></span>

<span data-ttu-id="5b3c4-161">Dessa data kan vara strukturerad, används Androids paket klass (faktiskt, den fungerar som extra parametrar i Android avsikter).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-161">This data can be structured, it uses Android's Bundle class (actually, it works like extra parameters in Android Intents).</span></span> <span data-ttu-id="5b3c4-162">Observera att ett paket kan innehålla matriser eller ett annat paket instanser.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-162">Note that a Bundle can contain arrays or another Bundle instances.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b3c4-163">Om du lägger till i parcelable eller serializable parametrar, kontrollerar du att deras `toString()` metoden är implementerad tooreturn läsbart sträng.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-163">If you put in parcelable or serializable parameters, make sure their `toString()` method is implemented tooreturn a human-readable string.</span></span> <span data-ttu-id="5b3c4-164">Serialiserbara klasser som innehåller icke tillfälligt fält som inte kan serialiseras gör Android kraschar när du anropar`bundle.putSerializable("key",value);`</span><span class="sxs-lookup"><span data-stu-id="5b3c4-164">Serializable classes that contain non transient fields that are not serializable will make Android crash when you will call `bundle.putSerializable("key",value);`</span></span>
> 
> [!WARNING]
> <span data-ttu-id="5b3c4-165">Glesa matriser i extra parametrar stöds inte, det vill säga den kommer inte att serialisera som en matris.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-165">Sparse arrays in extra parameters are not supported, that is, it won't be serialized as an array.</span></span> <span data-ttu-id="5b3c4-166">Du måste konvertera dem till standard matriser innan den används i extra parametrar.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-166">You should convert them into standard arrays before using it in extra parameters.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="5b3c4-167">Exempel</span><span class="sxs-lookup"><span data-stu-id="5b3c4-167">Example</span></span>
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="5b3c4-168">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="5b3c4-168">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="5b3c4-169">Nycklar</span><span class="sxs-lookup"><span data-stu-id="5b3c4-169">Keys</span></span>
<span data-ttu-id="5b3c4-170">Varje nyckel i hello `Bundle` måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="5b3c4-170">Each key in hello `Bundle` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="5b3c4-171">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-171">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="5b3c4-172">Storlek</span><span class="sxs-lookup"><span data-stu-id="5b3c4-172">Size</span></span>
<span data-ttu-id="5b3c4-173">Tillägg är begränsad för**1024** tecken per anrop (efter kodning i JSON av hello Engagement service).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-173">Extras are limited too**1024** characters per call (once encoded in JSON by hello Engagement service).</span></span>

<span data-ttu-id="5b3c4-174">I hello är 58 tecken i föregående exempel hello JSON skickas toohello server:</span><span class="sxs-lookup"><span data-stu-id="5b3c4-174">In hello previous example, hello JSON sent toohello server is 58 characters long:</span></span>

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="5b3c4-175">Rapportering programinformation</span><span class="sxs-lookup"><span data-stu-id="5b3c4-175">Reporting Application Information</span></span>
<span data-ttu-id="5b3c4-176">Du kan manuellt rapportera spåra information (eller andra program specifik information) med hjälp av hello `sendAppInfo()` funktion.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-176">You can manually report tracking information (or any other application specific information) using hello `sendAppInfo()` function.</span></span>

<span data-ttu-id="5b3c4-177">Observera att denna information kan skickas inkrementellt: endast hello senaste värdet för en viss nyckel sparas för en viss enhet.</span><span class="sxs-lookup"><span data-stu-id="5b3c4-177">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="5b3c4-178">Hello paket klass är används tooabstract programinformation som händelsen tillägg, Observera att matriser eller underordnad paketerar behandlas som flat strängar (med JSON-serialisering).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-178">Like event extras, hello Bundle class is used tooabstract application information, note that arrays or sub-bundles will be treated as flat strings (using JSON serialization).</span></span>

### <a name="example"></a><span data-ttu-id="5b3c4-179">Exempel</span><span class="sxs-lookup"><span data-stu-id="5b3c4-179">Example</span></span>
<span data-ttu-id="5b3c4-180">Här är en kod exempel toosend användaren kön och födelsedatum:</span><span class="sxs-lookup"><span data-stu-id="5b3c4-180">Here is a code sample toosend user gender and birthdate:</span></span>

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="5b3c4-181">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="5b3c4-181">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="5b3c4-182">Nycklar</span><span class="sxs-lookup"><span data-stu-id="5b3c4-182">Keys</span></span>
<span data-ttu-id="5b3c4-183">Varje nyckel i hello `Bundle` måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="5b3c4-183">Each key in hello `Bundle` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="5b3c4-184">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-184">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="5b3c4-185">Storlek</span><span class="sxs-lookup"><span data-stu-id="5b3c4-185">Size</span></span>
<span data-ttu-id="5b3c4-186">Information om programmet är begränsad för**1024** tecken per anrop (efter kodning i JSON av hello Engagement service).</span><span class="sxs-lookup"><span data-stu-id="5b3c4-186">Application information are limited too**1024** characters per call (once encoded in JSON by hello Engagement service).</span></span>

<span data-ttu-id="5b3c4-187">I hello är 44 tecken i föregående exempel hello JSON skickas toohello server:</span><span class="sxs-lookup"><span data-stu-id="5b3c4-187">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"expiration":"2016-12-07","status":"premium"}
