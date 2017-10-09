---
title: aaaAzure Mobile Engagement Troubleshooting Guide - SDK
description: "Felsökning av problem med SDK integration i Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: de265cf1-2f88-43ef-8616-156ada5be7b6
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1c082b81d898f4bdb47b8efe6cfbacfd83fe9279
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a><span data-ttu-id="cc8af-103">Felsökningsguide för SDK-integrationsproblem</span><span class="sxs-lookup"><span data-stu-id="cc8af-103">Troubleshooting guide for SDK integration issues</span></span>
<span data-ttu-id="cc8af-104">hello följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement ska integreras i ditt program.</span><span class="sxs-lookup"><span data-stu-id="cc8af-104">hello following are possible issues you may encounter with how Azure Mobile Engagement integrates into your application.</span></span>

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a><span data-ttu-id="cc8af-105">SDK-problem som identifieras av ett fel i en annan del av ditt program</span><span class="sxs-lookup"><span data-stu-id="cc8af-105">SDK issues discovered by a failure in another area of your application</span></span>
### <a name="issue"></a><span data-ttu-id="cc8af-106">Problem</span><span class="sxs-lookup"><span data-stu-id="cc8af-106">Issue</span></span>
* <span data-ttu-id="cc8af-107">Användargränssnittet data collection-fel (Analytics, övervakning, segmentering eller instrumentpaneler).</span><span class="sxs-lookup"><span data-stu-id="cc8af-107">UI data collection failure (in Analytics, Monitoring, Segmentation, or Dashboards).</span></span>
* <span data-ttu-id="cc8af-108">Push-fel (push-meddelanden fungerar inte i appen, utanför appen eller båda).</span><span class="sxs-lookup"><span data-stu-id="cc8af-108">Push Failures (Pushes don't work in app, out of app, or both).</span></span>
* <span data-ttu-id="cc8af-109">Avancerade funktionen fel (spårnings, Geolokalisering eller plattform specifika push-meddelanden inte fungerar).</span><span class="sxs-lookup"><span data-stu-id="cc8af-109">Advanced Feature Failures (Tracking, Geolocation, or platform specific Pushes don’t work).</span></span>
* <span data-ttu-id="cc8af-110">API-fel (API: er misslyckas ofta tyst utan felmeddelanden).</span><span class="sxs-lookup"><span data-stu-id="cc8af-110">API Failures (APIs fail often silently without error messages).</span></span>
* <span data-ttu-id="cc8af-111">Tjänstrelaterade (ingen av Azure Mobile Engagement fungerar för tillämpningsprogrammet).</span><span class="sxs-lookup"><span data-stu-id="cc8af-111">Service Failures (none of Azure Mobile Engagement works for your application).</span></span>

### <a name="causes"></a><span data-ttu-id="cc8af-112">Orsaker</span><span class="sxs-lookup"><span data-stu-id="cc8af-112">Causes</span></span>
* <span data-ttu-id="cc8af-113">De flesta problem som måste matchas med hello Azure Mobile Engagement SDK toobe identifieras av ett fel i ditt program (till exempel ett UI data collection fel, push-fel, avancerad funktion fel, API-fel, programkrascher eller tydligt service avbrott).</span><span class="sxs-lookup"><span data-stu-id="cc8af-113">Most issues that need toobe resolved with hello Azure Mobile Engagement SDK will be discovered by a failure in your application (such as a UI data collection failure, push failure, advanced feature failure, API failure, Application crashes, or apparent service outage).</span></span>  
* <span data-ttu-id="cc8af-114">Om en viss funktion i Azure Mobile Engagement har aldrig fungerat appen innan, behöver toocomplete hello-integration.</span><span class="sxs-lookup"><span data-stu-id="cc8af-114">If a particular feature of Azure Mobile Engagement has never worked in your app before, you will need toocomplete hello integration.</span></span> 
* <span data-ttu-id="cc8af-115">Om en viss funktion i Azure Mobile Engagement fungerade och stoppas kommer behöva du tooupgrade toohello senaste versionen med hello Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="cc8af-115">If a particular feature of Azure Mobile Engagement was working and stopped, you may need tooupgrade toohello last version with hello Azure Mobile Engagement SDK.</span></span> <span data-ttu-id="cc8af-116">Kom ihåg att det finns en annan version av hello Azure Mobile Engagement SDK för varje plattform som stöds av Azure Mobile Engagement (Android, iOS, Windows och Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="cc8af-116">Remember that there is a different version of hello Azure Mobile Engagement SDK for each platform supported by Azure Mobile Engagement (Android, iOS, Windows, and Windows Phone).</span></span>

#### <a name="sdk-integration"></a><span data-ttu-id="cc8af-117">SDK-integrering</span><span class="sxs-lookup"><span data-stu-id="cc8af-117">SDK Integration</span></span>
* <span data-ttu-id="cc8af-118">Azure Mobile Engagement integrerad inte korrekt i SDK (Analytics).</span><span class="sxs-lookup"><span data-stu-id="cc8af-118">Azure Mobile Engagement not correctly integrated in SDK (Analytics).</span></span>
* <span data-ttu-id="cc8af-119">Nå inte korrekt integrerad i SDK: N (i appen och ut från appen push-meddelanden).</span><span class="sxs-lookup"><span data-stu-id="cc8af-119">Reach not correctly integrated in SDK (In App and Out of App Pushes).</span></span>
* <span data-ttu-id="cc8af-120">Certifikatet har upphört att gälla eller är felaktig PROD vs. DEV (endast iOS).</span><span class="sxs-lookup"><span data-stu-id="cc8af-120">Certificate expired or incorrect PROD vs. DEV (iOS only).</span></span>
* <span data-ttu-id="cc8af-121">GCM eller ADM inte korrekt integrerat i SDK (Android endast - tjänsten specifika push-meddelanden).</span><span class="sxs-lookup"><span data-stu-id="cc8af-121">GCM or ADM not correctly integrated in SDK (Android only - Service Specific Pushes).</span></span>
* <span data-ttu-id="cc8af-122">Integrerad spårning inte korrekt i SDK (installera store spårning).</span><span class="sxs-lookup"><span data-stu-id="cc8af-122">Tracking not correctly integrated in SDK (Install store tracking).</span></span>
* <span data-ttu-id="cc8af-123">Lazy plats eller GPS-plats integreras inte korrekt i SDK (målinriktning av geografiska plats).</span><span class="sxs-lookup"><span data-stu-id="cc8af-123">Lazy Location or GPS Location not correctly integrated in SDK (Targeting by geo-location).</span></span>

<span data-ttu-id="cc8af-124">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="cc8af-124">**See also:**</span></span>

* <span data-ttu-id="cc8af-125">[Dokumentationen för SDK - Integration guider][Link 5]</span><span class="sxs-lookup"><span data-stu-id="cc8af-125">[SDK Documentation - Integration Guides][Link 5]</span></span> 
* <span data-ttu-id="cc8af-126">[Felsökningsguide för - Push][Link 23]</span><span class="sxs-lookup"><span data-stu-id="cc8af-126">[Troubleshooting Guide - Push][Link 23]</span></span>

#### <a name="sdk-upgrade"></a><span data-ttu-id="cc8af-127">SDK-uppgradering</span><span class="sxs-lookup"><span data-stu-id="cc8af-127">SDK Upgrade</span></span>
* <span data-ttu-id="cc8af-128">Måste tooupgrade SDK tooresolve problem med äldre versioner av hello SDK (ofta relaterade toonewer versioner av hello enhetens operativsystem).</span><span class="sxs-lookup"><span data-stu-id="cc8af-128">Need tooupgrade SDK tooresolve issues with older versions of hello SDK (often related toonewer versions of hello device OS).</span></span>
* <span data-ttu-id="cc8af-129">Avinstallera alla tidigare versioner av appen från din enhet och installera om hello senaste versionen av appen, hello registrera ditt enhets-ID från hello Azure Mobile Engagement UI tooconfirm att enheten använder hello senaste versionen av din app.</span><span class="sxs-lookup"><span data-stu-id="cc8af-129">Uninstall all previous versions of your app from your device and reinstall hello newest version of your app, hello re-register your Device ID from hello Azure Mobile Engagement UI tooconfirm that your device is using hello newest version of your app.</span></span>

<span data-ttu-id="cc8af-130">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="cc8af-130">**See also:**</span></span>

* [<span data-ttu-id="cc8af-131">SDK-dokumentation – viktig information</span><span class="sxs-lookup"><span data-stu-id="cc8af-131">SDK Documentation - Release Notes</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [<span data-ttu-id="cc8af-132">SDK-dokumentation – uppgradera guider</span><span class="sxs-lookup"><span data-stu-id="cc8af-132">SDK Documentation - Upgrade Guides</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a><span data-ttu-id="cc8af-133">Andra SDK</span><span class="sxs-lookup"><span data-stu-id="cc8af-133">SDK Other</span></span>
* <span data-ttu-id="cc8af-134">Fel i programmets Manifest ”AndroidManifest.xml” kan orsaka Azure Mobile Engagement inte toowork (endast Android).</span><span class="sxs-lookup"><span data-stu-id="cc8af-134">Errors in Application Manifest "AndroidManifest.xml" can cause Azure Mobile Engagement not toowork (Android only).</span></span>
* <span data-ttu-id="cc8af-135">Ett vanligt problem med SDK-integration och API-användning är tooconfuse hello SDK-nyckeln och hello API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="cc8af-135">A common issue with SDK integration and API usage is tooconfuse hello SDK Key and hello API Key.</span></span>

<span data-ttu-id="cc8af-136">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="cc8af-136">**See also:**</span></span>

* <span data-ttu-id="cc8af-137">[Begrepp - ordlista][Link 6]</span><span class="sxs-lookup"><span data-stu-id="cc8af-137">[Concepts - Glossary][Link 6]</span></span>

## <a name="advanced-coding-issues"></a><span data-ttu-id="cc8af-138">Avancerade kodning problem</span><span class="sxs-lookup"><span data-stu-id="cc8af-138">Advanced coding issues</span></span>
### <a name="issue"></a><span data-ttu-id="cc8af-139">Problem</span><span class="sxs-lookup"><span data-stu-id="cc8af-139">Issue</span></span>
* <span data-ttu-id="cc8af-140">Specifika plattformskod relaterat inte direkt tooAzure Mobile Engagement kan orsaka problem på iOS, Android och Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="cc8af-140">Platform specific code not directly related tooAzure Mobile Engagement can cause issues on iOS, Android, and Windows Phone.</span></span>

### <a name="causes"></a><span data-ttu-id="cc8af-141">Orsaker</span><span class="sxs-lookup"><span data-stu-id="cc8af-141">Causes</span></span>
* <span data-ttu-id="cc8af-142">Många avancerade kodning problem med Azure Mobile Engagement på grund av felaktigt skrivet plattformen specifik kod inte är direkt relaterat tooAzure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="cc8af-142">Many advanced coding issues with Azure Mobile Engagement are caused by improperly written platform specific code not directly related tooAzure Mobile Engagement.</span></span> <span data-ttu-id="cc8af-143">Du behöver tooconsult dokumentationen specifika toohello plattform som du utvecklar för dessutom tooAzure Mobile Engagement dokumentationen (Android, iOS, webbprogram, Windows och Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="cc8af-143">You will need tooconsult documentation specific toohello platform you are developing for in addition tooAzure Mobile Engagement documentation (Android, iOS, Web, Windows, and Windows Phone).</span></span>
* <span data-ttu-id="cc8af-144">Konfigurera inte korrekt ”kategorier”, förhindrar länka från en avisering tooanother plats i eller utanför hello app (endast Android).</span><span class="sxs-lookup"><span data-stu-id="cc8af-144">Not correctly configuring "categories", prevents linking from a notification tooanother location either inside or outside of hello app (Android only).</span></span> 
* <span data-ttu-id="cc8af-145">Inte inställningen ”UIKit.framework” för ”valfritt” i din iOS-kod, visas en ”det gick inte att hitta symbolen” och/eller kraschar på äldre iOS-enheter (endast iOS).</span><span class="sxs-lookup"><span data-stu-id="cc8af-145">Not setting "UIKit.framework" too"optional" in your iOS code, shows a "Symbol not found error" and/or crashes on older iOS devices (iOS only).</span></span>
* <span data-ttu-id="cc8af-146">Utgångna certifikat eller inte korrekt använder hello DEV eller produktprenumeration version av hello certifikat, orsaker push utfärdar (endast iOS).</span><span class="sxs-lookup"><span data-stu-id="cc8af-146">Expired certificates or not correctly using hello DEV or Prod version of hello cert, causes push issues (iOS only).</span></span>
* <span data-ttu-id="cc8af-147">Det finns vissa begränsningar inbyggd tooa plattform som Azure Mobile Engagement inte kan styra (t.ex. hur hello systemcenter fungerar för utanför appen push-meddelanden i Android och iOS).</span><span class="sxs-lookup"><span data-stu-id="cc8af-147">There are some limitations inherent tooa platform that Azure Mobile Engagement can't control (like how hello system center works for out of app pushes in Android and iOS).</span></span>
* <span data-ttu-id="cc8af-148">Azure Mobile Engagement publicerar en fullständig lista över hello interna paket som används av Azure Mobile Engagement för iOS och Android som referens.</span><span class="sxs-lookup"><span data-stu-id="cc8af-148">Azure Mobile Engagement publishes a full list of hello internal packages used by Azure Mobile Engagement for iOS and Android for reference.</span></span> <span data-ttu-id="cc8af-149">Tänk på att vissa funktioner i Azure Mobile Engagement är särskilda toohello plattform (Android, iOS, webbprogram, Windows och Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="cc8af-149">Keep in mind that some features of Azure Mobile Engagement are specific toohello platform (Android, iOS, Web, Windows, and Windows Phone).</span></span>

### <a name="see-also"></a><span data-ttu-id="cc8af-150">Se även</span><span class="sxs-lookup"><span data-stu-id="cc8af-150">See also</span></span>
* <span data-ttu-id="cc8af-151">[Felsökningsguide för - Push][Link 23]</span><span class="sxs-lookup"><span data-stu-id="cc8af-151">[Troubleshooting Guide - Push][Link 23]</span></span> 
* <span data-ttu-id="cc8af-152">[SDK-dokumentation – viktig information][Link 5]</span><span class="sxs-lookup"><span data-stu-id="cc8af-152">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="cc8af-153">[SDK-dokumentation – uppgradera guider][Link 5]</span><span class="sxs-lookup"><span data-stu-id="cc8af-153">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="application-crashes"></a><span data-ttu-id="cc8af-154">Programkrascher</span><span class="sxs-lookup"><span data-stu-id="cc8af-154">Application crashes</span></span>
### <a name="issue"></a><span data-ttu-id="cc8af-155">Problem</span><span class="sxs-lookup"><span data-stu-id="cc8af-155">Issue</span></span>
* <span data-ttu-id="cc8af-156">Programmet kraschar på hello slutanvändarens enhet.</span><span class="sxs-lookup"><span data-stu-id="cc8af-156">Your application crashes on hello end users' device.</span></span>

### <a name="causes"></a><span data-ttu-id="cc8af-157">Orsaker</span><span class="sxs-lookup"><span data-stu-id="cc8af-157">Causes</span></span>
* <span data-ttu-id="cc8af-158">Information om kraschen uppges kan visas i hello *Analytics UI* eller hello *Analytics API*</span><span class="sxs-lookup"><span data-stu-id="cc8af-158">Crash information can be viewed in hello *Analytics UI* or hello *Analytics API*</span></span>
* <span data-ttu-id="cc8af-159">Du kan hitta hello enhets-ID för enheten test och vidta hello samma åtgärd som har orsakat ditt program toocrash för en slutanvändare toohelp identifiera hello orsaken till din krascher.</span><span class="sxs-lookup"><span data-stu-id="cc8af-159">You can find hello Device ID of your test device and take hello same action that caused your application toocrash for an end user toohelp identify hello cause of your crash.</span></span>
* <span data-ttu-id="cc8af-160">Kända problem med hello Azure Mobile Engagement SDK som orsakar program toocrash matchas ibland genom att uppgradera toohello senaste versionen av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="cc8af-160">Known issues with hello Azure Mobile Engagement SDK that cause applications toocrash are sometimes resolved by upgrading toohello latest version of hello SDK.</span></span> <span data-ttu-id="cc8af-161">Se till att toocheck hello viktig information om din plattform när du undersöker kraschar.</span><span class="sxs-lookup"><span data-stu-id="cc8af-161">Make sure toocheck hello release notes about your platform when investigating crashes.</span></span>

### <a name="see-also"></a><span data-ttu-id="cc8af-162">Se även</span><span class="sxs-lookup"><span data-stu-id="cc8af-162">See also</span></span>
* <span data-ttu-id="cc8af-163">[SDK-dokumentation – viktig information][Link 5]</span><span class="sxs-lookup"><span data-stu-id="cc8af-163">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="cc8af-164">[SDK-dokumentation – uppgradera guider][Link 5]</span><span class="sxs-lookup"><span data-stu-id="cc8af-164">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="app-store-upload-failures"></a><span data-ttu-id="cc8af-165">App store Överför fel</span><span class="sxs-lookup"><span data-stu-id="cc8af-165">App store upload failures</span></span>
### <a name="issue"></a><span data-ttu-id="cc8af-166">Problem</span><span class="sxs-lookup"><span data-stu-id="cc8af-166">Issue</span></span>
* <span data-ttu-id="cc8af-167">Fel relaterade toouploading hello senaste versionen av din app tooApple, Google, eller hello App för Windows store.</span><span class="sxs-lookup"><span data-stu-id="cc8af-167">Errors related toouploading hello latest version of your app tooApple, Google, or hello Windows App store.</span></span>

### <a name="causes"></a><span data-ttu-id="cc8af-168">Orsaker</span><span class="sxs-lookup"><span data-stu-id="cc8af-168">Causes</span></span>
* <span data-ttu-id="cc8af-169">Appen lagrar ibland blockera appar med vissa funktioner som aktiveras (t.ex. hello Apple Store förhindrar hello användningen av IDFV i appar i store hello och hello GooglePlay store förhindrar hello delning av programinformationen mellan appar).</span><span class="sxs-lookup"><span data-stu-id="cc8af-169">App stores sometimes block apps with certain features enabled (e.g. hello Apple Store prevents hello use of IDFV in apps in hello store and hello GooglePlay store prevents hello sharing of application information between apps).</span></span> 
* <span data-ttu-id="cc8af-170">Kontrollera att du kontrollerar hello viktig information om din plattform och den aktuella versionen av hello SDK om du har svårt att ladda upp en toohello appbutik.</span><span class="sxs-lookup"><span data-stu-id="cc8af-170">Make sure that you check hello release notes about your platform and current version of hello SDK if you have difficulty uploading an app toohello store.</span></span>

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

