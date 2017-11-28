---
title: "aaaHow tooUse hello Engagement API på iOS"
description: "Senaste iOS SDK - hur tooUse hello Engagement API på iOS"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1fb4509e-3804-46c1-949f-1cf727f91f9f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7fb9b95ad319cf3b1e2de81b5d6aee5b30266069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-ios"></a><span data-ttu-id="82028-103">Hur tooUse hello Engagement API på iOS</span><span class="sxs-lookup"><span data-stu-id="82028-103">How tooUse hello Engagement API on iOS</span></span>
<span data-ttu-id="82028-104">Det här dokumentet är ett tillägg toohello hur tooIntegrate Engagement för iOS: ger i djup information om hur hello toouse Engagement API tooreport programstatistik.</span><span class="sxs-lookup"><span data-stu-id="82028-104">This document is an add-on toohello document How tooIntegrate Engagement on iOS: it provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="82028-105">Tänk på att om du bara vill Engagement tooreport programmets sessioner, aktiviteter, krascher och teknisk information, sedan hello enklaste vägen är toomake dina anpassade `UIViewController` objekt ärver från hello motsvarande `EngagementViewController` klass .</span><span class="sxs-lookup"><span data-stu-id="82028-105">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your custom `UIViewController` objects inherit from hello corresponding `EngagementViewController` class.</span></span>

<span data-ttu-id="82028-106">Om du vill toodo mer, till exempel om du behöver tooreport programmet specifika händelser, fel och jobb, eller om du har tooreport ditt programs aktiviteter i ett annat sätt än hello en implementeras i hello `EngagementViewController` klasser måste toouse hello Engagement API.</span><span class="sxs-lookup"><span data-stu-id="82028-106">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementViewController` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="82028-107">Hej Engagement API tillhandahålls av hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="82028-107">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="82028-108">En instans av den här klassen kan hämtas genom att anropa hello `[EngagementAgent shared]` statisk metod (Observera att hello `EngagementAgent` objektet som returnerades är en singleton).</span><span class="sxs-lookup"><span data-stu-id="82028-108">An instance of this class can be retrieved by calling hello `[EngagementAgent shared]` static method (note that hello `EngagementAgent` object returned is a singleton).</span></span>

<span data-ttu-id="82028-109">Innan alla API-anrop, hello `EngagementAgent` objektet måste initieras genom att anropa metoden hello`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span><span class="sxs-lookup"><span data-stu-id="82028-109">Before any API calls, hello `EngagementAgent` object must be initialized by calling hello method `[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="82028-110">Koncept i engagement</span><span class="sxs-lookup"><span data-stu-id="82028-110">Engagement concepts</span></span>
<span data-ttu-id="82028-111">hello följande delar förfina hello vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md) för hello iOS-plattformen.</span><span class="sxs-lookup"><span data-stu-id="82028-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for hello iOS platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="82028-112">`Session` och `Activity`</span><span class="sxs-lookup"><span data-stu-id="82028-112">`Session` and `Activity`</span></span>
<span data-ttu-id="82028-113">En *aktiviteten* är ofta kopplade till en skärm på hello-program som är toosay hello *aktiviteten* när hello-skärmen visas och stoppas när hello-skärmen stängs: Detta är hello fall när Hej Engagement SDK är integrerad med hjälp av hello `EngagementViewController` klasser.</span><span class="sxs-lookup"><span data-stu-id="82028-113">An *activity* is usually associated with one screen of hello application, that is toosay hello *activity* starts when hello screen is displayed and stops when hello screen is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementViewController` classes.</span></span>

<span data-ttu-id="82028-114">Men *aktiviteter* kan också kontrolleras manuellt med hjälp av hello Engagement API.</span><span class="sxs-lookup"><span data-stu-id="82028-114">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="82028-115">Detta gör toosplit en viss skärm i flera sub delar tooget mer information om hello användning av den här skärmen (till exempel hur ofta tooknown och hur länge dialogrutor används i den här skärmen).</span><span class="sxs-lookup"><span data-stu-id="82028-115">This allows toosplit a given screen in several sub parts tooget more details about hello usage of this screen (for example tooknown how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="82028-116">Rapportering</span><span class="sxs-lookup"><span data-stu-id="82028-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="82028-117">Användaren startar en ny aktivitet</span><span class="sxs-lookup"><span data-stu-id="82028-117">User starts a new Activity</span></span>
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

<span data-ttu-id="82028-118">Du behöver toocall `startActivity()` varje gång hello användaraktivitet ändras.</span><span class="sxs-lookup"><span data-stu-id="82028-118">You need toocall `startActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="82028-119">hello första anropet toothis funktionen startar en ny session.</span><span class="sxs-lookup"><span data-stu-id="82028-119">hello first call toothis function starts a new user session.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="82028-120">Användaren avslutar sin aktuella aktiviteten</span><span class="sxs-lookup"><span data-stu-id="82028-120">User ends his current Activity</span></span>
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> <span data-ttu-id="82028-121">Du bör **aldrig** anropa den här funktionen själv, utom om du vill toosplit den användning av programmet till flera sessioner: ett anrop toothis funktionen skulle avslutas hello aktuell session omedelbart, så ett efterföljande anrop för`startActivity()`kan starta en ny session.</span><span class="sxs-lookup"><span data-stu-id="82028-121">You should **NEVER** call this function by yourself, except if you want toosplit one use of your application into several sessions: a call toothis function would end hello current session immediately, so, a subsequent call too`startActivity()` would start a new session.</span></span> <span data-ttu-id="82028-122">Den här funktionen anropas automatiskt av hello SDK när programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="82028-122">This function is automatically called by hello SDK when your application is closed.</span></span>
> 
> 

## <a name="reporting-events"></a><span data-ttu-id="82028-123">Rapporteringshändelser</span><span class="sxs-lookup"><span data-stu-id="82028-123">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="82028-124">Sessionshändelser</span><span class="sxs-lookup"><span data-stu-id="82028-124">Session events</span></span>
<span data-ttu-id="82028-125">Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under sin session.</span><span class="sxs-lookup"><span data-stu-id="82028-125">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

<span data-ttu-id="82028-126">**Exempel utan extra data:**</span><span class="sxs-lookup"><span data-stu-id="82028-126">**Example without extra data:**</span></span>

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

<span data-ttu-id="82028-127">**Exempel med extra data:**</span><span class="sxs-lookup"><span data-stu-id="82028-127">**Example with extra data:**</span></span>

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a><span data-ttu-id="82028-128">Fristående händelser</span><span class="sxs-lookup"><span data-stu-id="82028-128">Standalone events</span></span>
<span data-ttu-id="82028-129">Motstridiga toosession händelser, fristående händelser kan användas utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="82028-129">Contrary toosession events, standalone events can be used outside of hello context of a session.</span></span>

<span data-ttu-id="82028-130">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="82028-130">**Example:**</span></span>

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a><span data-ttu-id="82028-131">Rapporterat fel</span><span class="sxs-lookup"><span data-stu-id="82028-131">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="82028-132">Sessionen fel</span><span class="sxs-lookup"><span data-stu-id="82028-132">Session errors</span></span>
<span data-ttu-id="82028-133">Sessionen felen är oftast används tooreport hello fel påverkar hello användaren under sin session.</span><span class="sxs-lookup"><span data-stu-id="82028-133">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

<span data-ttu-id="82028-134">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="82028-134">**Example:**</span></span>

    /** hello user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* hello user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="82028-135">Fristående fel</span><span class="sxs-lookup"><span data-stu-id="82028-135">Standalone errors</span></span>
<span data-ttu-id="82028-136">Motstridiga toosession fel, fristående fel kan användas utanför hello kontexten för en session.</span><span class="sxs-lookup"><span data-stu-id="82028-136">Contrary toosession errors, standalone errors can be used outside of hello context of a session.</span></span>

<span data-ttu-id="82028-137">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="82028-137">**Example:**</span></span>

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a><span data-ttu-id="82028-138">Rapportering av jobb</span><span class="sxs-lookup"><span data-stu-id="82028-138">Reporting Jobs</span></span>
<span data-ttu-id="82028-139">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="82028-139">**Example:**</span></span>

<span data-ttu-id="82028-140">Anta att du vill tooreport hello varaktighet logga in:</span><span class="sxs-lookup"><span data-stu-id="82028-140">Suppose you want tooreport hello duration of your login process:</span></span>

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a><span data-ttu-id="82028-141">Rapportera fel under ett jobb</span><span class="sxs-lookup"><span data-stu-id="82028-141">Report Errors during a Job</span></span>
<span data-ttu-id="82028-142">Fel kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="82028-142">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="82028-143">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="82028-143">**Example:**</span></span>

<span data-ttu-id="82028-144">Anta att du vill ha tooreport ett fel under din inloggningen:</span><span class="sxs-lookup"><span data-stu-id="82028-144">Suppose you want tooreport an error during your login process:</span></span>

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try toosign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a><span data-ttu-id="82028-145">Händelser under ett jobb</span><span class="sxs-lookup"><span data-stu-id="82028-145">Events during a job</span></span>
<span data-ttu-id="82028-146">Händelser kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="82028-146">Events can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="82028-147">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="82028-147">**Example:**</span></span>

<span data-ttu-id="82028-148">Anta att vi har ett sociala nätverk och vi använder en jobbet tooreport hello total tid under vilken hello användare är anslutna toohello server.</span><span class="sxs-lookup"><span data-stu-id="82028-148">Suppose we have a social network, and we use a job tooreport hello total time during which hello user is connected toohello server.</span></span> <span data-ttu-id="82028-149">hello användare kan ta emot meddelanden från sina vänner, detta är en jobbhändelse.</span><span class="sxs-lookup"><span data-stu-id="82028-149">hello user can receive messages from his friends, this is a job event.</span></span>

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

## <a name="extra-parameters"></a><span data-ttu-id="82028-150">Extra parametrar</span><span class="sxs-lookup"><span data-stu-id="82028-150">Extra parameters</span></span>
<span data-ttu-id="82028-151">Godtycklig data kan vara anslutna tooevents, fel, aktiviteter och jobb.</span><span class="sxs-lookup"><span data-stu-id="82028-151">Arbitrary data can be attached tooevents, errors, activities and jobs.</span></span>

<span data-ttu-id="82028-152">Dessa data kan vara strukturerad, används Ios's NSDictionary klass.</span><span class="sxs-lookup"><span data-stu-id="82028-152">This data can be structured, it uses iOS's NSDictionary class.</span></span>

<span data-ttu-id="82028-153">Observera att tillägg kan innehålla `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` eller andra `NSDictionary` instanser.</span><span class="sxs-lookup"><span data-stu-id="82028-153">Note that extras can contain `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` or other `NSDictionary` instances.</span></span>

> [!NOTE]
> <span data-ttu-id="82028-154">Hej serialiseras extraparametern i JSON.</span><span class="sxs-lookup"><span data-stu-id="82028-154">hello extra parameter is serialized in JSON.</span></span> <span data-ttu-id="82028-155">Om du vill toopass olika objekt än hello som beskrivs ovan, måste du implementera hello följande metod i klassen:</span><span class="sxs-lookup"><span data-stu-id="82028-155">If you want toopass different objects than hello ones described above, you must implement hello following method in your class:</span></span>
> 
> <span data-ttu-id="82028-156">-(NSString*) JSONRepresentation;</span><span class="sxs-lookup"><span data-stu-id="82028-156">-(NSString*)JSONRepresentation;</span></span>
> 
> <span data-ttu-id="82028-157">hello-metoden ska returnera en JSON-representation av objektet.</span><span class="sxs-lookup"><span data-stu-id="82028-157">hello method should return a JSON representation of your object.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="82028-158">Exempel</span><span class="sxs-lookup"><span data-stu-id="82028-158">Example</span></span>
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a><span data-ttu-id="82028-159">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="82028-159">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="82028-160">Nycklar</span><span class="sxs-lookup"><span data-stu-id="82028-160">Keys</span></span>
<span data-ttu-id="82028-161">Varje nyckel i hello `NSDictionary` måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="82028-161">Each key in hello `NSDictionary` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="82028-162">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="82028-162">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="82028-163">Storlek</span><span class="sxs-lookup"><span data-stu-id="82028-163">Size</span></span>
<span data-ttu-id="82028-164">Tillägg är begränsad för**1024** tecken per anrop (efter kodning i JSON av hello Engagement agent).</span><span class="sxs-lookup"><span data-stu-id="82028-164">Extras are limited too**1024** characters per call (once encoded in JSON by hello Engagement agent).</span></span>

<span data-ttu-id="82028-165">I hello är 58 tecken i föregående exempel hello JSON skickas toohello server:</span><span class="sxs-lookup"><span data-stu-id="82028-165">In hello previous example, hello JSON sent toohello server is 58 characters long:</span></span>

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="82028-166">Rapportering programinformation</span><span class="sxs-lookup"><span data-stu-id="82028-166">Reporting Application Information</span></span>
<span data-ttu-id="82028-167">Du kan manuellt rapportera spåra information (eller andra program specifik information) med hjälp av hello `sendAppInfo:` funktion.</span><span class="sxs-lookup"><span data-stu-id="82028-167">You can manually report tracking information (or any other application specific information) using hello `sendAppInfo:` function.</span></span>

<span data-ttu-id="82028-168">Observera att denna information kan skickas inkrementellt: endast hello senaste värdet för en viss nyckel sparas för en viss enhet.</span><span class="sxs-lookup"><span data-stu-id="82028-168">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="82028-169">Som extra händelse, hello `NSDictionary` klass är används tooabstract programinformationen, Observera att matriser eller underordnade ordböcker behandlas som flat strängar (med JSON-serialisering).</span><span class="sxs-lookup"><span data-stu-id="82028-169">Like event extras, hello `NSDictionary` class is used tooabstract application information, note that arrays or sub-dictionaries will be treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="82028-170">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="82028-170">**Example:**</span></span>

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a><span data-ttu-id="82028-171">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="82028-171">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="82028-172">Nycklar</span><span class="sxs-lookup"><span data-stu-id="82028-172">Keys</span></span>
<span data-ttu-id="82028-173">Varje nyckel i hello `NSDictionary` måste matcha hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="82028-173">Each key in hello `NSDictionary` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="82028-174">Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).</span><span class="sxs-lookup"><span data-stu-id="82028-174">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="82028-175">Storlek</span><span class="sxs-lookup"><span data-stu-id="82028-175">Size</span></span>
<span data-ttu-id="82028-176">Information om programmet är begränsad för**1024** tecken per anrop (efter kodning i JSON av hello Engagement agent).</span><span class="sxs-lookup"><span data-stu-id="82028-176">Application information are limited too**1024** characters per call (once encoded in JSON by hello Engagement agent).</span></span>

<span data-ttu-id="82028-177">I hello är 44 tecken i föregående exempel hello JSON skickas toohello server:</span><span class="sxs-lookup"><span data-stu-id="82028-177">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
