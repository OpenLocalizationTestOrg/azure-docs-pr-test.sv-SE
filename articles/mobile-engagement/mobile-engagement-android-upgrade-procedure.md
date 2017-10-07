---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 11618586-c709-49ca-bcd8-745323ff1af6
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: df5c82812fe0a242eaa5df8c906030237215b7eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="e88ca-103">Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="e88ca-103">Upgrade procedures</span></span>
<span data-ttu-id="e88ca-104">Om du redan har integrerat en äldre version av våra SDK i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.</span><span class="sxs-lookup"><span data-stu-id="e88ca-104">If you already have integrated an older version of our SDK into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="e88ca-105">Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="e88ca-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="e88ca-106">Till exempel om du migrerar från 1.4.0 too1.6.0 som du har toofirst följer hello ”från 1.4.0 too1.5.0” proceduren sedan hello ”från 1.5.0 too1.6.0” proceduren.</span><span class="sxs-lookup"><span data-stu-id="e88ca-106">For example if you migrate from 1.4.0 too1.6.0 you have toofirst follow hello "from 1.4.0 too1.5.0" procedure then hello "from 1.5.0 too1.6.0" procedure.</span></span>

<span data-ttu-id="e88ca-107">Oavsett vilken hello-version som du uppgraderar från, du har tooreplace hello `mobile-engagement-VERSION.jar` med hello ny.</span><span class="sxs-lookup"><span data-stu-id="e88ca-107">Whatever hello version you upgrade from, you have tooreplace hello `mobile-engagement-VERSION.jar` with hello new one.</span></span>

## <a name="from-420-too421"></a><span data-ttu-id="e88ca-108">Från 4.2.0 too4.2.1</span><span class="sxs-lookup"><span data-stu-id="e88ca-108">From 4.2.0 too4.2.1</span></span>
<span data-ttu-id="e88ca-109">Det här steget kan faktiskt utförs på alla versioner av hello SDK, är en säkerhetsförbättring av när du integrerar räckvidden aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="e88ca-109">This step can actually be done on any version of hello SDK, its a security improvement when you integrate Reach activities.</span></span>

<span data-ttu-id="e88ca-110">Du bör nu lägga till `exported="false"` tooall Reach-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="e88ca-110">You should now add `exported="false"` tooall Reach activities.</span></span>

<span data-ttu-id="e88ca-111">Reach-aktiviteter bör nu se ut så här din `AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="e88ca-111">Reach activities should now look like this on your `AndroidManifest.xml`:</span></span>

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

## <a name="from-400-too410"></a><span data-ttu-id="e88ca-112">Från 4.0.0 too4.1.0</span><span class="sxs-lookup"><span data-stu-id="e88ca-112">From 4.0.0 too4.1.0</span></span>
<span data-ttu-id="e88ca-113">hello SDK nu hantera nya behörighetsmodellen från Android M.</span><span class="sxs-lookup"><span data-stu-id="e88ca-113">hello SDK now handle new permission model from Android M.</span></span>

<span data-ttu-id="e88ca-114">Om du använder funktioner för plats eller stor bild meddelanden Läs [i det här avsnittet](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="e88ca-114">If you use location features or big picture notifications please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>

<span data-ttu-id="e88ca-115">Dessutom toohello nya behörighetsmodellen vi stöder nu konfigurera funktioner för plats vid körning.</span><span class="sxs-lookup"><span data-stu-id="e88ca-115">In addition toohello new permission model, we now support configuring location features at runtime.</span></span>
<span data-ttu-id="e88ca-116">Vi är fortfarande är kompatibla med hello manifestet parametrar för plats men den är nu föråldrad.</span><span class="sxs-lookup"><span data-stu-id="e88ca-116">We are still compatible with hello manifest parameters for location but it's now deprecated.</span></span> <span data-ttu-id="e88ca-117">toouse runtime-konfiguration, ta bort hello de följande avsnitten från din ``AndroidManifest.xml``:</span><span class="sxs-lookup"><span data-stu-id="e88ca-117">toouse runtime configuration, remove hello following sections from your ``AndroidManifest.xml``:</span></span>

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

<span data-ttu-id="e88ca-118">och läsa [uppdateras det här förfarandet](mobile-engagement-android-integrate-engagement.md#location-reporting) toouse runtime konfiguration i stället.</span><span class="sxs-lookup"><span data-stu-id="e88ca-118">and read [this updated procedure](mobile-engagement-android-integrate-engagement.md#location-reporting) toouse runtime configuration instead.</span></span>

## <a name="from-300-too400"></a><span data-ttu-id="e88ca-119">Från 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="e88ca-119">From 3.0.0 too4.0.0</span></span>
### <a name="native-push"></a><span data-ttu-id="e88ca-120">Intern push</span><span class="sxs-lookup"><span data-stu-id="e88ca-120">Native push</span></span>
<span data-ttu-id="e88ca-121">Intern push (GCM/ADM) nu används också för i appen meddelanden så att du måste konfigurera hello native push-autentiseringsuppgifter för alla typer av push-kampanj.</span><span class="sxs-lookup"><span data-stu-id="e88ca-121">Native push (GCM/ADM) is now also used for in app notifications so you must configure hello native push credentials for any type of push campaign.</span></span>

<span data-ttu-id="e88ca-122">Om den inte redan Följ [proceduren](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span><span class="sxs-lookup"><span data-stu-id="e88ca-122">If not already done please follow [this procedure](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="e88ca-123">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="e88ca-123">AndroidManifest.xml</span></span>
<span data-ttu-id="e88ca-124">Reach-integrering har ändrats i ``AndroidManifest.xml``.</span><span class="sxs-lookup"><span data-stu-id="e88ca-124">Reach integration has been modified in ``AndroidManifest.xml``.</span></span>

<span data-ttu-id="e88ca-125">Ersätt detta:</span><span class="sxs-lookup"><span data-stu-id="e88ca-125">Replace this:</span></span>

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

<span data-ttu-id="e88ca-126">Av</span><span class="sxs-lookup"><span data-stu-id="e88ca-126">By</span></span>

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

<span data-ttu-id="e88ca-127">Det finns eventuellt en skärm för inläsning nu när du klickar på ett meddelande (med text/webbinnehåll) eller en avsökning.</span><span class="sxs-lookup"><span data-stu-id="e88ca-127">There is possibly a loading screen now when you click on an announcement (with text/web content) or a poll.</span></span>
<span data-ttu-id="e88ca-128">Du har tooadd detta för de kampanjer toowork i 4.0.0:</span><span class="sxs-lookup"><span data-stu-id="e88ca-128">You have tooadd this for those campaigns toowork in 4.0.0:</span></span>

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a><span data-ttu-id="e88ca-129">Resurser</span><span class="sxs-lookup"><span data-stu-id="e88ca-129">Resources</span></span>
<span data-ttu-id="e88ca-130">Bädda in hello nya `res/layout/engagement_loading.xml` filen i projektet.</span><span class="sxs-lookup"><span data-stu-id="e88ca-130">Embed hello new `res/layout/engagement_loading.xml` file into your project.</span></span>

## <a name="from-240-too300"></a><span data-ttu-id="e88ca-131">Från 2.4.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="e88ca-131">From 2.4.0 too3.0.0</span></span>
<span data-ttu-id="e88ca-132">hello beskrivs nedan hur toomigrate SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS i en app med Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e88ca-132">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> <span data-ttu-id="e88ca-133">Om du migrerar från en tidigare version finns hello Capptain webbplats toomigrate too2.4.0 först och sedan använda hello nedan.</span><span class="sxs-lookup"><span data-stu-id="e88ca-133">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too2.4.0 first and then apply hello following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e88ca-134">Capptain Mobile Engagement är inte hello samma tjänster och hello proceduren som anges nedan endast highlights hur toomigrate hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="e88ca-134">Capptain and Mobile Engagement are not hello same services, and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="e88ca-135">Migrera hello SDK i hello app migreras inte data från hello Capptain servrar toohello Mobile Engagement-servrar.</span><span class="sxs-lookup"><span data-stu-id="e88ca-135">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers.</span></span>
> 
> 

### <a name="jar-file"></a><span data-ttu-id="e88ca-136">JAR-filen</span><span class="sxs-lookup"><span data-stu-id="e88ca-136">JAR file</span></span>
<span data-ttu-id="e88ca-137">Ersätt `capptain.jar` av `mobile-engagement-VERSION.jar` i din `libs` mapp.</span><span class="sxs-lookup"><span data-stu-id="e88ca-137">Replace `capptain.jar` by `mobile-engagement-VERSION.jar` in your `libs` folder.</span></span>

### <a name="resource-files"></a><span data-ttu-id="e88ca-138">Resursfiler</span><span class="sxs-lookup"><span data-stu-id="e88ca-138">Resource files</span></span>
<span data-ttu-id="e88ca-139">Varje resursfil som vi angav (föregås av `capptain_`) har toobe ersättas med hello nya (med prefixet `engagement_`).</span><span class="sxs-lookup"><span data-stu-id="e88ca-139">Every resource file that we provided (prefixed by `capptain_`) has toobe replaced by hello new ones (prefixed with `engagement_`).</span></span>

<span data-ttu-id="e88ca-140">Om du har anpassat dessa filer, har du toore-tillämpa anpassningen på nya filer hello **alla hello identifierare i hello resursfiler har bytt**.</span><span class="sxs-lookup"><span data-stu-id="e88ca-140">If you customized those files, you have toore-apply your customization on hello new files, **all hello identifiers in hello resource files have also been renamed**.</span></span>

### <a name="application-id"></a><span data-ttu-id="e88ca-141">Program-ID:t</span><span class="sxs-lookup"><span data-stu-id="e88ca-141">Application ID</span></span>
<span data-ttu-id="e88ca-142">Engagement använder nu en anslutning tooconfigure hello SDK strängidentifierare, till exempel hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="e88ca-142">Now Engagement uses a connection string tooconfigure hello SDK identifiers such as hello application identifier.</span></span>

<span data-ttu-id="e88ca-143">Du har toouse `EngagementAgent.init` metod i din Starta aktivitet så här:</span><span class="sxs-lookup"><span data-stu-id="e88ca-143">You have toouse `EngagementAgent.init` method in your launcher activity like this:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="e88ca-144">hello anslutningssträngen för ditt program visas på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="e88ca-144">hello connection string for your application is displayed on Azure Portal.</span></span>

<span data-ttu-id="e88ca-145">Ta bort alla anrop för`CapptainAgent.configure` som `EngagementAgent.init` ersätter den här metoden.</span><span class="sxs-lookup"><span data-stu-id="e88ca-145">Please remove any call too`CapptainAgent.configure` as `EngagementAgent.init` replaces that method.</span></span>

<span data-ttu-id="e88ca-146">Hej `appId` inte längre kan konfigureras med `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="e88ca-146">hello `appId` can no longer be configured using `AndroidManifest.xml`.</span></span>

<span data-ttu-id="e88ca-147">Ta bort det här avsnittet från din `AndroidManifest.xml` om du har:</span><span class="sxs-lookup"><span data-stu-id="e88ca-147">Please remove this section from your `AndroidManifest.xml` if you have it:</span></span>

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a><span data-ttu-id="e88ca-148">Java-API</span><span class="sxs-lookup"><span data-stu-id="e88ca-148">Java API</span></span>
<span data-ttu-id="e88ca-149">Varje anrop tooany Java-klass av våra SDK har toobe har fått nytt namn; till exempel `CapptainAgent.getInstance(this)` måste ändras `EngagementAgent.getInstance(this)`, `extends CapptainActivity` måste ändras `extends EngagementActivity` etc...</span><span class="sxs-lookup"><span data-stu-id="e88ca-149">Every call tooany Java class of our SDK has toobe renamed; for example, `CapptainAgent.getInstance(this)` must be renamed `EngagementAgent.getInstance(this)`, `extends CapptainActivity` must be renamed `extends EngagementActivity` etc...</span></span>

<span data-ttu-id="e88ca-150">Om du har integrerat med standard agent inställningsfiler hello Standardfilnamnet är nu `engagement.agent` och hello nyckel `engagement:agent`.</span><span class="sxs-lookup"><span data-stu-id="e88ca-150">If you were integrated with default agent preference files, hello default file name is now `engagement.agent` and hello key is `engagement:agent`.</span></span>

<span data-ttu-id="e88ca-151">När du skapar web meddelanden hello Javascript binder är nu `engagementReachContent`.</span><span class="sxs-lookup"><span data-stu-id="e88ca-151">When creating web announcements, hello Javascript binder is now `engagementReachContent`.</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="e88ca-152">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="e88ca-152">AndroidManifest.xml</span></span>
<span data-ttu-id="e88ca-153">En mängd ändringar har hänt det hello tjänsten delas inte längre och mycket mottagare kan inte exporteras längre.</span><span class="sxs-lookup"><span data-stu-id="e88ca-153">A lot of changes happened there, hello service is not shared anymore, and a lot of receivers are not exportable anymore.</span></span>

<span data-ttu-id="e88ca-154">Hej tjänstedeklaration är nu enklare; ta bort hello avsiktshantering filter och alla metadata i den och lägga till `exportable=false`.</span><span class="sxs-lookup"><span data-stu-id="e88ca-154">hello service declaration is now simpler; remove hello intent filter and all meta-data inside it, and add `exportable=false`.</span></span>

<span data-ttu-id="e88ca-155">Dessutom allt har bytt namn till toouse engagement.</span><span class="sxs-lookup"><span data-stu-id="e88ca-155">Plus everything is renamed toouse engagement.</span></span>

<span data-ttu-id="e88ca-156">Nu ser ut som:</span><span class="sxs-lookup"><span data-stu-id="e88ca-156">It now looks like:</span></span>

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

<span data-ttu-id="e88ca-157">Om du vill tooenable test loggar hello metadata har nu flyttats toohello programmet taggen och har bytt namn:</span><span class="sxs-lookup"><span data-stu-id="e88ca-157">When you want tooenable test logs, hello meta-data has now been moved toohello application tag and has been renamed:</span></span>

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

<span data-ttu-id="e88ca-158">Alla andra metadata har precis bytt namn, här är hello fullständig lista (självklart byta endast hello som du använder):</span><span class="sxs-lookup"><span data-stu-id="e88ca-158">All other meta-data have just been renamed, here is hello full list (of course rename only hello ones you use):</span></span>

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>

            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

<span data-ttu-id="e88ca-159">Google Play och SmartAd spårning har tagits bort från SDK du precis har tooremove detta utan byte:</span><span class="sxs-lookup"><span data-stu-id="e88ca-159">Google Play and SmartAd tracking has been removed from SDK you just have tooremove this without replacement:</span></span>

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

<span data-ttu-id="e88ca-160">hello Reach aktiviteter deklareras nu så här:</span><span class="sxs-lookup"><span data-stu-id="e88ca-160">hello Reach activities are now declared like this:</span></span>

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

<span data-ttu-id="e88ca-161">Om du har anpassade Reach-aktiviteter, du behöver bara toochange hello avsiktshantering åtgärder toomatch antingen `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` eller `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span><span class="sxs-lookup"><span data-stu-id="e88ca-161">If you have custom Reach activities, you need only toochange hello intent actions toomatch either `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` or `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span></span>

<span data-ttu-id="e88ca-162">hello broadcast mottagare har bytt namn plus vi nu lägga till `exported=false`.</span><span class="sxs-lookup"><span data-stu-id="e88ca-162">hello broadcast receivers have been renamed, plus we now add `exported=false`.</span></span> <span data-ttu-id="e88ca-163">Här är hello fullständig lista över hello mottagare med hello nya specifikationen (självklart byta endast hello som du använder):</span><span class="sxs-lookup"><span data-stu-id="e88ca-163">Here is hello full list of hello receivers with hello new specification, (of course rename only hello ones you use):</span></span>

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

<span data-ttu-id="e88ca-164">Spåra mottagaren har tagits bort, så att du har tooremove i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="e88ca-164">Tracking receiver has been removed, so you have tooremove this section:</span></span>

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

<span data-ttu-id="e88ca-165">Observera att hello deklarationen av din implementering av hello sänder mottagaren **EngagementMessageReceiver** har ändrats i hello `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="e88ca-165">Note that hello declaration of your implementation of hello broadcast receiver **EngagementMessageReceiver** has changed in hello `AndroidManifest.xml`.</span></span> <span data-ttu-id="e88ca-166">Detta beror på att hello API toosend och ta bort valfri XMPP meddelanden från valfri XMPP entiteter och hello API toosend och ta emot meddelanden mellan enheter har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="e88ca-166">This is because hello API toosend and remove arbitrary XMPP messages from arbitrary XMPP entities and hello API toosend and receive messages between devices have been removed.</span></span> <span data-ttu-id="e88ca-167">Du har alltså också toodelete hello efter återanrop från din **EngagementMessageReceiver** implementering:</span><span class="sxs-lookup"><span data-stu-id="e88ca-167">Thus, you have also toodelete hello following callbacks from your **EngagementMessageReceiver** implementation :</span></span>

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

<span data-ttu-id="e88ca-168">och</span><span class="sxs-lookup"><span data-stu-id="e88ca-168">and</span></span>

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

<span data-ttu-id="e88ca-169">ta bort alla anrop på **EngagementAgent** för:</span><span class="sxs-lookup"><span data-stu-id="e88ca-169">then delete any call on **EngagementAgent** for :</span></span>

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

<span data-ttu-id="e88ca-170">och</span><span class="sxs-lookup"><span data-stu-id="e88ca-170">and</span></span>

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a><span data-ttu-id="e88ca-171">Proguard</span><span class="sxs-lookup"><span data-stu-id="e88ca-171">Proguard</span></span>
<span data-ttu-id="e88ca-172">Proguard konfiguration kan påverkas av rebranding hello regler nu ser ut som:</span><span class="sxs-lookup"><span data-stu-id="e88ca-172">Proguard configuration can be impacted by rebranding, hello rules are now looking like:</span></span>

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

