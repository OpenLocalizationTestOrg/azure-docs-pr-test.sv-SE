---
title: "aaaAzure användargränssnitt för Mobile Engagement - segment"
description: "Lär dig hur toocreate och hantera segment av användare tooidentify användningsmönster med Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6a4f2205-4a3c-406e-a04f-5e6f2a36653f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: bb214c45d05ebfbf243978658a7e331d4a7c6e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-segments-of-users-tooidentify-usage-patterns"></a>Hur toocreate och hantera segment av användare tooidentify användningsmönster
Den här artikeln beskriver hello **segment** för hello **Mobile Engagement** portal. Du använder hello **Mobile Engagement** portal toomonitor och hantera dina mobila appar.

hello segment avsnittet av hello Användargränssnittet kan du toowork på segmentera dina användare baserat på hello olika beteenden och analyser som du kan hämta från hello programmet och kan också komma åt via hello segment API. Segment beräknas först 24 timmar efter att de skapas och de recomputed per dygn baserat på hello senaste analytics informationen. När ett segment beräknas visar ett diagram med ”dag tooday historik” varje dag.

> [!NOTE]
> Många avsnitt av hello **Mobile Engagement** portalens användargränssnitt innehåller hello **Visa hjälp** knappen. Tryck på den här knappen tooget mer detaljerad information om ett avsnitt.
> 
> 

## <a name="create-segments"></a>Skapa segment
Du kan skapa ett segment baserat på too10 kriterier i en viss period in too60 dagar i hello tidigare från hello analytics avsnitt. Du kan till exempel skapa ett segment baserat på hello personer som har visat vissa sidor eller söka efter vissa innehållstyper i din app i hello senaste 10 dagarna. Den här informationen finns i avsnittet för hello analytics. Så du kan använda den toocreate ett segment och sedan konfigurera tootarget en push-meddelanden med den här delmängden av användare tooget dem toocome tillbaka toohello program. 

> [!NOTE]
> När ett segment har beräknats kan inte redigeras; Det kan bara klona (kopierade) eller förstörs (borttagna). Ett segment kan klonas i hello samma program (med hello samma AppID), och även kan klonas i andra program (med en annan AppID). 

 ![segments1][35] 

## <a name="examples-segments"></a>Exempel segment
 ![segments2][36]

Segment kan du toosegment hello slutanvändare av ditt program.
Segmentera dina användare är en viktig marknadsföringsstrategi. Azure Mobile Engagement kan du tooget historiska data och skapa anpassade segment. Detta kraftfulla verktyg kan du toolearn om kundernas upplevelse i ditt program. Du kan enkelt analysera segmenten och använda segment som push-mål.
Är ett vanligt användningsfall som du vill toosend push tooencourage ett meddelande som dina slutanvändare toorate ditt program i hello store. I stället för att skicka ett meddelande tooall slutanvändarna kan skapa du ett segment som anger enbart användare som har använt ditt program varje dag för hello förra månaden och har haft en bra användarmiljö. När du skickar färre, avgränsade push-meddelanden får du en bättre avkastning på investeringen.

 ![segments3][37]

### <a name="segments-you-can-create-based-on-hello-major-azure-mobile-engagement-elements"></a>Du kan skapa segment baserat på hello viktiga Azure Mobile Engagement-element:
* Händelse: skapa ett segment som är riktad mot en specifika händelser för hello-program som har hänt fler än två gånger per vecka. 
* Sessionen: skapa ett segment med användare som har använt hello programmet mer än 5 gånger föregående vecka.
* Aktiviteten: skapa ett segment med användare som har använt en sida eller innehåll mer eller mindre än 10 tid senaste månaden.
* Jobbet: skapa ett segment med användare som har slutfört ett jobb som är mer än två gånger om dagen.
* Krasch: skapa ett segment med alla hello-användare som har fått en krasch fler än 10 gånger föregående vecka. (Du kan push segmentet med en apology eller även om kuponger!)
* Fel: skapa ett segment med alla hello-användare som har ett fel uppstått mer än 100 gånger senaste tre dagarna.
* App-Info: skapa ett segment som riktar sig till en anpassad App-Info som inträffade under hello senaste 25 dagarna.
  
  ![segments4][38]

toouse Segment optimalt, du måste har gjort en anpassad integrering av hello SDK i ditt program med en märkning plan för ”App-Info”-taggar.
Gå sedan toohello startsidan för hello-gränssnittet, Välj hello program du vill använda och klicka på hello ”segment” avsnittet.

1. Markera hello ”segment”.
2. Klicka på hello ”nytt segment” knappen toocreate ett nytt segment.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Den verkliga livslängd exempel: Skapa ett enkelt segment baserat på ”sessionsinformation”
Skapa ett segment av alla hello slutanvändare som har använt appen minst 50 gånger i hello föregående vecka. Hitta endast hello slutanvändare som har använt minst 30 sekunder i din app per session därifrån. Det här alternativet visas alla hello slutanvändare som har en positiv upplevelse i din app. Sedan hello segment skapas kunde använda toopush ett meddelande toothese slutanvändare tooask dem toorate din app i hello lagra.

 ![segments5][39]

1. Namnge ditt segment i ordning toofind det snabbt i hello ”Segment”-listan.
2. Klicka på hello ”skapa”.
   
   ![segments6][40]

Markera Session.

 ![segments7][41]

1. Välj hello period för ”senaste veckan”.
2. Klicka på Nästa.
   
   ![segments8][42]
3. Välj hello Operator som är relevant hello listan: =; ≥ ≤.
4. Ange hello antal som du vill använda.
5. Välj hello förekomsten som du vill använda. 
6. Klicka på Nästa.
   Det här exemplet är inställd så att över hello förra veckan matchar användare som har gjort minst 50 sessioner.
   
   ![segments9][43]

Hello Session segmentering du hello längd sessioner som ett villkor.

1. Välj hello operatorn hello-listan.
2. Ange hello längd per session.
3. Klicka på Nästa.
   I det här exemplet står det att över alla hello sessioner som har varit segmenterade på hello förekomsten avsnittet väljer endast hello-användare som har använt mer än 30 sekunder per session.
   
   ![segments10][44]

Namnge ditt kriterium i ordning tooretrieve i hello slutföra tratt och klicka på Slutför.

 ![segments11][45]

När du har konfigurerat dina villkor, visas den i hello segment tratten.
Eftersom ett segment är baserat på analysdata, beräknas segment en gång per dag.
I det här exemplet 47,7% av totalt hello-slutanvändare matchade hello kriterium. De bör vara hello-användare som har haft en bra upplevelse och kommer troligen tooprovide en högre klassificeringen om du överför ett meddelande frågar dem toorate hello app i hello store.

## <a name="see-also"></a>Se även
* [Begrepp][Link 6]
* [Felsöka Guide Service][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

