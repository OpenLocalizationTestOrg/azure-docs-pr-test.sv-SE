---
title: "aaaAdvanced konfigurationen för Azure Mobile Engagement Android SDK"
description: Beskriver hello avancerade konfigurationsalternativ inklusive hello Android Manifest med Azure Mobile Engagement Android SDK
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 37d2c09a-86fa-473d-8987-c7e35a0eb3e8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 757abf362021fd018f444cae6305524623e77062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="71878-103">Avancerad konfiguration för Azure Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="71878-103">Advanced configuration for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="71878-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="71878-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="71878-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="71878-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="71878-106">iOS</span><span class="sxs-lookup"><span data-stu-id="71878-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="71878-107">Android</span><span class="sxs-lookup"><span data-stu-id="71878-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
>
>

<span data-ttu-id="71878-108">Den här proceduren beskriver hur tooconfigure olika konfigurationsalternativ för Azure Mobile Engagement Android-appar.</span><span class="sxs-lookup"><span data-stu-id="71878-108">This procedure describes how tooconfigure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71878-109">Krav</span><span class="sxs-lookup"><span data-stu-id="71878-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a><span data-ttu-id="71878-110">Behörighet som krävs</span><span class="sxs-lookup"><span data-stu-id="71878-110">Permission Requirements</span></span>
<span data-ttu-id="71878-111">Vissa alternativ kräver särskilda behörigheter, som anges här referens- och raden i hello specifika funktioner.</span><span class="sxs-lookup"><span data-stu-id="71878-111">Some options require specific permissions, all of which are listed here for reference, and in-line in hello specific feature.</span></span> <span data-ttu-id="71878-112">Lägg till dessa behörigheter toohello AndroidManifest.xml i ditt projekt omedelbart före eller efter hello `<application>` tagg.</span><span class="sxs-lookup"><span data-stu-id="71878-112">Add these permissions toohello AndroidManifest.xml of your project immediately before or after hello `<application>` tag.</span></span>

<span data-ttu-id="71878-113">hello behörighet kod måste toolook som hello följande, där du fyller i hello behörighet från hello i tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="71878-113">hello permission code needs toolook like hello following, where you fill in hello appropriate permission from hello table that follows.</span></span>

    <uses-permission android:name="android.permission.[specific permission]"/>


| <span data-ttu-id="71878-114">Behörighet</span><span class="sxs-lookup"><span data-stu-id="71878-114">Permission</span></span> | <span data-ttu-id="71878-115">När den används</span><span class="sxs-lookup"><span data-stu-id="71878-115">When used</span></span> |
| --- | --- |
| <span data-ttu-id="71878-116">INTERNET</span><span class="sxs-lookup"><span data-stu-id="71878-116">INTERNET</span></span> |<span data-ttu-id="71878-117">Krävs.</span><span class="sxs-lookup"><span data-stu-id="71878-117">Required.</span></span> <span data-ttu-id="71878-118">För grundläggande rapportering</span><span class="sxs-lookup"><span data-stu-id="71878-118">For basic reporting</span></span> |
| <span data-ttu-id="71878-119">ACCESS_NETWORK_STATE</span><span class="sxs-lookup"><span data-stu-id="71878-119">ACCESS_NETWORK_STATE</span></span> |<span data-ttu-id="71878-120">Krävs.</span><span class="sxs-lookup"><span data-stu-id="71878-120">Required.</span></span> <span data-ttu-id="71878-121">För grundläggande rapportering</span><span class="sxs-lookup"><span data-stu-id="71878-121">For basic reporting</span></span> |
| <span data-ttu-id="71878-122">RECEIVE_BOOT_COMPLETED</span><span class="sxs-lookup"><span data-stu-id="71878-122">RECEIVE_BOOT_COMPLETED</span></span> |<span data-ttu-id="71878-123">Krävs.</span><span class="sxs-lookup"><span data-stu-id="71878-123">Required.</span></span> <span data-ttu-id="71878-124">tooshow in hello meddelanden center efter omstart av enheten</span><span class="sxs-lookup"><span data-stu-id="71878-124">tooshow up hello notifications center after device reboot</span></span> |
| <span data-ttu-id="71878-125">WAKE_LOCK</span><span class="sxs-lookup"><span data-stu-id="71878-125">WAKE_LOCK</span></span> |<span data-ttu-id="71878-126">Rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="71878-126">Recommended.</span></span> <span data-ttu-id="71878-127">Gör det möjligt att samla in data när du använder Wi-Fi eller när skärmen är inaktiverat</span><span class="sxs-lookup"><span data-stu-id="71878-127">Enables collecting data when using WiFi or when screen is off</span></span> |
| <span data-ttu-id="71878-128">VIBRERAR</span><span class="sxs-lookup"><span data-stu-id="71878-128">VIBRATE</span></span> |<span data-ttu-id="71878-129">Valfri.</span><span class="sxs-lookup"><span data-stu-id="71878-129">Optional.</span></span> <span data-ttu-id="71878-130">Aktiverar vibration när meddelanden tas emot</span><span class="sxs-lookup"><span data-stu-id="71878-130">Enables vibration when notifications are received</span></span> |
| <span data-ttu-id="71878-131">DOWNLOAD_WITHOUT_NOTIFICATION</span><span class="sxs-lookup"><span data-stu-id="71878-131">DOWNLOAD_WITHOUT_NOTIFICATION</span></span> |<span data-ttu-id="71878-132">Valfri.</span><span class="sxs-lookup"><span data-stu-id="71878-132">Optional.</span></span> <span data-ttu-id="71878-133">Aktiverar Android helheten meddelanden</span><span class="sxs-lookup"><span data-stu-id="71878-133">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="71878-134">WRITE_EXTERNAL_STORAGE</span><span class="sxs-lookup"><span data-stu-id="71878-134">WRITE_EXTERNAL_STORAGE</span></span> |<span data-ttu-id="71878-135">Valfri.</span><span class="sxs-lookup"><span data-stu-id="71878-135">Optional.</span></span> <span data-ttu-id="71878-136">Aktiverar Android helheten meddelanden</span><span class="sxs-lookup"><span data-stu-id="71878-136">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="71878-137">ACCESS_COARSE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="71878-137">ACCESS_COARSE_LOCATION</span></span> |<span data-ttu-id="71878-138">Valfri.</span><span class="sxs-lookup"><span data-stu-id="71878-138">Optional.</span></span> <span data-ttu-id="71878-139">Aktiverar rapportering i realtid plats</span><span class="sxs-lookup"><span data-stu-id="71878-139">Enables Real-time location reporting</span></span> |
| <span data-ttu-id="71878-140">ACCESS_FINE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="71878-140">ACCESS_FINE_LOCATION</span></span> |<span data-ttu-id="71878-141">Valfri.</span><span class="sxs-lookup"><span data-stu-id="71878-141">Optional.</span></span> <span data-ttu-id="71878-142">Aktiverar GPS-baserad plats reporting</span><span class="sxs-lookup"><span data-stu-id="71878-142">Enables GPS-based location reporting</span></span> |

<span data-ttu-id="71878-143">Från och med Android M [vissa behörigheter hanteras vid körning](mobile-engagement-android-location-reporting.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="71878-143">Starting with Android M, [some permissions are managed at run time](mobile-engagement-android-location-reporting.md#android-m-permissions).</span></span>

<span data-ttu-id="71878-144">Om du redan använder ``ACCESS_FINE_LOCATION``, och du inte behöver tooalso använder ``ACCESS_COARSE_LOCATION``.</span><span class="sxs-lookup"><span data-stu-id="71878-144">If you are already using ``ACCESS_FINE_LOCATION``, then you don't need tooalso use ``ACCESS_COARSE_LOCATION``.</span></span>

## <a name="android-manifest-configuration-options"></a><span data-ttu-id="71878-145">Android Manifest konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="71878-145">Android Manifest configuration options</span></span>
### <a name="crash-report"></a><span data-ttu-id="71878-146">Kraschrapport</span><span class="sxs-lookup"><span data-stu-id="71878-146">Crash report</span></span>
<span data-ttu-id="71878-147">toodisable kraschrapporter, Lägg till denna kod mellan hello `<application>` och `</application>` taggar:</span><span class="sxs-lookup"><span data-stu-id="71878-147">toodisable crash reports, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="71878-148">Burst tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="71878-148">Burst threshold</span></span>
<span data-ttu-id="71878-149">Som standard loggas hello Engagement service-rapporter i realtid.</span><span class="sxs-lookup"><span data-stu-id="71878-149">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="71878-150">Om din rapport programloggarna ofta varierar, är det bättre toobuffer hello loggar och tooreport dem på en gång på vanlig taget base (kallas ”burst läge”).</span><span class="sxs-lookup"><span data-stu-id="71878-150">If your application report logs vary frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (called "burst mode").</span></span> <span data-ttu-id="71878-151">toodo Lägg därför till den här koden mellan hello `<application>` och `</application>` taggar:</span><span class="sxs-lookup"><span data-stu-id="71878-151">toodo so, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="71878-152">Burst läge något ökar hello batterilivslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb varaktighet är avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas).</span><span class="sxs-lookup"><span data-stu-id="71878-152">Burst mode slightly increases hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration are rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="71878-153">Burst-tröskelvärde ska vara längre än 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="71878-153">Your burst threshold should be no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="71878-154">Tidsgräns för session</span><span class="sxs-lookup"><span data-stu-id="71878-154">Session timeout</span></span>
 <span data-ttu-id="71878-155">Du kan avsluta en aktivitet genom att trycka på hello **Start** eller **tillbaka** nyckel genom att ange hello phone inaktiv eller genom att hoppa över i ett annat program.</span><span class="sxs-lookup"><span data-stu-id="71878-155">You can end an activity by pressing hello **Home** or **Back** key, by setting hello phone idle or by jumping into another application.</span></span> <span data-ttu-id="71878-156">Som standard avslutas session tio sekunder efter den senaste aktiviteten hello slut.</span><span class="sxs-lookup"><span data-stu-id="71878-156">By default, a session is ended ten seconds after hello end of its last activity.</span></span> <span data-ttu-id="71878-157">Detta förhindrar en session-delning varje gång hello användare avslutas och returnerar toohello program snabbt, vilket kan hända när hello användare hämtar en avbildning, kontrollerar en avisering, osv. Du kanske vill toomodify den här parametern.</span><span class="sxs-lookup"><span data-stu-id="71878-157">This avoids a session split each time hello user exits and returns toohello application quickly, which can happen when hello user picks up an image, checks a notification, etc. You may want toomodify this parameter.</span></span> <span data-ttu-id="71878-158">toodo Lägg därför till den här koden mellan hello `<application>` och `</application>` taggar:</span><span class="sxs-lookup"><span data-stu-id="71878-158">toodo so, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="71878-159">Inaktivera rapportering av logg</span><span class="sxs-lookup"><span data-stu-id="71878-159">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="71878-160">Med hjälp av ett metodanrop</span><span class="sxs-lookup"><span data-stu-id="71878-160">Using a method call</span></span>
<span data-ttu-id="71878-161">Om du vill Engagement toostop skicka loggar, kan du anropa:</span><span class="sxs-lookup"><span data-stu-id="71878-161">If you want Engagement toostop sending logs, you can call:</span></span>

    EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="71878-162">Det här anropet är beständiga: filen delade inställningar används.</span><span class="sxs-lookup"><span data-stu-id="71878-162">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="71878-163">Det kan ta en minut för hello service toostop om Engagement är aktiv när du anropar den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="71878-163">If Engagement is active when you call this function, it may take one minute for hello service toostop.</span></span> <span data-ttu-id="71878-164">Men det kommer inte att starta hello-tjänsten på alla hello nästa gång du startar programmet hello.</span><span class="sxs-lookup"><span data-stu-id="71878-164">However it won't launch hello service at all hello next time you launch hello application.</span></span>

<span data-ttu-id="71878-165">Du kan aktivera loggen reporting igen genom att anropa hello samma fungerar med `true`.</span><span class="sxs-lookup"><span data-stu-id="71878-165">You can enable log reporting again by calling hello same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="71878-166">Integrering i din egen`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="71878-166">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="71878-167">I stället för att anropa den här funktionen kan du också integrera inställningen direkt i din befintliga `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="71878-167">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="71878-168">Du kan konfigurera Engagement toouse inställningarna filen (med hello läge) i hello `AndroidManifest.xml` filen med `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="71878-168">You can configure Engagement toouse your preferences file (with hello desired mode) in hello `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="71878-169">Hej `engagement:agent:settings:name` nyckeln är används toodefine hello namnet på hello delade inställningsfilen.</span><span class="sxs-lookup"><span data-stu-id="71878-169">hello `engagement:agent:settings:name` key is used toodefine hello name of hello shared preferences file.</span></span>
* <span data-ttu-id="71878-170">Hej `engagement:agent:settings:mode` nyckeln är används toodefine hello läge hello delade inställningsfilen.</span><span class="sxs-lookup"><span data-stu-id="71878-170">hello `engagement:agent:settings:mode` key is used toodefine hello mode of hello shared preferences file.</span></span> <span data-ttu-id="71878-171">Använd hello samma läge som i din `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="71878-171">Use hello same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="71878-172">hello-läge måste skickas som ett tal: Om du använder en kombination av konstant flaggor i koden, kontrollera hello totala värdet.</span><span class="sxs-lookup"><span data-stu-id="71878-172">hello mode must be passed as a number: if you are using a combination of constant flags in your code, check hello total value.</span></span>

<span data-ttu-id="71878-173">Engagement alltid använder hello `engagement:key` booleskt nyckel i hello inställningsfilen för att hantera den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="71878-173">Engagement always uses hello `engagement:key` boolean key within hello preferences file for managing this setting.</span></span>

<span data-ttu-id="71878-174">Hej följande exempel på `AndroidManifest.xml` visar hello standardvärden:</span><span class="sxs-lookup"><span data-stu-id="71878-174">hello following example of `AndroidManifest.xml` shows hello default values:</span></span>

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

<span data-ttu-id="71878-175">Du kan lägga till en `CheckBoxPreference` i layouten inställningar som hello efter:</span><span class="sxs-lookup"><span data-stu-id="71878-175">Then you can add a `CheckBoxPreference` in your preference layout like hello following one:</span></span>

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
