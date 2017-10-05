---
title: "Avancerad konfiguration för Azure Mobile Engagement Android SDK"
description: Beskriver de avancerade konfigurationsalternativ inklusive Android Manifest med Azure Mobile Engagement Android SDK
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
ms.openlocfilehash: 0301f71c76872714aa1bf727a6c21dd7a63db036
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="b08fc-103">Avancerad konfiguration för Azure Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="b08fc-103">Advanced configuration for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b08fc-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="b08fc-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="b08fc-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="b08fc-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="b08fc-106">iOS</span><span class="sxs-lookup"><span data-stu-id="b08fc-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="b08fc-107">Android</span><span class="sxs-lookup"><span data-stu-id="b08fc-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
>
>

<span data-ttu-id="b08fc-108">Den här proceduren beskriver hur du konfigurerar olika konfigurationsalternativ för Azure Mobile Engagement Android-appar.</span><span class="sxs-lookup"><span data-stu-id="b08fc-108">This procedure describes how to configure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b08fc-109">Krav</span><span class="sxs-lookup"><span data-stu-id="b08fc-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a><span data-ttu-id="b08fc-110">Behörighet som krävs</span><span class="sxs-lookup"><span data-stu-id="b08fc-110">Permission Requirements</span></span>
<span data-ttu-id="b08fc-111">Vissa alternativ kräver särskilda behörigheter, som anges här referens- och rad i den specifika funktionen.</span><span class="sxs-lookup"><span data-stu-id="b08fc-111">Some options require specific permissions, all of which are listed here for reference, and in-line in the specific feature.</span></span> <span data-ttu-id="b08fc-112">Lägg till följande behörigheter AndroidManifest.xml i ditt projekt omedelbart före eller efter den `<application>` tagg.</span><span class="sxs-lookup"><span data-stu-id="b08fc-112">Add these permissions to the AndroidManifest.xml of your project immediately before or after the `<application>` tag.</span></span>

<span data-ttu-id="b08fc-113">Behörighet-koden måste ska se ut som följande, där du fyller i behörighet från tabellen som följer.</span><span class="sxs-lookup"><span data-stu-id="b08fc-113">The permission code needs to look like the following, where you fill in the appropriate permission from the table that follows.</span></span>

    <uses-permission android:name="android.permission.[specific permission]"/>


| <span data-ttu-id="b08fc-114">Behörighet</span><span class="sxs-lookup"><span data-stu-id="b08fc-114">Permission</span></span> | <span data-ttu-id="b08fc-115">När den används</span><span class="sxs-lookup"><span data-stu-id="b08fc-115">When used</span></span> |
| --- | --- |
| <span data-ttu-id="b08fc-116">INTERNET</span><span class="sxs-lookup"><span data-stu-id="b08fc-116">INTERNET</span></span> |<span data-ttu-id="b08fc-117">Krävs.</span><span class="sxs-lookup"><span data-stu-id="b08fc-117">Required.</span></span> <span data-ttu-id="b08fc-118">För grundläggande rapportering</span><span class="sxs-lookup"><span data-stu-id="b08fc-118">For basic reporting</span></span> |
| <span data-ttu-id="b08fc-119">ACCESS_NETWORK_STATE</span><span class="sxs-lookup"><span data-stu-id="b08fc-119">ACCESS_NETWORK_STATE</span></span> |<span data-ttu-id="b08fc-120">Krävs.</span><span class="sxs-lookup"><span data-stu-id="b08fc-120">Required.</span></span> <span data-ttu-id="b08fc-121">För grundläggande rapportering</span><span class="sxs-lookup"><span data-stu-id="b08fc-121">For basic reporting</span></span> |
| <span data-ttu-id="b08fc-122">RECEIVE_BOOT_COMPLETED</span><span class="sxs-lookup"><span data-stu-id="b08fc-122">RECEIVE_BOOT_COMPLETED</span></span> |<span data-ttu-id="b08fc-123">Krävs.</span><span class="sxs-lookup"><span data-stu-id="b08fc-123">Required.</span></span> <span data-ttu-id="b08fc-124">Meddelanden center visas efter omstart av enheten</span><span class="sxs-lookup"><span data-stu-id="b08fc-124">To show up the notifications center after device reboot</span></span> |
| <span data-ttu-id="b08fc-125">WAKE_LOCK</span><span class="sxs-lookup"><span data-stu-id="b08fc-125">WAKE_LOCK</span></span> |<span data-ttu-id="b08fc-126">Rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="b08fc-126">Recommended.</span></span> <span data-ttu-id="b08fc-127">Gör det möjligt att samla in data när du använder Wi-Fi eller när skärmen är inaktiverat</span><span class="sxs-lookup"><span data-stu-id="b08fc-127">Enables collecting data when using WiFi or when screen is off</span></span> |
| <span data-ttu-id="b08fc-128">VIBRERAR</span><span class="sxs-lookup"><span data-stu-id="b08fc-128">VIBRATE</span></span> |<span data-ttu-id="b08fc-129">Valfri.</span><span class="sxs-lookup"><span data-stu-id="b08fc-129">Optional.</span></span> <span data-ttu-id="b08fc-130">Aktiverar vibration när meddelanden tas emot</span><span class="sxs-lookup"><span data-stu-id="b08fc-130">Enables vibration when notifications are received</span></span> |
| <span data-ttu-id="b08fc-131">DOWNLOAD_WITHOUT_NOTIFICATION</span><span class="sxs-lookup"><span data-stu-id="b08fc-131">DOWNLOAD_WITHOUT_NOTIFICATION</span></span> |<span data-ttu-id="b08fc-132">Valfri.</span><span class="sxs-lookup"><span data-stu-id="b08fc-132">Optional.</span></span> <span data-ttu-id="b08fc-133">Aktiverar Android helheten meddelanden</span><span class="sxs-lookup"><span data-stu-id="b08fc-133">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="b08fc-134">WRITE_EXTERNAL_STORAGE</span><span class="sxs-lookup"><span data-stu-id="b08fc-134">WRITE_EXTERNAL_STORAGE</span></span> |<span data-ttu-id="b08fc-135">Valfri.</span><span class="sxs-lookup"><span data-stu-id="b08fc-135">Optional.</span></span> <span data-ttu-id="b08fc-136">Aktiverar Android helheten meddelanden</span><span class="sxs-lookup"><span data-stu-id="b08fc-136">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="b08fc-137">ACCESS_COARSE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="b08fc-137">ACCESS_COARSE_LOCATION</span></span> |<span data-ttu-id="b08fc-138">Valfri.</span><span class="sxs-lookup"><span data-stu-id="b08fc-138">Optional.</span></span> <span data-ttu-id="b08fc-139">Aktiverar rapportering i realtid plats</span><span class="sxs-lookup"><span data-stu-id="b08fc-139">Enables Real-time location reporting</span></span> |
| <span data-ttu-id="b08fc-140">ACCESS_FINE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="b08fc-140">ACCESS_FINE_LOCATION</span></span> |<span data-ttu-id="b08fc-141">Valfri.</span><span class="sxs-lookup"><span data-stu-id="b08fc-141">Optional.</span></span> <span data-ttu-id="b08fc-142">Aktiverar GPS-baserad plats reporting</span><span class="sxs-lookup"><span data-stu-id="b08fc-142">Enables GPS-based location reporting</span></span> |

<span data-ttu-id="b08fc-143">Från och med Android M [vissa behörigheter hanteras vid körning](mobile-engagement-android-location-reporting.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="b08fc-143">Starting with Android M, [some permissions are managed at run time](mobile-engagement-android-location-reporting.md#android-m-permissions).</span></span>

<span data-ttu-id="b08fc-144">Om du redan använder ``ACCESS_FINE_LOCATION``, inte måste du också använda ``ACCESS_COARSE_LOCATION``.</span><span class="sxs-lookup"><span data-stu-id="b08fc-144">If you are already using ``ACCESS_FINE_LOCATION``, then you don't need to also use ``ACCESS_COARSE_LOCATION``.</span></span>

## <a name="android-manifest-configuration-options"></a><span data-ttu-id="b08fc-145">Android Manifest konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="b08fc-145">Android Manifest configuration options</span></span>
### <a name="crash-report"></a><span data-ttu-id="b08fc-146">Kraschrapport</span><span class="sxs-lookup"><span data-stu-id="b08fc-146">Crash report</span></span>
<span data-ttu-id="b08fc-147">Om du vill inaktivera kraschrapporter, lägger du till den här koden mellan den `<application>` och `</application>` taggar:</span><span class="sxs-lookup"><span data-stu-id="b08fc-147">To disable crash reports, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="b08fc-148">Burst tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="b08fc-148">Burst threshold</span></span>
<span data-ttu-id="b08fc-149">Som standard loggas Engagement service-rapporter i realtid.</span><span class="sxs-lookup"><span data-stu-id="b08fc-149">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="b08fc-150">Om din rapport programloggarna ofta varierar, är det bättre att buffra loggarna och rapportera dem på en gång i en grundläggande reguljära tid (kallas ”burst läge”).</span><span class="sxs-lookup"><span data-stu-id="b08fc-150">If your application report logs vary frequently, it is better to buffer the logs and to report them all at once on a regular time base (called "burst mode").</span></span> <span data-ttu-id="b08fc-151">Gör du genom att lägga till den här koden mellan den `<application>` och `</application>` taggar:</span><span class="sxs-lookup"><span data-stu-id="b08fc-151">To do so, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="b08fc-152">Burst läge något ökar för batterilivslängd men påverkar Engagement-Övervakare: alla sessioner och jobb varaktighet avrundas till burst tröskelvärdet (alltså sessioner och jobb som är kortare än tröskelvärdet burst inte kanske visas).</span><span class="sxs-lookup"><span data-stu-id="b08fc-152">Burst mode slightly increases the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration are rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="b08fc-153">Burst-tröskelvärde ska vara längre än 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="b08fc-153">Your burst threshold should be no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="b08fc-154">Tidsgräns för session</span><span class="sxs-lookup"><span data-stu-id="b08fc-154">Session timeout</span></span>
 <span data-ttu-id="b08fc-155">Du kan avsluta en aktivitet genom att trycka på **Start** eller **tillbaka** nyckel genom att ange telefonnummer som inaktiv eller genom att hoppa över i ett annat program.</span><span class="sxs-lookup"><span data-stu-id="b08fc-155">You can end an activity by pressing the **Home** or **Back** key, by setting the phone idle or by jumping into another application.</span></span> <span data-ttu-id="b08fc-156">Som standard avslutas session tio sekunder efter den senaste aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="b08fc-156">By default, a session is ended ten seconds after the end of its last activity.</span></span> <span data-ttu-id="b08fc-157">Detta förhindrar en session delning varje gång användaren avslutar och återgår till programmet snabbt, vilket kan hända när användaren tar upp en bild, kontrollerar en avisering, osv. Du kanske vill ändra den här parametern.</span><span class="sxs-lookup"><span data-stu-id="b08fc-157">This avoids a session split each time the user exits and returns to the application quickly, which can happen when the user picks up an image, checks a notification, etc. You may want to modify this parameter.</span></span> <span data-ttu-id="b08fc-158">Gör du genom att lägga till den här koden mellan den `<application>` och `</application>` taggar:</span><span class="sxs-lookup"><span data-stu-id="b08fc-158">To do so, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="b08fc-159">Inaktivera rapportering av logg</span><span class="sxs-lookup"><span data-stu-id="b08fc-159">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="b08fc-160">Med hjälp av ett metodanrop</span><span class="sxs-lookup"><span data-stu-id="b08fc-160">Using a method call</span></span>
<span data-ttu-id="b08fc-161">Om du vill Engagement slutar skicka loggar, kan du anropa:</span><span class="sxs-lookup"><span data-stu-id="b08fc-161">If you want Engagement to stop sending logs, you can call:</span></span>

    EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="b08fc-162">Det här anropet är beständiga: filen delade inställningar används.</span><span class="sxs-lookup"><span data-stu-id="b08fc-162">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="b08fc-163">Det kan ta en minut att stoppa tjänsten om Engagement är aktiv när du anropar den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b08fc-163">If Engagement is active when you call this function, it may take one minute for the service to stop.</span></span> <span data-ttu-id="b08fc-164">Men det inte kommer starta tjänsten alls nästa gång startar du programmet.</span><span class="sxs-lookup"><span data-stu-id="b08fc-164">However it won't launch the service at all the next time you launch the application.</span></span>

<span data-ttu-id="b08fc-165">Du kan aktivera loggen reporting igen genom att anropa funktionen samma med `true`.</span><span class="sxs-lookup"><span data-stu-id="b08fc-165">You can enable log reporting again by calling the same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="b08fc-166">Integrering i din egen`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="b08fc-166">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="b08fc-167">I stället för att anropa den här funktionen kan du också integrera inställningen direkt i din befintliga `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="b08fc-167">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="b08fc-168">Du kan konfigurera Engagement att använda inställningar-fil (med läge) i den `AndroidManifest.xml` filen med `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="b08fc-168">You can configure Engagement to use your preferences file (with the desired mode) in the `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="b08fc-169">Den `engagement:agent:settings:name` nyckel används för att ange namnet på filen med delade inställningar.</span><span class="sxs-lookup"><span data-stu-id="b08fc-169">The `engagement:agent:settings:name` key is used to define the name of the shared preferences file.</span></span>
* <span data-ttu-id="b08fc-170">Den `engagement:agent:settings:mode` nyckel används för att definiera läget för filen delade inställningar.</span><span class="sxs-lookup"><span data-stu-id="b08fc-170">The `engagement:agent:settings:mode` key is used to define the mode of the shared preferences file.</span></span> <span data-ttu-id="b08fc-171">Använd samma läge som i din `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="b08fc-171">Use the same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="b08fc-172">Läget måste skickas som ett tal: Om du använder en kombination av konstant flaggor i koden, kontrollera det totala värdet.</span><span class="sxs-lookup"><span data-stu-id="b08fc-172">The mode must be passed as a number: if you are using a combination of constant flags in your code, check the total value.</span></span>

<span data-ttu-id="b08fc-173">Engagement alltid använder den `engagement:key` booleskt nyckel i filen med inställningar för att hantera den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="b08fc-173">Engagement always uses the `engagement:key` boolean key within the preferences file for managing this setting.</span></span>

<span data-ttu-id="b08fc-174">Följande exempel visar `AndroidManifest.xml` visar standardvärden:</span><span class="sxs-lookup"><span data-stu-id="b08fc-174">The following example of `AndroidManifest.xml` shows the default values:</span></span>

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

<span data-ttu-id="b08fc-175">Du kan lägga till en `CheckBoxPreference` i layouten inställningar som den följande:</span><span class="sxs-lookup"><span data-stu-id="b08fc-175">Then you can add a `CheckBoxPreference` in your preference layout like the following one:</span></span>

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
