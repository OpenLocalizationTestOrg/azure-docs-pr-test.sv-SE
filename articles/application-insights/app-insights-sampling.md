---
title: aaaTelemetry provtagning i Azure Application Insights | Microsoft Docs
description: Hur tookeep hello telemetrivolym under kontroll.
services: application-insights
documentationcenter: windows
author: vgorbenko
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bwren
ms.openlocfilehash: e19c350d0a5f16736c100322a9922fcfbf5d7b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sampling-in-application-insights"></a>Sampling i Application Insights


Sampling är en funktion i [Azure Application Insights](app-insights-overview.md). Det är hello rekommenderade sätt tooreduce telemetri trafik och lagring, samtidigt som en statistiskt korrekt analys av programdata. hello filter markerar objekt som är relaterade så att du kan bläddra mellan objekten när du gör diagnostiska undersökningar.
När mått Antal presenteras tooyou hello-portalen, de renormalized tootake hänsyn hello provtagning, toominimize någon effekten på hello statistik.

Samplingsfrekvensen minskar kostnaderna för trafik och data och hjälper dig att undvika begränsning.

## <a name="in-brief"></a>Kort sagt:
* Sampling behåller 1 i * n * registrerar och ignorerar hello rest. Det kan till exempel lagra 1: 5 händelser, en samplingsfrekvensen 20%. 
* Sampling sker automatiskt om programmet skickar mycket telemetri i ASP.NET web server apps.
* Du kan också ange provtagning manuellt, antingen i hello Företagsportalen på hello priser för sidan. eller i hello SDK för ASP.NET i hello .config-fil, minska tooalso hello nätverkstrafik.
* Om du loggar anpassade händelser och du vill toomake till att en uppsättning händelser är antingen bevaras eller tas bort tillsammans, se till att de har hello samma åtgärds-ID-värde.
* hello provtagning divisor * n * rapporteras i varje post i hello egenskapen `itemCount`, som i sökningen visas under hello eget namn ”begäran antal” eller ”händelseantal”. När provtagning inte är i drift, `itemCount==1`.
* Om du skriver Analytics-frågor, bör du [ta hänsyn till provtagning](app-insights-analytics-tour.md#counting-sampled-data). I synnerhet i stället för bara räknar poster, du bör använda `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Typer av provtagning
Det finns tre alternativ provtagningsmetoder:

* **Anpassningsbar provtagning** justeras automatiskt hello telemetrivolym skickas från hello SDK i din ASP.NET-app. Som standard från SDK v 2.0.0-beta3. För närvarande tillgänglig för ASP.NET serversidan telemetri. 
* **Fast räntesats provtagning** minskar hello telemetrivolym skickas från både din ASP.NET-server och från användarnas webbläsare. Du kan ange hello hastighet. hello-klienten och servern ska synkronisera sina provtagning så att, i sökningen som du kan navigera mellan relaterade sidvisningar och förfrågningar.
* **Införandet provtagning** fungerar i hello Azure-portalen. Den tar bort vissa av hello telemetri som tas emot från din app med en hastighet som du anger. Det minskar inte telemetri trafik, men hjälper dig att hålla inom den månatliga kvoten. hello stor nytta av införandet provtagning är att du kan ange den utan att omdistribuera din app och det fungerar enhetligt för alla servrar och klienter. 

Om Adaptiv eller fast hastighet provtagning är i drift, har införandet provtagning inaktiverats.

## <a name="ingestion-sampling"></a>Införandet provtagning
Det här formuläret för provtagning fungerar på hello punkt där hello telemetri från webbservern, webbläsare och enheter når hello Application Insights tjänstslutpunkten. Även om det inte minska hello telemetri trafik som skickas från din app, det minskar hello bearbetas och lagras (och debiteras för) av Application Insights.

Använd den här typen av samplingsfrekvensen om din app ofta färdas över månatliga kvoten och du inte har hello möjlighet att använda antingen hello SDK-baserade typer av provtagning. 

Ange hello samplingsfrekvensen i hello kvoter och priser bladet:

![Klicka på inställningar, kvot, prover, hello program Översikt bladet och sedan välja en samplingsfrekvensen och klicka på Uppdatera.](./media/app-insights-sampling/04.png)

Precis som andra typer av provtagning behåller hello algoritmen relaterad telemetri objekt. Till exempel, när du undersöks hello telemetri i sökning, du ska kunna toofind hello begäran relaterade tooa särskilda undantag. Måttet räknar, till exempel förfrågningar och undantag hastighet behålls på rätt sätt.

Datapunkter som ignoreras av provtagning är inte tillgängliga i alla Application Insights-funktioner som [löpande Export](app-insights-export-telemetry.md).

Införandet provtagning fungerar inte när SDK-baserade anpassningsbara eller fasta hastighet provtagning är i drift. Om hello samplingsfrekvensen på hello SDK är mindre än 100%, ignoreras hello införandet samplingsfrekvensen som du ställer in.

> [!WARNING]
> hello-värde som visas på panelen hello anger hello-värdet som du anger för införandet provtagning. Den representera inte hello faktiska samplingsfrekvensen om SDK provtagning är i drift.
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a>Anpassningsbar provtagning på din webbserver
Anpassningsbar provtagning är tillgänglig för hello Application Insights SDK för ASP.NET v 2.0.0-beta3 och senare och är aktiverad som standard. 

Anpassningsbar provtagning påverkar hello telemetrivolym skickas från din app toohello för web server Application Insights-tjänsten. hello volym justeras automatiskt tookeep inom en angiven maximala mängden trafik.

Det fungerar inte på låg mängder telemetri, så en app i felsökning eller en webbplats med låg belastning påverkas inte.

tooachieve hello målvolymen, vissa hello telemetri genereras ignoreras. Men precis som andra typer av provtagning hello algoritmen behåller relaterad telemetri objekt. Till exempel, när du undersöks hello telemetri i sökning, du ska kunna toofind hello begäran relaterade tooa särskilda undantag. 

Måttet räknas som förfrågningar och undantag hastighet är justerade toocompensate för hello samplingsfrekvens, så att de visar ungefär rätt värden i måttet Explorer.

**Uppdatera ditt projekt NuGet** paket toohello senaste *förhandsversionen* version av Application Insights: Högerklicka på hello projektet i Solution Explorer, välja hantera NuGet-paket, kontrollera **inkludera förhandsversionen** och Sök efter Microsoft.ApplicationInsights.Web. 

I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), kan du justera flera parametrar i hello `AdaptiveSamplingTelemetryProcessor` nod. hello siffror som visas är hello standardvärden:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    hello mål hastigheten som hello anpassningsbar algoritmen syftar för **på varje server-värd**. Om ditt webbprogram kör på flera värdar, minska värdet så tooremain inom dina mål mängden trafik på hello Application Insights-portalen.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    hello intervall på vilka hello aktuellt antal telemetri görs en ny utvärdering. Utvärderingen utförs som en glidande medelvärde. Du kanske vill tooshorten intervallet om din telemetri är ansvariga toosudden belastning.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    När provtagning procentandel ändras, hur snart efter är tillåtna vi toolower provtagning procentandel igen toocapture mindre data.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    När provtagning procentandel ändras, hur snart när vi får tooincrease provtagning procentandel igen toocapture mer data.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Eftersom provtagning procentandel varierar, vad är hello minimivärdet vi är tillåtna tooset.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Eftersom provtagning procentandel varierar, vad är hello maxvärdet vi är tillåtna tooset.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    Hello beräkningen av hello glidande medelvärde, tilldelas hello vikt toohello senaste värde. Använd ett värde lika tooor som är mindre än 1. Lägre värden ändrar hello algoritmen reagera mindre toosudden.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    hello-värdet som tilldelas när hello app har startats. Inte minska detta när du felsöker. 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    En semikolonavgränsad lista över typer som du inte vill att sampla toobe. Identifiera typer är: beroende, händelse, undantag, PageView, begäran, spårning. Alla instanser av hello angetts typer överförs. hello-typer som inte har angetts samplas.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    En semikolonavgränsad lista över typer som du vill toobe provtagning. Identifiera typer är: beroende, händelse, undantag, PageView, begäran, spårning. hello angetts typer samplas; alla instanser av hello andra typer kommer alltid att överföras.


**tooswitch av** anpassningsbar provtagning, ta bort hello AdaptiveSamplingTelemetryProcessor nod från applicationinsights-config.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternativ: Konfigurera anpassningsbar provtagning i koden
Du kan använda koden i stället för att justera samplingsfrekvensen i hello .config-fil. Detta ger dig toospecify Återanropsfunktionen som anropas när hello samplingsfrekvensen är ny utvärdering. Du kan använda detta, exempelvis toofind reda på vilka samplingsfrekvensen används.

Ta bort hello `AdaptiveSamplingTelemetryProcessor` noden från hello .config-fil.

*C#*

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust hello settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report hello sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Lär dig mer om telemetri processorer](app-insights-api-filtering-sampling.md#filtering).)

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a>Samplingsfrekvensen för webbsidor med JavaScript
Du kan konfigurera webbsidor för fast räntesats provtagning från alla servrar. 

När du [konfigurera hello webbsidor för Application Insights](app-insights-javascript.md), ändra hello fragment som du får från hello Application Insights-portalen. (I ASP.NET appar hello fragment vanligtvis först i _Layout.cshtml.)  Infoga en rad som `samplingPercentage: 10,` innan hello instrumentation nyckel:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

Välj ett procentvärde som är nära too100/N, där N är ett heltal för hello samplingsprocenten.  Samlar för närvarande stöd inte för andra värden.

Om du även aktivera fast räntesats provtagning i hello server synkroniserar hello klienter och servrar så att, i sökningen som du kan navigera mellan relaterade sidvisningar och förfrågningar.

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a>Fast räntesats samplingsfrekvensen för ASP.NET-webbplatser
Fasta samplingsfrekvensen minskar hello trafik som skickas från webbservern och webbläsare. Till skillnad från anpassningsbar samplingsfrekvensen minskar den telemetri till en fast kostnad som du valt. Det synkroniserar även hello klienten och servern provtagning så att relaterade objekt bevaras – till exempel så att om du tittar på vyn sida i sökningen kan du hitta relaterade begäran.

hello provtagning algoritmen behåller relaterade objekt. För varje HTTP-begäran är händelsen som den och dess relaterade händelser antingen ignoreras eller skickas. 

I Metrics Explorer multipliceras priser som begäran och undantag räknas med en faktor toocompensate för hello samplingsfrekvensen, så att de är ungefär korrekt.

1. **Uppdatera ditt projekt NuGet-paket** toohello senaste *förhandsversionen* version av Application Insights. Högerklicka på hello projektet i Solution Explorer, välja hantera NuGet-paket, kontrollera **inkludera förhandsversion** och Sök efter Microsoft.ApplicationInsights.Web. 
2. **Inaktivera anpassningsbar provtagning**: I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), ta bort eller kommentera ut hello `AdaptiveSamplingTelemetryProcessor` nod.
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. **Aktivera hello fast räntesats provtagning modulen.** Lägg till den här fragment för[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> Välj ett procentvärde som är nära too100/N, där N är ett heltal för hello samplingsprocenten.  Samlar för närvarande stöd inte för andra värden.
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternativ: aktivera fast räntesats provtagning i serverkoden
I stället för att ange hello provtagning parameter i hello .config-fil kan använda du koden. 

*C#*

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Lär dig mer om telemetri processorer](app-insights-api-filtering-sampling.md#filtering).)

## <a name="when-toouse-sampling"></a>När toouse provtagning?
Anpassningsbar provtagning aktiveras automatiskt om du använder hello SDK för ASP.NET version 2.0.0-beta3 eller senare. Du kan använda införandet provtagning (på vår server) oavsett vilken SDK-version som du använder.

Du behöver inte samplingsfrekvensen för de flesta program för små och medelstora storlek. hello mest användbara diagnostisk information och mest korrekta statistik erhålls genom att samla in data på alla användaraktiviteter. 

hello huvudsakliga fördelarna med samplingsfrekvensen är:

* Application Insights service således (”begränsas”) datapunkter när appen skickar en mycket hög andel telemetri kort sagt tidsintervall. 
* tookeep inom hello [kvot](app-insights-pricing.md) datapunkter för din prisnivå. 
* tooreduce nätverkstrafik från hello samling telemetri. 

### <a name="which-type-of-sampling-should-i-use"></a>Vilken typ av provtagning ska jag använda?
**Använd införandet provtagning om:**

* Ofta gå igenom den månatliga kvoten av telemetri.
* Du använder en version av hello SDK som inte stöder provtagning – till exempel, hello Java SDK eller ASP.NET-versioner tidigare än 2.
* Du får mycket telemetri från användarnas webbläsare.

**Använd fast räntesats provtagning om:**

* Du använder hello Application Insights SDK för ASP.NET web services version 2.0.0 eller senare, och
* Du vill synkroniserade provtagning mellan klienten och servern, så att när du vill veta händelser i [Sök](app-insights-diagnostic-search.md), du kan navigera mellan relaterade händelser på hello-klienten och servern, till exempel sidvisningar och HTTP-begäranden.
* Du är säker på av hello lämpliga samplingsprocenten för din app. Det bör vara tillräckligt högt tooget exakta mått, men nedan hello hastigheten som överskrider din prisnivå kvot och hello throttling-begränsning. 

**Använda anpassningsbar provtagning:**

I annat fall rekommenderar vi anpassningsbar provtagning. Detta är aktiverat som standard i hello ASP.NET server SDK version 2.0.0-beta3 eller senare. Det minska inte trafik till en viss minsta hastighet, så att den inte påverkar en plats med låg användning.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Hur vet jag om sampling är i drift?
toodiscover hello faktiska samplingsfrekvens oavsett om den har tillämpats, använda en [Analytics query](app-insights-analytics.md) sådana här:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

Bevaras i varje post, `itemCount` anger hello antal ursprungliga poster som den representerar lika too1 + hello antal tidigare borttagna poster. 

## <a name="how-does-sampling-work"></a>Hur fungerar provtagning?
Fast räntesats och anpassningsbar provtagning är en funktion i hello SDK i ASP.NET-versioner från 2.0.0 och senare. Införandet provtagning är en funktion i hello Application Insights-tjänsten och kan vara i drift om hello SDK inte utför provtagning. 

hello provtagning algoritmen bestämmer vilka telemetri objekt toodrop och vilka tookeep som omfattas av viktiga och som (om det är i hello SDK eller hello Application Insights-tjänsten). hello provtagning beslut baseras på flera regler som syftar toopreserve alla relaterade-datapunkter är intakta, underhålla Avbrottsfritt diagnostik i Application Insights som är tillämplig och tillförlitlig även med en minskad datauppsättning. Till exempel om din app skickar ytterligare telemetri objekt (till exempel undantag och spårningar som loggats från denna begäran) för en misslyckad begäran, provtagning inte delar upp denna begäran och andra telemetri. Den fortsätter eller utelämnar dem samtidigt. När du tittar på hello Frågedetaljer i Application Insights kan du därför alltid se hello begäran tillsammans med dess associerade telemetri-objekt. 

För program som definierar ”användare” (det vill säga de vanligaste webbprogram), hello provtagning beslut baserat på hello hash för hello användar-id, vilket innebär att all telemetri för en viss användare är antingen bevaras eller tas bort. För hello typer av program som inte definierar användare (till exempel webbtjänster) baserat hello provtagning beslutet på hello åtgärds-id för hello-begäran. Slutligen för hello telemetri artiklar som varken har användaren eller åtgärden-id som angetts (till exempel telemetri objekt som rapporterats från asynkron trådar med ingen kontext för http) provtagning bara samlar in en del av telemetri varje typ av objekt. 

När presenterar telemetri tillbaka tooyou hello hello Application Insights-tjänsten justerar hello mått med samma samplingsprocenten som används för närvarande hello mängden toocompensate för hello saknar datapunkter. När du tittar på hello telemetri i Application Insights därför ser hello användare statistiskt korrekt approximeringar som är mycket nära toohello reellt tal.

hello korrekt hello uppskattning är i stort sett beror på hello konfigurerade samplingsprocenten. Dessutom ökar hello noggrannhet för program som hanterar stora mängder vanligtvis liknande begäranden från många olika användare. Hej däremot för program som inte fungerar med en stor belastning på, provtagning behövs inte dessa program kan skicka all telemetri vanligtvis när du befinner dig inom hello kvoten utan att orsaka förlust av data från begränsning. 

Observera att Application Insights exempel inte mått och sessioner telemetri typer eftersom för dessa typer, minskad hello precisionen kan vara hög oönskade. 

### <a name="adaptive-sampling"></a>Anpassningsbar provtagning
Anpassningsbar provtagning lägger till en komponent som övervakar hello aktuella överföringshastigheten från hello SDK och justerar hello provtagning procentandel tootry toostay inom hello mål högsta överföringshastighet. hello justeringen beräknas med jämna mellanrum och baseras på ett glidande medelvärde för hello utgående överföringshastigheten.

## <a name="sampling-and-hello-javascript-sdk"></a>Provtagning och hello JavaScript SDK
hello på klientsidan (JavaScript) SDK deltar i fast räntesats provtagning tillsammans med hello serversidan SDK. Hej instrumenterats sidor bara skicka telemetri för klientsidan från hello samma användare för vilka hello serversidan gjorts beslutet för ”sample i”. Den här logiken är utformad toomaintain integriteten hos användarsession över - och server-klienten. Därför från ett visst telemetri objekt i Application Insights hittar du alla andra telemetri objekt för den här användaren eller sessionen. 

*Klient- och serversidan telemetri Visa inte samordnas exempel som beskrivs ovan.*

* Kontrollera att du har aktiverat fast räntesats provtagning både servern och klienten.
* Kontrollera att hello SDK version 2.0 eller senare.
* Kontrollera att du ställer in hello samma provtagning procent i både hello klient och server.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
*Varför inte provtagning en enkel ”samla in X procent av varje typ av telemetri”?*

* När den här metoden för provtagning skulle ge en mycket hög exakta mått approximeringar, skulle brytas möjlighet toocorrelate diagnostikdata per användare, session och begäran som är viktiga för diagnostik. Därför provtagning fungerar bättre med ”samla in alla telemetri artiklar för X procent av app-användare” eller ”samla in all telemetri för X procent av begäranden som app” logik. Hello telemetri objekt som inte är associerade med hello-begäranden (till exempel asynkron behandling i bakgrunden) är hello faller tillbaka för ”samla in X procent av alla objekt för varje typ av telemetri”. 

*Hej samplingsprocenten kan förändras över tid?*

* Ja, anpassningsbar provtagning gradvis ändrar hello samplingsprocenten baserat på hello som för närvarande observerats hello telemetrivolym.

*Om jag använder fast räntesats provtagning hur vet jag vilka provtagning procentandel fungerar hello bäst för min app?*

* Ett sätt är toostart med anpassningsbar provtagning, ta reda på vad ge ett omdöme reglerar på (se hello ovan fråga), och sedan växla toofixed-hastighet provtagning med denna kurs. 
  
    Annars kan få du tooguess. Analysera ditt nuvarande bruk som telemetri i AI, se någon begränsning som inträffar och uppskattning hello telemetrivolym hello samlas in. Dessa tre indata, tillsammans med din valda prisnivå förslag på hur mycket du kanske vill tooreduce hello telemetrivolym hello samlas in. Dock kan en ökning av hello antal användare eller vissa SKIFT i hello telemetrivolym ogiltig uppskattningen.

*Vad händer om jag konfigurera samplingsprocenten för lågt?*

* Låg samplingsprocenten (over-aggressive provtagning) minskar hello riktighet hello approximeringar när Application Insights försöker toocompensate hello visualisering av hello data för hello data volym minska. Dessutom diagnostiska upplevelse kan påverkas negativt, med hello sällan misslyckas eller långsam begäranden prov ut.

*Vad händer om jag konfigurera samplingsprocenten för hög?*

* Konfiguration för hög provtagning procentandel (inte aggressivt tillräckligt) resulterar i en otillräcklig minskning i hello mängd hello samlas in telemetri. Du kan fortfarande se telemetri dataförlust relaterade toothrottling och hello kostnaden för att använda Application Insights kan vara högre än du planerat på grund av toooverage avgifter.

*På vilka plattformar kan du använda provtagning?*

* Införandet sampling kan ske automatiskt för alla telemetri över en viss volym om hello SDK inte utför provtagning. Detta fungerar, till exempel om din app använder en Java-server, eller om du använder en äldre version av hello SDK för ASP.NET.
* Om du använder SDK för ASP.NET-versioner 2.0.0 och senare (finns i Azure eller på din server), får du anpassningsbar provtagning som standard men du kan växla toofixed hastighet som beskrivs ovan. Med fast räntesats provtagning synkroniserar hello webbläsare SDK automatiskt toosample relaterade händelser. 

*Det finns vissa ovanliga händelser som jag vill alltid toosee. Hur kan jag få dem tidigare hello provtagning modulen?*

* Initiera en separat instans av TelemetryClient med en ny TelemetryConfiguration (inte hello standard aktiva). Använd den toosend sällsynta händelserna.

## <a name="next-steps"></a>Nästa steg
* [Filtrering](app-insights-api-filtering-sampling.md) kan ge mer strikt kontroll över din SDK skickar.

