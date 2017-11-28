---
title: aaaAzure Mobile Engagement iOS SDK-Integration | Microsoft Docs
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
ms.openlocfilehash: 66ce34efabede7d882caa8a91431a8df71e4fb59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-ios"></a><span data-ttu-id="e6f2e-103">Hur tooIntegrate Engagement för iOS</span><span class="sxs-lookup"><span data-stu-id="e6f2e-103">How tooIntegrate Engagement on iOS</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e6f2e-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="e6f2e-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="e6f2e-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="e6f2e-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="e6f2e-106">iOS</span><span class="sxs-lookup"><span data-stu-id="e6f2e-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="e6f2e-107">Android</span><span class="sxs-lookup"><span data-stu-id="e6f2e-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
>
>

<span data-ttu-id="e6f2e-108">Den här proceduren beskriver hello enklaste sättet tooactivate Engagement analyser och övervakningsfunktionerna i ditt iOS-program.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your iOS application.</span></span>

<span data-ttu-id="e6f2e-109">Hej Engagement SDK kräver iOS7 + och Xcode 8 +: hello distributionsmålet för ditt program måste vara minst iOS 7.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-109">hello Engagement SDK requires iOS7+ and Xcode 8+: hello deployment target of your application must be at least iOS 7.</span></span>

> [!NOTE]
> <span data-ttu-id="e6f2e-110">Om du verkligen är beroende av XCode 7 så att du kan använda hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-110">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="e6f2e-111">Det finns ett känt fel på hello Reach-modulen för den här tidigare version när du kör på iOS 10 enheter Se [hello reach-modulen integration](mobile-engagement-ios-integrate-engagement-reach.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-111">There is a known bug on hello Reach module of this previous version while running on iOS 10 devices see [hello reach module integration](mobile-engagement-ios-integrate-engagement-reach.md) for more details.</span></span> <span data-ttu-id="e6f2e-112">Om du väljer toouse hello SDK v3.2.4 bara hoppa hello `UserNotifications.framework` importera i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-112">If you choose toouse hello SDK v3.2.4 then just skip hello `UserNotifications.framework` import in hello next step.</span></span>
>
>

<span data-ttu-id="e6f2e-113">hello följande är tillräckligt med tooactivate hello loggar behövs en rapport för toocompute all statistik om användare, sessioner, aktiviteter, krascher och Technicals.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-113">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="e6f2e-114">hello loggar behövs en rapport toocompute annan statistik som händelser, fel och jobb måste göras manuellt med hjälp av hello Engagement API (se [hur toouse hello avancerade Mobile Engagement API-märkning i din iOS-app](mobile-engagement-ios-use-engagement-api.md) sedan statistiken är programmet beroende.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-114">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API  (see [How toouse hello advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-hello-engagement-sdk-into-your-ios-project"></a><span data-ttu-id="e6f2e-115">Bädda in hello Engagement SDK i din iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="e6f2e-115">Embed hello Engagement SDK into your iOS project</span></span>
* <span data-ttu-id="e6f2e-116">Hämta hello iOS SDK från [här](http://aka.ms/qk2rnj).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-116">Download hello iOS SDK from [here](http://aka.ms/qk2rnj).</span></span>
* <span data-ttu-id="e6f2e-117">Lägg till hello Engagement SDK tooyour iOS-projektet: i Xcode, högerklicka på projektet och välj **”Lägg till filer för...”** och välj hello `EngagementSDK` mapp.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-117">Add hello Engagement SDK tooyour iOS project: in Xcode, right click on your project and select **"Add files too..."** and choose hello `EngagementSDK` folder.</span></span>
* <span data-ttu-id="e6f2e-118">Engagement kräver ytterligare ramverk toowork: i hello Projektutforskaren, öppna ditt projekt och välj hello rätt mål.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-118">Engagement requires additional frameworks toowork: in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="e6f2e-119">Öppna hello **”Build-faser”** fliken och i hello **”länka binär med bibliotek”** menyn Lägg till dessa ramverk:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-119">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add these frameworks:</span></span>

  * <span data-ttu-id="e6f2e-120">`UserNotifications.framework`-Ange hello länka som`Optional`</span><span class="sxs-lookup"><span data-stu-id="e6f2e-120">`UserNotifications.framework` - set hello link as `Optional`</span></span>
  * <span data-ttu-id="e6f2e-121">`AdSupport.framework`-Ange hello länka som`Optional`</span><span class="sxs-lookup"><span data-stu-id="e6f2e-121">`AdSupport.framework` - set hello link as `Optional`</span></span>
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> <span data-ttu-id="e6f2e-122">Hej AdSupport framework kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-122">hello AdSupport framework can be removed.</span></span> <span data-ttu-id="e6f2e-123">Engagement måste den här framework toocollect hello IDFA.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-123">Engagement needs this framework toocollect hello IDFA.</span></span> <span data-ttu-id="e6f2e-124">Men du kan inaktivera IDFA-insamling \<ios-sdk-engagement-idfa\> toocomply med hello nya Apple policy för detta ID.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-124">However, IDFA collection can be disabled \<ios-sdk-engagement-idfa\> toocomply with hello new Apple policy regarding this ID.</span></span>
>
>

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="e6f2e-125">Initiera hello Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="e6f2e-125">Initialize hello Engagement SDK</span></span>
<span data-ttu-id="e6f2e-126">Du behöver toomodify Programdelegaten:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-126">You need toomodify your Application Delegate:</span></span>

* <span data-ttu-id="e6f2e-127">Importera hello Engagement agent hello över din implementering fil:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-127">At hello top of your implementation file, import hello Engagement agent:</span></span>

      [...]
      #import "EngagementAgent.h"
* <span data-ttu-id="e6f2e-128">Initiera Engagement inuti hello metoden '**applicationDidFinishLaunching:**'eller'**program: didFinishLaunchingWithOptions:**':</span><span class="sxs-lookup"><span data-stu-id="e6f2e-128">Initialize Engagement inside hello method '**applicationDidFinishLaunching:**' or '**application:didFinishLaunchingWithOptions:**':</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a><span data-ttu-id="e6f2e-129">Grundläggande reporting</span><span class="sxs-lookup"><span data-stu-id="e6f2e-129">Basic reporting</span></span>
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a><span data-ttu-id="e6f2e-130">Rekommenderad metod: överlagra din `UIViewController` klasser</span><span class="sxs-lookup"><span data-stu-id="e6f2e-130">Recommended method: overload your `UIViewController` classes</span></span>
<span data-ttu-id="e6f2e-131">I ordning tooactivate hello rapport med alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik, kan du bara göra alla dina `UIViewController` underordnade klasser ärver från hello `EngagementViewController` klasser (samma regel för `UITableViewController`  - \> `EngagementTableViewController`).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-131">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `UIViewController` sub-classes inherit from hello `EngagementViewController` classes (same rule for `UITableViewController` -\> `EngagementTableViewController`).</span></span>

<span data-ttu-id="e6f2e-132">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="e6f2e-132">**Without Engagement :**</span></span>

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

<span data-ttu-id="e6f2e-133">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="e6f2e-133">**With Engagement :**</span></span>

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="e6f2e-134">Alternativ metod: anropa `startActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="e6f2e-134">Alternate method: call `startActivity()` manually</span></span>
<span data-ttu-id="e6f2e-135">Om du inte kan eller inte vill att toooverload din `UIViewController` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent`'s metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-135">If you cannot or do not want toooverload your `UIViewController` classes, you can instead start your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e6f2e-136">hello iOS SDK automatiskt anropar hello `endActivity()` metod när hello programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-136">hello iOS SDK automatically calls hello `endActivity()` method when hello application is closed.</span></span> <span data-ttu-id="e6f2e-137">Därför är det *hög* rekommenderas toocall hello `startActivity` metod när hello aktiviteten hos hello användaren ändrar och för*aldrig* anrop hello `endActivity` metoden sedan anropar den här metoden tvingar hello aktuell session toobe avslutades.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-137">Thus, it is *HIGHLY* recommended toocall hello `startActivity` method whenever hello activity of hello user change, and too*NEVER* call hello `endActivity` method, since calling this method forces hello current session toobe ended.</span></span>
>
>

## <a name="location-reporting"></a><span data-ttu-id="e6f2e-138">Platsrapportering</span><span class="sxs-lookup"><span data-stu-id="e6f2e-138">Location reporting</span></span>
<span data-ttu-id="e6f2e-139">Apple användarvillkoren tillåter inte program toouse spårning för endast statistik ändamål.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-139">Apple terms of service do not allow applications toouse location tracking for statistics purpose only.</span></span> <span data-ttu-id="e6f2e-140">Därför rekommenderas tooenable plats rapporter bara om programmet använder också hello spårning av en annan anledning.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-140">Thus, it is recommended tooenable location reports only if your application also use hello location tracking for another reason.</span></span>

<span data-ttu-id="e6f2e-141">Från och med iOS 8, måste du ange en beskrivning av hur din app använder platstjänster genom att ange en sträng för nyckeln hello [NSLocationWhenInUseUsageDescription] eller [NSLocationAlwaysUsageDescription]i filen Info.plist för din app.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-141">Starting with iOS 8, you must provide a description for how your app uses location services by setting a string for hello key [NSLocationWhenInUseUsageDescription] or [NSLocationAlwaysUsageDescription] in your app's Info.plist file.</span></span> <span data-ttu-id="e6f2e-142">Om du vill tooreport plats i hello bakgrund med Engagement kan du lägga till hello nyckel NSLocationAlwaysUsageDescription.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-142">If you want tooreport location in hello background with Engagement, add hello key NSLocationAlwaysUsageDescription.</span></span> <span data-ttu-id="e6f2e-143">Lägga till hello nyckel NSLocationWhenInUseUsageDescription i andra fall.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-143">In all other cases, add hello key NSLocationWhenInUseUsageDescription.</span></span> <span data-ttu-id="e6f2e-144">Observera att du måste både NSLocationAlwaysAndWhenInUseUsageDescription och NSLocationWhenInUseUsageDescription tooreport bakgrund plats på iOS 11.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-144">Note that you need both NSLocationAlwaysAndWhenInUseUsageDescription and NSLocationWhenInUseUsageDescription tooreport background location on iOS 11.</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="e6f2e-145">Områdesrapportering</span><span class="sxs-lookup"><span data-stu-id="e6f2e-145">Lazy area location reporting</span></span>
<span data-ttu-id="e6f2e-146">Områdesrapportering tillåter tooreport hello land, region och plats som är associerade toodevices.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-146">Lazy area location reporting allows tooreport hello country, region and locality associated toodevices.</span></span> <span data-ttu-id="e6f2e-147">Den här typen av plats reporting används endast nätverksplatser (baserat på cellen ID eller Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-147">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="e6f2e-148">hello enheten området rapporteras högst en gång per session.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-148">hello device area is reported at most once per session.</span></span> <span data-ttu-id="e6f2e-149">hello GPS aldrig används och därmed den här typen av plats rapport har bara ett fåtal (inte toosay inga) inverkan på hello batteri.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-149">hello GPS is never used, and thus this type of location report has very few (not toosay no) impact on hello battery.</span></span>

<span data-ttu-id="e6f2e-150">Rapporterat områden är används toocompute geografiska statistik om användare, sessioner, händelser och fel.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-150">Reported areas are used toocompute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="e6f2e-151">De kan också användas som kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-151">They can also be used as criterion in Reach campaigns.</span></span> <span data-ttu-id="e6f2e-152">hello senast fungerande område som rapporterats för en enhet kan vara hämtade tack toohello [Device API].</span><span class="sxs-lookup"><span data-stu-id="e6f2e-152">hello last known area reported for a device can be retrieved thanks toohello [Device API].</span></span>

<span data-ttu-id="e6f2e-153">tooenable lazy Områdesplats rapportering, Lägg till följande rad efter initiering hello Engagement agent hello:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-153">tooenable lazy area location reporting, add hello following line after initializing hello Engagement agent:</span></span>

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a><span data-ttu-id="e6f2e-154">Rapportering av platsen i realtid</span><span class="sxs-lookup"><span data-stu-id="e6f2e-154">Real time location reporting</span></span>
<span data-ttu-id="e6f2e-155">Realtid plats kan tooreport hello latitud och longitud associerade toodevices.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-155">Real time location reporting allows tooreport hello latitude and longitude associated toodevices.</span></span> <span data-ttu-id="e6f2e-156">Den här typen av plats reporting använder endast nätverksplatser (baserat på cellen ID eller Wi-Fi) som standard och hello reporting är bara aktivt när hello-program körs i förgrunden (dvs. under en session).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-156">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and hello reporting is only active when hello application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="e6f2e-157">Realtid platser är *inte* används toocompute statistik.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-157">Real time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="e6f2e-158">Syftet endast är tooallow hello användning av realtid geobegränsning \<Reach-målgruppen-geofencing\> kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-158">Their only purpose is tooallow hello use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="e6f2e-159">tooenable realtid plats rapportering, Lägg till följande rad efter initiering hello Engagement agent hello:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-159">tooenable real time location reporting, add hello following line after initializing hello Engagement agent:</span></span>

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a><span data-ttu-id="e6f2e-160">GPS baseras reporting</span><span class="sxs-lookup"><span data-stu-id="e6f2e-160">GPS based reporting</span></span>
<span data-ttu-id="e6f2e-161">Som standard används rapportering av platsen i realtid endast baserat nätverksplatser.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-161">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="e6f2e-162">tooenable hello användning av GPS-baserade platser (vilket är mycket mer exakt), lägga till:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-162">tooenable hello use of GPS based locations (which are far more precise), add:</span></span>

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a><span data-ttu-id="e6f2e-163">Rapportering i bakgrunden</span><span class="sxs-lookup"><span data-stu-id="e6f2e-163">Background reporting</span></span>
<span data-ttu-id="e6f2e-164">Som standard är realtid plats reporting bara aktivt när hello-program körs i förgrunden (dvs. under en session).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-164">By default, real time location reporting is only active when hello application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="e6f2e-165">tooenable hello reporting även i bakgrunden, lägger du till:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-165">tooenable hello reporting also in background, add:</span></span>

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> <span data-ttu-id="e6f2e-166">När hello-program körs i bakgrunden, endast baserat nätverksplatser rapporteras, även om du har aktiverat hello GPS.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-166">When hello application runs in background, only network based locations are reported, even if you enabled hello GPS.</span></span>
>
>

<span data-ttu-id="e6f2e-167">Implementering av den här funktionen ska ringa [startMonitoringSignificantLocationChanges] när programmet blir hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-167">Implementation of this function will call [startMonitoringSignificantLocationChanges] when your application goes into hello background.</span></span> <span data-ttu-id="e6f2e-168">Tänk på att den automatiskt ska starta tillämpningsprogrammet till hello bakgrund om en ny plats händelse anländer.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-168">Be aware that it will automatically relaunch your application into hello background if a new location event arrives.</span></span>

## <a name="advanced-reporting"></a><span data-ttu-id="e6f2e-169">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="e6f2e-169">Advanced reporting</span></span>
<span data-ttu-id="e6f2e-170">Du kan också om du vill tooreport programmet specifika händelser, fel och jobb, måste toouse hello Engagement API via hello metoder för hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-170">Optionally, if you want tooreport application specific events, errors and jobs, you need toouse hello Engagement API through hello methods of hello `EngagementAgent` class.</span></span> <span data-ttu-id="e6f2e-171">Ett objekt av den här klassen kan hämtas genom att anropa hello `[EngagementAgent shared]` statisk metod.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-171">An object of this class can be retrieved by calling hello `[EngagementAgent shared]` static method.</span></span>

<span data-ttu-id="e6f2e-172">Hej Engagement API kan toouse alla Engagement avancerade funktioner och beskrivs i hello hur tooUse Engagement API på iOS (samt som i hello tekniska dokumentationen för hello `EngagementAgent` klassen).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-172">hello Engagement API allows toouse all of Engagement's advanced capabilities and is detailed in hello How tooUse the Engagement API on iOS (as well as in hello technical documentation of hello `EngagementAgent` class).</span></span>

## <a name="disable-idfa-collection"></a><span data-ttu-id="e6f2e-173">Inaktivera IDFA-insamling</span><span class="sxs-lookup"><span data-stu-id="e6f2e-173">Disable IDFA collection</span></span>
<span data-ttu-id="e6f2e-174">Som standard använder Engagement hello [IDFA] toouniquely identifiera en användare.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-174">By default, Engagement will use hello [IDFA] toouniquely identify a user.</span></span> <span data-ttu-id="e6f2e-175">Men om du inte använder annonserar någon annanstans i hello app kan du kanske avvisas av hello granskningsprocess App Store.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-175">But if you’re not using advertising elsewhere in hello app, you might be rejected by hello App Store review process.</span></span> <span data-ttu-id="e6f2e-176">IDFA-insamling kan inaktiveras genom att lägga till hello preprocessor makrot `ENGAGEMENT_DISABLE_IDFA` i filen pch (eller i hello `Build Settings` av programmet).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-176">IDFA collection can be disabled by adding hello preprocessor macro `ENGAGEMENT_DISABLE_IDFA` in your pch file (or in hello `Build Settings` of your application).</span></span> <span data-ttu-id="e6f2e-177">Detta säkerställer att det inte finns några referenser för`ASIdentifierManager`, `advertisingIdentifier` eller `isAdvertisingTrackingEnabled` i hello programmet build.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-177">This will ensure that there is no references too`ASIdentifierManager`, `advertisingIdentifier` or `isAdvertisingTrackingEnabled` in hello application build.</span></span>

<span data-ttu-id="e6f2e-178">Integrering i hello **prefix.pch** fil:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-178">Integration in hello **prefix.pch** file:</span></span>

    #define ENGAGEMENT_DISABLE_IDFA
    ...

<span data-ttu-id="e6f2e-179">Du kan kontrollera att hello IDFA-insamling korrekt är inaktiverad i ditt program genom att kontrollera hello Engagement test loggar.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-179">You can verify that hello IDFA collection is properly disabled in your application by checking hello Engagement test logs.</span></span> <span data-ttu-id="e6f2e-180">Se hello Integration testa\<ios sdk-engagement-test-idfa\> dokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-180">See hello Integration Test\<ios-sdk-engagement-test-idfa\> documentation for further information.</span></span>

## <a name="disable-log-reporting"></a><span data-ttu-id="e6f2e-181">Inaktivera rapportering av logg</span><span class="sxs-lookup"><span data-stu-id="e6f2e-181">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="e6f2e-182">Med hjälp av ett metodanrop</span><span class="sxs-lookup"><span data-stu-id="e6f2e-182">Using a method call</span></span>
<span data-ttu-id="e6f2e-183">Om du vill Engagement toostop skicka loggar, kan du anropa:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-183">If you want Engagement toostop sending logs, you can call:</span></span>

    [[EngagementAgent shared] setEnabled:NO];

<span data-ttu-id="e6f2e-184">Det här anropet är beständiga: den använder `NSUserDefaults` toostore hello information.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-184">This call is persistent: it uses `NSUserDefaults` toostore hello information.</span></span>

<span data-ttu-id="e6f2e-185">Du kan aktivera loggen reporting igen genom att anropa hello samma fungerar med `YES`.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-185">You can enable log reporting again by calling hello same function with `YES`.</span></span>

### <a name="integration-in-your-settings-bundle"></a><span data-ttu-id="e6f2e-186">Integrering i inställningar-paket</span><span class="sxs-lookup"><span data-stu-id="e6f2e-186">Integration in your settings bundle</span></span>
<span data-ttu-id="e6f2e-187">I stället för att anropa den här funktionen kan du också integrera inställningen direkt i din befintliga `Settings.bundle` fil.</span><span class="sxs-lookup"><span data-stu-id="e6f2e-187">Instead of calling this function, you can also integrate this setting directly in your existing `Settings.bundle` file.</span></span> <span data-ttu-id="e6f2e-188">Hej sträng `engagement_agent_enabled` måste användas som en identifierare för hello-inställningar och den måste vara associerad tooa växla växel (`PSToggleSwitchSpecifier`).</span><span class="sxs-lookup"><span data-stu-id="e6f2e-188">hello string `engagement_agent_enabled` must be used as a hello preference identifier and it must be associated tooa toggle switch(`PSToggleSwitchSpecifier`).</span></span>

<span data-ttu-id="e6f2e-189">Hej följande exempel på `Settings.bundle` visar hur tooimplement den:</span><span class="sxs-lookup"><span data-stu-id="e6f2e-189">hello following example of `Settings.bundle` shows how tooimplement it:</span></span>

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
[Device API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
