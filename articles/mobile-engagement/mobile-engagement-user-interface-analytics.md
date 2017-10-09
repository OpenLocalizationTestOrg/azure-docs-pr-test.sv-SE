---
title: "aaaAzure användargränssnitt för Mobile Engagement - Analytics"
description: "Lär dig hur tooanalyze historiska data om ditt program med Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6b2533ac-b8ec-4e35-872c-d563895bdc0c
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4a9df11226fed6710cfb1337ae84ece7596d482f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooanalyze-historical-data-about-your-application"></a>Hur tooanalyze historiska data om ditt program
Den här artikeln beskriver hello **ANALYTICS** för hello **Mobile Engagement** portal. Du använder hello **Mobile Engagement** portal toomonitor och hantera dina mobila appar. Observera att toostart med hello-portalen måste du först toocreate en **Azure Mobile Engagement** konto.

hello Analytics avsnitt i hello Användargränssnittet innehåller aktuell information om ditt program som bygger på historiska data som uppdateras var 24: e timme. hello information som visas på olika instrumentpaneler som består av rad/fält/cirkeldiagram, rutnät och kartor. hello data kan också hämtas som CSV-filer. De flesta av samma information som finns i realtid i hello övervakaren avsnitt i hello UI och kan även nås från hello Analytics API.

> [!NOTE]
> Många avsnitt av hello **Mobile Engagement** portalens användargränssnitt innehåller hello **Visa hjälp** knappen. Tryck på den här knappen tooget mer detaljerad information om ett avsnitt.

## <a name="standard-and-custom-analytics"></a>Standard- och anpassade Analytics
Azure Mobile Engagement tillhandahåller en uppsättning basic, standard analytiska information om ditt program som kan visas i diagram registreringen så snart du integrera din app med hello SDK. Azure Mobile Engagement innehåller också hello möjlighet toogather ytterligare anpassade analys information som du vill om slutanvändarnas beteende. Du kan göra detta genom att skapa en taggplan av anpassade ”taggar (app-info)”, som skapas från **inställningar** så att Azure Mobile Engagement kan samla in ytterligare data för dig.

## <a name="analytics"></a>Analys
* Instrumentpanelen: Visar allmän information om nya och består användarna och deras trender.
* Användare: Användare identifieras genom sina enhets-ID: Detta ID är unikt för varje enhet (en ny användare är faktiskt en ny enhet). En användare anses vara ny under ett visst tidsintervall om han eller hon har genomfört sin första session under tidsintervallet. En användare anses som kvarhållen om han eller hon har genomfört minst en session under hello senaste 7 dagarna. Aktiva användare kan användare som gjort minst en session under en given tidsperiod. Du kan sortera efter, varje månad, varje vecka, varje dag eller varje timme tidsperioder. Alla hello diagram likna men kan du toofilter av olika funktioner, till exempel hello version av programmet och sedan toosort av en viss tidsperiod. hello standard information som samlats in genom att integrera hello SDK omfattar hello följande: aktiva användare, ny användare, sessioner, längden på varje session, teknisk information om hello land, lokala variabler, plats, språk operatör, enheter, inbyggd programvara, nätverk (Wi-Fi), versioner av hello appen och SDK, som används av kunder. Den här informationen kan visas i realtid från hello övervakaren avsnitt.

> [!NOTE]
> hello baseras tidsperioden på hello datum från hello användarnas Enhetsinställningar, så att en användare vars telefonen har hello datum felaktigt som kan visas i hello fel tidsperiod.

* Kvarhållning: En användare anses vara kvarhållen under ett visst tidsintervall om han eller hon har genomfört sin första session under tidsintervallet. Du kan ändra hello tidsintervaller under vilka kvarhållna användare (och nya användare) ska räknas toohours, dagar, veckor eller månader. hello kvarhållning analyser bygger på kohorter. En kommittén är hello uppsättning alla hello-nya användare upptäckta för en viss period (d.v.s. hello uppsättning användare som utför sin första session under denna tid). Vi använder kohorter 1 dag, 2 dagar, 4 dagar, 7 dagar eller 1 månad. Den angivna en kommittén, varje dag 1, 2 dagar, 4 dagar, 7 dagar eller 1-månads Azure Mobile Engagement beräknar hello uppsättning av alla användare som hör toohello kommittén och är fortfarande aktiv (d.v.s. hello uppsättning användare som utförs på minst en session under hello punkt). Den här uppsättningen med användare kallas en kommittén version. (Azure Mobile Engagement kan du visa hur många användare fortfarande använder din app, men endast hello plattform specifika store ser du hur många användare avinstalleras din app – till exempel GooglePlay iTunes Windows Store, etc.).
* Sessioner: En användning av programmet hello av en användare. Sessioner genereras från hello sekvensen av aktiviteter som utförs av användare (en aktivitet är ofta kopplade toohello användning av en skärmbild av programmet hello, men detta kan variera beroende på hello sätt hello SDK: N har integrerats i programmet hello). En användare kan bara utföra en aktivitet i taget: sessionen startar när hello användaren startar den första aktiviteten och slutar när användarens sista aktivitet är klar. Om en användare är fler än några få sekunder utan att utföra alla aktiviteter, är hans sekvensen av aktiviteter uppdelad i två olika sessioner.
* Aktiviteter: hello namnen på varje skärm i ditt program och användarna hello längd spendera på varje skärm. Aktiviteter är ett anpassat analytiska alternativ som motsvarar toohello ”app-info”-taggar som du har ställt in för din egen app:
* Sökväg till användare: Visar hur dina användare navigerar genom ditt programs aktiviteter (skärmar). Du kan flytta hello skjutreglaget tooadjust hello detaljnivån. Blå noder representerar ditt programs aktiviteter. Deras storlek är proportionell toohello användarna ägnat åt den. Vita noder representerar start och stopp för sessioner. Röda noder representerar krascher. Länkar representerar övergångar mellan ditt programs aktiviteter (eller mellan aktiviteter och krascher). Klicka på en nod eller en länk toodisplay en knappbeskrivning med mer information om dina data: hello tid som ägnats åt en viss skärm hello antalet övergångar och hello procentandelen övergångar från hello källa aktiviteten toohello målaktiviteten. (En---60%---> B innebär att användare på aktiviteten en går tooactivity B 60% hello tid.) Du kan ordna hello diagram som du vill tooclarify. dess position sparas varje gång du gör en ändring. Du kan visa eller Dölj hello krascher toolighten hello diagram.
* Händelser: Vissa åtgärder som utförs av en användare i hello program. hello distribution av händelser visas som hello antal händelser per användare per session. En händelse representerar en omedelbar åtgärd, till exempel klickar på en knapp eller hello mottagandet av en avisering. (hello innebörden av olika händelser beror på hur hello SDK har integrerats i programmet hello.) En händelse kan inträffa under en session eller ett jobb eller kan vara fristående.
* Jobb: Liknande tooevents utom de fokuserar på hello tidslängd hello-åtgärd. Jobb kan exempelvis ange du teknisk information om hur lång tid det tar innehåll tooload eller anropet tooweb tjänst. Det kan också visa hur lång tid det tar att en användare toofill i ett formulär, skapa ett konto eller göra ett inköp. Ett jobb representerar en uppgifts varaktighet hello, för exempel, hello varaktighet för en aktivitet för hämtning eller hello tid en banderoll visas på hello-skärmen. (hello innebörden av ett jobb beror på hur hello SDK har integrerats i programmet hello.) Jobb är ofta kopplade till bakgrundsuppgifter som genomförs utanför hello omfånget för en session (dvs utan att användaren är inblandad).
* Technicals: Teknisk information om hello enheter hello användare i appen som du kan följa, till exempel hello språk, operatör, nätverk, enhet, inbyggd programvara och skärmen storleken på hello användarnas enheter och hello Version av appen och hello SDK-version som används i din app.
* Fel: Information om tekniska fel i hello-program som inte orsakar hello programmet toocrash. Ett fel representerar ett snabbmeddelanden problem, till exempel nätverksfel eller dålig hantering. (hello innebörden av olika händelser beror på hur hello SDK har integrerats i programmet hello.) Ett fel kan inträffa under en session eller ett jobb eller kan vara fristående.
* Krascher: Information om fel som gör programmet-toocrash. En krasch är ett oväntat tillstånd där hello programmet slutar att fungera som förväntat och måste stoppas. En krasch beror oftast på grund av tooa programfel i hello program.

![Analytics2][11]

## <a name="accessing-hello-retention-overview"></a>Åtkomst till hello kvarhållning översikt
![Analytics3][12]

hello kvarhållning översikt är uppdelad i hello mitten i flera kort, varje visar hello översikt för en viss period. kvarhållningsperiod för hello 2 dagar visas i exemplet hello. hello visar andra kort hello 4 dagar och 7 dagars kvarhållningsperioder.

## <a name="understanding-hello-retention-overview-cards"></a>Förstå hello kvarhållning översikt-kort
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Varje kort består av 3 delar:
1. 1: hello kommittén och hello period
2. 2-4: hello kvarhållning för hello aktuella perioden
3. 5: miniatyrdiagram hello historik

### <a name="here-is-detailed-information-about-each-element"></a>Här är detaljerad information om varje element:
1. Kommittén och period: den här rubriken ger hello typ av kommittén. Här innebär ”2-dagarsperiod” att vi ska titta på hello beteendet för användare över 2 dagar användare kommit över en tidsperiod 2 dagar, och om de ansluter i hello följande block 2 dagar. hello-exemplet ovan anser hello aktivitet användare mellan hello 21 och 22 november.
2. Detta ger ett hello kvarhållning hastighet hello 21 och 22 i November för hello användare inkommer på 19 och 20 i November. Här vi hade 1 aktiva användare mellan Hej 21 och 22nd, över hello 3 som var nya användare mellan hello 19 och 20.
3. Den här visuella indikator ger hello grafiskt samma information som ovan. (hello tredje hello cirkel är från hello 33% nummer.) hello färg ger ytterligare information: grönt anger antalet växer från föregående hello-beräkningen. Gult innebär stabilt och röda sätt minskar.
4. Detta anger hello-värden som används för beräkning av hello.
5. Det här är ett miniatyrdiagram av hello tidigare hello kvarhållning värden. Det gör att du toosee hello värden i hello tidigare toohave en omfattande vy av hur det utvecklades.

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
