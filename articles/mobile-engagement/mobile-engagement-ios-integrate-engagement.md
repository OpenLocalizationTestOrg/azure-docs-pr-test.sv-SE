---
title: Azure Mobile Engagement iOS SDK-Integration | Microsoft Docs
description: "Senaste uppdateringarna och procedurer för iOS SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 947ea44b-00c1-450f-9a3b-74437954dc56
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 01fdbb43c21ac6932e8462f4a6507fc63e50542d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-integrate-engagement-on-ios"></a><span data-ttu-id="c9d05-103">Hur du integrerar Engagement för iOS</span><span class="sxs-lookup"><span data-stu-id="c9d05-103">How to Integrate Engagement on iOS</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c9d05-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="c9d05-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="c9d05-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="c9d05-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="c9d05-106">iOS</span><span class="sxs-lookup"><span data-stu-id="c9d05-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="c9d05-107">Android</span><span class="sxs-lookup"><span data-stu-id="c9d05-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
>
>

<span data-ttu-id="c9d05-108">Den här proceduren beskriver det enklaste sättet att aktivera Engagement analyser och övervakningsfunktionerna i ditt iOS-program.</span><span class="sxs-lookup"><span data-stu-id="c9d05-108">This procedure describes the simplest way to activate Engagement's Analytics and Monitoring functions in your iOS application.</span></span>

<span data-ttu-id="c9d05-109">Kräver Engagement SDK iOS7 + och Xcode 8 +: av distributionsmålet för ditt program måste vara minst iOS 7.</span><span class="sxs-lookup"><span data-stu-id="c9d05-109">The Engagement SDK requires iOS7+ and Xcode 8+: the deployment target of your application must be at least iOS 7.</span></span>

> [!NOTE]
> <span data-ttu-id="c9d05-110">Om du verkligen är beroende av XCode 7 så att du kan använda den [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="c9d05-110">If you really depend on XCode 7 then you may use the [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="c9d05-111">Det finns ett känt fel på Reach-modulen för den här tidigare version när du kör på iOS 10 enheter Se [reach-modulen integration](mobile-engagement-ios-integrate-engagement-reach.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c9d05-111">There is a known bug on the Reach module of this previous version while running on iOS 10 devices see [the reach module integration](mobile-engagement-ios-integrate-engagement-reach.md) for more details.</span></span> <span data-ttu-id="c9d05-112">Om du använder SDK v3.2.4 sedan bara hoppa den `UserNotifications.framework` importera i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="c9d05-112">If you choose to use the SDK v3.2.4 then just skip the `UserNotifications.framework` import in the next step.</span></span>
>
>

<span data-ttu-id="c9d05-113">Följande steg är tillräckligt för att aktivera rapporten i loggar som behövs för att beräkna all statistik om användare, sessioner, aktiviteter, krascher och Technicals.</span><span class="sxs-lookup"><span data-stu-id="c9d05-113">The following steps are enough to activate the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="c9d05-114">Rapporten loggar som behövs för att beräkna andra statistik som händelser, fel och jobb måste göras manuellt med hjälp av Engagement-API (se [hur du använder avancerade Mobile Engagement API-märkning i din iOS-app](mobile-engagement-ios-use-engagement-api.md) eftersom statistiken beroende program.</span><span class="sxs-lookup"><span data-stu-id="c9d05-114">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API  (see [How to use the advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-the-engagement-sdk-into-your-ios-project"></a><span data-ttu-id="c9d05-115">Bädda in Engagement SDK i din iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="c9d05-115">Embed the Engagement SDK into your iOS project</span></span>
* <span data-ttu-id="c9d05-116">Hämta iOS SDK från [här](http://aka.ms/qk2rnj).</span><span class="sxs-lookup"><span data-stu-id="c9d05-116">Download the iOS SDK from [here](http://aka.ms/qk2rnj).</span></span>
* <span data-ttu-id="c9d05-117">Lägg till Engagement-SDK för iOS-projekt: i Xcode, högerklicka på projektet och välj **”Lägg till filer i...”** och välj den `EngagementSDK` mapp.</span><span class="sxs-lookup"><span data-stu-id="c9d05-117">Add the Engagement SDK to your iOS project: in Xcode, right click on your project and select **"Add files to ..."** and choose the `EngagementSDK` folder.</span></span>
* <span data-ttu-id="c9d05-118">Engagement kräver ytterligare ramverk ska fungera: öppna ditt projekt i Projektutforskaren, och välj rätt mål.</span><span class="sxs-lookup"><span data-stu-id="c9d05-118">Engagement requires additional frameworks to work: in the project explorer, open your project pane and select the correct target.</span></span> <span data-ttu-id="c9d05-119">Öppna den **”Build-faser”** fliken och i den **”länka binär med bibliotek”** menyn Lägg till dessa ramverk:</span><span class="sxs-lookup"><span data-stu-id="c9d05-119">Then, open the **"Build phases"** tab and in the **"Link Binary With Libraries"** menu, add these frameworks:</span></span>

  * <span data-ttu-id="c9d05-120">`UserNotifications.framework`-Ange länken som`Optional`</span><span class="sxs-lookup"><span data-stu-id="c9d05-120">`UserNotifications.framework` - set the link as `Optional`</span></span>
  * <span data-ttu-id="c9d05-121">`AdSupport.framework`-Ange länken som`Optional`</span><span class="sxs-lookup"><span data-stu-id="c9d05-121">`AdSupport.framework` - set the link as `Optional`</span></span>
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> <span data-ttu-id="c9d05-122">AdSupport framework kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="c9d05-122">The AdSupport framework can be removed.</span></span> <span data-ttu-id="c9d05-123">Engagement måste det här ramverket för att samla in IDFA.</span><span class="sxs-lookup"><span data-stu-id="c9d05-123">Engagement needs this framework to collect the IDFA.</span></span> <span data-ttu-id="c9d05-124">Men du kan inaktivera IDFA-insamling \<ios-sdk-engagement-idfa\> att följa den nya principen Apple om detta ID.</span><span class="sxs-lookup"><span data-stu-id="c9d05-124">However, IDFA collection can be disabled \<ios-sdk-engagement-idfa\> to comply with the new Apple policy regarding this ID.</span></span>
>
>

## <a name="initialize-the-engagement-sdk"></a><span data-ttu-id="c9d05-125">Initiera Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="c9d05-125">Initialize the Engagement SDK</span></span>
<span data-ttu-id="c9d05-126">Du behöver ändra Programdelegaten:</span><span class="sxs-lookup"><span data-stu-id="c9d05-126">You need to modify your Application Delegate:</span></span>

* <span data-ttu-id="c9d05-127">Importera Engagement agenten längst upp i implementeringsfilen:</span><span class="sxs-lookup"><span data-stu-id="c9d05-127">At the top of your implementation file, import the Engagement agent:</span></span>

      [...]
      #import "EngagementAgent.h"
* <span data-ttu-id="c9d05-128">Initiera Engagement inuti metoden '**applicationDidFinishLaunching:**'eller'**program: didFinishLaunchingWithOptions:**':</span><span class="sxs-lookup"><span data-stu-id="c9d05-128">Initialize Engagement inside the method '**applicationDidFinishLaunching:**' or '**application:didFinishLaunchingWithOptions:**':</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a><span data-ttu-id="c9d05-129">Grundläggande reporting</span><span class="sxs-lookup"><span data-stu-id="c9d05-129">Basic reporting</span></span>
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a><span data-ttu-id="c9d05-130">Rekommenderad metod: överlagra din `UIViewController` klasser</span><span class="sxs-lookup"><span data-stu-id="c9d05-130">Recommended method: overload your `UIViewController` classes</span></span>
<span data-ttu-id="c9d05-131">För att aktivera rapporten i alla loggar som krävs av Engagement att beräkna användare, sessioner, aktiviteter, krascher och tekniska statistik, kan du bara göra alla dina `UIViewController` underordnade klasser ärver från den `EngagementViewController` klasser (samma regel för `UITableViewController`  -\> `EngagementTableViewController`).</span><span class="sxs-lookup"><span data-stu-id="c9d05-131">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `UIViewController` sub-classes inherit from the `EngagementViewController` classes (same rule for `UITableViewController` -\> `EngagementTableViewController`).</span></span>

<span data-ttu-id="c9d05-132">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="c9d05-132">**Without Engagement :**</span></span>

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

<span data-ttu-id="c9d05-133">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="c9d05-133">**With Engagement :**</span></span>

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="c9d05-134">Alternativ metod: anropa `startActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="c9d05-134">Alternate method: call `startActivity()` manually</span></span>
<span data-ttu-id="c9d05-135">Om du inte kan eller inte vill överlagra din `UIViewController` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent`'s metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="c9d05-135">If you cannot or do not want to overload your `UIViewController` classes, you can instead start your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c9d05-136">IOS SDK automatiskt anropar den `endActivity()` metoden när programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="c9d05-136">The iOS SDK automatically calls the `endActivity()` method when the application is closed.</span></span> <span data-ttu-id="c9d05-137">Därför är det *hög* rekommenderas att anropa den `startActivity` metod när aktiviteten användaren ändrar och *aldrig* anropa den `endActivity` metoden, eftersom den här metoden anropas tvingar aktuellt sessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="c9d05-137">Thus, it is *HIGHLY* recommended to call the `startActivity` method whenever the activity of the user change, and to *NEVER* call the `endActivity` method, since calling this method forces the current session to be ended.</span></span>
>
>

## <a name="location-reporting"></a><span data-ttu-id="c9d05-138">Platsrapportering</span><span class="sxs-lookup"><span data-stu-id="c9d05-138">Location reporting</span></span>
<span data-ttu-id="c9d05-139">Apple användarvillkoren tillåter inte att program kan använda spårning för endast statistik ändamål.</span><span class="sxs-lookup"><span data-stu-id="c9d05-139">Apple terms of service do not allow applications to use location tracking for statistics purpose only.</span></span> <span data-ttu-id="c9d05-140">Därför rekommenderas att aktivera plats rapporter bara om programmet använder också platsen för spårning av en annan anledning.</span><span class="sxs-lookup"><span data-stu-id="c9d05-140">Thus, it is recommended to enable location reports only if your application also use the location tracking for another reason.</span></span>

<span data-ttu-id="c9d05-141">Från och med iOS 8, måste du ange en beskrivning av hur din app använder platstjänster genom att ange en sträng för nyckeln [NSLocationWhenInUseUsageDescription] eller [NSLocationAlwaysUsageDescription]i filen Info.plist för din app.</span><span class="sxs-lookup"><span data-stu-id="c9d05-141">Starting with iOS 8, you must provide a description for how your app uses location services by setting a string for the key [NSLocationWhenInUseUsageDescription] or [NSLocationAlwaysUsageDescription] in your app's Info.plist file.</span></span> <span data-ttu-id="c9d05-142">Lägg till nyckel NSLocationAlwaysUsageDescription om du vill rapporten platsen i bakgrunden med Engagement.</span><span class="sxs-lookup"><span data-stu-id="c9d05-142">If you want to report location in the background with Engagement, add the key NSLocationAlwaysUsageDescription.</span></span> <span data-ttu-id="c9d05-143">Lägg till nyckel NSLocationWhenInUseUsageDescription i andra fall.</span><span class="sxs-lookup"><span data-stu-id="c9d05-143">In all other cases, add the key NSLocationWhenInUseUsageDescription.</span></span> <span data-ttu-id="c9d05-144">Observera att du måste både NSLocationAlwaysAndWhenInUseUsageDescription och NSLocationWhenInUseUsageDescription rapportens bakgrund plats på iOS 11.</span><span class="sxs-lookup"><span data-stu-id="c9d05-144">Note that you need both NSLocationAlwaysAndWhenInUseUsageDescription and NSLocationWhenInUseUsageDescription to report background location on iOS 11.</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="c9d05-145">Områdesrapportering</span><span class="sxs-lookup"><span data-stu-id="c9d05-145">Lazy area location reporting</span></span>
<span data-ttu-id="c9d05-146">Områdesrapportering kan rapportera land, region och ort som är kopplade till enheter.</span><span class="sxs-lookup"><span data-stu-id="c9d05-146">Lazy area location reporting allows to report the country, region and locality associated to devices.</span></span> <span data-ttu-id="c9d05-147">Den här typen av plats reporting används endast nätverksplatser (baserat på cellen ID eller Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="c9d05-147">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="c9d05-148">Området enheten rapporteras som mest en gång per session.</span><span class="sxs-lookup"><span data-stu-id="c9d05-148">The device area is reported at most once per session.</span></span> <span data-ttu-id="c9d05-149">GPS används aldrig och därmed den här typen av plats rapport har bara ett fåtal (ska säga Nej) inverkan på batteridrift.</span><span class="sxs-lookup"><span data-stu-id="c9d05-149">The GPS is never used, and thus this type of location report has very few (not to say no) impact on the battery.</span></span>

<span data-ttu-id="c9d05-150">Rapporterat områden används för att beräkna geografiska statistik om användare, sessioner, händelser och fel.</span><span class="sxs-lookup"><span data-stu-id="c9d05-150">Reported areas are used to compute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="c9d05-151">De kan också användas som kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="c9d05-151">They can also be used as criterion in Reach campaigns.</span></span> <span data-ttu-id="c9d05-152">Det senaste kända området som rapporterats för en enhet kan hämtas tack till den [Device API].</span><span class="sxs-lookup"><span data-stu-id="c9d05-152">The last known area reported for a device can be retrieved thanks to the [Device API].</span></span>

<span data-ttu-id="c9d05-153">Om du vill aktivera områdesrapportering, lägger du till följande rad efter initiering Engagement agenten:</span><span class="sxs-lookup"><span data-stu-id="c9d05-153">To enable lazy area location reporting, add the following line after initializing the Engagement agent:</span></span>

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a><span data-ttu-id="c9d05-154">Rapportering av platsen i realtid</span><span class="sxs-lookup"><span data-stu-id="c9d05-154">Real time location reporting</span></span>
<span data-ttu-id="c9d05-155">Realtid plats reporting kan rapportera latituden och longituden som är kopplade till enheter.</span><span class="sxs-lookup"><span data-stu-id="c9d05-155">Real time location reporting allows to report the latitude and longitude associated to devices.</span></span> <span data-ttu-id="c9d05-156">Den här typen av plats reporting använder endast nätverksplatser (baserat på cellen ID eller Wi-Fi) som standard och reporting är bara aktivt när programmet körs i förgrunden (dvs. under en session).</span><span class="sxs-lookup"><span data-stu-id="c9d05-156">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and the reporting is only active when the application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="c9d05-157">Realtid platser är *inte* används för att beräkna statistik.</span><span class="sxs-lookup"><span data-stu-id="c9d05-157">Real time locations are *NOT* used to compute statistics.</span></span> <span data-ttu-id="c9d05-158">Deras endast syfte är att tillåta användning av realtid geobegränsning \<Reach-målgruppen-geofencing\> kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="c9d05-158">Their only purpose is to allow the use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="c9d05-159">För att aktivera rapportering om realtid plats, lägger du till följande rad efter initiering Engagement agenten:</span><span class="sxs-lookup"><span data-stu-id="c9d05-159">To enable real time location reporting, add the following line after initializing the Engagement agent:</span></span>

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a><span data-ttu-id="c9d05-160">GPS baseras reporting</span><span class="sxs-lookup"><span data-stu-id="c9d05-160">GPS based reporting</span></span>
<span data-ttu-id="c9d05-161">Som standard används rapportering av platsen i realtid endast baserat nätverksplatser.</span><span class="sxs-lookup"><span data-stu-id="c9d05-161">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="c9d05-162">Lägg till om du vill aktivera användning av GPS-baserade platser (vilket är mycket mer exakt):</span><span class="sxs-lookup"><span data-stu-id="c9d05-162">To enable the use of GPS based locations (which are far more precise), add:</span></span>

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a><span data-ttu-id="c9d05-163">Rapportering i bakgrunden</span><span class="sxs-lookup"><span data-stu-id="c9d05-163">Background reporting</span></span>
<span data-ttu-id="c9d05-164">Som standard är realtid plats reporting endast aktiv när programmet körs i förgrunden (dvs. under en session).</span><span class="sxs-lookup"><span data-stu-id="c9d05-164">By default, real time location reporting is only active when the application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="c9d05-165">Om du vill aktivera reporting även i bakgrunden, lägger du till:</span><span class="sxs-lookup"><span data-stu-id="c9d05-165">To enable the reporting also in background, add:</span></span>

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> <span data-ttu-id="c9d05-166">När programmet körs i bakgrunden, endast nätverksbaserat platser rapporteras, även om du har aktiverat GPS.</span><span class="sxs-lookup"><span data-stu-id="c9d05-166">When the application runs in background, only network based locations are reported, even if you enabled the GPS.</span></span>
>
>

<span data-ttu-id="c9d05-167">Implementering av den här funktionen ska ringa [startMonitoringSignificantLocationChanges] när programmet försätts i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="c9d05-167">Implementation of this function will call [startMonitoringSignificantLocationChanges] when your application goes into the background.</span></span> <span data-ttu-id="c9d05-168">Tänk på att den automatiskt ska starta programmet i bakgrunden om en ny plats händelse anländer.</span><span class="sxs-lookup"><span data-stu-id="c9d05-168">Be aware that it will automatically relaunch your application into the background if a new location event arrives.</span></span>

## <a name="advanced-reporting"></a><span data-ttu-id="c9d05-169">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="c9d05-169">Advanced reporting</span></span>
<span data-ttu-id="c9d05-170">Du kan också om du vill rapportera programmet specifika händelser, fel och jobb du behöver använda Engagement API via metoderna i det `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="c9d05-170">Optionally, if you want to report application specific events, errors and jobs, you need to use the Engagement API through the methods of the `EngagementAgent` class.</span></span> <span data-ttu-id="c9d05-171">Ett objekt av den här klassen kan hämtas genom att anropa den `[EngagementAgent shared]` statisk metod.</span><span class="sxs-lookup"><span data-stu-id="c9d05-171">An object of this class can be retrieved by calling the `[EngagementAgent shared]` static method.</span></span>

<span data-ttu-id="c9d05-172">Engagement-API kan använda alla Engagement avancerade funktioner och beskrivs i hur du använder API: et Engagement på iOS (samt som i den tekniska dokumentationen för den `EngagementAgent` klassen).</span><span class="sxs-lookup"><span data-stu-id="c9d05-172">The Engagement API allows to use all of Engagement's advanced capabilities and is detailed in the How to Use the Engagement API on iOS (as well as in the technical documentation of the `EngagementAgent` class).</span></span>

## <a name="disable-idfa-collection"></a><span data-ttu-id="c9d05-173">Inaktivera IDFA-insamling</span><span class="sxs-lookup"><span data-stu-id="c9d05-173">Disable IDFA collection</span></span>
<span data-ttu-id="c9d05-174">Som standard Engagement kommer att använda den [IDFA] att unikt identifiera en användare.</span><span class="sxs-lookup"><span data-stu-id="c9d05-174">By default, Engagement will use the [IDFA] to uniquely identify a user.</span></span> <span data-ttu-id="c9d05-175">Men om du inte använder annonserar någon annanstans i appen, du kan inte avvisa granskningsprocess App Store.</span><span class="sxs-lookup"><span data-stu-id="c9d05-175">But if you’re not using advertising elsewhere in the app, you might be rejected by the App Store review process.</span></span> <span data-ttu-id="c9d05-176">IDFA-insamling kan inaktiveras genom att lägga till preprocessor makrot `ENGAGEMENT_DISABLE_IDFA` i filen pch (eller i den `Build Settings` av programmet).</span><span class="sxs-lookup"><span data-stu-id="c9d05-176">IDFA collection can be disabled by adding the preprocessor macro `ENGAGEMENT_DISABLE_IDFA` in your pch file (or in the `Build Settings` of your application).</span></span> <span data-ttu-id="c9d05-177">Detta säkerställer att det inte finns några referenser till `ASIdentifierManager`, `advertisingIdentifier` eller `isAdvertisingTrackingEnabled` i bygga program.</span><span class="sxs-lookup"><span data-stu-id="c9d05-177">This will ensure that there is no references to `ASIdentifierManager`, `advertisingIdentifier` or `isAdvertisingTrackingEnabled` in the application build.</span></span>

<span data-ttu-id="c9d05-178">Integrering i den **prefix.pch** fil:</span><span class="sxs-lookup"><span data-stu-id="c9d05-178">Integration in the **prefix.pch** file:</span></span>

    #define ENGAGEMENT_DISABLE_IDFA
    ...

<span data-ttu-id="c9d05-179">Du kan kontrollera att IDFA-insamling korrekt är inaktiverad i ditt program genom att kontrollera Engagement test loggarna.</span><span class="sxs-lookup"><span data-stu-id="c9d05-179">You can verify that the IDFA collection is properly disabled in your application by checking the Engagement test logs.</span></span> <span data-ttu-id="c9d05-180">Se integrering testa\<ios sdk-engagement-test-idfa\> dokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="c9d05-180">See the Integration Test\<ios-sdk-engagement-test-idfa\> documentation for further information.</span></span>

## <a name="disable-log-reporting"></a><span data-ttu-id="c9d05-181">Inaktivera rapportering av logg</span><span class="sxs-lookup"><span data-stu-id="c9d05-181">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="c9d05-182">Med hjälp av ett metodanrop</span><span class="sxs-lookup"><span data-stu-id="c9d05-182">Using a method call</span></span>
<span data-ttu-id="c9d05-183">Om du vill Engagement slutar skicka loggar, kan du anropa:</span><span class="sxs-lookup"><span data-stu-id="c9d05-183">If you want Engagement to stop sending logs, you can call:</span></span>

    [[EngagementAgent shared] setEnabled:NO];

<span data-ttu-id="c9d05-184">Det här anropet är beständiga: den använder `NSUserDefaults` att lagra informationen.</span><span class="sxs-lookup"><span data-stu-id="c9d05-184">This call is persistent: it uses `NSUserDefaults` to store the information.</span></span>

<span data-ttu-id="c9d05-185">Du kan aktivera loggen reporting igen genom att anropa funktionen samma med `YES`.</span><span class="sxs-lookup"><span data-stu-id="c9d05-185">You can enable log reporting again by calling the same function with `YES`.</span></span>

### <a name="integration-in-your-settings-bundle"></a><span data-ttu-id="c9d05-186">Integrering i inställningar-paket</span><span class="sxs-lookup"><span data-stu-id="c9d05-186">Integration in your settings bundle</span></span>
<span data-ttu-id="c9d05-187">I stället för att anropa den här funktionen kan du också integrera inställningen direkt i din befintliga `Settings.bundle` fil.</span><span class="sxs-lookup"><span data-stu-id="c9d05-187">Instead of calling this function, you can also integrate this setting directly in your existing `Settings.bundle` file.</span></span> <span data-ttu-id="c9d05-188">Strängen `engagement_agent_enabled` måste användas som en identifierare för inställningar och den måste vara kopplad till en växel växla (`PSToggleSwitchSpecifier`).</span><span class="sxs-lookup"><span data-stu-id="c9d05-188">The string `engagement_agent_enabled` must be used as a the preference identifier and it must be associated to a toggle switch(`PSToggleSwitchSpecifier`).</span></span>

<span data-ttu-id="c9d05-189">Följande exempel visar `Settings.bundle` visar hur du implementerar den:</span><span class="sxs-lookup"><span data-stu-id="c9d05-189">The following example of `Settings.bundle` shows how to implement it:</span></span>

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
<span data-ttu-id="c9d05-190">[Device API]: http://go.microsoft.com/?linkid=9876094</span><span class="sxs-lookup"><span data-stu-id="c9d05-190">[Device API]: http://go.microsoft.com/?linkid=9876094</span></span>
<span data-ttu-id="c9d05-191">[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26</span><span class="sxs-lookup"><span data-stu-id="c9d05-191">[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26</span></span>
<span data-ttu-id="c9d05-192">[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18</span><span class="sxs-lookup"><span data-stu-id="c9d05-192">[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18</span></span>
<span data-ttu-id="c9d05-193">[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges</span><span class="sxs-lookup"><span data-stu-id="c9d05-193">[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges</span></span>
<span data-ttu-id="c9d05-194">[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier</span><span class="sxs-lookup"><span data-stu-id="c9d05-194">[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier</span></span>
