---
title: aaaSmart identifiering - fel avvikelser i Application Insights | Microsoft Docs
description: "Aviserar toounusual ändringar i hello hastighet för misslyckade begäranden tooyour webbapp och ger diagnostiska analys. Ingen konfiguration krävs."
services: application-insights
documentationcenter: 
author: yorac
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: bwren
ms.openlocfilehash: dfd178ca9546294be91f27b0c1b846d519539b77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection---failure-anomalies"></a>Identifiering – fel avvikelser för smartkort
[Application Insights](app-insights-overview.md) automatiskt meddelar dig i nära realtid om ditt webbprogram påträffar en onormal ökning hello antal misslyckade begäranden. Den identifierar en onormal ökning av hello frekvensen för HTTP-begäranden eller beroendeanrop som rapporteras som misslyckad. För begäranden är misslyckade begäranden vanligtvis de med svarskoder 400 eller högre. toohelp du hantera och diagnostisera problem hello en analys av hello egenskaper hello fel och relaterad telemetri tillhandahålls i hello-meddelande. Det finns även länkar toohello Application Insights-portalen för ytterligare diagnos. hello funktionen behöver ingen installation eller konfiguration, eftersom den använder maskininlärning algoritmer toopredict hello normal felintervall.

Den här funktionen fungerar för Java- och ASP.NET-webbprogram, finns i molnet hello eller egna servrar. Den fungerar även för alla appar som genererar begäran eller beroende telemetri – till exempel om du har en arbetsroll som anropar [TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) eller [TrackDependency()](app-insights-api-custom-events-metrics.md#trackdependency).

När du har installerat [Programinsikter för ditt projekt](app-insights-overview.md), och om din app genererar en viss minsta mängd telemetri, Smart identifiering av fel avvikelser tar 24 timmar toolearn hello normalt beteende för din app, innan den slås på och skicka aviseringar.

Här är aviseringen.

![Smart identifiering aviseringen visar klustret analys runt fel](./media/app-insights-proactive-failure-diagnostics/013.png)

> [!NOTE]
> Som standard kan du få ett kortare format e-postmeddelande än det här exemplet. Men du kan [växel toothis detaljerat format](#configure-alerts).
>
>

Observera att du får meddelande:

* Hej felintervall jämfört med toonormal Apps beteende.
* Hur många användare påverkas – så att du vet hur mycket tooworry.
* Ett servicenivåmått mönster som är associerade med hello-fel. I det här exemplet finns ett visst svarskod, begäran namn (åtgärden) och programversion. Som anger du omedelbart där toostart söker i koden. Andra möjligheter kan vara ett specifikt operativsystem webbläsare eller klienten.
* hello undantag och loggspårningar beroendefel (databaser eller andra externa komponenter) som visas toobe som är associerade med hello kännetecknas fel.
* Länkar direkt toorelevant sökningar hello telemetri i Application Insights.

## <a name="benefits-of-smart-detection"></a>Fördelarna med Smart identifiering
Vanlig [mått aviseringar](app-insights-alerts.md) berättar om det kan finnas problem. Men Smart identifiering startar hello diagnostiska arbete du utför mycket hello analys som du annars skulle ha toodo själv. Du snabbt hello resultat snyggt paketeras hjälper dig att tooget toohello rot hello problem.

## <a name="how-it-works"></a>Hur det fungerar
Smart identifiering övervakar hello telemetri som tagits emot från din app och i särskilda hello fel priser. Den här regeln är hello antalet begäranden efter vilket Hej `Successful request` egenskapen är false och hello antal beroende anrop för vilka hello `Successful call` egenskapen är false. För begäranden som standard `Successful request == (resultCode < 400)` (om du har skrivit anpassad kod för[filter](app-insights-api-filtering-sampling.md#filtering) eller skapa egna [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest) anrop). 

Prestanda för din app har en typisk beteendemönstret. Vissa begäranden eller beroendeanrop kommer att orsaka toofailure än andra. och hello övergripande felintervall kan gå upp när belastningen ökar. Smart identifiering använder maskininlärning toofind dessa avvikelser.

Eftersom telemetri kommer till Application Insights från ditt webbprogram, jämför Smart identifiering hello aktuella beteende med hello mönster ses över hello senaste dagarna. Om en onormal ökning av antalet misslyckade observeras förhållande tidigare prestanda, utlöses en analys.

När en analys utlöses utför hello-tjänsten en kluster-analys hello misslyckade begäranden, tootry tooidentify ett mönster av värden som karakteriserar hello-fel. I hello-exemplet ovan identifierade hello analys att de flesta fel är om en specifik Resultatkod begäran namn, Server-URL-värden och rollinstans. Däremot har hello analys upptäckt att hello klientegenskap operativsystem distribueras över flera värden och därför det inte visas.

När tjänsten är försett med dessa telemetri anrop, söker hello mätaren efter ett undantag och ett beroendefel som är associerade med begäranden i hello kluster har identifierat tillsammans med ett exempel på några loggar som är associerade med de begäranden.

hello resulterande analys skickas tooyou som aviseringen om du har inte konfigurerat den.

Som hello [aviseringar du manuellt ange](app-insights-alerts.md), du kan inspektera hello tillståndet för hello avisering och konfigurerar det i hello aviseringar bladet för Application Insights-resurs. Men till skillnad från andra aviseringar som du inte behöver tooset upp eller konfigurerar Smart identifiering. Om du vill kan du inaktivera det eller ändra dess mål e-postadresser.

## <a name="configure-alerts"></a>Konfigurera aviseringar
Du kan inaktivera Smart identifiering, ändra hello e-postmottagare, skapa en webhook eller välja toomore detaljerad Varna meddelanden.

Öppna hello aviseringar. Fel avvikelser ingår tillsammans med eventuella aviseringar som du har angett manuellt och du kan se om det är för närvarande hello tillstånd för avisering.

![På översiktssidan för hello klickar du på panelen för aviseringar. Eller på alla mått klickar du på knappen för aviseringar.](./media/app-insights-proactive-failure-diagnostics/021.png)

Klicka på hello varning tooconfigure den.

![Konfiguration](./media/app-insights-proactive-failure-diagnostics/032.png)

Observera att du kan inaktivera Smart identifiering, men du kan inte ta bort (eller skapa en ny).

#### <a name="detailed-alerts"></a>Detaljerad aviseringar
Om du väljer ”Ta fram mer detaljerad diagnostik” innehåller flera diagnostikinformation hello e-post. Ibland kommer du att kunna toodiagnose hello problemet bara från hello data i hello e-post.

Det finns en mindre risk att hello mer detaljerad avisering kan innehålla känslig information, eftersom den innehåller undantag och spåra meddelanden. Detta kan dock bara hända om koden möjliggör känslig information i dessa meddelanden.

## <a name="triaging-and-diagnosing-an-alert"></a>Triaging och diagnostisering av en avisering
En avisering som anger att en onormal ökning hello misslyckade förfrågningar har identifierats. Det är troligt att det finns problem med din app eller dess miljö.

Från hello procent av begäranden och antalet användare som påverkas, kan du bestämma hur brådskande hello problemet är. I hello-exemplet ovan, hello antalet misslyckade 22,5% Jämför med en normal andel % 1, anger att något händer. Hej på andra sidan, 11 endast användare som påverkats. Du kommer att kan tooassess hur allvarlig som om det vore din app.

I många fall, kommer du att kunna toodiagnose hello problemet snabbt från hello begäran namn, undantag, beroende felet och spåra informationen.

Det finns några andra ledtrådar. Hello beroende felintervall i det här exemplet är hello samtidigt som hello undantag hastighet (89.3%). Detta tyder på att hello undantag uppstår direkt från hello beroendefel - ger en tydlig bild av var toostart söker i koden.

tooinvestigate dessutom hello länkar i varje avsnitt leder raka tooa [söksidan](app-insights-diagnostic-search.md) filtrerade toohello relevanta begäranden, undantag, beroenden eller spår. Du kan också öppna hello [Azure-portalen](https://portal.azure.com), navigera toohello Application Insights-resurs för din app och öppna bladet för hello-fel.

I det här exemplet öppnar genom att klicka på hello ”visa beroende fel information” länken hello Application Insights Sök bladet. Den visar hello SQL-uttryck som innehåller ett exempel på hello rotorsaken: null-värden har angetts i obligatoriska fält och klarade inte valideringen under hello åtgärden för att spara.

![Diagnostiksökning](./media/app-insights-proactive-failure-diagnostics/051.png)

## <a name="review-recent-alerts"></a>Granska de senaste aviseringarna

Klicka på **Smart identifiering** tooget toohello senaste avisering:

![Sammanfattning av aviseringar](./media/app-insights-proactive-failure-diagnostics/070.png)


## <a name="whats-hello-difference-"></a>Vad är skillnaden hello...
Smart identifiering av fel avvikelser kompletterar andra liknande men olika funktioner i Application Insights.

* [Mått aviseringar](app-insights-alerts.md) anges av du och kan övervaka en mängd olika mått, till exempel CPU användandet begäran priser, sidinläsningstider och så vidare. Du kan använda dem toowarn du till exempel om du behöver tooadd mer resurser. Däremot omfattar Smart identifiering av fel avvikelser en liten uppsättning viktiga mått (för närvarande endast misslyckade förfrågningar), utformats toonotify dig i nära realtid sätt när misslyckade förfrågningar för ditt webbprogram ökar avsevärt jämfört med tooweb app normalt beteende.

    Smart identifiering justeras automatiskt sitt tröskelvärde i svaret tooprevailing villkor.

    Smart identifiering startar hello diagnostiska fungerar.
* [Identifiering av prestandaavvikelser för smartkort](app-insights-proactive-performance-diagnostics.md) också använder datorn intelligence toodiscover ovanliga mönster i din mått och krävs ingen konfiguration av dig. Men till skillnad från Smart identifiering av fel avvikelser hello Smart identifiering av prestandaavvikelser syftar toofind segment för din användning samlingsrör som felaktigt kan hanteras, till exempel specifika sidor på en viss typ av webbläsare. hello analys utförs varje dag och om något resultat hittas, är det troligt toobe mycket mindre brådskande än en avisering. Däremot hello analys för fel avvikelser utförs kontinuerligt inkommande telemetri och du meddelas inom minuter om priser för server-felet är större än förväntat.

## <a name="if-you-receive-a-smart-detection-alert"></a>Om du får en avisering om smarta identifiering
*Varför fått den här aviseringen?*

* Vi har upptäckt en onormal ökning av misslyckade begäranden hastighet jämfört med toohello normal baslinje för hello föregående period. Vi tror att det finns ett problem som bör du fundera på att efter analys av hello fel och associerade telemetri.

*Innebär hello-meddelande jag definitivt har problem?*

* Vi försöker tooalert på app avbrott eller försämring, men bara du kan förstå hello semantik och hello påverkan på hello app eller användare.

*Så är fallet bör du guys tittar på Mina data?*

* Nej. hello-tjänsten är helt automatisk. Bara hämta hello meddelanden. Dina data är [privata](app-insights-data-retention-privacy.md).

*Måste jag toosubscribe toothis aviseringen?*

* Nej. Alla program som skickar begärande telemetri har hello Smart avisering identifieringsregeln.

*Kan jag säga upp prenumerationen eller hämta hello-meddelanden som skickas toomy kollegor i stället?*

* Ja, i Varningsregler, klicka på hello Smart identifiering regeln tooconfigure den. Du kan inaktivera hello aviseringen eller ändra mottagarna för hello aviseringen.

*Gick förlorad hello e-post. Var hittar jag hello meddelanden i hello portal?*

* I hello aktivitetsloggar. Öppna hello Application Insights-resurs för din app i Azure, och välj aktivitetsloggar.

*Vissa av hello aviseringar om kända problem och vill inte tooreceive dem.*

* Vi har aviseringsundertryckning på vår eftersläpning.

## <a name="next-steps"></a>Nästa steg
Dessa verktyg för Nätverksdiagnostik hjälpa dig att inspektera hello telemetri från din app:

* [Mått explorer](app-insights-metrics-explorer.md)
* [Sök explorer](app-insights-diagnostic-search.md)
* [Analytics - frågespråket kraftfulla](app-insights-analytics-tour.md)

Smart identifieringar är helt automatisk. Men du kanske vill tooset in några fler aviseringar?

* [Manuellt konfigurerade mått aviseringar](app-insights-alerts.md)
* [Tillgänglighetstester för webbprogram](app-insights-monitor-web-app-availability.md)
