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
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Felsökningsguide för SDK-integrationsproblem
hello följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement ska integreras i ditt program.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>SDK-problem som identifieras av ett fel i en annan del av ditt program
### <a name="issue"></a>Problem
* Användargränssnittet data collection-fel (Analytics, övervakning, segmentering eller instrumentpaneler).
* Push-fel (push-meddelanden fungerar inte i appen, utanför appen eller båda).
* Avancerade funktionen fel (spårnings, Geolokalisering eller plattform specifika push-meddelanden inte fungerar).
* API-fel (API: er misslyckas ofta tyst utan felmeddelanden).
* Tjänstrelaterade (ingen av Azure Mobile Engagement fungerar för tillämpningsprogrammet).

### <a name="causes"></a>Orsaker
* De flesta problem som måste matchas med hello Azure Mobile Engagement SDK toobe identifieras av ett fel i ditt program (till exempel ett UI data collection fel, push-fel, avancerad funktion fel, API-fel, programkrascher eller tydligt service avbrott).  
* Om en viss funktion i Azure Mobile Engagement har aldrig fungerat appen innan, behöver toocomplete hello-integration. 
* Om en viss funktion i Azure Mobile Engagement fungerade och stoppas kommer behöva du tooupgrade toohello senaste versionen med hello Azure Mobile Engagement SDK. Kom ihåg att det finns en annan version av hello Azure Mobile Engagement SDK för varje plattform som stöds av Azure Mobile Engagement (Android, iOS, Windows och Windows Phone).

#### <a name="sdk-integration"></a>SDK-integrering
* Azure Mobile Engagement integrerad inte korrekt i SDK (Analytics).
* Nå inte korrekt integrerad i SDK: N (i appen och ut från appen push-meddelanden).
* Certifikatet har upphört att gälla eller är felaktig PROD vs. DEV (endast iOS).
* GCM eller ADM inte korrekt integrerat i SDK (Android endast - tjänsten specifika push-meddelanden).
* Integrerad spårning inte korrekt i SDK (installera store spårning).
* Lazy plats eller GPS-plats integreras inte korrekt i SDK (målinriktning av geografiska plats).

**Se även:**

* [Dokumentationen för SDK - Integration guider][Link 5] 
* [Felsökningsguide för - Push][Link 23]

#### <a name="sdk-upgrade"></a>SDK-uppgradering
* Måste tooupgrade SDK tooresolve problem med äldre versioner av hello SDK (ofta relaterade toonewer versioner av hello enhetens operativsystem).
* Avinstallera alla tidigare versioner av appen från din enhet och installera om hello senaste versionen av appen, hello registrera ditt enhets-ID från hello Azure Mobile Engagement UI tooconfirm att enheten använder hello senaste versionen av din app.

**Se även:**

* [SDK-dokumentation – viktig information](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [SDK-dokumentation – uppgradera guider](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Andra SDK
* Fel i programmets Manifest ”AndroidManifest.xml” kan orsaka Azure Mobile Engagement inte toowork (endast Android).
* Ett vanligt problem med SDK-integration och API-användning är tooconfuse hello SDK-nyckeln och hello API-nyckel.

**Se även:**

* [Begrepp - ordlista][Link 6]

## <a name="advanced-coding-issues"></a>Avancerade kodning problem
### <a name="issue"></a>Problem
* Specifika plattformskod relaterat inte direkt tooAzure Mobile Engagement kan orsaka problem på iOS, Android och Windows Phone.

### <a name="causes"></a>Orsaker
* Många avancerade kodning problem med Azure Mobile Engagement på grund av felaktigt skrivet plattformen specifik kod inte är direkt relaterat tooAzure Mobile Engagement. Du behöver tooconsult dokumentationen specifika toohello plattform som du utvecklar för dessutom tooAzure Mobile Engagement dokumentationen (Android, iOS, webbprogram, Windows och Windows Phone).
* Konfigurera inte korrekt ”kategorier”, förhindrar länka från en avisering tooanother plats i eller utanför hello app (endast Android). 
* Inte inställningen ”UIKit.framework” för ”valfritt” i din iOS-kod, visas en ”det gick inte att hitta symbolen” och/eller kraschar på äldre iOS-enheter (endast iOS).
* Utgångna certifikat eller inte korrekt använder hello DEV eller produktprenumeration version av hello certifikat, orsaker push utfärdar (endast iOS).
* Det finns vissa begränsningar inbyggd tooa plattform som Azure Mobile Engagement inte kan styra (t.ex. hur hello systemcenter fungerar för utanför appen push-meddelanden i Android och iOS).
* Azure Mobile Engagement publicerar en fullständig lista över hello interna paket som används av Azure Mobile Engagement för iOS och Android som referens. Tänk på att vissa funktioner i Azure Mobile Engagement är särskilda toohello plattform (Android, iOS, webbprogram, Windows och Windows Phone).

### <a name="see-also"></a>Se även
* [Felsökningsguide för - Push][Link 23] 
* [SDK-dokumentation – viktig information][Link 5]
* [SDK-dokumentation – uppgradera guider][Link 5]

## <a name="application-crashes"></a>Programkrascher
### <a name="issue"></a>Problem
* Programmet kraschar på hello slutanvändarens enhet.

### <a name="causes"></a>Orsaker
* Information om kraschen uppges kan visas i hello *Analytics UI* eller hello *Analytics API*
* Du kan hitta hello enhets-ID för enheten test och vidta hello samma åtgärd som har orsakat ditt program toocrash för en slutanvändare toohelp identifiera hello orsaken till din krascher.
* Kända problem med hello Azure Mobile Engagement SDK som orsakar program toocrash matchas ibland genom att uppgradera toohello senaste versionen av hello SDK. Se till att toocheck hello viktig information om din plattform när du undersöker kraschar.

### <a name="see-also"></a>Se även
* [SDK-dokumentation – viktig information][Link 5]
* [SDK-dokumentation – uppgradera guider][Link 5]

## <a name="app-store-upload-failures"></a>App store Överför fel
### <a name="issue"></a>Problem
* Fel relaterade toouploading hello senaste versionen av din app tooApple, Google, eller hello App för Windows store.

### <a name="causes"></a>Orsaker
* Appen lagrar ibland blockera appar med vissa funktioner som aktiveras (t.ex. hello Apple Store förhindrar hello användningen av IDFV i appar i store hello och hello GooglePlay store förhindrar hello delning av programinformationen mellan appar). 
* Kontrollera att du kontrollerar hello viktig information om din plattform och den aktuella versionen av hello SDK om du har svårt att ladda upp en toohello appbutik.

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

