---
title: "aaaOverview av Azure Application Insights för DevOps | Microsoft Docs"
description: "Lär dig hur toouse Application Insights i en Dev Ops-miljö."
author: CFreemanwa
services: application-insights
documentationcenter: 
manager: carmonm
ms.assetid: 6ccab5d4-34c4-4303-9d3b-a0f1b11e6651
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: bwren
ms.openlocfilehash: 42139f4645e815f26378726f4716a9bfbdc78551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-application-insights-for-devops"></a>Översikt över Application Insights för DevOps

Med [Programinsikter](app-insights-overview.md), du snabbt få reda på hur din app har dålig och som används när den är aktiv. Om det finns ett problem, får du reda på om den, kan du utvärdera effekten av hello och hjälper dig att avgöra hello orsak.

Här är ett konto från ett team som utvecklar webbprogram:

* *”Ett par dagar sedan, distribuerade vi en” mindre ”snabbkorrigering. Det gick inte att köra en bred testet, men vissa oväntad ändring har tyvärr samman i hello-nyttolast som orsakar inkompatibilitet mellan hello främre och bakre slutar. Direkt undantag ökade kraftigt, vår aviseringen utlöses och vi har gjorts medveten om hello situation. Några få klick bort på hello Application Insights-portalen kan vi har fått tillräckligt med information från undantag callstacks toonarrow ned hello problem. Vi återställde omedelbart och begränsad hello skador. Application Insights har gjort den här delen av hello devops växla mycket enkel och tillförlitlig ”.*

I den här artikeln vi följer ett team i Fabrikam Bank som utvecklar hello online bankwebbplatser system (OBS) toosee hur de använder Application Insights tooquickly svara toocustomers och göra uppdateringar.  

hello-teamet fungerar på en DevOps cykel avbildade i hello följande bild:

![DevOps cykel](./media/app-insights-detect-triage-diagnose/00-devcycle.png)

Krav som matas in deras utveckling eftersläpning (uppgiftslista). De fungerar i korthet sprints, vilket ofta ger fungerande programvara - vanligtvis i hello form av förbättringar och tillägg toohello befintliga program. hello aktiva app uppdateras regelbundet med nya funktioner. När den är aktiv övervakar hello team den om prestanda och användning med hello hjälp av Application Insights. APM-data feeds tillbaka till deras utveckling eftersläpning.

hello team använder Application Insights toomonitor hello live webbprogram nära för:

* Prestanda. De vill toounderstand hur svarstiderna varierar beroende på antalet begäranden; hur mycket CPU, nätverk, disk och andra resurser som används; och där hello flaskhalsar.
* Fel. Om det finns undantag eller misslyckade begäranden, eller om en prestandaräknare går utanför räckvidd föredrar, hello team behov tooknow snabbt så att de kan vidta åtgärder.
* Användning. När en ny funktion släpps hello team vill tooknow toowhat omfattning används, och om användare har problem med den.

Nu ska vi fokusera på hello feedback tillhör hello cykel:

![Identifiera-prioritering-diagnostisera](./media/app-insights-detect-triage-diagnose/01-pipe1.png)

## <a name="detect-poor-availability"></a>Identifiera dålig tillgänglighet
Marcela Markova är en erfaren utvecklare i hello OBS team och tar hello lead på övervaka online prestanda. Hon konfigurerar flera [tillgänglighetstester](app-insights-monitor-web-app-availability.md):

* En enskild URL-test för hello huvudsakliga Landningssida för hello app http://fabrikambank.com/onlinebanking/. Hon anger kriterierna för HTTP-kod 200 och text Välkommen!. Om det här testet misslyckas är något allvarligt fel med hello nätverk eller hello servrar eller kan vara ett distributionsproblem med. (Eller någon har ändrat hello Välkommen! meddelande på hello sidan utan att låta hennes informerad).
* En djupare flera steg testet som loggar in och hämtar en aktuella kontot lista, kontrollera några viktiga uppgifter på varje sida. Det här testet kontrollerar att hello länken toohello kontodatabasen fungerar. Hon använder fiktiva kund-id: några av dem bevaras för testning.

Med dessa tester som ställer in är Marcela säker på att hello team snabbt vet om eventuella avbrott.  

Fel visas som röda punkter i hello web test diagram:

![Visning av webbtester som har kört över hello föregående period](./media/app-insights-detect-triage-diagnose/04-webtests.png)

Men viktigare är en avisering om ett fel med e-post toohello Utvecklingsteamet. På så sätt kan känner de till den innan nästan alla hello kunder.

## <a name="monitor-performance"></a>Övervaka prestanda
På översiktssidan i Application Insights hello är ett diagram som visar olika [nyckeln mått](app-insights-web-monitor-performance.md).

![Olika mått](./media/app-insights-detect-triage-diagnose/05-perfMetrics.png)

Sidhämtningstid härleds från telemetri skickas direkt från webbsidor. Serversvarstid, antalet för server-begäran och antalet misslyckade begäranden alla mätt i hello webbservern och skickas tooApplication insikter därifrån.

Marcela är något berörda med hello server svar graph. Det här diagrammet visar hello Genomsnittlig tid mellan när hello servern tar emot en HTTP-begäran från en användares webbläsare och när den returnerar hello-svar. Det är inte ovanliga toosee variationen i det här diagrammet eftersom belastningen på hello system varierar. Men i det här fallet systemdiskar toobe en korrelation mellan små ökar i hello antal begäranden, och stor stiger hello svarstid. Som kan tyda på att hello system fungerar bara vid gränsen.

Hon öppnar hello servrar diagram:

![Olika mått](./media/app-insights-detect-triage-diagnose/06.png)

Det verkar toobe några tecken på begränsade resurser, så kanske hello stötar i hello server svar diagram är bara en plötslig.

## <a name="set-alerts-toomeet-goals"></a>Ställ in aviseringar toomeet mål
Dock vill hon tookeep koll på hello svarstider. Om de reser för hög vill hon tooknow om den omedelbart.

Så hon anger ett [avisering](app-insights-metrics-explorer.md), för svarstider som är större än ett tröskelvärde för vanliga. Detta ger sitt förtroende som hon vet om det. Om svarstider är långsam.

![Lägg till avisering bladet](./media/app-insights-detect-triage-diagnose/07-alerts.png)

Aviseringar kan ställas in på en mängd andra mått. Du kan till exempel ta emot e-postmeddelanden om hello undantag antal blir hög eller hello tillgängligt minne blir låg, eller om det är en topp klientbegäranden.

## <a name="stay-informed-with-smart-detection-alerts"></a>Håll dig informerad med aviseringar för Smart identifiering
Nästa dag, en varning e-post tas emot från Application Insights. Men när hon öppnas hon hittar inte hello svar tid aviseringen som hon har angetts. I stället den information om det har en plötslig ökning av misslyckade begäranden – det vill säga begäranden som har returnerats felkoder på 500 eller mer.

Misslyckade förfrågningar är där användare har sett ett fel - vanligtvis efter ett undantag uppstod i hello kod. Kanske ser de ett meddelande om ”tyvärr vi inte kunde uppdatera din information just nu”. Eller absolut onödiga sämsta en stackdump visas på skärmen hello användarens tillstånd hello webbservern.

Den här aviseringen är en oväntat eftersom hello senast hon tittar på det, hello misslyckade begäranden var antalet encouragingly låg. Ett litet antal fel är toobe förväntas i en upptagen server.

Det har också en oväntat för henne lite eftersom hon inte hade tooconfigure aviseringen. Application Insights innehålla smarta identifiering. Den justeras automatiskt tooyour app mönster av vanliga fel och ”hämtar används för att” fel på en viss sida eller under hög belastning eller länkade tooother mått. Den genererar hello larm om det finns en ökning kommer tooexpect ovan IT-avdelningen.

![proaktiv diagnostik e-post](./media/app-insights-detect-triage-diagnose/21.png)

Det här är en användbar e-post. Den öka inte bara larm. Det har mycket hello prioritering och diagnostik arbete för.

Den visar hur många kunder påverkas, och vilka webbsidor eller åtgärder. Marcela kan bestämma om hon tooget hello hela teamet arbetar på detta som en ökad fire eller om den kan ignoreras tills nästa vecka.

hello e-post visas också att en viss undantag uppstod och - ännu mer intressant - hello felet är associerad med misslyckade anrop tooa viss databas. Det förklarar varför hello fel plötsligt fanns även om Marcela's team inte har distribuerats eventuella uppdateringar nyligen.

Marcella pingar hello som styr hello databasen team baserat på den här e-post. Hon lär sig de tillgängliga en snabbkorrigering i hello senaste halvtimme; och OJ, kanske kan har en mindre schemaändring...

Hello problem är därför på hello sätt toobeing fast även innan du undersöker loggar och inom 15 minuter från det som följer. Dock klickar Marcela hello länken tooopen Application Insights. Öppnas direkt till en misslyckad begäran och hon kan se misslyckade anrop i hello associerade lista över beroendeanrop databasen.

![misslyckade begäranden](./media/app-insights-detect-triage-diagnose/23.png)

## <a name="detect-exceptions"></a>Identifiera undantag
Med lite installationen [undantag](app-insights-asp-net-exceptions.md) är rapporterade tooApplication insikter automatiskt. De kan också läggas till explicit genom att infoga anrop för[TrackException()](app-insights-api-custom-events-metrics.md#trackexception) till hello kod:  

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send hello exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }


hello Fabrikam Bank team har utvecklats hello praxis alltid skicka telemetri vid ett undantag såvida det inte finns en tydlig återställning.  

I själva verket sina strategin är även bredare än: de skicka telemetri i samtliga fall där hello kunden är frustrerade i vilka de ville toodo, om den motsvarar tooan undantag i hello-kod eller inte. Till exempel om hello externa mellan bank överföring system returnerar meddelandet ”Det går inte att slutföra den här transaktionen” önskemål operativa (inga fel hello kunden) spåra sedan de händelsen.

    var successCode = AttemptTransfer(transferAmount, ...);
    if (successCode < 0)
    {
       var properties = new Dictionary <string, string>
            {{ "Code", returnCode, ... }};
       var measurements = new Dictionary <string, double>
         {{"Value", transferAmount}};
       telemetry.TrackEvent("transfer failed", properties, measurements);
    }

TrackException är används tooreport undantag eftersom den skickar en kopia av hello stacken. TrackEvent är används tooreport andra händelser. Du kan koppla eventuella egenskaper som kan vara användbar vid diagnos.

Undantag och händelser som visas i hello [diagnostiska Sök](app-insights-diagnostic-search.md) bladet. Du kan detaljerat dem toosee hello ytterligare egenskaper och stackspårning.

![Använda filter tooshow vissa typer av data i diagnostiska sökning](./media/app-insights-detect-triage-diagnose/appinsights-333facets.png)


## <a name="monitor-proactively"></a>Övervaka proaktivt
Marcela sitta inte bara runt väntar på aviseringar. Strax efter varje Omdistributionen hon tar en titt på [svarstider](app-insights-web-monitor-performance.md) – både hello övergripande bild och hello tabell med långsammast begäranden som undantag räknas.  

![Svaret tid diagram och rutnät för serversvarstider.](./media/app-insights-detect-triage-diagnose/09-dependencies.png)

Hon kan bedöma hello prestanda effekten av varje distribution vanligtvis jämföra varje vecka med hello senast. Om det finns en plötslig försämras, genererar hon som med relevanta hello-utvecklare.

## <a name="triage-issues"></a>Prioritering problem
Prioritering - bedöma hello allvarlighetsgrad och omfattningen av ett problem - är hello görs efter identifiering. Ska vi kallar ut hello team vid midnatt? Eller kan den finnas kvar tills hello nästa praktiskt lucka i hello eftersläpning? Det finns några viktiga frågor i prioritering.

Hur ofta det händer? hello diagram på hello översikt bladet ge perspektiv tooa problem. Till exempel genereras hello Fabrikam programmet fyra web test varningar en natt. Tittar på hello diagram hello morgonen, hello gruppen kan se att det fanns verkligen vissa röda punkter om fortfarande de flesta hello tester vore grön. Vidaresökning i hello tillgänglighet diagram var det tydligt att alla dessa återkommande problem var från ett test-plats. Detta var naturligtvis ett nätverksproblem som påverkar endast en och troligen avmarkerar sig själv.  

Däremot dramatisk och stabil ökning hello diagram över antalet undantag eller svarstider är naturligtvis något toopanic om.

En användbar prioritering skräpposten är försök den själv. Om du stöter på hello samma problem du vet att det är verkliga.

Vilken del av användare som påverkas? tooobtain en grov svaret delar hello felintervall med hello session count.

![Diagram över misslyckade begäranden och sessioner](./media/app-insights-detect-triage-diagnose/10-failureRate.png)

När det finns långsamt svar, jämför hello tabell med långsammast svarar på begäranden med hello användning frekvensen för varje sida.

Det är viktigt hello blockeras scenario? Om det här är ett funktionellt problem som blockerar en viss användare artikel spelar det roll mycket? Om kunder inte kan betala för sina växlar, är det allvarliga; Om de inte kan ändra preferenser skärmen färg, kan kanske den vänta. Hej detail hello händelse eller undantag eller hello identitet hello långsam sida, anger du där kunder har problem med.

## <a name="diagnose-issues"></a>Diagnostisera problem
Diagnostik är inte helt hello samma som felsökning. Innan du börjar spårning genom hello koden du bör ha en grov uppfattning om varför, var och när hello problemet uppstår.

**När sker det?**  hello historiska vyn i hello händelser och mått diagram gör det enkelt toocorrelate effekter med möjliga orsaker. Om det finns återkommande toppar i svaret tid eller undantag priser, titta på hello begäran antal: om den toppar som pekar åt på hello samma tid, och sedan det ser ut som ett resursproblem. Behöver du tooassign mer CPU eller minne? Eller är ett beroende som inte kan hantera belastningen hello?

**Är det oss?**  Om du har en plötslig nedgång i prestanda för en viss typ av begäran - till exempel när hello kunden uttrycket konto - och sedan är möjligheten kan det vara ett externt undersystem i stället för ditt webbprogram. Välj hello beroendefel frekvens och varaktighet för beroende priser och jämföra deras historik över hello efter några timmar eller dagar med hello-problemet du identifierade i Metrics Explorer. Om det korrelerar ändringar, kan en extern undersystemet vara tooblame.  

![Diagram över beroendefel och varaktighet för anrop toodependencies](./media/app-insights-detect-triage-diagnose/11-dependencies.png)

Vissa problem med långsam beroende är geolokalisering problem. Fabrikam banken använder Azure virtuella datorer och identifierade att de hade oavsiktligt finns sina webbservern och kontot server i olika länder. En dramatisk förbättring har medfört genom att migrera en av dem.

**Vad vi?** Om hello problemet inte visas toobe ett beroende, och om det inte alltid det, beror det förmodligen på en ändring. hello historiska perspektiv som tillhandahålls av hello mått och händelse diagram gör det enkelt toocorrelate ändringar plötslig med distributioner. Som begränsar ned hello letar hello problem.

**Vad är det som händer?** Vissa problem uppstå sällan och kan vara svårt tootrack ned genom att testa offline. Vi kan göra bara tootry toocapture hello programfel när det uppstår live. Du kan inspektera hello stackdump i undantag rapporter. Dessutom kan du skriva spårning anrop med din favorit loggningsramverk eller med TrackTrace() eller trackevent ().  

Fabrikam hade ett tillfälligt problem med överföringar mellan kontot, men endast med vissa typer av konton. toounderstand bättre vad händer infogas de TrackTrace() samtal vid huvudpunkter i hello kod, bifoga hello kontotyp som ett anrop för egenskapen tooeach. Som gjort det enkelt toofilter ut de spårningar diagnostiska sökning. De anslutna också parametervärden som egenskaper och åtgärder toohello spårning av anrop.

## <a name="respond-toodiscovered-issues"></a>Svara toodiscovered problem
När du har diagnostiserats hello problem du kan göra en plan toofix den. Du kanske behöver tooroll tillbaka en ändring eller kanske du kan bara gå vidare och åtgärda problemet. När hello korrigering är klart får Application Insights du om du är klar.  

Fabrikam Bank Utvecklingsteamet ta ett mer strukturerade metoden tooperformance mått än de använde toobefore de använde Application Insights.

* De kan ange prestandamål vad gäller särskilda åtgärder för hello Application Insights översiktssidan.
* De utforma åtgärderna som utförs i hello program från hello start, till exempel hello mått som mäter användaren förloppet skorstenar.  


## <a name="monitor-user-activity"></a>Övervakaren användaraktivitet
När svarstiden är konsekvent bra och få undantag, flytta hello dev-teamet på toousability. De kan fundera på hur tooimprove hello användarnas upplevelse och hur tooencourage flera användare tooachieve hello önskad mål.

Application Insights kan också vara används toolearn vad användare göra med en app. När den körs smidigt som hello team tooknow vilka funktioner som är mest populär hello vad användare gillar och har problem med och hur ofta de kommer tillbaka. Som hjälper dem att prioritera kommande arbetet. Och de kan planera toomeasure hello framgången för varje funktion som en del av hello utvecklingscykeln. 

En typisk användare resa via hello-webbplats har till exempel en tydlig ”tratten”. Många kunder titta på hello frekvens av olika typer av lån. Ett mindre antal gå toofill i hello citattecken form. Av de som får en offert några gå vidare och ta ut hello lån.

![Vyn sida räknar](./media/app-insights-detect-triage-diagnose/12-funnel.png)

Genom att beakta där hello största antal kunder släppa, fungerar hello business ut hur tooget fler användare via toohello längst ned på hello tratt. Det kan finnas ett user experience (UX) fel i vissa fall – till exempel hello ' Nästa ' är hårda toofind eller hello instruktioner inte tydligt. Det finns mer sannolikt större affärsskäl för bortfall: kanske hello lån är för högt.

Oavsett hello skäl, hjälper hello data hello-teamet arbetar reda på vad användarna gör. Flera spårning anrop kan vara infogas toowork mer detaljer. Trackevent () kan vara används toocount några användaråtgärder från hello detaljnivå av enskilda knappen klickar toosignificant prestationer, till exempel betala av ett lån.

hello-teamet får använda toohaving information om användaraktivitet. Idag används när de utformar en ny funktion, fungerar de reda på hur de ska få feedback om dess användning. De utforma spårning anrop till hello funktionen från hello start. De funktionen hello feedback tooimprove hello i varje utvecklingscykeln.

[Läs mer om att spåra användning](app-insights-usage-overview.md).

## <a name="apply-hello-devops-cycle"></a>Tillämpa hello DevOps cykel
Detta är hur ett team att använda Application Insights inte bara toofix enskilda problem, men tooimprove utvecklingslivscykeln. Jag hoppas det du har fått några tips om hur Application Insights kan hjälpa dig med hantering av prestanda i dina program.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nästa steg
Du kan komma igång på flera sätt, beroende på hello egenskaper för programmet. Välj vad passar dig bäst:

* [ASP.NET-webbprogram](app-insights-asp-net.md)
* [Java-webbapp](app-insights-java-get-started.md)
* [Node.js-webbapp](app-insights-nodejs.md)
* Redan distribuerade appar, finns på [IIS](app-insights-monitor-web-app-availability.md), [J2EE](app-insights-java-live.md), eller [Azure](app-insights-azure.md).
* [Webbsidor](app-insights-javascript.md) -den enda sidan App eller vanlig webbsida - använder det på sin egen eller i tillägg tooany av hello serveralternativ.
* [Tillgänglighetstester](app-insights-monitor-web-app-availability.md) tootest appen från hello offentliga internet.
