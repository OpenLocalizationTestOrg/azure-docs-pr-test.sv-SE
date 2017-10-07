---
title: "aaaAzure Mobile Engagement-implementering för Media App"
description: Media app scenariot tooimplement Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48201cc8-4e04-485c-a8dc-d6406d23f3ed
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 6a495eb790993a30d5c03802aa9e6404fea983d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-media-app"></a>Implementerar Mobile Engagement med Media App
## <a name="overview"></a>Översikt
John är mobila projektledare för ett stort media företag. Han startas nyligen en ny app som har en mycket hög hämtning. Han har nått sin mål för nedladdning men fortfarande hans returnera på Investment(ROI) per användare uppfyller inte sitt behov. 

John har redan identifierats av varför sin Räntabilitet är för lågt. Användare som ofta sluta använda sin app efter bara två veckor och de flesta gå aldrig tillbaka. Han vill tooincrease hello kvarhållning av sin app.

Efter en initial test han har lärt dig när han snabbt tillkallar sina användare med push-meddelanden, brukar de toocontinue med sin app. Användare som har inaktiv returneras också ofta toohello app beroende på meddelanden som han skickar dem. John beslutar tooinvest i någon form av Program för användarinteraktion för sin app som använder avancerade mål med push-meddelanden.

John har nyligen Läs hello [Azure Mobile Engagement – komma igång med bästa praxis](mobile-engagement-getting-started-best-practices.md) och har valt tooimplement hello rekommendationerna från hello guide.

## <a name="objectives-and-kpis"></a>Mål och KPI: er
Viktiga intressenter för Johns appen uppfylla. Företag genereras från annonser som användarna använda sin media. John ökar sin intäkter genom att öka innehåll som används per användare. Komma överens om en Huvudsyftet: tooincrease försäljning från annonser med 25%. De skapar toomeasure (Business Key Performance Indicator) och enheten detta mål

* Antal annonsklickningar klickade per användare
* Hur många artikel sidor besökt (per användare / per session / per vecka / per månad...)
* Vad är favoritkategorier

Han har definierat sin KPI: er baserat på Johns möte med viktiga intressenter. Han följer del 1 av hello [Azure Mobile Engagement – komma igång med bästa praxis](mobile-engagement-getting-started-best-practices.md). 

Han skapar sedan hello följande Engagement KPI: er tooensure som mål har uppnåtts:

* Övervaka kvarhållning över hello följande intervall: varje dag, varje vecka, varannan vecka och månad.
* Antal aktiva användare
* Hej appklassificering i hello app lagrar

Baserat på rekommendationer från hello IT-teamet hello följande tekniska KPI: er har lagts till tooanswer hello följande frågor:

* Vad är Mina användare sökväg (vilken sida besökta, hur många användare som tid spendera på den)
* Antal krascher eller buggar påträffade sessioner?
* Vilka operativsystemversioner körs Mina användare?
* Vad är hello genomsnittlig storlek på skärmen för Mina användare?
* Vilken typ av internet-anslutningar har Mina användare?

Han klassificerar hello data som krävs för varje KPI och han registrerar den i hello rätt plats för sin playbook.

## <a name="engagement-program-and-integration"></a>Program för användarinteraktion och integrering
Nu definiera sin KPI: er som Johan har slutförts startar han fasen sin Engagement-strategi genom att definiera 4 program och sina mål:![][1]

Sedan går John djupare med information om push-meddelanden för varje program. Push-meddelanden definieras av fem element:

1. Mål: Vad är hello mål av hello-meddelande
2. Hur hello målet kan nås
3. Mål: vem som ska få hello-meddelande?
4. Innehåll: Vad är hello ordval och hello format för hello-meddelande (i App/Out av App)
5. När: Vad är hello bästa tidpunkten toosend push-meddelanden
   
    ![][2]

Mer information finns i toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Enligt toohello del 2 av hello vitboken John använder mål avsnittet toodefine vilka data som han har toocollect och skrivningar hans taggen planera tillsammans med IT-teamet tooimplement hello lösning. Efter en vecka för genomförande och användargodkännande starta John slutligen sina program.

## <a name="program-results"></a>Programmet resultat
fyra månader senare John går igenom resultaten av program. hello välkomstprogram och hello veckovisa Program uppfyller sina mål. hello antal användare med endast en session minskar, funktionerna i appen hello används och dubblerat hello antalet anslutningar per vecka.

Hej **inaktiva Program** hjälper John förstå användaren beteenden. Det verkar som 15% av hello inaktiva användare komma tillbaka toohello app. Men de flesta av dem inte förblir aktiv mer än 1 månad. John förutser en potentiell optimering av denna sekvens med ytterligare meddelanden och expandera sitt innehåll val.

Hej **identifiera programmet** inte fungerar väl. Den ökar mellan sälja men finns inte tillräckligt med tooreach sina mål. John identifierar att han inte har tillräckligt med data toomake relevanta mål och föreslå rätt innehåll. Han stoppar programmet och fokuserar på Skicka ”redaktionell push-meddelanden” med Azure Mobile Engagement. Hans journalister har redan en CMS lösning toosend push-meddelanden och de inte toochange.

John beslutar toouse hello nå API: T som är en HTTP-REST-API som kan hantera Reach-kampanjer utan toouse AZME webbgränssnitt. Med den här metoden John kan samla in hello data han måste och Tillåt sin skribenter tookeep med hello CMS-lösning.

tooensure som funktionen fungerar korrekt ber John IT-teamet toobe vaksam på hello följande punkter:

1. **Åtgärden system** : alla har sina egna regler tooadministrate push-meddelanden, så John toolist alla fall och kontrollerar om hantering av hello API: er.
   T.ex.: Android push-system kan stor bild som inte är fallet hello med iOS.
2. **Tidsintervall**: John vill en API som hello tidsintervall och en end-toocampaigns. Han vill toopreserve användare från någon störande meddelande bombing.
3. **Kategorier**: marknadsföring team förbereder mallen för varje typ av avisering. John frågar IT-teamet tooset kategorier i hello API.

Efter vissa tester är John uppfyllda. Tack toothis API journalister kan fortfarande skicka push-meddelanden med deras CMS och Azure Mobile Engagement samlar in alla beteendebaserade data för dem.

När dessa 4 först månader, resultaten avspeglar en bra övergripande prestanda och ger förtroende för John och hans board Räntabilitet per användare ökar per 15% och mobila försäljning representerar 17.5% av total försäljning en ökning av 7.5% bara fyra månader.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
