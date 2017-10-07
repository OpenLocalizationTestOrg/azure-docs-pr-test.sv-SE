---
title: "aaaData kvarhållning och lagring i Azure Application Insights | Microsoft Docs"
description: "Kvarhållning och sekretess Principframställning"
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: a6268811-c8df-42b5-8b1b-1d5a7e94cbca
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: bwren
ms.openlocfilehash: 7823431d03a57db5268d2724a0604e40666009f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Datainsamling, kvarhållning och lagring i Application Insights


När du installerar [Azure Application Insights] [ start] SDK i din app, skickas telemetri om din app toohello moln. Naturligtvis finns vill ansvarar utvecklare tooknow exakt vilka data som skickas, vad som händer toohello data och hur de kan behålla kontrollen över den. I synnerhet kunde känsliga data skickas, där är den lagrade och hur säker är det? 

Först hello kort svar:

* hello standard telemetri moduler som kör ”out of box hello” är inte troligt toosend känsliga data toohello service. hello telemetri är bekymrad över belastning, prestanda och användning mått, undantag rapporter och andra diagnostikdata. hello huvudanvändaren data visas i hello diagnostiska rapporter är URL: er; men din app i varje fall inte bör Placera känsliga data i oformaterad text i en URL.
* Du kan skriva kod som skickar ytterligare telemetri om anpassade toohelp till diagnostik- och användningsdata för övervakning. (Den här utökningsbarhet är en bra funktion i Application Insights.) Det skulle vara möjligt, av misstag, toowrite detta kod så att den innehåller personlig och andra känsliga data. Om ditt program fungerar med sådana uppgifter, installerar du en omfattande granska processer tooall hello koden du skriver.
* När du utvecklar och testar din app, är det enkelt tooinspect vad som ska skickas av hello SDK. hello data visas i hello felsökning utdata windows hello IDE och webbläsare. 
* hello data lagras i [Microsoft Azure](http://azure.com) servrar i hello USA eller Europa. (Men din app kan köras var som helst.) Azure har [stark säkerhet processer och uppfyller ett brett spektrum av efterlevnadsstandarder](https://azure.microsoft.com/support/trust-center/). Endast du och din avsedda grupp har åtkomst till tooyour data. Microsoft-Personal kan ha begränsad åtkomst tooit endast under vissa begränsade omständigheter med dina kunskaper. Den är krypterad under överföringen, dock inte i hello-servrar.

hello resten av den här artikeln utvecklar mer komplett svaren på dessa frågor. Det har utformats för toobe självständiga, så att du kan visa den toocolleagues som inte är en del av din arbetsgrupp.

## <a name="what-is-application-insights"></a>Vad är Application Insights?
[Azure Application Insights] [ start] är en tjänst från Microsoft som hjälper dig att förbättra hello prestanda och användbarhet för live programmet. Det övervakar programmet alla hello gång den körs, både under testningen och när du har publicerat eller distribuera den. Application Insights skapar diagram och tabeller som visar, till exempel vilka tidpunkter på dagen som de flesta användare hur responsiv hello appen är och hur den hanteras av en extern källa för tjänster som den är beroende. Om det finns krascher, fel eller prestandaproblem kan söka du igenom hello telemetridata i detalj toodiagnose hello orsak. Och hello tjänsten skickar e-postmeddelanden om det finns ändringar i hello tillgänglighet och prestanda för din app.

I ordning tooget den här funktionen kan du installera en Application Insights SDK i ditt program blir en del av koden. När appen körs hello SDK övervakar driften och skickar telemetri toohello Application Insights-tjänsten. Det här är en molntjänst av [Microsoft Azure](http://azure.com). (Men Application Insights fungerar för alla program, inte bara de som finns i Azure.)

![hello SDK i din app skickar telemetri toohello Application Insights-tjänsten.](./media/app-insights-data-retention-privacy/01-scheme.png)

hello Application Insights-tjänsten lagrar och analyserar hello telemetri. toosee hello analys eller Sök genom hello lagras telemetri du loggar in tooyour Azure-konto och öppna hello Application Insights-resurs för ditt program. Du kan också dela access toohello data med andra medlemmar i gruppen, eller med angivna Azure-prenumeranter.

Du kan ha data exporterats från hello Application Insights-tjänsten, till exempel tooa databas eller tooexternal verktyg. Du kan ange varje verktyg med en särskild nyckel som du har fått från hello-tjänsten. hello nyckel kan återkallas om det behövs. 

Application Insights SDK är tillgängliga för en mängd olika typer: webbtjänster som finns i din egen J2EE eller ASP.NET-servrar eller i Azure; webbklienter – det vill säga hello kod som körs på en webbsida; -program och tjänster. appar för enheter, till exempel Windows Phone, iOS och Android. Alla skicka telemetri toohello samma tjänst.

## <a name="what-data-does-it-collect"></a>Vilka data samlar det?
### <a name="how-is-hello-data-is-collected"></a>Hur är hello data samlas in?
Det finns tre datakällor för data:

* hello SDK, som du integrerar med din app antingen [under utveckling](app-insights-asp-net.md) eller [vid körning](app-insights-monitor-performance-live-website-now.md). Det finns olika SDK: er för olika programtyper. Det finns också en [SDK för webbsidor](app-insights-javascript.md), som läses in i hello slutanvändarens webbläsare tillsammans med hello-sidan.
  
  * Varje SDK innehåller ett antal [moduler](app-insights-configuration-with-applicationinsights-config.md), som använder olika tekniker toocollect olika typer av telemetri.
  * Om du installerar hello SDK under utveckling, kan du använda dess API toosend egna telemetri i tillägg toohello standard moduler. Den här telemetri om anpassade kan innehålla alla data som du vill toosend.
* I vissa webbservrar finns också agenter som kör tillsammans med hello app och skicka telemetri om CPU, minne och användandet av nätverket. Till exempel virtuella Azure-datorer Docker-värdar och [J2EE servrar](app-insights-java-agent.md) kan ha dessa agenter.
* [Tillgänglighetstester](app-insights-monitor-web-app-availability.md) processer som körs av Microsoft som skickar begäranden tooyour webbprogrammet med jämna mellanrum. hello resultat skickas toohello Application Insights-tjänsten.

### <a name="what-kinds-of-data-are-collected"></a>Vilka typer av data samlas in?
hello huvudkategorier är:

* [Web server telemetri](app-insights-asp-net.md) -HTTP-begäranden.  URI tidsåtgång tooprocess hello begäran, svarskod, klientens IP-adress. Sessions-id.
* [Webbsidor](app-insights-javascript.md) -sidan, användare och session räknas. Sidinläsningstider. Undantag. AJAX-anrop.
* Prestandaräknare - minne, CPU, IO, användandet av nätverket.
* Klienten och servern kontext - OS, språk, typ av enhet, webbläsare, skärmupplösning.
* [Undantag](app-insights-asp-net-exceptions.md) och krascher - **stacken Dumpar**, skapa id, processortyp. 
* [Beroenden](app-insights-asp-net-dependencies.md) -tooexternal tjänster, till exempel vila, SQL, AJAX-anrop. URI: N eller anslutningssträngen, varaktighet, lyckas, kommandot.
* [Tillgänglighetstester](app-insights-monitor-web-app-availability.md) -varaktighet för test och steg, svar.
* [Spårningsloggar](app-insights-asp-net-trace-logs.md) och [telemetri om anpassade](app-insights-api-custom-events-metrics.md) - **något du kod till din loggar eller telemetri**.

[Mer information om](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>Hur kan jag bekräfta vad samlas?
Om du utvecklar hello-app med Visual Studio kör hello appen i felsökningsläge (F5). hello telemetri visas i utdatafönstret hello. Därifrån kan du kopiera den och formatera den som JSON för enkel kontroll. 

![](./media/app-insights-data-retention-privacy/06-vs.png)

Det finns också en mer lättläst vy i hello Diagnostics-fönstret.

Öppna din webbläsare felsökning fönster för webbsidor.

![Trycka på F12 och öppna fliken för hello-nätverk.](./media/app-insights-data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-toofilter-hello-telemetry-before-it-is-sent"></a>Kan jag skriva kod toofilter hello telemetri innan den skickas?
Detta skulle vara möjligt genom att skriva en [telemetri processor plugin](app-insights-api-filtering-sampling.md).

## <a name="how-long-is-hello-data-kept"></a>Hur lång tid är hello data sparas?
Rådata datapunkter (objekt som du kan fråga i Analytics och inspektera i sökningen) hålls för in too90 dagar. Om du behöver tookeep data längre än den som du kan använda [löpande export](app-insights-export-telemetry.md) toocopy den tooa storage-konto.

Sammanställda data (det vill säga antal, medelvärden och andra statistiska data som du ser i måttet Explorer) finns kvar på en kornighet på 1 minut under 90 dagar.

## <a name="who-can-access-hello-data"></a>Vem som kan komma åt hello data?
hello data är synliga tooyou och, om du har en organisationskonto gruppmedlemmarna. 

Det kan exporteras av dig och dina gruppmedlemmar och kan vara kopierade tooother platser och vidarebefordras på tooother personer.

#### <a name="what-does-microsoft-do-with-hello-information-my-app-sends-tooapplication-insights"></a>Vad har Microsoft göra med hello information min app skickar tooApplication insikter?
Microsoft använder hello data endast i ordning tooprovide hello tjänsten tooyou.

## <a name="where-is-hello-data-held"></a>Där lagras hello data?
* I hello USA eller Europa. Du kan välja hello plats när du skapar en ny Application Insights-resurs. 


#### <a name="does-that-mean-my-app-has-toobe-hosted-in-hello-usa-or-europe"></a>Betyder det min app har toobe finns i hello USA eller Europa?
* Nej. Programmet kan köras var som helst, i din egen lokala värdar eller i hello molnet.

## <a name="how-secure-is-my-data"></a>Hur säker är Mina data?
Application Insights är en Azure-tjänst. Säkerhetsprinciper beskrivs i hello [Azure-säkerhet, sekretess och kompatibilitet vitboken](http://go.microsoft.com/fwlink/?linkid=392408).

hello data lagras i Microsoft Azure-servrar. Begränsningar för konton i hello Azure Portal beskrivs i hello [Azure-säkerhet, sekretess och kompatibilitet dokumentet](http://go.microsoft.com/fwlink/?linkid=392408).

Åtkomst till tooyour data av Microsoft-Personal är begränsad. Vi har åtkomst till data tillåtelse och om det är nödvändigt toosupport din användning av Application Insights. 

Data i mängd i våra kunders program (till exempel överföringshastighet och genomsnittlig storlek på spårningar) är används tooimprove Application Insights.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>Det gick någon annans telemetri som störa Application Insights-data?
De kan skicka ytterligare telemetri tooyour konto med hjälp av hello instrumentation nyckel som finns i hello kod webbsidor. Med tillräckligt med ytterligare data skulle dina inte korrekt återger appens prestanda och användning.

Om du delar kod med andra projekt kan du komma ihåg tooremove instrumentation-nyckel.

## <a name="is-hello-data-encrypted"></a>Krypteras hello data?
Inte i hello-servrar för närvarande.

Krypteras alla data som flyttas mellan datacenter.

#### <a name="is-hello-data-encrypted-in-transit-from-my-application-tooapplication-insights-servers"></a>Krypteras hello data under överföring från Mina program tooApplication insikter servrar?
Ja, kan vi använda https toosend dataportalen toohello från nästan alla SDK: er, inklusive webbservrar, enheter och HTTPS-webbsidor. hello enda undantaget är data som skickas från vanlig HTTP-webbsidor. 

## <a name="personally-identifiable-information"></a>Personligt identifierbar Information
#### <a name="could-personally-identifiable-information-pii-be-sent-tooapplication-insights"></a>Kunde personligt identifierbar Information (PII) skickas tooApplication insikter?
Ja, det är möjligt. 

Som en allmän vägledning:

* De flesta standard telemetri (det vill säga telemetri som skickas utan att du skriva någon kod) innehåller inte explicit personligt identifierbar information. Det kan dock vara möjligt tooidentify personer genom härledning från en samling av händelser.
* Undantag och spåra meddelanden kan innehålla personligt identifierbar information
* Anpassad telemetri - anrop som är till exempel TrackEvent som du kan skriva i kod med hello API eller loggfil spårningar - kan innehålla alla data som du väljer.

hello tabellen hello slutet av det här dokumentet innehåller mer detaljerade beskrivningar av hello data som samlas in.

#### <a name="am-i-responsible-for-complying-with-laws-and-regulations-in-regard-toopii"></a>Kan jag ansvarar för att följa lagar och förordningar i beaktande tooPII?
Ja. Det är ditt ansvar tooensure som hello insamling och användning av hello data överensstämmer med lagar och förordningar och hello Microsoft Online Services-villkor.

Du bör på lämpligt sätt informera kunderna om hello data programmet samlar in och hur hello data används.

#### <a name="can-my-users-turn-off-application-insights"></a>Mina användare kan stänga av Application Insights?
Inte direkt. Vi inte tillhandahålla en växel som dina användare kan arbeta tooturn av Application Insights.

Men kan du implementera en sådan funktion i ditt program. Alla hello SDK innehåller en API-inställning som inaktiverar telemetri samling. 

#### <a name="my-application-is-unintentionally-collecting-sensitive-information-can-application-insights-scrub-this-data-so-it-isnt-retained"></a>Mina program oavsiktligt samlar in känslig information. Application Insights Skrubba kan dessa data så att den inte finns kvar?
Application Insights inte filtrera eller ta bort dina data. Du bör hantera hello data på rätt sätt och undvika att skicka sådana data tooApplication insikter.

## <a name="data-sent-by-application-insights"></a>Data som skickas av Application Insights
hello SDK variera mellan plattformar och det finns flera komponenter som du kan installera. (Se för[Application Insights - översikt][start].) Varje komponent skickar olika data.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Typer av data som skickas i olika scenarier
| Åtgärden | Dataklasser som samlas in (se nästa tabell) |
| --- | --- |
| [Lägg till Application Insights SDK tooa .NET-webbprojekt][greenbrown] |ServerContext<br/>Härleda<br/>Prestandaräknarna<br/>Begäranden<br/>**Undantag**<br/>Session<br/>användare |
| [Installera statusövervakaren på IIS][redfield] |Beroenden<br/>ServerContext<br/>Härleda<br/>Prestandaräknarna |
| [Lägg till Application Insights SDK tooa Java-webbapp][java] |ServerContext<br/>Härleda<br/>Förfrågan<br/>Session<br/>användare |
| [Lägg till JavaScript SDK tooweb sida][client] |ClientContext <br/>Härleda<br/>Sidan<br/>ClientPerf<br/>AJAX |
| [Definiera standardegenskaper][apiproperties] |**Egenskaper för** för alla händelser som standard och anpassade |
| [Anropa TrackMetric][api] |Numeriska värden<br/>**Egenskaper** |
| [Anropa spåra *][api] |händelsenamnet<br/>**Egenskaper** |
| [Anropa TrackException][api] |**Undantag**<br/>Stackdump<br/>**Egenskaper** |
| SDK kan samla in data. Exempel: <br/> -Det går inte att komma åt prestandaräknarna<br/> -undantag i telemetri initieraren |SDK-diagnostik |

För [SDK: er för andra plattformar][platforms], se dokumenten.

#### <a name="hello-classes-of-collected-data"></a>hello klasser av insamlade data
| Insamlade dataklass | Innehåller (inte en fullständig förteckning) |
| --- | --- |
| **Egenskaper** |**Några data - bestäms av din kod** |
| DeviceContext |ID, IP-, språk, enhetsmodell, nätverk, nätverkstyp, OEM-namnet, skärmupplösning Rollinstansen, Role-namn, typ av enhet |
| ClientContext |OS, språk, språk, nätverk, fönstret upplösning |
| Session |Sessions-id |
| ServerContext |Namnet på datorn, språk, OS, enhet, användarens session, användarkontext, åtgärden |
| Härleda |geoplats från IP-adress, timestamp, OS, webbläsare |
| Mått |Tjänstmåttets namn och värde |
| Händelser |Händelsenamn och värde |
| PageViews |Sidan och URL eller inloggningsnamn |
| Klienten perf |URL-sida namn, inläsningstid för webbläsare |
| AJAX |HTTP-anrop från webbsidan tooserver |
| Begäranden |URL: en varaktighet, svarskod |
| Beroenden |Typ (SQL, HTTP,...), anslutningssträngen eller URI, sync/asynkrona, varaktighet, lyckas, SQL-uttryck (med Status Monitor) |
| **Undantag** |Typ, **meddelande**, anropa stackar, käll-fil- och tal, tråd-id |
| Kraschar |Process-id, överordnade process-id, tråd-id som kraschar. programkorrigering av-id, build;  Undantagstyp, adress, reason; dolda symboler och register, binära start- och adresser, binärt namn och sökväg, cpu-typ |
| Spårning |**Meddelandet** och allvarlighetsgrad |
| Prestandaräknarna |Processortid, minne, förfrågningar, undantag hastighet, process privata byte, i/o-hastighet, varaktighet för begäran Kölängd för begäran |
| Tillgänglighet |Web test svarskod, varaktighet för varje steg, namn på testet, timestamp, lyckade, svarstid, test-plats |
| SDK-diagnostik |Spårningsmeddelande eller ett undantag |

Du kan [inaktivera vissa hello data genom att redigera ApplicationInsights.config][config]

## <a name="credits"></a>Krediter
Den här produkten innehåller GeoLite2 data som skapats av MaxMind, från [http://www.maxmind.com](http://www.maxmind.com).



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[platforms]: app-insights-platforms.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

