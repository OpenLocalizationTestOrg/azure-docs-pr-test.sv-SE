---
title: aaaSmart identifiering - prestandaavvikelser | Microsoft Docs
description: "Application Insights utför smart analys av din app telemetri och varnar dig om potentiella problem. Den här funktionen behöver ingen installation."
services: application-insights
documentationcenter: windows
author: antonfrMSFT
manager: carmonm
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: bwren
ms.openlocfilehash: 60f10612188920330030129f7464e2f398ef1f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection---performance-anomalies"></a>Identifiering - Prestandaavvikelser för smartkort

[Application Insights](app-insights-overview.md) automatiskt analyserar hello prestanda för ditt webbprogram och varna dig om potentiella problem. Du kanske att läsa det eftersom du har fått en av våra meddelanden för smart identifiering.

Den här funktionen kräver några särskilda inställningar, förutom att konfigurera din app för Application Insights (på [ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), eller [Node.js](app-insights-nodejs.md), och i [webbsida kod](app-insights-javascript.md)). Det är aktivt när genererar tillräckligt med telemetri för din app.

## <a name="when-would-i-get-a-smart-detection-notification"></a>När kan jag får ett meddelande om smarta identifiering?

Application Insights har upptäckt att hello prestanda har degraderats i något av följande sätt:

* **Svaret tid försämring** -appen har startats svarar toorequests långsammare än det tidigare. hello ändring kan ha blivit snabb, till exempel eftersom det var en regressionsmodell i senaste distributionen. Eller så kanske har blivit gradvis, kanske på grund av en minnesläcka. 
* **Beroende varaktighet försämring** -appen gör anrop tooa REST API, databaser eller andra beroende. hello beroende svarar långsammare än det tidigare.
* **Långsam mönster** – appen visas toohave en prestanda problem som påverkar endast vissa begäranden. Till exempel inläsning sidor långsammare på en typ av webbläsare än andra. eller begäranden behandlas långsammare från en viss server. För närvarande titta våra algoritmer på sidinläsningstider, begäran svarstider och beroende svarstider.  

Smart identifiering kräver minst 8 dagar telemetri vid en fungerande volym i ordning tooestablish en baslinje för normal prestanda. När programmet har körts under den tid, så resulterar eventuella viktiga problem i ett meddelande.


## <a name="does-my-app-definitely-have-a-problem"></a>Har min app definitivt problem?

Nej, ett meddelande innebär inte att appen verkligen har problem. Det är bara ett förslag om något du kanske vill toolook på närmare.

## <a name="how-do-i-fix-it"></a>Hur kan jag göra?

hello meddelanden innehåller diagnostisk information. Här är ett exempel:


![Här är ett exempel på servern svar tid försämring identifiering](./media/app-insights-proactive-diagnostics/server_response_time_degradation.png)

1. **Prioritering**. hello-meddelande visar hur många användare eller hur många åtgärder som påverkas. Detta kan hjälpa dig att tilldela en prioritet toohello problem.
2. **Scope**. Hello problem som påverkar all trafik, eller bara vissa sidor Är begränsad it tooparticular webbläsare eller platser? Den här informationen kan hämtas från hello-meddelande.
3. **Diagnostisera**. Ofta föreslås hello diagnostisk information i hello-meddelande hello uppbyggnad hello problem. Om svarstiden saktar ned när frågehastigheten är för hög, föreslår som till exempel din server eller beroenden är överbelastad. 

    I annat fall öppna hello prestanda bladet i Application Insights. Där hittar du [Profiler](app-insights-profiler.md) data. Om undantag utlöses, du kan också försöka hello [ögonblicksbild felsökare](app-insights-snapshot-debugger.md).



## <a name="configure-email-notifications"></a>Konfigurera e-postaviseringar

Meddelanden för smart identifiering är aktiverat som standard och skickas toothose som har [ägare, deltagare och läsare åt toohello Application Insights-resurs](app-insights-resources-roles-access-control.md). toochange detta, antingen på **konfigurera** hello e-postmeddelande eller öppna smarta identifieringsinställningar i Application Insights. 
  
  ![Identifieringsinställningar för smartkort](./media/app-insights-proactive-diagnostics/smart_detection_configuration.png)
  
  * Du kan använda hello **avbryta prenumerationen** länken i hello Smart identifiering e-toostop hello e-postmeddelanden.

E-post om identifieringar som Smart prestandaavvikelser är begränsad tooone e-post per dag per Application Insights-resurs. hello e-post skickas endast om det finns minst en ny problem som har identifierats på den dagen. Du kommer inte upprepas för varje meddelande. 

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

* *Så är fallet bör du guys tittar på Mina data?*
  * Nej. hello-tjänsten är helt automatisk. Bara hämta hello meddelanden. Dina data är [privata](app-insights-data-retention-privacy.md).
* *Du analysera alla hello data som samlas in av Application Insights?*
  * Inte för närvarande. För närvarande kan analyserar vi begäran svarstid, beroende-svarstid och sidan hämtningstid. Analys av ytterligare mått finns på vår eftersläpning väntar.

* Vilka typer av program fungerar detta för?
  * Dessa degradations identifieras i alla program som genererar hello lämpliga telemetri. Om du har installerat Application Insights i ditt webbprogram sedan spåras begäranden och beroenden automatiskt. Men i backend-tjänster eller andra appar, om du har infogat anrop för[TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) eller [TrackDependency](app-insights-api-custom-events-metrics.md#trackdependency), och sedan Smart identifiering fungerar i hello samma sätt.

* *Kan jag skapa egna avvikelseidentifiering regler för identifiering eller anpassa befintliga regler?*

  * Inte ännu, men du kan:
    * [Ställa in aviseringar](app-insights-alerts.md) som anger om måttet överskrider ett tröskelvärde.
    * [Exportera telemetri](app-insights-export-telemetry.md) tooa [databasen](app-insights-code-sample-export-sql-stream-analytics.md) eller [tooPowerBI](app-insights-export-power-bi.md), där du kan analysera dem själv.
* *Hur ofta utförs hello analys?*

  * Vi kör hello analys dagligen hello telemetri från hello föregående dag (fullständig dag i UTC-tidszonen).
* *Det gör även detta ersätta [mått aviseringar](app-insights-alerts.md)?*
  * Nej.  Vi genomföra inte toodetecting varje beteende som du kan överväga att onormal.


* *Om jag inte göra någonting i respons tooa meddelande kommer jag en påminnelse?*
  * Nej, du får ett meddelande om varje problemet bara en gång. Om hello problemet kvarstår kommer att uppdateras i hello Smart identifiering feed bladet.
* *Gick förlorad hello e-post. Var hittar jag hello meddelanden i hello portal?*
  * Klicka på hello i hello Application Insights översikten över appen, **Smart identifiering** panelen. Det kommer du att kunna toofind alla meddelanden in too90 dagar tillbaka.

## <a name="how-can-i-improve-performance"></a>Hur kan jag för att förbättra prestanda?
Långsam och misslyckade svar är en av hello största frustrationen för webbplatsanvändare som du känner till från din egen erfarenheter. Det är så viktigt tooaddress hello problem.

### <a name="triage"></a>Prioritering
Först spelar det roll? Om en sida är alltid långsam tooload, men endast 1% av webbplatsens användare har toolook den, kanske har du viktigare saker toothink om. På hello däremot om endast 1% av användare öppna det, men varje gång, kan det vara värt att undersöka den utlöser undantag.

Använda hello påverkan-uttryck (användare som påverkas eller % av trafik) som en allmän vägledning, men tänk på att den inte hello hela artikeln. Samla in andra bevis tooconfirm.

Överväg att hello parametrarna för hello problemet. Om det är beroende geografi, ställa in [tillgänglighetstester](app-insights-monitor-web-app-availability.md) inklusive den regionen: det kanske bara nätverksproblem i detta område.

### <a name="diagnose-slow-page-loads"></a>Diagnostisera långsam sidan läses in
Där är hello problem? Är hello servern långsam toorespond, är mycket lång hello-sida eller hello webbläsare har toodo mycket arbete toodisplay den?

Öppna hello webbläsare mått bladet. hello segmenterat webbläsare belastningen tid visar vart hello tid är på väg att visa. 

* Om **skicka tid för begäran** är hög, antingen hello servern svarar långsamt eller hello-begäran är en post med stora mängder data. Titta på hello [prestandamått](app-insights-web-monitor-performance.md#metrics) tooinvestigate svarstider.
* Ställ in [beroende spårning](app-insights-asp-net-dependencies.md) toosee om hello långsamt arbete är på grund av tooexternal tjänster eller din databas.
* Om **ta emot svar** är övervägande, sidan och dess beroende delar - JavaScript, CSS, bilder och så vidare (men inte asynkront inlästa data) är för lång. Konfigurera en [tillgänglighetstest](app-insights-monitor-web-app-availability.md), och vara säker på att tooset hello alternativet tooload beroende delar. När du får några resultat kan öppna hello detaljerat resultat och expandera den toosee hello ladda gånger olika filer.
* Hög **klienten bearbetningstid** föreslår skript körs långsamt. Om hello orsak inte visas, Överväg att lägga till kod tidsinställning och skicka hello gånger i trackMetric-anrop.

### <a name="improve-slow-pages"></a>Förbättra långsamma sidor
Det finns en fullständig råd om att förbättra din serversvar och sidinläsningstider, så vi inte försöka toorepeat webbplats alla här. Här följer några tips du förmodligen redan vet om bara tooget funderar du:

* Långsam lästes in på grund av stora filer: Läs in hello skript och andra delar asynkront. Använd skriptet paketering. Bryt hello huvudsidan i widgetar som läser in sina data separat. Skicka inte vanlig gamla HTML för långa tabeller: använda ett skript toorequest hello data som JSON- eller andra komprimerat format och sedan fylla hello tabellen på plats. Det är bra ramverk toohelp med allt detta. (De även innebära ett stort skript naturligtvis.)
* Långsamma server beroenden: Överväg att hello geografiska platser av komponenter. Till exempel om du använder Azure Kontrollera hello webbservern och hello databasen är i hello samma region. Frågor ska hämta mer information än de behöver? Skulle cachelagrar eller batchbearbetning hjälp?
* Kapacitet problem: Leta hello serverstatistik av svarstider och begäran räknas. Om svarstider högsta oproportionerligt med toppar i begäran räknas, är det troligt att servrarna har sträckts ut.


## <a name="server-response-time-degradation"></a>Servern svar tid försämring

hello svar tid försämring meddelandet kan du se:

* hello svarstid jämfört med toonormal svarstid för den här åtgärden.
* Hur många användare påverkas.
* Genomsnittlig svarstid och 90: e percentilen svarstiden för den här åtgärden hello dag av hello identifiering och 7 dagar innan. 
* Antalet åtgärden förfrågningar på hello dag i hello identifiering och 7 dagar innan.
* Korrelation mellan försämring i den här åtgärden och degradations i relaterade beroenden. 
* Länkar toohelp du diagnostisera hello problem.
  * Profileraren spårar toohelp du visa där åtgärden tid det tar (hello länk är tillgänglig om profileraren trace exempel samlades in för den här åtgärden under identifieringsperioden hello). 
  * Prestandarapporter i måttet Explorer, där du kan statistikforskning tid intervallfilter för den här åtgärden.
  * Sök efter detta anropar tooview specifika anrop egenskaper.
  * Fel rapporteras - om räkna > 1 detta innebära att det inträffade fel under den här åtgärden som kanske har bidragit tooperformance försämras.

## <a name="dependency-duration-degradation"></a>Beroende varaktighet försämring

Moderna program anta mer och mer micro services Designmetoden, vilket i många fall leder tooheavy tillförlitlighet på externa tjänster. Till exempel om programmet är beroende av vissa dataplattform eller om du skapar egna bot tjänst du förmodligen vidarebefordrar på vissa kognitiva services provider tooenable din robotar toointeract mer mänsklig sätt och vissa data lagrar tjänst för bot toopull hello svar från.  

Exempel beroende omvandlings-meddelande:

![Här är ett exempel på beroende varaktighet försämring identifiering](./media/app-insights-proactive-diagnostics/dependency_duration_degradation.png)

Observera att du får meddelande:

* hello varaktighet jämfört med toonormal svarstid för den här åtgärden
* Hur många användare som påverkas
* Genomsnittlig varaktighet och 90: e percentilen varaktigheten för detta beroende på hello dag i hello identifiering och 7 dagar innan
* Antal beroende anrop på hello dag i hello identifiering och 7 dagar innan
* Länkar toohelp du diagnostisera hello problem
  * Prestandarapporter i måttet Explorer för detta beroende
  * Sök efter detta beroende anropar tooview anrop egenskaper
  * Fel rapporter - om antalet > 1 betyder att det fanns misslyckade beroende anropar under hello identifiering period som kanske har bidragit tooduration försämras. 
  * Öppna Analytics med frågor som utför den här beroende varaktighet och antal  

## <a name="smart-detection-of-slow-performing-patterns"></a>Smart identifiering av långsam prestanda mönster 

Application Insights söker efter prestandaproblem som påverkar endast en del av användarna eller påverkar endast användare i vissa fall. Till exempel meddelanden om sidor belastningen är slowler på en typ av webbläsare än på andra typer av webbläsare, eller om begäranden som betjänats långsammare från en viss server. Problem med kombinationer av egenskaper, kan också identifieras som långsam sidan läses in i ett geografiskt område för klienter som använder operativsystemet.  

Avvikelser som dessa är väldigt svår toodetect genom att kontrollera hello data, men är vanligare än du tror. De yta ofta endast när dina kunder klagar. Vid den tidpunkten är för sent: hello påverkade användare redan byter tooyour konkurrenter!

För närvarande titta våra algoritmer på sidinläsningstider och begäran svarstider på hello servern beroende svarstider.  

Du inte har tooset eventuella tröskelvärden eller konfigurera regler. Machine learning och datautvinningsalgoritmer kan du använda toodetect onormala mönster.

![Klicka på hello länken tooopen hello diagnostikrapport i Azure hello e-postavisering](./media/app-insights-proactive-performance-diagnostics/03.png)

* **När** visar hello tid hello problemet upptäcktes.
* **Vad** beskrivs:

  * hello problem som har identifierats;
  * hello-egenskaperna hos hello uppsättning händelser som påträffades visas hello problemet.
* hello tabell jämförs hello ineffektivt fungerande uppsättning med hello genomsnittlig beteendet för alla andra händelser.

Klicka på hello länkar tooopen mått Explorer och Sök på relevanta rapporter, filtreras i hello tid och egenskaperna för hello långsamt utför uppsättningen.

Ändra hello tid intervall och filter tooexplore hello telemetri.

## <a name="next-steps"></a>Nästa steg
Dessa verktyg för Nätverksdiagnostik hjälpa dig att inspektera hello telemetri från din app:

* [Profiler](app-insights-profiler.md) 
* [Snapshot-felsökare](app-insights-snapshot-debugger.md)
* [Analys](app-insights-analytics-tour.md)
* [Diagnostik för Analytics-smartkort](app-insights-analytics-diagnostics.md)

Smart identifieringar är helt automatisk. Men du kanske vill tooset in några fler aviseringar?

* [Manuellt konfigurerade mått aviseringar](app-insights-alerts.md)
* [Tillgänglighetstester för webbprogram](app-insights-monitor-web-app-availability.md)
