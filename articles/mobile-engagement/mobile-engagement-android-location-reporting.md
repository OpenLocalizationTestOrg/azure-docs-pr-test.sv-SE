---
title: "Plats för Azure Mobile Engagement Android SDK"
description: "Beskriver hur du konfigurerar platsen för Azure Mobile Engagement Android SDK"
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
ms.openlocfilehash: 777d5719cce505b55dfb61c91dcac7e713b077a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="4e42e-103">Plats för Azure Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="4e42e-103">Location Reporting for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4e42e-104">Android</span><span class="sxs-lookup"><span data-stu-id="4e42e-104">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="4e42e-105">Det här avsnittet beskriver hur du gör plats rapportering för din Android-App.</span><span class="sxs-lookup"><span data-stu-id="4e42e-105">This topic describes how to do location reporting for your Android application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e42e-106">Krav</span><span class="sxs-lookup"><span data-stu-id="4e42e-106">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a><span data-ttu-id="4e42e-107">Platsrapportering</span><span class="sxs-lookup"><span data-stu-id="4e42e-107">Location reporting</span></span>
<span data-ttu-id="4e42e-108">Om du vill att platser som ska rapporteras som du behöver lägga till några rader configuration (mellan den `<application>` och `</application>` taggar).</span><span class="sxs-lookup"><span data-stu-id="4e42e-108">If you want locations to be reported, you need to add a few lines of configuration (between the `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="4e42e-109">Områdesrapportering</span><span class="sxs-lookup"><span data-stu-id="4e42e-109">Lazy area location reporting</span></span>
<span data-ttu-id="4e42e-110">Områdesrapportering kan reporting land, region och ort som är associerade med enheter.</span><span class="sxs-lookup"><span data-stu-id="4e42e-110">Lazy area location reporting enables reporting the country, region, and locality associated with devices.</span></span> <span data-ttu-id="4e42e-111">Den här typen av plats reporting används endast nätverksplatser (baserat på cellen ID eller Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="4e42e-111">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="4e42e-112">Området enheten rapporteras som mest en gång per session.</span><span class="sxs-lookup"><span data-stu-id="4e42e-112">The device area is reported at most once per session.</span></span> <span data-ttu-id="4e42e-113">GPS används aldrig och därmed den här typen av plats rapporten har låg påverkan på batteridrift.</span><span class="sxs-lookup"><span data-stu-id="4e42e-113">The GPS is never used, and thus this type of location report has low impact on the battery.</span></span>

<span data-ttu-id="4e42e-114">Rapporterat områden används för att beräkna geografiska statistik om användare, sessioner, händelser och fel.</span><span class="sxs-lookup"><span data-stu-id="4e42e-114">Reported areas are used to compute geographic statistics about users, sessions, events, and errors.</span></span> <span data-ttu-id="4e42e-115">De kan också användas som kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="4e42e-115">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="4e42e-116">Du aktiverar lazy Områdesplats rapportering genom att använda konfigurationen som nämnts tidigare i den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="4e42e-116">You enable lazy area location reporting by using the configuration previously mentioned in this procedure:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="4e42e-117">Du måste också ange en plats-behörighet.</span><span class="sxs-lookup"><span data-stu-id="4e42e-117">You also need to specify a location permission.</span></span> <span data-ttu-id="4e42e-118">Den här koden använder ``COARSE`` behörighet:</span><span class="sxs-lookup"><span data-stu-id="4e42e-118">This code uses ``COARSE`` permission:</span></span>

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="4e42e-119">Om din app kräver det, kan du använda ``ACCESS_FINE_LOCATION`` i stället.</span><span class="sxs-lookup"><span data-stu-id="4e42e-119">If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="4e42e-120">Rapportering i realtid plats</span><span class="sxs-lookup"><span data-stu-id="4e42e-120">Real-time location reporting</span></span>
<span data-ttu-id="4e42e-121">Rapportering i realtid plats kan reporting latituden och longituden som är associerade med enheter.</span><span class="sxs-lookup"><span data-stu-id="4e42e-121">Real-time location reporting enables reporting the latitude and longitude associated with devices.</span></span> <span data-ttu-id="4e42e-122">Som standard används den här typen av plats reporting endast platser i nätverket, baserat på cellen ID eller Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="4e42e-122">By default, this type of location reporting only uses network locations, based on Cell ID or WIFI.</span></span> <span data-ttu-id="4e42e-123">Reporting är endast aktiv när programmet körs i förgrunden (till exempel under en session).</span><span class="sxs-lookup"><span data-stu-id="4e42e-123">The reporting is only active when the application runs in foreground (for example, during a session).</span></span>

<span data-ttu-id="4e42e-124">Realtid platser är *inte* används för att beräkna statistik.</span><span class="sxs-lookup"><span data-stu-id="4e42e-124">Real-time locations are *NOT* used to compute statistics.</span></span> <span data-ttu-id="4e42e-125">Deras enda syfte är att tillåta användning av geobegränsning i realtid \<Reach-målgruppen-geofencing\> kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="4e42e-125">Their only purpose is to allow the use of real-time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="4e42e-126">Lägga till en rad med kod där du kan ange anslutningssträngen Engagement i aktiviteten starta om du vill aktivera realtidsskydd plats reporting.</span><span class="sxs-lookup"><span data-stu-id="4e42e-126">To enable real-time location reporting, add a line of code to where you set the Engagement connection string in the launcher activity.</span></span> <span data-ttu-id="4e42e-127">Resultatet ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="4e42e-127">The result looks like the following:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a><span data-ttu-id="4e42e-128">GPS baseras reporting</span><span class="sxs-lookup"><span data-stu-id="4e42e-128">GPS based reporting</span></span>
<span data-ttu-id="4e42e-129">Som standard använder rapportering i realtid plats endast nätverksbaserade platser.</span><span class="sxs-lookup"><span data-stu-id="4e42e-129">By default, real-time location reporting only uses network-based locations.</span></span> <span data-ttu-id="4e42e-130">Använd konfigurationsobjektet för att aktivera användning av GPS-baserade platser, vilket är mycket mer precisa:</span><span class="sxs-lookup"><span data-stu-id="4e42e-130">To enable the use of GPS-based locations, which are far more precise, use the configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="4e42e-131">Du måste också lägga till följande behörigheter om de saknas:</span><span class="sxs-lookup"><span data-stu-id="4e42e-131">You also need to add the following permission if missing:</span></span>

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="4e42e-132">Rapportering i bakgrunden</span><span class="sxs-lookup"><span data-stu-id="4e42e-132">Background reporting</span></span>
<span data-ttu-id="4e42e-133">Som standard är rapportering i realtid plats endast aktiv när programmet körs i förgrunden (till exempel under en session).</span><span class="sxs-lookup"><span data-stu-id="4e42e-133">By default, real-time location reporting is only active when the application runs in foreground (for example, during a session).</span></span> <span data-ttu-id="4e42e-134">Aktivera reporting även i bakgrunden med det här konfigurationsobjektet:</span><span class="sxs-lookup"><span data-stu-id="4e42e-134">To enable the reporting also in background, use this configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="4e42e-135">När programmet körs i bakgrunden, nätverksbaserade platser rapporteras även om du har aktiverat GPS.</span><span class="sxs-lookup"><span data-stu-id="4e42e-135">When the application runs in background, only network-based locations are reported, even if you enabled the GPS.</span></span>
> 
> 

<span data-ttu-id="4e42e-136">Om användaren startar om sin enhet, har rapportens bakgrund plats stoppats.</span><span class="sxs-lookup"><span data-stu-id="4e42e-136">If the user reboots their device, the background location report is stopped.</span></span> <span data-ttu-id="4e42e-137">Lägg till denna kod så att den startar om automatiskt när datorn startas.</span><span class="sxs-lookup"><span data-stu-id="4e42e-137">To make it automatically restart at boot time, add this code.</span></span>

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

<span data-ttu-id="4e42e-138">Du måste också lägga till följande behörigheter om de saknas:</span><span class="sxs-lookup"><span data-stu-id="4e42e-138">You also need to add the following permission if missing:</span></span>

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a><span data-ttu-id="4e42e-139">Android M-behörigheter</span><span class="sxs-lookup"><span data-stu-id="4e42e-139">Android M permissions</span></span>
<span data-ttu-id="4e42e-140">Från och med Android M kan vissa behörigheter hanteras vid körning och kräver användargodkännande.</span><span class="sxs-lookup"><span data-stu-id="4e42e-140">Starting with Android M, some permissions are managed at runtime and need user approval.</span></span>

<span data-ttu-id="4e42e-141">Om du riktar Android API-nivå 23 är behörigheterna runtime inaktiverade som standard för nya installationer av appar.</span><span class="sxs-lookup"><span data-stu-id="4e42e-141">If you target Android API level 23, the runtime permissions are turned off by default for new app installations.</span></span> <span data-ttu-id="4e42e-142">Annars är aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="4e42e-142">Otherwise they are turned on by default.</span></span>

<span data-ttu-id="4e42e-143">Du kan aktivera och inaktivera dessa behörigheter på inställningsmenyn för enheten.</span><span class="sxs-lookup"><span data-stu-id="4e42e-143">You can enable/disable those permissions from the device settings menu.</span></span> <span data-ttu-id="4e42e-144">Om du inaktiverar behörigheter från system-menyn stängs bakgrundsprocesser av programmet, vilket är en systembeteende och har ingen inverkan på förmåga att ta emot push i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="4e42e-144">Turning off permissions from the system menu kills the background processes of the application, which is a system behavior, and has no impact on ability to receive push in background.</span></span>

<span data-ttu-id="4e42e-145">I samband med Mobile Engagement plats reporting är de behörigheter som kräver godkännande vid körning:</span><span class="sxs-lookup"><span data-stu-id="4e42e-145">In the context of Mobile Engagement location reporting, the permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

<span data-ttu-id="4e42e-146">Begär behörighet från den användare som använder en standard systemdialogruta.</span><span class="sxs-lookup"><span data-stu-id="4e42e-146">Request permissions from the user using a standard system dialog.</span></span> <span data-ttu-id="4e42e-147">Om användaren godkänner berätta ``EngagementAgent`` göra den ändringen i beräkningen i realtid.</span><span class="sxs-lookup"><span data-stu-id="4e42e-147">If the user approves, tell ``EngagementAgent`` to take that change into account in real-time.</span></span> <span data-ttu-id="4e42e-148">Annars bearbetas ändringen nästa gång användaren startar programmet.</span><span class="sxs-lookup"><span data-stu-id="4e42e-148">Otherwise the change is processed the next time the user launches the application.</span></span>

<span data-ttu-id="4e42e-149">Här följer ett kodexempel på ska användas i en aktivitet på ditt program begär behörighet och vidarebefordra resultatet om positivt till ``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="4e42e-149">Here is a code sample to use in an activity of your application to request permissions and forward the result if positive to ``EngagementAgent``:</span></span>

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
