---
title: "Azure Mobile Engagement felsökningsguide för - SDK"
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
ms.openlocfilehash: 4d9d6165deb4bd0c65f1841aa7c457363a1f2865
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a><span data-ttu-id="b0fb6-103">Felsökningsguide för SDK-integrationsproblem</span><span class="sxs-lookup"><span data-stu-id="b0fb6-103">Troubleshooting guide for SDK integration issues</span></span>
<span data-ttu-id="b0fb6-104">Följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement ska integreras i ditt program.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-104">The following are possible issues you may encounter with how Azure Mobile Engagement integrates into your application.</span></span>

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a><span data-ttu-id="b0fb6-105">SDK-problem som identifieras av ett fel i en annan del av ditt program</span><span class="sxs-lookup"><span data-stu-id="b0fb6-105">SDK issues discovered by a failure in another area of your application</span></span>
### <a name="issue"></a><span data-ttu-id="b0fb6-106">Problem</span><span class="sxs-lookup"><span data-stu-id="b0fb6-106">Issue</span></span>
* <span data-ttu-id="b0fb6-107">Användargränssnittet data collection-fel (Analytics, övervakning, segmentering eller instrumentpaneler).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-107">UI data collection failure (in Analytics, Monitoring, Segmentation, or Dashboards).</span></span>
* <span data-ttu-id="b0fb6-108">Push-fel (push-meddelanden fungerar inte i appen, utanför appen eller båda).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-108">Push Failures (Pushes don't work in app, out of app, or both).</span></span>
* <span data-ttu-id="b0fb6-109">Avancerade funktionen fel (spårnings, Geolokalisering eller plattform specifika push-meddelanden inte fungerar).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-109">Advanced Feature Failures (Tracking, Geolocation, or platform specific Pushes don’t work).</span></span>
* <span data-ttu-id="b0fb6-110">API-fel (API: er misslyckas ofta tyst utan felmeddelanden).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-110">API Failures (APIs fail often silently without error messages).</span></span>
* <span data-ttu-id="b0fb6-111">Tjänstrelaterade (ingen av Azure Mobile Engagement fungerar för tillämpningsprogrammet).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-111">Service Failures (none of Azure Mobile Engagement works for your application).</span></span>

### <a name="causes"></a><span data-ttu-id="b0fb6-112">Orsaker</span><span class="sxs-lookup"><span data-stu-id="b0fb6-112">Causes</span></span>
* <span data-ttu-id="b0fb6-113">De flesta problem som behöver lösas med Azure Mobile Engagement SDK identifieras av ett fel i ditt program (till exempel ett UI data collection fel push-fel, avancerad funktion fel, API-fel, programkrascher eller tydligt avbrott).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-113">Most issues that need to be resolved with the Azure Mobile Engagement SDK will be discovered by a failure in your application (such as a UI data collection failure, push failure, advanced feature failure, API failure, Application crashes, or apparent service outage).</span></span>  
* <span data-ttu-id="b0fb6-114">Om en viss funktion i Azure Mobile Engagement har aldrig fungerat appen innan, behöver du slutföra integrationen.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-114">If a particular feature of Azure Mobile Engagement has never worked in your app before, you will need to complete the integration.</span></span> 
* <span data-ttu-id="b0fb6-115">Om en viss funktion i Azure Mobile Engagement fungerade och stoppas, kan du behöva uppgradera till den senaste versionen med Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-115">If a particular feature of Azure Mobile Engagement was working and stopped, you may need to upgrade to the last version with the Azure Mobile Engagement SDK.</span></span> <span data-ttu-id="b0fb6-116">Kom ihåg att det finns en annan version av Azure Mobile Engagement SDK för varje plattform som stöds av Azure Mobile Engagement (Android, iOS, Windows och Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-116">Remember that there is a different version of the Azure Mobile Engagement SDK for each platform supported by Azure Mobile Engagement (Android, iOS, Windows, and Windows Phone).</span></span>

#### <a name="sdk-integration"></a><span data-ttu-id="b0fb6-117">SDK-integrering</span><span class="sxs-lookup"><span data-stu-id="b0fb6-117">SDK Integration</span></span>
* <span data-ttu-id="b0fb6-118">Azure Mobile Engagement integrerad inte korrekt i SDK (Analytics).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-118">Azure Mobile Engagement not correctly integrated in SDK (Analytics).</span></span>
* <span data-ttu-id="b0fb6-119">Nå inte korrekt integrerad i SDK: N (i appen och ut från appen push-meddelanden).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-119">Reach not correctly integrated in SDK (In App and Out of App Pushes).</span></span>
* <span data-ttu-id="b0fb6-120">Certifikatet har upphört att gälla eller är felaktig PROD vs. DEV (endast iOS).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-120">Certificate expired or incorrect PROD vs. DEV (iOS only).</span></span>
* <span data-ttu-id="b0fb6-121">GCM eller ADM inte korrekt integrerat i SDK (Android endast - tjänsten specifika push-meddelanden).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-121">GCM or ADM not correctly integrated in SDK (Android only - Service Specific Pushes).</span></span>
* <span data-ttu-id="b0fb6-122">Integrerad spårning inte korrekt i SDK (installera store spårning).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-122">Tracking not correctly integrated in SDK (Install store tracking).</span></span>
* <span data-ttu-id="b0fb6-123">Lazy plats eller GPS-plats integreras inte korrekt i SDK (målinriktning av geografiska plats).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-123">Lazy Location or GPS Location not correctly integrated in SDK (Targeting by geo-location).</span></span>

<span data-ttu-id="b0fb6-124">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="b0fb6-124">**See also:**</span></span>

* <span data-ttu-id="b0fb6-125">[Dokumentationen för SDK - Integration guider][Link 5]</span><span class="sxs-lookup"><span data-stu-id="b0fb6-125">[SDK Documentation - Integration Guides][Link 5]</span></span> 
* <span data-ttu-id="b0fb6-126">[Felsökningsguide för - Push][Link 23]</span><span class="sxs-lookup"><span data-stu-id="b0fb6-126">[Troubleshooting Guide - Push][Link 23]</span></span>

#### <a name="sdk-upgrade"></a><span data-ttu-id="b0fb6-127">SDK-uppgradering</span><span class="sxs-lookup"><span data-stu-id="b0fb6-127">SDK Upgrade</span></span>
* <span data-ttu-id="b0fb6-128">Om du behöver uppgradera SDK för att lösa problem med äldre versioner av SDK (ofta relaterade till senare versioner av enhetens operativsystem).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-128">Need to upgrade SDK to resolve issues with older versions of the SDK (often related to newer versions of the device OS).</span></span>
* <span data-ttu-id="b0fb6-129">Avinstallera alla tidigare versioner av appen från din enhet och installera om den senaste versionen av din app på registrera ditt enhets-ID från Azure Mobile Engagement Gränssnittet för att bekräfta att enheten använder den senaste versionen av din app.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-129">Uninstall all previous versions of your app from your device and reinstall the newest version of your app, the re-register your Device ID from the Azure Mobile Engagement UI to confirm that your device is using the newest version of your app.</span></span>

<span data-ttu-id="b0fb6-130">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="b0fb6-130">**See also:**</span></span>

* [<span data-ttu-id="b0fb6-131">SDK-dokumentation – viktig information</span><span class="sxs-lookup"><span data-stu-id="b0fb6-131">SDK Documentation - Release Notes</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [<span data-ttu-id="b0fb6-132">SDK-dokumentation – uppgradera guider</span><span class="sxs-lookup"><span data-stu-id="b0fb6-132">SDK Documentation - Upgrade Guides</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a><span data-ttu-id="b0fb6-133">Andra SDK</span><span class="sxs-lookup"><span data-stu-id="b0fb6-133">SDK Other</span></span>
* <span data-ttu-id="b0fb6-134">Fel i programmets Manifest ”AndroidManifest.xml” kan orsaka Azure Mobile Engagement inte ska fungera (endast Android).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-134">Errors in Application Manifest "AndroidManifest.xml" can cause Azure Mobile Engagement not to work (Android only).</span></span>
* <span data-ttu-id="b0fb6-135">Ett vanligt problem med SDK-integration och API-användning är försvåra SDK-nyckeln och API-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-135">A common issue with SDK integration and API usage is to confuse the SDK Key and the API Key.</span></span>

<span data-ttu-id="b0fb6-136">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="b0fb6-136">**See also:**</span></span>

* <span data-ttu-id="b0fb6-137">[Begrepp - ordlista][Link 6]</span><span class="sxs-lookup"><span data-stu-id="b0fb6-137">[Concepts - Glossary][Link 6]</span></span>

## <a name="advanced-coding-issues"></a><span data-ttu-id="b0fb6-138">Avancerade kodning problem</span><span class="sxs-lookup"><span data-stu-id="b0fb6-138">Advanced coding issues</span></span>
### <a name="issue"></a><span data-ttu-id="b0fb6-139">Problem</span><span class="sxs-lookup"><span data-stu-id="b0fb6-139">Issue</span></span>
* <span data-ttu-id="b0fb6-140">Specifika plattformskod inte direkt relaterar till Azure Mobile Engagement kan orsaka problem på iOS, Android och Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-140">Platform specific code not directly related to Azure Mobile Engagement can cause issues on iOS, Android, and Windows Phone.</span></span>

### <a name="causes"></a><span data-ttu-id="b0fb6-141">Orsaker</span><span class="sxs-lookup"><span data-stu-id="b0fb6-141">Causes</span></span>
* <span data-ttu-id="b0fb6-142">Många avancerade kodning problem med Azure Mobile Engagement orsakas av felaktigt skrivet specifika plattformskod inte direkt relaterar till Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-142">Many advanced coding issues with Azure Mobile Engagement are caused by improperly written platform specific code not directly related to Azure Mobile Engagement.</span></span> <span data-ttu-id="b0fb6-143">Behöver du dokumentationen som är specifika för den plattform som du utvecklar för utöver Azure Mobile Engagement-dokumentation (Android, iOS, webbprogram, Windows och Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-143">You will need to consult documentation specific to the platform you are developing for in addition to Azure Mobile Engagement documentation (Android, iOS, Web, Windows, and Windows Phone).</span></span>
* <span data-ttu-id="b0fb6-144">Konfigurera inte korrekt ”kategorier”, förhindrar länkar från ett meddelande till en annan plats i eller utanför appen (endast Android).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-144">Not correctly configuring "categories", prevents linking from a notification to another location either inside or outside of the app (Android only).</span></span> 
* <span data-ttu-id="b0fb6-145">Inte ställa in ”UIKit.framework” till ”valfritt” i din iOS-kod, visas en ”det gick inte att hitta symbolen” och/eller kraschar på äldre iOS-enheter (endast iOS).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-145">Not setting "UIKit.framework" to "optional" in your iOS code, shows a "Symbol not found error" and/or crashes on older iOS devices (iOS only).</span></span>
* <span data-ttu-id="b0fb6-146">Utgångna certifikat eller inte korrekt med utvecklings- eller produktprenumeration-versionen av certifikat, orsaker push utfärdar (endast iOS).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-146">Expired certificates or not correctly using the DEV or Prod version of the cert, causes push issues (iOS only).</span></span>
* <span data-ttu-id="b0fb6-147">Det finns vissa begränsningar som ingår i en plattform som Azure Mobile Engagement inte kan styra (t.ex. hur systemcenter fungerar för utanför appen push-meddelanden i Android och iOS).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-147">There are some limitations inherent to a platform that Azure Mobile Engagement can't control (like how the system center works for out of app pushes in Android and iOS).</span></span>
* <span data-ttu-id="b0fb6-148">Azure Mobile Engagement publicerar en fullständig lista över de interna paket som används av Azure Mobile Engagement för iOS och Android för referens.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-148">Azure Mobile Engagement publishes a full list of the internal packages used by Azure Mobile Engagement for iOS and Android for reference.</span></span> <span data-ttu-id="b0fb6-149">Tänk på att vissa funktioner i Azure Mobile Engagement är specifika för plattformen (Android, iOS, webbprogram, Windows och Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-149">Keep in mind that some features of Azure Mobile Engagement are specific to the platform (Android, iOS, Web, Windows, and Windows Phone).</span></span>

### <a name="see-also"></a><span data-ttu-id="b0fb6-150">Se även</span><span class="sxs-lookup"><span data-stu-id="b0fb6-150">See also</span></span>
* <span data-ttu-id="b0fb6-151">[Felsökningsguide för - Push][Link 23]</span><span class="sxs-lookup"><span data-stu-id="b0fb6-151">[Troubleshooting Guide - Push][Link 23]</span></span> 
* <span data-ttu-id="b0fb6-152">[SDK-dokumentation – viktig information][Link 5]</span><span class="sxs-lookup"><span data-stu-id="b0fb6-152">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="b0fb6-153">[SDK-dokumentation – uppgradera guider][Link 5]</span><span class="sxs-lookup"><span data-stu-id="b0fb6-153">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="application-crashes"></a><span data-ttu-id="b0fb6-154">Programkrascher</span><span class="sxs-lookup"><span data-stu-id="b0fb6-154">Application crashes</span></span>
### <a name="issue"></a><span data-ttu-id="b0fb6-155">Problem</span><span class="sxs-lookup"><span data-stu-id="b0fb6-155">Issue</span></span>
* <span data-ttu-id="b0fb6-156">Programmet kraschar på slutanvändarens enhet.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-156">Your application crashes on the end users' device.</span></span>

### <a name="causes"></a><span data-ttu-id="b0fb6-157">Orsaker</span><span class="sxs-lookup"><span data-stu-id="b0fb6-157">Causes</span></span>
* <span data-ttu-id="b0fb6-158">Information om kraschen uppges kan visas i den *Analytics UI* eller *Analytics API*</span><span class="sxs-lookup"><span data-stu-id="b0fb6-158">Crash information can be viewed in the *Analytics UI* or the *Analytics API*</span></span>
* <span data-ttu-id="b0fb6-159">Du kan hitta enhets-ID för enheten test och dra samma åtgärd som gjorde att programmet kraschar för en användare för att identifiera orsaken till din krascher.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-159">You can find the Device ID of your test device and take the same action that caused your application to crash for an end user to help identify the cause of your crash.</span></span>
* <span data-ttu-id="b0fb6-160">Kända problem med Azure Mobile Engagement SDK som medför att kraschar matchas ibland genom att uppgradera till den senaste versionen av SDK.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-160">Known issues with the Azure Mobile Engagement SDK that cause applications to crash are sometimes resolved by upgrading to the latest version of the SDK.</span></span> <span data-ttu-id="b0fb6-161">Se till att kontrollera viktig information om din plattform när du undersöker kraschar.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-161">Make sure to check the release notes about your platform when investigating crashes.</span></span>

### <a name="see-also"></a><span data-ttu-id="b0fb6-162">Se även</span><span class="sxs-lookup"><span data-stu-id="b0fb6-162">See also</span></span>
* <span data-ttu-id="b0fb6-163">[SDK-dokumentation – viktig information][Link 5]</span><span class="sxs-lookup"><span data-stu-id="b0fb6-163">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="b0fb6-164">[SDK-dokumentation – uppgradera guider][Link 5]</span><span class="sxs-lookup"><span data-stu-id="b0fb6-164">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="app-store-upload-failures"></a><span data-ttu-id="b0fb6-165">App store Överför fel</span><span class="sxs-lookup"><span data-stu-id="b0fb6-165">App store upload failures</span></span>
### <a name="issue"></a><span data-ttu-id="b0fb6-166">Problem</span><span class="sxs-lookup"><span data-stu-id="b0fb6-166">Issue</span></span>
* <span data-ttu-id="b0fb6-167">Fel som rör överföra den senaste versionen av din app till Apple eller Google App för Windows store.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-167">Errors related to uploading the latest version of your app to Apple, Google, or the Windows App store.</span></span>

### <a name="causes"></a><span data-ttu-id="b0fb6-168">Orsaker</span><span class="sxs-lookup"><span data-stu-id="b0fb6-168">Causes</span></span>
* <span data-ttu-id="b0fb6-169">Appen lagrar ibland blockera appar med vissa funktioner som aktiveras (t.ex. Apple Store hindrar användningen av IDFV i appar i store och arkivet GooglePlay förhindrar delning av programinformationen mellan appar).</span><span class="sxs-lookup"><span data-stu-id="b0fb6-169">App stores sometimes block apps with certain features enabled (e.g. the Apple Store prevents the use of IDFV in apps in the store and the GooglePlay store prevents the sharing of application information between apps).</span></span> 
* <span data-ttu-id="b0fb6-170">Kontrollera att du viktig information om din plattform och den aktuella versionen av SDK om du har svårt att överföra en app till arkivet.</span><span class="sxs-lookup"><span data-stu-id="b0fb6-170">Make sure that you check the release notes about your platform and current version of the SDK if you have difficulty uploading an app to the store.</span></span>

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

