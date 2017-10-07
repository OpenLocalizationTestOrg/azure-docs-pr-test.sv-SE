---
title: "aaaLocation rapportering för Azure Mobile Engagement Android SDK"
description: "Beskriver hur tooconfigure plats rapportering för Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6cab5ed1-b767-46ac-9f0b-48a4e249d88c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: c2cb097df2a77bee2d56ffe9509dc116548db408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="2f61e-103">Plats för Azure Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="2f61e-103">Location Reporting for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2f61e-104">Android</span><span class="sxs-lookup"><span data-stu-id="2f61e-104">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="2f61e-105">Det här avsnittet beskrivs hur toodo plats rapportering för din Android-App.</span><span class="sxs-lookup"><span data-stu-id="2f61e-105">This topic describes how toodo location reporting for your Android application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f61e-106">Krav</span><span class="sxs-lookup"><span data-stu-id="2f61e-106">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a><span data-ttu-id="2f61e-107">Platsrapportering</span><span class="sxs-lookup"><span data-stu-id="2f61e-107">Location reporting</span></span>
<span data-ttu-id="2f61e-108">Om du vill att platser toobe rapporterade måste tooadd configuration några rader (mellan hello `<application>` och `</application>` taggar).</span><span class="sxs-lookup"><span data-stu-id="2f61e-108">If you want locations toobe reported, you need tooadd a few lines of configuration (between hello `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="2f61e-109">Områdesrapportering</span><span class="sxs-lookup"><span data-stu-id="2f61e-109">Lazy area location reporting</span></span>
<span data-ttu-id="2f61e-110">Områdesrapportering kan reporting hello land, region och plats som är associerade med enheter.</span><span class="sxs-lookup"><span data-stu-id="2f61e-110">Lazy area location reporting enables reporting hello country, region, and locality associated with devices.</span></span> <span data-ttu-id="2f61e-111">Den här typen av plats reporting används endast nätverksplatser (baserat på cellen ID eller Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="2f61e-111">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="2f61e-112">hello enheten området rapporteras högst en gång per session.</span><span class="sxs-lookup"><span data-stu-id="2f61e-112">hello device area is reported at most once per session.</span></span> <span data-ttu-id="2f61e-113">hello GPS används aldrig och därmed den här typen av plats rapporten har låg påverkan på hello batteri.</span><span class="sxs-lookup"><span data-stu-id="2f61e-113">hello GPS is never used, and thus this type of location report has low impact on hello battery.</span></span>

<span data-ttu-id="2f61e-114">Rapporterat områden är används toocompute geografiska statistik om användare, sessioner, händelser och fel.</span><span class="sxs-lookup"><span data-stu-id="2f61e-114">Reported areas are used toocompute geographic statistics about users, sessions, events, and errors.</span></span> <span data-ttu-id="2f61e-115">De kan också användas som kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="2f61e-115">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="2f61e-116">Du aktiverar lazy Områdesplats rapportering med hello-konfiguration som tidigare nämnts i den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="2f61e-116">You enable lazy area location reporting by using hello configuration previously mentioned in this procedure:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="2f61e-117">Du måste också toospecify en plats-behörighet.</span><span class="sxs-lookup"><span data-stu-id="2f61e-117">You also need toospecify a location permission.</span></span> <span data-ttu-id="2f61e-118">Den här koden använder ``COARSE`` behörighet:</span><span class="sxs-lookup"><span data-stu-id="2f61e-118">This code uses ``COARSE`` permission:</span></span>

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="2f61e-119">Om din app kräver det, kan du använda ``ACCESS_FINE_LOCATION`` i stället.</span><span class="sxs-lookup"><span data-stu-id="2f61e-119">If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="2f61e-120">Rapportering i realtid plats</span><span class="sxs-lookup"><span data-stu-id="2f61e-120">Real-time location reporting</span></span>
<span data-ttu-id="2f61e-121">Rapportering i realtid plats kan reporting hello-latitud och longitud som är associerade med enheter.</span><span class="sxs-lookup"><span data-stu-id="2f61e-121">Real-time location reporting enables reporting hello latitude and longitude associated with devices.</span></span> <span data-ttu-id="2f61e-122">Som standard används den här typen av plats reporting endast platser i nätverket, baserat på cellen ID eller Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="2f61e-122">By default, this type of location reporting only uses network locations, based on Cell ID or WIFI.</span></span> <span data-ttu-id="2f61e-123">hello reporting är endast aktiv när hello-program körs i förgrunden (till exempel under en session).</span><span class="sxs-lookup"><span data-stu-id="2f61e-123">hello reporting is only active when hello application runs in foreground (for example, during a session).</span></span>

<span data-ttu-id="2f61e-124">Realtid platser är *inte* används toocompute statistik.</span><span class="sxs-lookup"><span data-stu-id="2f61e-124">Real-time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="2f61e-125">Syftet endast är tooallow hello användning av geobegränsning i realtid \<Reach-målgruppen-geofencing\> kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="2f61e-125">Their only purpose is tooallow hello use of real-time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="2f61e-126">tooenable realtid plats rapportering, lägga till en rad med kod toowhere som hello Engagement anslutningssträngen i hello starta aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="2f61e-126">tooenable real-time location reporting, add a line of code toowhere you set hello Engagement connection string in hello launcher activity.</span></span> <span data-ttu-id="2f61e-127">hello resultatet ser ut så hello följande:</span><span class="sxs-lookup"><span data-stu-id="2f61e-127">hello result looks like hello following:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need toospecify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a><span data-ttu-id="2f61e-128">GPS baseras reporting</span><span class="sxs-lookup"><span data-stu-id="2f61e-128">GPS based reporting</span></span>
<span data-ttu-id="2f61e-129">Som standard använder rapportering i realtid plats endast nätverksbaserade platser.</span><span class="sxs-lookup"><span data-stu-id="2f61e-129">By default, real-time location reporting only uses network-based locations.</span></span> <span data-ttu-id="2f61e-130">tooenable hello GPS-baserade platser som är mycket mer exakt, använda hello konfigurationsobjekt:</span><span class="sxs-lookup"><span data-stu-id="2f61e-130">tooenable hello use of GPS-based locations, which are far more precise, use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="2f61e-131">Du måste också tooadd hello efter behörighet om saknas:</span><span class="sxs-lookup"><span data-stu-id="2f61e-131">You also need tooadd hello following permission if missing:</span></span>

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="2f61e-132">Rapportering i bakgrunden</span><span class="sxs-lookup"><span data-stu-id="2f61e-132">Background reporting</span></span>
<span data-ttu-id="2f61e-133">Som standard är rapportering i realtid plats bara aktivt när hello-program körs i förgrunden (till exempel under en session).</span><span class="sxs-lookup"><span data-stu-id="2f61e-133">By default, real-time location reporting is only active when hello application runs in foreground (for example, during a session).</span></span> <span data-ttu-id="2f61e-134">tooenable hello reporting även i bakgrunden, Använd det här konfigurationsobjektet:</span><span class="sxs-lookup"><span data-stu-id="2f61e-134">tooenable hello reporting also in background, use this configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="2f61e-135">När hello-program körs i bakgrunden, nätverksbaserade platser rapporteras, även om du har aktiverat hello GPS.</span><span class="sxs-lookup"><span data-stu-id="2f61e-135">When hello application runs in background, only network-based locations are reported, even if you enabled hello GPS.</span></span>
> 
> 

<span data-ttu-id="2f61e-136">Om hello användaren startar om sin enhet, har hello bakgrund plats rapporten stoppats.</span><span class="sxs-lookup"><span data-stu-id="2f61e-136">If hello user reboots their device, hello background location report is stopped.</span></span> <span data-ttu-id="2f61e-137">toomake den startas om när datorn startas, lägga till den här koden.</span><span class="sxs-lookup"><span data-stu-id="2f61e-137">toomake it automatically restart at boot time, add this code.</span></span>

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

<span data-ttu-id="2f61e-138">Du måste också tooadd hello efter behörighet om saknas:</span><span class="sxs-lookup"><span data-stu-id="2f61e-138">You also need tooadd hello following permission if missing:</span></span>

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a><span data-ttu-id="2f61e-139">Android M-behörigheter</span><span class="sxs-lookup"><span data-stu-id="2f61e-139">Android M permissions</span></span>
<span data-ttu-id="2f61e-140">Från och med Android M kan vissa behörigheter hanteras vid körning och kräver användargodkännande.</span><span class="sxs-lookup"><span data-stu-id="2f61e-140">Starting with Android M, some permissions are managed at runtime and need user approval.</span></span>

<span data-ttu-id="2f61e-141">Om du riktar Android API-nivå 23 är hello runtime behörigheter inaktiverade som standard för nya installationer av appar.</span><span class="sxs-lookup"><span data-stu-id="2f61e-141">If you target Android API level 23, hello runtime permissions are turned off by default for new app installations.</span></span> <span data-ttu-id="2f61e-142">Annars är aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="2f61e-142">Otherwise they are turned on by default.</span></span>

<span data-ttu-id="2f61e-143">Du kan aktivera och inaktivera dessa behörigheter hello enheten inställningsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2f61e-143">You can enable/disable those permissions from hello device settings menu.</span></span> <span data-ttu-id="2f61e-144">Om du inaktiverar behörigheter hello system menyn stängs hello bakgrundsprocesser av hello programmet, vilket är en systembeteende och har ingen inverkan på möjlighet tooreceive push i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="2f61e-144">Turning off permissions from hello system menu kills hello background processes of hello application, which is a system behavior, and has no impact on ability tooreceive push in background.</span></span>

<span data-ttu-id="2f61e-145">Hello gäller Mobile Engagement plats reporting är är hello behörigheten som kräver godkännande vid körning:</span><span class="sxs-lookup"><span data-stu-id="2f61e-145">In hello context of Mobile Engagement location reporting, hello permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

<span data-ttu-id="2f61e-146">Begär behörighet från hello-användare som använder en standard systemdialogruta.</span><span class="sxs-lookup"><span data-stu-id="2f61e-146">Request permissions from hello user using a standard system dialog.</span></span> <span data-ttu-id="2f61e-147">Berätta om hello användaren godkänner ``EngagementAgent`` tootake ändra hänsyn i realtid.</span><span class="sxs-lookup"><span data-stu-id="2f61e-147">If hello user approves, tell ``EngagementAgent`` tootake that change into account in real-time.</span></span> <span data-ttu-id="2f61e-148">Annars är hello ändra bearbetade hello nästa gång hello startar hello användarprogram.</span><span class="sxs-lookup"><span data-stu-id="2f61e-148">Otherwise hello change is processed hello next time hello user launches hello application.</span></span>

<span data-ttu-id="2f61e-149">Här är en kod exempel toouse i en aktivitet i dina program toorequest behörigheter och vidarebefordra hello resultat om positivt för``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="2f61e-149">Here is a code sample toouse in an activity of your application toorequest permissions and forward hello result if positive too``EngagementAgent``:</span></span>

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this doesn't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
