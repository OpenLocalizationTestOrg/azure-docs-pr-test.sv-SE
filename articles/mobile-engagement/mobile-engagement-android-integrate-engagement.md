---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a5487793-1a12-4f6c-a1cf-587c5a671e6b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4f79936ea0fa6102023dec2b4682032a4a81fa9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-android"></a><span data-ttu-id="f92f1-103">Hur tooIntegrate Engagement på Android</span><span class="sxs-lookup"><span data-stu-id="f92f1-103">How tooIntegrate Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f92f1-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="f92f1-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="f92f1-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="f92f1-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="f92f1-106">iOS</span><span class="sxs-lookup"><span data-stu-id="f92f1-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="f92f1-107">Android</span><span class="sxs-lookup"><span data-stu-id="f92f1-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="f92f1-108">Den här proceduren beskriver hello enklaste sättet tooactivate Engagement analyser och övervakningsfunktionerna i din Android-App.</span><span class="sxs-lookup"><span data-stu-id="f92f1-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your Android application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f92f1-109">Den lägsta Android SDK-API-nivån måste vara 10 eller senare (Android 2.3.3 eller högre).</span><span class="sxs-lookup"><span data-stu-id="f92f1-109">Your minimum Android SDK API level must be 10 or higher (Android 2.3.3 or higher).</span></span>
> 
> 

<span data-ttu-id="f92f1-110">hello följande är tillräckligt med tooactivates hello loggar behövs en rapport för toocompute all statistik om användare, sessioner, aktiviteter, krascher och Technicals.</span><span class="sxs-lookup"><span data-stu-id="f92f1-110">hello following steps are enough tooactivates hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="f92f1-111">hello loggar behövs en rapport toocompute annan statistik som händelser, fel och jobb måste göras manuellt med hjälp av hello Engagement API (se [hur toouse hello avancerade Mobile Engagement i din Android-märkning API](mobile-engagement-android-use-engagement-api.md) eftersom dessa statistik är programberoende.</span><span class="sxs-lookup"><span data-stu-id="f92f1-111">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Android](mobile-engagement-android-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-hello-engagement-sdk-and-service-into-your-android-project"></a><span data-ttu-id="f92f1-112">Bädda in hello Engagement SDK och tjänsten i din Android-projekt</span><span class="sxs-lookup"><span data-stu-id="f92f1-112">Embed hello Engagement SDK and service into your Android project</span></span>
<span data-ttu-id="f92f1-113">Hämta hello Android SDK från [här](https://aka.ms/vq9mfn) hämta `mobile-engagement-VERSION.jar` och placera dem i hello `libs` mappen för din Android-projekt (skapa hello biblioteksmappen om det inte finns ännu).</span><span class="sxs-lookup"><span data-stu-id="f92f1-113">Download hello Android SDK from [here](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` and put them into hello `libs` folder of your Android project (create hello libs folder if it does not exist yet).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f92f1-114">Om du skapar ditt programpaket med ProGuard måste tookeep vissa klasser.</span><span class="sxs-lookup"><span data-stu-id="f92f1-114">If you build your application package with ProGuard, you need tookeep some classes.</span></span> <span data-ttu-id="f92f1-115">Du kan använda följande konfiguration fragment hello:</span><span class="sxs-lookup"><span data-stu-id="f92f1-115">You can use hello following configuration snippet:</span></span>
> 
> <span data-ttu-id="f92f1-116">-Behåll publik klass * utökar android.os.IInterface-hålla klassen com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {</span><span class="sxs-lookup"><span data-stu-id="f92f1-116">-keep public class * extends android.os.IInterface -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {</span></span>
> 
> <span data-ttu-id="f92f1-117"><methods>; }</span><span class="sxs-lookup"><span data-stu-id="f92f1-117"><methods>; }</span></span>
> 
> 

<span data-ttu-id="f92f1-118">Ange anslutningssträngen Engagement genom att anropa hello följande metod i hello starta aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="f92f1-118">Specify your Engagement connection string by calling hello following method in hello launcher activity:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="f92f1-119">hello anslutningssträngen för ditt program visas på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f92f1-119">hello connection string for your application is displayed on Azure Portal.</span></span>

* <span data-ttu-id="f92f1-120">Om saknas, lägger du till följande Android behörigheter hello (innan hello `<application>` tagg):</span><span class="sxs-lookup"><span data-stu-id="f92f1-120">If missing, add hello following Android permissions (before hello `<application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* <span data-ttu-id="f92f1-121">Lägg till följande avsnitt hello (mellan hello `<application>` och `</application>` taggar):</span><span class="sxs-lookup"><span data-stu-id="f92f1-121">Add hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* <span data-ttu-id="f92f1-122">Ändra `<Your application name>` av hello namnet på ditt program.</span><span class="sxs-lookup"><span data-stu-id="f92f1-122">Change `<Your application name>` by hello name of your application.</span></span>

> [!TIP]
> <span data-ttu-id="f92f1-123">Hej `android:label` attribut kan du toochoose hello namnet på hello Engagement tjänsten som det visas toohello slutanvändare i hello ”körs tjänster” skärmen i sin telefon.</span><span class="sxs-lookup"><span data-stu-id="f92f1-123">hello `android:label` attribute allows you toochoose hello name of hello Engagement service as it will appear toohello end-users in hello "Running services" screen of their phone.</span></span> <span data-ttu-id="f92f1-124">Det är rekommenderat tooset attributet för`"<Your application name>Service"` (t.ex. `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="f92f1-124">It is recommended tooset this attribute too`"<Your application name>Service"` (e.g. `"AcmeFunGameService"`).</span></span>
> 
> 

<span data-ttu-id="f92f1-125">Att ange hello `android:process` attributet säkerställer att hello Engagement tjänsten kommer att köras i en egen process (kör Engagement hello samma process som programmet gör main/UI-tråden potentiellt mindre känslig).</span><span class="sxs-lookup"><span data-stu-id="f92f1-125">Specifying hello `android:process` attribute ensures that hello Engagement service will run in its own process (running Engagement in hello same process as your application will make your main/UI thread potentially less responsive).</span></span>

> [!NOTE]
> <span data-ttu-id="f92f1-126">All kod som du `Application.onCreate()` och andra program återanrop ska köras för ditt programs processer, inklusive hello Engagement service.</span><span class="sxs-lookup"><span data-stu-id="f92f1-126">Any code you place in `Application.onCreate()` and other application callbacks will be run for all your application's processes, including hello Engagement service.</span></span> <span data-ttu-id="f92f1-127">Det kan ha oönskade sidoeffekter (till exempel onödiga minnesallokering och trådar i hello Engagement process, dubbla broadcast mottagare eller tjänster).</span><span class="sxs-lookup"><span data-stu-id="f92f1-127">It may have unwanted side effects (like unneeded memory allocations and threads in hello Engagement's process, duplicate broadcast receivers or services).</span></span>
> 
> 

<span data-ttu-id="f92f1-128">Om du åsidosätter `Application.onCreate()`, är det rekommenderade tooadd hello följande kodavsnitt hello början av din `Application.onCreate()` funktionen:</span><span class="sxs-lookup"><span data-stu-id="f92f1-128">If you override `Application.onCreate()`, it's recommended tooadd hello following code snippet at hello beginning of your `Application.onCreate()` function:</span></span>

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

<span data-ttu-id="f92f1-129">Du kan göra samma sak för hello `Application.onTerminate()`, `Application.onLowMemory()` och `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="f92f1-129">You can do hello same thing for `Application.onTerminate()`, `Application.onLowMemory()` and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="f92f1-130">Du kan också utöka `EngagementApplication` i stället för att utöka `Application`: hello återanrop `Application.onCreate()` hello processen kontroll och anrop `Application.onApplicationProcessCreate()` endast om hello aktuella processen inte hello en hello Engagement värdtjänsten hello samma regler gäller för Hej andra återanrop.</span><span class="sxs-lookup"><span data-stu-id="f92f1-130">You can also extend `EngagementApplication` instead of extending `Application`: hello callback `Application.onCreate()` does hello process check and calls `Application.onApplicationProcessCreate()` only if hello current process is not hello one hosting hello Engagement service, hello same rules apply for hello other callbacks.</span></span>

## <a name="basic-reporting"></a><span data-ttu-id="f92f1-131">Grundläggande reporting</span><span class="sxs-lookup"><span data-stu-id="f92f1-131">Basic reporting</span></span>
### <a name="recommended-method-overload-your-activity-classes"></a><span data-ttu-id="f92f1-132">Rekommenderad metod: överlagra din `Activity` klasser</span><span class="sxs-lookup"><span data-stu-id="f92f1-132">Recommended method: overload your `Activity` classes</span></span>
<span data-ttu-id="f92f1-133">I ordning tooactivate hello rapport alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik du precis har toomake alla dina `*Activity` underordnade klasser ärver från hello motsvarande `Engagement*Activity` klasser (t.ex. Om din bakåtkompatibelt utökar `ListActivity`, se den utökar `EngagementListActivity`).</span><span class="sxs-lookup"><span data-stu-id="f92f1-133">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you just have toomake all your `*Activity` sub-classes inherit from hello corresponding `Engagement*Activity` classes (e.g. if your legacy activity extends `ListActivity`, make it extends `EngagementListActivity`).</span></span>

<span data-ttu-id="f92f1-134">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="f92f1-134">**Without Engagement :**</span></span>

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

<span data-ttu-id="f92f1-135">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="f92f1-135">**With Engagement :**</span></span>

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [!IMPORTANT]
> <span data-ttu-id="f92f1-136">När du använder `EngagementListActivity` eller `EngagementExpandableListActivity`, kontrollera anrop för`requestWindowFeature(...);` görs före hello anrop för`super.onCreate(...);`, annars en krasch inträffar.</span><span class="sxs-lookup"><span data-stu-id="f92f1-136">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call too`requestWindowFeature(...);` is made before hello call too`super.onCreate(...);`, otherwise a crash will occur.</span></span>
> 
> 

<span data-ttu-id="f92f1-137">Du hittar de här klasserna i hello `src` mappen och kopiera dem till ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="f92f1-137">You can find these classes in hello `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="f92f1-138">hello klasser finns också i hello **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="f92f1-138">hello classes are also in hello **JavaDoc**.</span></span>

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="f92f1-139">Alternativ metod: anropa `startActivity()` och `endActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="f92f1-139">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="f92f1-140">Om du inte kan eller inte vill att toooverload din `Activity` klasser, kan du i stället börja och sluta dina aktiviteter genom att anropa `EngagementAgent`'s metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="f92f1-140">If you cannot or do not want toooverload your `Activity` classes, you can instead start and end your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f92f1-141">hello Android SDK anropar aldrig hello `endActivity()` metod, även om programmet hello avslutas (på Android program faktiskt aldrig stängs).</span><span class="sxs-lookup"><span data-stu-id="f92f1-141">hello Android SDK never calls hello `endActivity()` method, even when hello application is closed (on Android, applications are actually never closed).</span></span> <span data-ttu-id="f92f1-142">Därför är det *hög* rekommenderas toocall hello `startActivity()` metod i hello `onResume` återanrop av *alla* dina aktiviteter och hello `endActivity()` metod i hello `onPause()` återanrop av *alla* dina aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f92f1-142">Thus, it is *HIGHLY* recommended toocall hello `startActivity()` method in hello `onResume` callback of *ALL* your activities, and hello `endActivity()` method in hello `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="f92f1-143">Detta är hello endast sätt toobe till att sessioner inte spridas.</span><span class="sxs-lookup"><span data-stu-id="f92f1-143">This is hello only way toobe sure that sessions will not be leaked.</span></span> <span data-ttu-id="f92f1-144">Om en session är känd koppla hello Engagement service aldrig från hello Engagement-serverdelen (eftersom hello tjänsten fortfarande ansluten så länge en session väntar).</span><span class="sxs-lookup"><span data-stu-id="f92f1-144">If a session is leaked, hello Engagement service will never disconnect from hello Engagement backend (since hello service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="f92f1-145">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="f92f1-145">Here is an example:</span></span>

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

<span data-ttu-id="f92f1-146">Det här exemplet mycket lik toohello `EngagementActivity` klassen och dess varianter vars källkoden har angetts i hello `src` mapp.</span><span class="sxs-lookup"><span data-stu-id="f92f1-146">This example very similiar toohello `EngagementActivity` class and its variants, whose source code is provided in hello `src` folder.</span></span>

## <a name="test"></a><span data-ttu-id="f92f1-147">Testa</span><span class="sxs-lookup"><span data-stu-id="f92f1-147">Test</span></span>
<span data-ttu-id="f92f1-148">Nu verifiera din integrering genom att köra din mobila app i en emulator eller en enhet och verifiera att den registreras en session på fliken för övervakning av hello.</span><span class="sxs-lookup"><span data-stu-id="f92f1-148">Now please verify your integration by running your mobile app in an emulator or device and verifying that it registers a session on hello Monitor tab.</span></span>

<span data-ttu-id="f92f1-149">hello nästa avsnitt är valfria.</span><span class="sxs-lookup"><span data-stu-id="f92f1-149">hello next sections are optional.</span></span>

## <a name="location-reporting"></a><span data-ttu-id="f92f1-150">Platsrapportering</span><span class="sxs-lookup"><span data-stu-id="f92f1-150">Location reporting</span></span>
<span data-ttu-id="f92f1-151">Om du vill att platser toobe rapporterade måste tooadd configuration några rader (mellan hello `<application>` och `</application>` taggar).</span><span class="sxs-lookup"><span data-stu-id="f92f1-151">If you want locations toobe reported, you need tooadd a few lines of configuration (between hello `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="f92f1-152">Områdesrapportering</span><span class="sxs-lookup"><span data-stu-id="f92f1-152">Lazy area location reporting</span></span>
<span data-ttu-id="f92f1-153">Områdesrapportering tillåter tooreport hello land, region och plats som är associerade toodevices.</span><span class="sxs-lookup"><span data-stu-id="f92f1-153">Lazy area location reporting allows tooreport hello country, region and locality associated toodevices.</span></span> <span data-ttu-id="f92f1-154">Den här typen av plats reporting används endast nätverksplatser (baserat på cellen ID eller Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="f92f1-154">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="f92f1-155">hello enheten området rapporteras högst en gång per session.</span><span class="sxs-lookup"><span data-stu-id="f92f1-155">hello device area is reported at most once per session.</span></span> <span data-ttu-id="f92f1-156">hello GPS aldrig används och därmed den här typen av plats rapport har bara ett fåtal (inte toosay inga) inverkan på hello batteri.</span><span class="sxs-lookup"><span data-stu-id="f92f1-156">hello GPS is never used, and thus this type of location report has very few (not toosay no) impact on hello battery.</span></span>

<span data-ttu-id="f92f1-157">Rapporterat områden är används toocompute geografiska statistik om användare, sessioner, händelser och fel.</span><span class="sxs-lookup"><span data-stu-id="f92f1-157">Reported areas are used toocompute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="f92f1-158">De kan också användas som kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="f92f1-158">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="f92f1-159">tooenable lazy Områdesplats rapportering, du kan göra det med hjälp av hello configuration ovan i den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="f92f1-159">tooenable lazy area location reporting, you can do it by using hello configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="f92f1-160">Du måste också tooadd hello efter behörighet om saknas:</span><span class="sxs-lookup"><span data-stu-id="f92f1-160">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="f92f1-161">Eller du kan fortsätta att använda ``ACCESS_FINE_LOCATION`` om du redan använder den i ditt program.</span><span class="sxs-lookup"><span data-stu-id="f92f1-161">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="f92f1-162">Rapportering av platsen i realtid</span><span class="sxs-lookup"><span data-stu-id="f92f1-162">Real time location reporting</span></span>
<span data-ttu-id="f92f1-163">Realtid plats kan tooreport hello latitud och longitud associerade toodevices.</span><span class="sxs-lookup"><span data-stu-id="f92f1-163">Real time location reporting allows tooreport hello latitude and longitude associated toodevices.</span></span> <span data-ttu-id="f92f1-164">Den här typen av plats reporting använder endast nätverksplatser (baserat på cellen ID eller Wi-Fi) som standard och hello reporting är bara aktivt när hello-program körs i förgrunden (dvs. under en session).</span><span class="sxs-lookup"><span data-stu-id="f92f1-164">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and hello reporting is only active when hello application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="f92f1-165">Realtid platser är *inte* används toocompute statistik.</span><span class="sxs-lookup"><span data-stu-id="f92f1-165">Real time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="f92f1-166">Syftet endast är tooallow hello användning av realtid geobegränsning \<Reach-målgruppen-geofencing\> kriterium i Reach-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="f92f1-166">Their only purpose is tooallow hello use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="f92f1-167">tooenable realtid plats rapportering, du kan göra det med hjälp av hello configuration ovan i den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="f92f1-167">tooenable real time location reporting, you can do it by using hello configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="f92f1-168">Du måste också tooadd hello efter behörighet om saknas:</span><span class="sxs-lookup"><span data-stu-id="f92f1-168">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="f92f1-169">Eller du kan fortsätta att använda ``ACCESS_FINE_LOCATION`` om du redan använder den i ditt program.</span><span class="sxs-lookup"><span data-stu-id="f92f1-169">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

#### <a name="gps-based-reporting"></a><span data-ttu-id="f92f1-170">GPS baseras reporting</span><span class="sxs-lookup"><span data-stu-id="f92f1-170">GPS based reporting</span></span>
<span data-ttu-id="f92f1-171">Som standard används rapportering av platsen i realtid endast baserat nätverksplatser.</span><span class="sxs-lookup"><span data-stu-id="f92f1-171">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="f92f1-172">tooenable hello användning av GPS-baserade platser (vilket är mycket mer exakt), använder hello konfigurationsobjekt:</span><span class="sxs-lookup"><span data-stu-id="f92f1-172">tooenable hello use of GPS based locations (which are far more precise), use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="f92f1-173">Du måste också tooadd hello efter behörighet om saknas:</span><span class="sxs-lookup"><span data-stu-id="f92f1-173">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="f92f1-174">Rapportering i bakgrunden</span><span class="sxs-lookup"><span data-stu-id="f92f1-174">Background reporting</span></span>
<span data-ttu-id="f92f1-175">Som standard är realtid plats reporting bara aktivt när hello-program körs i förgrunden (dvs. under en session).</span><span class="sxs-lookup"><span data-stu-id="f92f1-175">By default, real time location reporting is only active when hello application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="f92f1-176">tooenable hello reporting även i bakgrunden, använda hello konfigurationsobjekt:</span><span class="sxs-lookup"><span data-stu-id="f92f1-176">tooenable hello reporting also in background, use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="f92f1-177">När hello-program körs i bakgrunden, endast baserat nätverksplatser rapporteras, även om du har aktiverat hello GPS.</span><span class="sxs-lookup"><span data-stu-id="f92f1-177">When hello application runs in background, only network based locations are reported, even if you enabled hello GPS.</span></span>
> 
> 

<span data-ttu-id="f92f1-178">hello bakgrund plats rapporten kommer att stoppas om hello användaren startar om sin enhet, kan du lägga till den här toomake den startas om när datorn startas:</span><span class="sxs-lookup"><span data-stu-id="f92f1-178">hello background location report will be stopped if hello user reboots its device, you can add this toomake it automatically restart at boot time:</span></span>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

<span data-ttu-id="f92f1-179">Du måste också tooadd hello efter behörighet om saknas:</span><span class="sxs-lookup"><span data-stu-id="f92f1-179">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a><span data-ttu-id="f92f1-180">Android M-behörigheter</span><span class="sxs-lookup"><span data-stu-id="f92f1-180">Android M permissions</span></span>
<span data-ttu-id="f92f1-181">Från och med Android M kan vissa behörigheter hanteras under körning och måste användare att godkänna.</span><span class="sxs-lookup"><span data-stu-id="f92f1-181">Starting with Android M, some permissions are managed at runtime and needs user approval.</span></span>

<span data-ttu-id="f92f1-182">hello runtime behörigheter inaktiveras som standard för nya installationer av appar om du riktar Android API-nivå 23.</span><span class="sxs-lookup"><span data-stu-id="f92f1-182">hello runtime permissions will be turned off by default for new app installations if you target Android API level 23.</span></span> <span data-ttu-id="f92f1-183">Annars aktiveras som standard.</span><span class="sxs-lookup"><span data-stu-id="f92f1-183">Otherwise it will be turned on by default.</span></span>

<span data-ttu-id="f92f1-184">hello användare kan aktivera och inaktivera dessa behörigheter hello enheten inställningsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f92f1-184">hello user can enable/disable those permissions from hello device settings menu.</span></span> <span data-ttu-id="f92f1-185">Om du inaktiverar behörigheter från system-menyn stängs bakgrundsprocesser av programmet hello, det här är ett systembeteende och har ingen inverkan på möjlighet tooreceive push i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="f92f1-185">Turning off permissions from system menu kills background processes of hello application, this is a system behavior and has no impact on ability tooreceive push in background.</span></span>

<span data-ttu-id="f92f1-186">Hello gäller Mobile Engagement är är hello behörigheten som kräver godkännande vid körning:</span><span class="sxs-lookup"><span data-stu-id="f92f1-186">In hello context of Mobile Engagement, hello permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* <span data-ttu-id="f92f1-187">`WRITE_EXTERNAL_STORAGE`(endast när måldatorn Android API-nivå 23 för den här)</span><span class="sxs-lookup"><span data-stu-id="f92f1-187">`WRITE_EXTERNAL_STORAGE` (only when targeting Android API level 23 for this one)</span></span>

<span data-ttu-id="f92f1-188">hello extern lagring används endast för Reach helheten funktionen.</span><span class="sxs-lookup"><span data-stu-id="f92f1-188">hello external storage is used only for Reach big picture feature.</span></span> <span data-ttu-id="f92f1-189">Om du hittar genom att be användarna den här behörigheten toobe störande, du kan ta bort den om används endast för Mobile Engagement men på hello kostnaden för att inaktivera funktionen för stor bild.</span><span class="sxs-lookup"><span data-stu-id="f92f1-189">If you find asking users this permission toobe disruptive, you can remove it if you used it only for Mobile Engagement but at hello cost of disabling big picture feature.</span></span>

<span data-ttu-id="f92f1-190">För hello plats funktioner, bör du begära behörigheter toouser med en standard systemdialogruta.</span><span class="sxs-lookup"><span data-stu-id="f92f1-190">For hello location features, you should request permissions toouser using a standard system dialog.</span></span> <span data-ttu-id="f92f1-191">Om hello användaren godkänner du behöver tootell ``EngagementAgent`` tootake ändra hänsyn verkliga tidpunkt (annars hello ändringen kommer att bearbetade hello nästa gång hello startar hello användarprogram).</span><span class="sxs-lookup"><span data-stu-id="f92f1-191">If hello user approves, you need tootell ``EngagementAgent`` tootake that change into account in real time (otherwise hello change will be processed hello next time hello user launches hello application).</span></span>

<span data-ttu-id="f92f1-192">Här är en kod exempel toouse i en aktivitet i dina program toorequest behörigheter och vidarebefordra hello resultat om positivt för``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="f92f1-192">Here is a code sample toouse in an activity of your application toorequest permissions and forward hello result if positive too``EngagementAgent``:</span></span>

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
         * Request location permission, but this won't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want tookeep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a><span data-ttu-id="f92f1-193">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="f92f1-193">Advanced reporting</span></span>
<span data-ttu-id="f92f1-194">Du kan också om du vill tooreport programmet specifika händelser, fel och jobb, måste toouse hello Engagement API via hello metoder för hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="f92f1-194">Optionally, if you want tooreport application specific events, errors and jobs, you need toouse hello Engagement API through hello methods of hello `EngagementAgent` class.</span></span> <span data-ttu-id="f92f1-195">Ett objekt av den här klassen kan vara retreived genom att anropa hello `EngagementAgent.getInstance()` statisk metod.</span><span class="sxs-lookup"><span data-stu-id="f92f1-195">An object of this class can be retreived by calling hello `EngagementAgent.getInstance()` static method.</span></span>

<span data-ttu-id="f92f1-196">Hej Engagement API kan toouse alla Engagement avancerade funktioner och beskrivs i hello hur tooUse Engagement API på Android (samt som i hello tekniska dokumentationen för hello `EngagementAgent` klassen).</span><span class="sxs-lookup"><span data-stu-id="f92f1-196">hello Engagement API allows toouse all of Engagement's advanced capabilities and is detailed in hello How tooUse the Engagement API on Android (as well as in hello technical documentation of hello `EngagementAgent` class).</span></span>

## <a name="advanced-configuration-in-androidmanifestxml"></a><span data-ttu-id="f92f1-197">Avancerad konfiguration (i AndroidManifest.xml)</span><span class="sxs-lookup"><span data-stu-id="f92f1-197">Advanced configuration (in AndroidManifest.xml)</span></span>
### <a name="wake-locks"></a><span data-ttu-id="f92f1-198">Aktivera Lås</span><span class="sxs-lookup"><span data-stu-id="f92f1-198">Wake locks</span></span>
<span data-ttu-id="f92f1-199">Om du vill att statistik skickas i realtid vid användning av Wi-Fi eller när hello-skärmen av toobe, lägger du till följande valfria behörighet hello:</span><span class="sxs-lookup"><span data-stu-id="f92f1-199">If you want toobe sure that statistics are sent in real time when using Wifi or when hello screen is off, add hello following optional permission:</span></span>

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a><span data-ttu-id="f92f1-200">Kraschrapport</span><span class="sxs-lookup"><span data-stu-id="f92f1-200">Crash report</span></span>
<span data-ttu-id="f92f1-201">Om du vill toodisable kraschrapporter, Lägg till denna (mellan hello `<application>` och `</application>` taggar):</span><span class="sxs-lookup"><span data-stu-id="f92f1-201">If you want toodisable crash reports, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="f92f1-202">Burst tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="f92f1-202">Burst threshold</span></span>
<span data-ttu-id="f92f1-203">Som standard loggas hello Engagement service-rapporter i realtid.</span><span class="sxs-lookup"><span data-stu-id="f92f1-203">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="f92f1-204">Om ditt program rapporterar loggar väldigt ofta, är det bättre toobuffer hello loggar och tooreport dem samtidigt på en återkommande tid för grundläggande (Detta kallas hello ”burst-läget”).</span><span class="sxs-lookup"><span data-stu-id="f92f1-204">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello "burst mode").</span></span> <span data-ttu-id="f92f1-205">toodo så, Lägg till denna (mellan hello `<application>` och `</application>` taggar):</span><span class="sxs-lookup"><span data-stu-id="f92f1-205">toodo so, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="f92f1-206">hello burst läge öka något hello batteri livslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb kommer att vara avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas).</span><span class="sxs-lookup"><span data-stu-id="f92f1-206">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="f92f1-207">Det är rekommenderat toouse ett tröskelvärde i burst 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="f92f1-207">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="f92f1-208">Tidsgräns för session</span><span class="sxs-lookup"><span data-stu-id="f92f1-208">Session timeout</span></span>
<span data-ttu-id="f92f1-209">Som standard är en session avslutades 10-tal efter hello slutet av den senaste aktiviteten (som vanligtvis genom att trycka på hello start eller säkerhetskopiera nyckeln genom att ange hello phone inaktiv eller genom att hoppa över i ett annat program).</span><span class="sxs-lookup"><span data-stu-id="f92f1-209">By default, a session is ended 10s after hello end of its last activity (which usually occurs by pressing hello Home or Back key, by setting hello phone idle or by jumping into another application).</span></span> <span data-ttu-id="f92f1-210">Det här är tooavoid en session-delning varje gång hello användare Avsluta och återgå toohello program snabbt (som kan hända när han hämta en avbildning, kontrollera en avisering, osv.).</span><span class="sxs-lookup"><span data-stu-id="f92f1-210">This is tooavoid a session split each time hello user exit and return toohello application very quickly (which can happen when he pick up a image, check a notification, etc.).</span></span> <span data-ttu-id="f92f1-211">Du kanske vill toomodify den här parametern.</span><span class="sxs-lookup"><span data-stu-id="f92f1-211">You may want toomodify this parameter.</span></span> <span data-ttu-id="f92f1-212">toodo så, Lägg till denna (mellan hello `<application>` och `</application>` taggar):</span><span class="sxs-lookup"><span data-stu-id="f92f1-212">toodo so, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="f92f1-213">Inaktivera rapportering av logg</span><span class="sxs-lookup"><span data-stu-id="f92f1-213">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="f92f1-214">Med hjälp av ett metodanrop</span><span class="sxs-lookup"><span data-stu-id="f92f1-214">Using a method call</span></span>
<span data-ttu-id="f92f1-215">Om du vill Engagement toostop skicka loggar, kan du anropa:</span><span class="sxs-lookup"><span data-stu-id="f92f1-215">If you want Engagement toostop sending logs, you can call:</span></span>

            EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="f92f1-216">Det här anropet är beständiga: filen delade inställningar används.</span><span class="sxs-lookup"><span data-stu-id="f92f1-216">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="f92f1-217">Det kan ta en minut för hello service toostop om Engagement är aktiv när du anropar den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f92f1-217">If Engagement is active when you call this function, it may take 1 minute for hello service toostop.</span></span> <span data-ttu-id="f92f1-218">Men det kommer inte att starta hello-tjänsten på alla hello nästa gång du startar programmet hello.</span><span class="sxs-lookup"><span data-stu-id="f92f1-218">However it won't launch hello service at all hello next time you launch hello application.</span></span>

<span data-ttu-id="f92f1-219">Du kan aktivera loggen reporting igen genom att anropa hello samma fungerar med `true`.</span><span class="sxs-lookup"><span data-stu-id="f92f1-219">You can enable log reporting again by calling hello same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="f92f1-220">Integrering i din egen`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="f92f1-220">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="f92f1-221">I stället för att anropa den här funktionen kan du också integrera inställningen direkt i din befintliga `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="f92f1-221">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="f92f1-222">Du kan konfigurera Engagement toouse inställningarna filen (med hello läge) i hello `AndroidManifest.xml` filen med `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="f92f1-222">You can configure Engagement toouse your preferences file (with hello desired mode) in hello `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="f92f1-223">Hej `engagement:agent:settings:name` nyckeln är används toodefine hello namnet på hello delade inställningsfilen.</span><span class="sxs-lookup"><span data-stu-id="f92f1-223">hello `engagement:agent:settings:name` key is used toodefine hello name of hello shared preferences file.</span></span>
* <span data-ttu-id="f92f1-224">Hej `engagement:agent:settings:mode` nyckeln är används toodefine hello läge hello delade inställningsfilen bör du använda hello samma läge som i din `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="f92f1-224">hello `engagement:agent:settings:mode` key is used toodefine hello mode of hello shared preferences file, you should use hello same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="f92f1-225">hello-läge måste skickas som ett tal: Om du använder en kombination av konstant flaggor i koden, kontrollera hello totala värdet.</span><span class="sxs-lookup"><span data-stu-id="f92f1-225">hello mode must be passed as a number: if you are using a combination of constant flags in your code, check hello total value.</span></span>

<span data-ttu-id="f92f1-226">Engagement alltid använda hello `engagement:key` booleskt nyckel i hello inställningsfilen för att hantera den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="f92f1-226">Engagement always use hello `engagement:key` boolean key within hello preferences file for managing this setting.</span></span>

<span data-ttu-id="f92f1-227">Hej följande exempel på `AndroidManifest.xml` visar hello standardvärden:</span><span class="sxs-lookup"><span data-stu-id="f92f1-227">hello following example of `AndroidManifest.xml` shows hello default values:</span></span>

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

<span data-ttu-id="f92f1-228">Du kan lägga till en `CheckBoxPreference` i layouten inställningar som hello efter:</span><span class="sxs-lookup"><span data-stu-id="f92f1-228">Then you can add a `CheckBoxPreference` in your preference layout like hello following one:</span></span>

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
