---
title: "aaaAnalyze navigering användarmönster med användaren flödar i Azure Application Insights | Microsoft docs"
description: "Analysera hur användarna navigera mellan hello sidor och funktioner i ditt webbprogram."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 08/02/2017
ms.author: cfreeman
ms.openlocfilehash: d3f35dc78e9874e4b7974604bf91c40a5e5b78eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Analysera navigering användarmönster med användaren flödar i Application Insights

![Application Insights användaren flödar verktyget](./media/app-insights-usage-flows/flows.png)

hello användaren flödar verktyget visualizes hur användarna navigera mellan hello sidor och funktioner på din plats. Det är bra för att besvara frågor som:
* Hur användarna navigera bort från en sida på webbplatsen?
* Vad användare klicka på en sida på webbplatsen?
* Där är hello placerar att användare omsättningsuppdateringar mesta möjliga av din webbplats?
* Finns det platser där användare Upprepa hello samma åtgärd flera gånger?

hello användaren flödar verktyg startar från en inledande sidan vyn eller den händelse som du anger. Den här vyn sida eller anpassade händelsen som användaren flödar hello visar sidvisningar och anpassade händelser som användare skickas omedelbart efteråt under en session två steg efteråt, och så vidare. Rader med olika tjocklek visar hur många gånger varje sökväg har följt av användare. Särskilda ”sessionen avslutats” noderna visar hur många användare skickas ingen sidvisningar eller anpassade händelser efter hello föregående nod, syntaxmarkering där användare förmodligen kvar din plats.



> [!NOTE]
> Application Insights-resursen måste innehålla sidvisningar eller anpassade händelser toouse hello användaren flödar verktyg. [Lär dig hur tooset upp appen toocollect sidan visar automatiskt med hello Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>Starta genom att välja en inledande sidan vy eller anpassade händelsen

![Välj en inledande händelse för användaren flödar](./media/app-insights-usage-flows/flows-initial-event.png)

tooget igång genom att besvara frågor med hello användaren flödar verktyget väljer ett inledande sidan vyn eller anpassade händelsen tooserve som startpunkt för hello visualiseringen hello:
1. Klicka på länken hello i hello ”vad användarna gör efter...”? title eller klicka hello redigera. 
2. Välj en vyn sida eller anpassade händelsen hello ”första händelsen” listrutan.
3. Klicka på ”Skapa diagram”.

Hej ”steg 1” kolumn av hello visualisering visar vad användare har oftast bara efter hello första händelsen, sorterade upp och ned från de flesta tooleast sällan. Hej ”steg 2” och efterföljande kolumner visar vad användarna har därefter skapa en bild av alla hello sätt användare har navigerat till din plats.

Som standard prov hello användaren flödar verktyget slumpmässigt endast hello senaste 24 timmarna sidvisningar och anpassade händelsen från webbplatsen. Du kan öka hello tidsintervall och ändra hello balans mellan prestanda och noggrannhet för slumpmässig provtagning hello Redigera-menyn.

Om några hello sidvisningar och anpassade händelser inte relevanta tooyou klickar du på hello ”X” hello noder du vill toohide. När du har valt hello-noder som du vill toohide Klicka hello ”skapa diagram” nedan hello visualiseringen. toosee alla hello noder du dolt, klicka hello redigera och titta på hello ”exkluderade händelser” avsnittet.

Om sidvisningar eller anpassade händelser saknar som du förväntar dig toosee på hello visualiseringen:
* Kontrollera hello ”exkluderade händelser” avsnittet hello Redigera-menyn.
* Använd hello ”detaljnivån” kontrollen i hello-Redigera-menyn tooinclude mer sällan händelser i hello visualiseringen.
* Om läget för hello-sida eller anpassade händelsen som du förväntar dig skickas sällan av användare, försök ökande hello tidsintervall för hello visualiseringen hello Redigera-menyn.
* Kontrollera att hello sidvy eller anpassade händelsen som du förväntar dig att konfigurera toobe som samlas in av hello Application Insights SDK hello källkoden på din plats. [Läs mer om att samla in anpassade händelser.](app-insights-api-custom-events-metrics.md)

Om du vill redigera toosee flera steg i hello visualiseringen, Använd hello ”antal steg” skjutreglaget i hello-menyn.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>När du besöker en sida eller en funktion där användare går och vad de på?

![Använd användaren flödar toounderstand där användare klickar på](./media/app-insights-usage-flows/flows-one-step.png)

Om din första händelsen är en sidvy, är hello första kolumnen (”steg 1) med hello visualiseringen ett snabbt sätt toounderstand vad användare har omedelbart efter att besöka sidan hello. Försök öppna webbplatsen i ett fönster nästa toohello användaren flödar visualiseringen. Jämför förväntningar för hur användarna samverkar med hello sidan toohello lista över händelser i kolumnen för hello ”steg 1”. Ofta kan ett UI-element på hello sida som verkar obetydlig tooyour team vara mellan hello mest använda på hello-sidan. Det kan vara en utmärkt utgångspunkt för design förbättringar tooyour plats.

Om din första händelsen är en anpassad händelse, hello första kolumn som visar vad användare gjorde när du utför denna åtgärd. Precis som med sidvyer överväga om hello observerats beteendet för dina användare matchar teamets mål och förväntningar. Om din valda första händelsen är ”tillagd objektet tooShopping kundvagn”, till exempel, leta toosee om ”gå tooCheckout” och ”slutföra köpet” som visas i hello visualiseringen strax därefter. Om användarbeteende skiljer sig mycket från dina förväntningar, använda hello visualiseringen toounderstand hur användare komma ”fångas” av aktuella designen för din webbplats.

## <a name="where-are-hello-places-that-users-churn-most-from-your-site"></a>Där är hello placerar att användare omsättningsuppdateringar mesta möjliga av din webbplats?

Håll utkik efter ”sessionen avslutats” noder som visas hög upp i en kolumn i hello visualiseringen särskilt tidigt i ett flöde. Det innebär att många användare som förmodligen churned från din plats när följande hello föregående sökväg för sidor och interaktioner Användargränssnittet. Ibland omsättningen - när du har slutfört ett inköp på en plats för e-handel, till exempel - men omsättning är vanligtvis ett tecken på problem, dåliga prestanda eller andra problem med din webbplats kan förbättras.

Tänk på att ”sessionen avslutats” noder endast baseras på telemetri som samlats in av Application Insights-resurs. Om Application Insights inte har fått telemetri för vissa användarinteraktioner, användare kan fortfarande har har åtgärdat med webbplatsen sig när hello användaren flödar verktyget står hello sessionen avslutades.

## <a name="are-there-places-where-users-repeat-hello-same-action-over-and-over"></a>Finns det platser där användare Upprepa hello samma åtgärd flera gånger?

Leta efter en vyn sida eller anpassade händelsen upprepas i efterföljande steg i hello visualiseringen av många användare. Detta innebär vanligen att användare utför återkommande åtgärder på webbplatsen. Om du hittar upprepning tänka ändra hello utformningen av din webbplats eller lägga till nya funktioner tooreduce upprepas. Om du hittar användare som utför återkommande åtgärder på varje rad i en tabellelement till exempel lägga till bulk redigera funktioner.

## <a name="common-questions"></a>Vanliga frågor

### <a name="why-do-steps-appear-disconnected"></a>Varför visas stegen frånkopplad?

![Användaren flöden med frånkopplade steg](./media/app-insights-usage-flows/flows-disconnected.png)

Om stegen (kolumner) i en visualisering användare flödar är frånkopplad, innebär det att ingen av hello sökvägar fattas av användare mellan hello steg var tillräckligt frekvent toobe visas. tooshow mindre ofta händelser på hello visualiseringen så hello steg visas ansluten, justera hello ”detaljnivån” skjutreglaget hello Redigera-menyn.

### <a name="does-hello-initial-event-represent-hello-first-time-hello-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Hello första händelsen representerar hello första gången hello händelse visas i en session eller när den visas i en session?

hello första händelsen i hello visualiseringen representerar hello bara första gången en användare skickas sidan vyn eller anpassade händelsen under en session. Om användarna kan skicka hello första händelsen flera gånger i en session och sedan hello ”steg 1” kolumn bara visar hur användare fungerar när hello *första* inledande händelse, inte alla instanser.

## <a name="next-steps"></a>Nästa steg

* [Översikt över användning](app-insights-usage-overview.md)
* [Användare, sessioner och händelser](app-insights-usage-segmentation.md)
* [Kvarhållning](app-insights-usage-retention.md)
* [Lägga till anpassade händelser tooyour app](app-insights-api-custom-events-metrics.md)
