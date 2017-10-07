---
title: "aaaApplication Insights API för anpassade händelser och mått | Microsoft Docs"
description: "Infoga ett fåtal rader med kod i din enhet eller skrivbordsapp, webbsida eller tjänst, tootrack användning och diagnostisera problem."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: f3d207a47bb4825efda806a19dd0c26540db7bdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Application Insights API för anpassade händelser och mått

Infoga några rader med kod i dina program toofind reda på vad användarna gör med den eller toohelp diagnostisera problem. Du kan skicka telemetri från enheten och den stationära appar, webbklienter och webbservrar. Använd hello [Azure Application Insights](app-insights-overview.md) telemetriska API: N toosend anpassade händelser och mått och egna versioner av standard telemetri. Detta API är hello samma API hello standarden Application Insights datainsamlare använder.

## <a name="api-summary"></a>API-sammanfattning
hello API är enhetligt för alla plattformar förutom några mindre variationer.

| Metod | Används för |
| --- | --- |
| [`TrackPageView`](#page-views) |Sidor, skärmar, blad eller formulär. |
| [`TrackEvent`](#trackevent) |Användaråtgärder och andra händelser. Använda tootrack användaren beteende eller toomonitor prestanda. |
| [`TrackMetric`](#trackmetric) |Prestandamått, till exempel kölängder inte relaterade toospecific händelser. |
| [`TrackException`](#trackexception) |Loggning undantag för diagnos. Spåra där de förekommer i relationen tooother händelser och undersöka stackspår. |
| [`TrackRequest`](#trackrequest) |Loggning hello frekvens och varaktighet för serverbegäranden för prestandaanalys. |
| [`TrackTrace`](#tracktrace) |Diagnostiska loggmeddelanden. Du kan också samla in loggar från tredje part. |
| [`TrackDependency`](#trackdependency) |Loggning hello varaktighet och frekvens för anrop tooexternal komponenter som din app är beroende av. |

Du kan [bifoga egenskaper och mått](#properties) toomost för anropen telemetri.

## <a name="prep"></a>Innan du börjar
Om du inte har en referens i Application Insights SDK ännu:

* Lägga till hello Application Insights SDK tooyour projekt:

  * [ASP.NET-projekt](app-insights-asp-net.md)
  * [Java-projekt](app-insights-java-get-started.md)
  * [JavaScript i varje webbsida](app-insights-javascript.md) 
* Inkludera i din enhet eller web serverkod:

    *C#:*`using Microsoft.ApplicationInsights;`

    *Visual Basic:*`Imports Microsoft.ApplicationInsights`

    *Java:*`import com.microsoft.applicationinsights.TelemetryClient;`

## <a name="constructing-a-telemetryclient-instance"></a>Hur du skapar en TelemetryClient-instans
Skapa en instans av `TelemetryClient` (förutom i JavaScript i webbsidor):

*C#*

    private TelemetryClient telemetry = new TelemetryClient();

*Visual Basic*

    Private Dim telemetry As New TelemetryClient

*Java*

    private TelemetryClient telemetry = new TelemetryClient();

TelemetryClient är trådsäkra.

Vi rekommenderar att du använder en instans av TelemetryClient för varje modul för din app. Du kan exempelvis har en TelemetryClient-instans i din web tooreport inkommande HTTP-begäranden och en annan i en mellanprogram klassen tooreport logik affärshändelser. Du kan ange egenskaper som `TelemetryClient.Context.User.Id` tootrack användare och sessioner, eller `TelemetryClient.Context.Device.Id` tooidentify hello datorn. Den här informationen är anslutna tooall händelser som hello instans skickar.

## <a name="trackevent"></a>TrackEvent
I Application Insights en *anpassade händelsen* är en datapunkt som du kan visa i [Metrics Explorer](app-insights-metrics-explorer.md) som en sammansatt antal och i [diagnostiska Sök](app-insights-diagnostic-search.md) som individuella förekomster. (Det är inte relaterade tooMVC eller andra framework ”händelser”.)

Infoga `TrackEvent` anropar i din kod toocount olika händelser. Hur ofta användare välja en viss funktion, hur ofta de uppnå specifika mål eller kanske hur ofta de göra vissa typer av misstag.

Till exempel skicka en händelse när en användare wins hello spelet i spelet app:

*JavaScript*

    appInsights.trackEvent("WinGame");

*C#*

    telemetry.TrackEvent("WinGame");

*Visual Basic*

    telemetry.TrackEvent("WinGame")

*Java*

    telemetry.trackEvent("WinGame");

### <a name="view-your-events-in-hello-microsoft-azure-portal"></a>Visa händelserna i hello Microsoft Azure-portalen
toosee antalet händelserna, öppna en [Metrics Explorer](app-insights-metrics-explorer.md) bladet Lägg till ett nytt diagram och välj **händelser**.  

![Finns ett antal anpassade händelser](./media/app-insights-api-custom-events-metrics/01-custom.png)

toocompare hello antal olika händelser, ställa in hello diagramtyp också**rutnätet**, och som händelsenamn:

![Ange hello diagramtyp och gruppering](./media/app-insights-api-custom-events-metrics/07-grid.png)

Klicka på hello rutnätet via en händelse namnet toosee enskilda förekomster av händelsen. toosee mer detaljerat - Klicka på någon av förekomsterna i hello-listan.

![Detaljvisning hello-händelser](./media/app-insights-api-custom-events-metrics/03-instances.png)

toofocus på specifika händelser i sökning eller Metrics Explorer set hello bladet toohello händelse Filternamn som du är intresserad av:

![Öppna filter, expanderar händelsenamnet och välj ett eller flera värden](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a>Anpassade händelser i Analytics

hello telemetri är tillgängliga i hello `customEvents` tabell i [Application Insights Analytics](app-insights-analytics.md). Varje rad representerar ett anrop för`trackEvent(..)` i din app. 

Om [provtagning](app-insights-sampling.md) är i drift, hello itemCount egenskapen visar ett värde större än 1. För exempel itemCount == 10 innebär att av 10 anrop tootrackEvent() hello provtagning processen bara skickas en av dem. tooget rätt antal anpassade händelser, bör du använda därför använda kod som `customEvent | summarize sum(itemCount)`.


## <a name="trackmetric"></a>TrackMetric

Application Insights kan diagram mått som inte är anslutna tooparticular händelser. Exempelvis kan du övervaka en Kölängd med jämna mellanrum. Hello individuella mätningar tar mindre än hello variationer och trender med statistik, och därför statistiska stapeldiagram är användbara.

Ordna toosend mått tooApplication insikter, du kan använda hello `TrackMetric(..)` API. Det finns två sätt toosend ett mått: 

* Enstaka värde. Varje gång du utför ett mått i ditt program kan skicka du hello motsvarande värde tooApplication insikter. Anta att du har ett mått som beskriver hello antalet objekt i en behållare. Du först lägga till tre objekt i behållaren hello och sedan ta bort två objekt under en viss tidsperiod. Därför bör du anropa `TrackMetric` två gånger: först skicka hello värdet `3` och hello värdet `-2`. Application Insights lagrar båda värdena för din räkning. 

* Aggregering. När du arbetar med mått är sällan varje enskild mätning av intresse. I stället är en sammanfattning av vad som hände under en viss tidsperiod viktigt. Sådana sammanfattning kallas _aggregering_. Hello ovan exempel hello mått summan för den tidsperioden är `1` och hello antal hello måttvärden är `2`. När du använder hello aggregering metod kan du bara anropa `TrackMetric` en gång per period och skicka hello sammanställda tidsvärden. Detta är hello rekommenderat tillvägagångssätt eftersom det kan avsevärt minska hello kostnad och prestanda försämras genom att skicka färre data pekar tooApplication insikter när fortfarande att samla in alla relevanta uppgifter.

### <a name="examples"></a>Exempel:

#### <a name="single-values"></a>Enstaka värden

toosend ett enda värde:

*JavaScript*

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

*C#, Java*

```C#
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

#### <a name="aggregating-metrics"></a>Sammanställa mått

Det rekommenderas tooaggregate mått innan de skickas från din app, tooreduce bandbredd, kostnad och tooimprove prestanda.
Här är ett exempel på sammanställa kod:

*C#*

```C#
using System;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

namespace MetricAggregationExample
{
    /// <summary>
    /// Aggregates metric values for a single time period.
    /// </summary>
    internal class MetricAggregator
    {
        private SpinLock _trackLock = new SpinLock();

        public DateTimeOffset StartTimestamp    { get; }
        public int Count                        { get; private set; }
        public double Sum                       { get; private set; }
        public double SumOfSquares              { get; private set; }
        public double Min                       { get; private set; }
        public double Max                       { get; private set; }
        public double Average                   { get { return (Count == 0) ? 0 : (Sum / Count); } }
        public double Variance                  { get { return (Count == 0) ? 0 : (SumOfSquares / Count)
                                                                                  - (Average * Average); } }
        public double StandardDeviation         { get { return Math.Sqrt(Variance); } }

        public MetricAggregator(DateTimeOffset startTimestamp)
        {
            this.StartTimestamp = startTimestamp;
        }

        public void TrackValue(double value)
        {
            bool lockAcquired = false;

            try
            {
                _trackLock.Enter(ref lockAcquired);

                if ((Count == 0) || (value < Min))  { Min = value; }
                if ((Count == 0) || (value > Max))  { Max = value; }
                Count++;
                Sum += value;
                SumOfSquares += value * value;
            }
            finally
            {
                if (lockAcquired)
                {
                    _trackLock.Exit();
                }
            }
        }
    }   // internal class MetricAggregator

    /// <summary>
    /// Accepts metric values and sends hello aggregated values at 1-minute intervals.
    /// </summary>
    public sealed class Metric : IDisposable
    {
        private static readonly TimeSpan AggregationPeriod = TimeSpan.FromSeconds(60);

        private bool _isDisposed = false;
        private MetricAggregator _aggregator = null;
        private readonly TelemetryClient _telemetryClient;

        public string Name { get; }

        public Metric(string name, TelemetryClient telemetryClient)
        {
            this.Name = name ?? "null";
            this._aggregator = new MetricAggregator(DateTimeOffset.UtcNow);
            this._telemetryClient = telemetryClient ?? throw new ArgumentNullException(nameof(telemetryClient));

            Task.Run(this.AggregatorLoopAsync);
        }

        public void TrackValue(double value)
        {
            MetricAggregator currAggregator = _aggregator;
            if (currAggregator != null)
            {
                currAggregator.TrackValue(value);
            }
        }

        private async Task AggregatorLoopAsync()
        {
            while (_isDisposed == false)
            {
                try
                {
                    // Wait for end end of hello aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap hello current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute hello actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct hello metric telemetry item and send:
                        var aggregatedMetricTelemetry = new MetricTelemetry(
                                Name,
                                prevAggregator.Count,
                                prevAggregator.Sum,
                                prevAggregator.Min,
                                prevAggregator.Max,
                                prevAggregator.StandardDeviation);
                        aggregatedMetricTelemetry.Properties["AggregationPeriod"] = aggPeriod.ToString("c");

                        _telemetryClient.Track(aggregatedMetricTelemetry);
                    }
                }
                catch(Exception ex)
                {
                    // log ex as appropriate for your application
                }
            }
        }

        void IDisposable.Dispose()
        {
            _isDisposed = true;
            _aggregator = null;
        }
    }   // public sealed class Metric
}
```

### <a name="custom-metrics-in-metrics-explorer"></a>Anpassade mått i Metrics Explorer

toosee hello resultat, öppna Metrics Explorer och Lägg till ett nytt diagram. Redigera hello diagram tooshow din mått.

> [!NOTE]
> Din anpassade mått kan ta flera minuter tooappear i hello lista över tillgängliga mått.
>

![Lägg till ett nytt schema eller välj ett diagram och under anpassad, väljer du din mått](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a>Anpassade mått i Analytics

hello telemetri är tillgängliga i hello `customMetrics` tabell i [Application Insights Analytics](app-insights-analytics.md). Varje rad representerar ett anrop för`trackMetric(..)` i din app.
* `valueSum`-Detta är hello summan av hello mått. tooget hello medelvärdet, division med `valueCount`.
* `valueCount`-hello antalet mått som har aggregerats till detta `trackMetric(..)` anropa.

## <a name="page-views"></a>Sidvisningar
I en enhet eller en webbsida app skickas sidan Visa telemetri som standard när varje skärmen eller sidan har lästs in. Men du kan ändra de tootrack sidvisningar vid ytterligare eller olika tidpunkter. Till exempel i en app som visar flikarna eller blad, kan du tootrack en sida när hello användare öppnar ett nytt blad.

![Användning linsen på bladet översikt](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

Användar-och sessionen skickas som egenskaper tillsammans med sidvyer så hello användar- och sessionsobjekt diagram kommer alive när sidan Visa telemetri.

### <a name="custom-page-views"></a>Anpassade sidvisningar
*JavaScript*

    appInsights.trackPageView("tab1");

*C#*

    telemetry.TrackPageView("GameReviewPage");

*Visual Basic*

    telemetry.TrackPageView("GameReviewPage")


Om du har flera flikar i olika HTML-sidor kan ange du hello URL för:

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a>Tidsinställning sidvisningar
Som standard hello gånger rapporteras som **inläsningstid för sidvisning** mäts från när hello webbläsaren skickar hello begäran förrän hello webbläsarens sidan händelsen anropas.

I stället kan du antingen:

* Ange en explicit varaktighet i hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) anropa: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.
* Använd hello sidan Visa tidsinställning samtal `startTrackPage` och `stopTrackPage`.

*JavaScript*

    // toostart timing a page:
    appInsights.startTrackPage("Page1");

...

    // toostop timing and log hello page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

Hej namn som du använder som hello första parametern associerar hello starta och stoppa anrop. Toohello aktuella sidnamn standardvärdet.

hello resulterande sidinläsning varaktighet som visas i Metrics Explorer härleds från hello intervallet mellan hello starta och stoppa anrop. Är det upp tooyou vilka intervall som du faktiskt tid.

### <a name="page-telemetry-in-analytics"></a>Sidan telemetri i Analytics

I [Analytics](app-insights-analytics.md) två tabeller visar data från funktioner i webbläsaren:

* Hej `pageViews` tabellen innehåller information om hello URL och sidrubriken
* Hej `browserTimings` tabellen innehåller information om klientens prestanda som hello tidsåtgång tooprocess hello inkommande data

toofind hur lång tid tar hello webbläsare tooprocess olika sidor:

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

toodiscover hello popularities av olika webbläsare:

```
pageViews | summarize count() by client_Browser
```

tooassociate vyer tooAJAX sidanrop, ansluta med beroenden:

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest
hello server SDK använder TrackRequest toolog HTTP-begäranden.

Du kan också anropa den själv om du vill toosimulate begäranden i ett sammanhang där du inte har hello web service-modulen körs.

Hello bör dock sätt toosend begärandetelemetri är där hello begäran fungerar som en <a href="#operation-context">åtgärdssammanhang</a>.

## <a name="operation-context"></a>Åtgärden kontext
Koppla ihop telemetri objekt genom att koppla toothem en gemensam åtgärds-ID. hello standard spårning av förfrågningar modulen sker för undantag och andra händelser som sänds när en HTTP-begäran bearbetas. I [Sök](app-insights-diagnostic-search.md) och [Analytics](app-insights-analytics.md), du kan använda hello ID tooeasily Sök alla händelser som är associerade med hello-begäran.

hello enklaste sättet tooset hello-ID är tooset en kontext för åtgärden med hjälp av det här mönstret:

*C#*

```C#
// Establish an operation context and associated telemetry item:
using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use hello same operation ID.
    ...
    telemetry.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetry.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

Tillsammans med inställningen en åtgärdssammanhang `StartOperation` skapar ett telemetri objekt hello-typ som du anger. Skickar den hello telemetri objektet automatiskt när hello igen, eller om du explicit anropa `StopOperation`. Om du använder `RequestTelemetry` som hello telemetri typ, varaktigheten har angetts toohello tidsgränsen intervallet mellan start och stopp.

Åtgärden kontexter kan inte kapslas. Om det finns redan en kontext för åtgärden, så att dess ID är associerad med alla hello finns artiklar, inklusive hello-objekt som skapats med `StartOperation`.

I sökning hello åtgärden kontext är används toocreate hello **relaterade objekt** lista:

![Relaterade objekt](./media/app-insights-api-custom-events-metrics/21.png)

Mer information om anpassade åtgärder spårning finns i [program – insikter-anpassad-operationer-tracking.md].

### <a name="requests-in-analytics"></a>Begäranden i Analytics 

I [Application Insights Analytics](app-insights-analytics.md), begär visa upp i hello `requests` tabell.

Om [provtagning](app-insights-sampling.md) är i drift, hello itemCount egenskapen visar ett värde större än 1. För exempel itemCount == 10 innebär att av 10 anrop tootrackRequest() hello provtagning processen bara skickas en av dem. tooget segmenterat rätt antal begäranden och genomsnittlig varaktighet per begäran namn, använda koden som:

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a>TrackException
Skicka undantag tooApplication insikter:

* för[räkna](app-insights-metrics-explorer.md), som en indikation på hello ofta ett problem.
* för[undersöka enskilda förekomster](app-insights-diagnostic-search.md).

hello-rapporterna innehåller hello stackspår.

*C#*

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

*JavaScript*

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

hello SDK fånga undantag som många automatiskt, så att du inte alltid har toocall TrackException explicit.

* ASP.NET: [skriva kod toocatch undantag](app-insights-asp-net-exceptions.md).
* J2EE: [undantag noteras automatiskt](app-insights-java-get-started.md#exceptions-and-request-failures).
* JavaScript: Undantag noteras automatiskt. Om du vill toodisable Automatisk insamling, lägger du till en rad toohello kodstycke som infogas i dina webbsidor:

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a>Undantag i Analytics

I [Application Insights Analytics](app-insights-analytics.md), undantag visas i hello `exceptions` tabell.

Om [provtagning](app-insights-sampling.md) är i drift, hello `itemCount` egenskapen visar ett värde större än 1. För exempel itemCount == 10 innebär att av 10 anrop tootrackException() hello provtagning processen bara skickas en av dem. tooget rätt antal undantag segmenterade av typ av undantag, använda koden som:

```
exceptions | summarize sum(itemCount) by type
```

De flesta hello viktiga Stackinformation redan extraherade i olika variabler, men du kan dra ifrån hello `details` struktur tooget mer. Eftersom den här strukturen är dynamiska, bör du omvandla hello toohello resultattyp du förväntar dig. Exempel:

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

tooassociate undantag med relaterade begäranden, Använd en koppling:

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a>TrackTrace
Använd TrackTrace toohelp diagnostisera problem genom att skicka en ”spåret spår” tooApplication insikter. Du kan skicka mängder diagnostikdata och granska dem i [diagnostiska Sök](app-insights-diagnostic-search.md).

[Logga kort](app-insights-asp-net-trace-logs.md) använda API toosend från tredje part loggar toohello portalen.

*C#*

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


Du kan söka i meddelandeinnehåll, men du kan inte filtrera (till skillnad från egenskapsvärden) på den.

hello storleksgränsen på `message` är mycket högre än hello gränsen på Egenskaper.
En fördel med TrackTrace är att du kan publicera relativt lång data i hello-meddelande. Koda till exempel det postdata.  

Dessutom kan du lägga till ett allvarlighetsgrad nivå tooyour meddelande. Och precis som andra telemetri, du kan lägga till egenskapen värden toohelp du filtrera eller söka efter olika uppsättningar av spår. Exempel:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

I [Sök](app-insights-diagnostic-search.md), du kan sedan enkelt filtreras alla hello meddelanden om en viss allvarlighetsgrad som rör tooa viss databas.


### <a name="traces-in-analytics"></a>Spåren i Analytics

I [Application Insights Analytics](app-insights-analytics.md), anropar tooTrackTrace visa i hello `traces` tabell.

Om [provtagning](app-insights-sampling.md) är i drift, hello itemCount egenskapen visar ett värde större än 1. För exempel itemCount == 10 innebär att 10 anropar för`trackTrace()`, hello provtagning processen skickas endast ett av dem. tooget rätt antal spårning av anrop du bör använda därför kod som `traces | summarize sum(itemCount)`.

## <a name="trackdependency"></a>TrackDependency
Använd hello TrackDependency anropa tootrack hello svarstider och slutförandefrekvenser för anrop tooan externt kodavsnitt. hello resultatet visas i hello beroendediagrammen i hello-portalen.

```C#
var success = false;
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
try
{
    success = dependency.Call();
}
finally
{
    timer.Stop();
    telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
}
```

Kom ihåg hello servern SDK innehåller en [beroende modulen](app-insights-asp-net-dependencies.md) som identifierar och spårar vissa beroendeanrop automatiskt – till exempel toodatabases och REST API: er. Du har tooinstall en agent på din server toomake hello modulen fungerar. Du kan använda det här anropet om du vill tootrack anrop som hello automatisk spårning inte fånga eller om du inte vill tooinstall hello agent.

Redigera tooturn av hello standard beroende spårning modulen [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) och ta bort hello-referens för`DependencyCollector.DependencyTrackingTelemetryModule`.

### <a name="dependencies-in-analytics"></a>Beroenden i Analytics

I [Application Insights Analytics](app-insights-analytics.md), trackDependency anrop visas i hello `dependencies` tabell.

Om [provtagning](app-insights-sampling.md) är i drift, hello itemCount egenskapen visar ett värde större än 1. För exempel itemCount == 10 innebär att av 10 anrop tootrackDependency() hello provtagning processen bara skickas en av dem. tooget rätt antal beroenden segmenterade av mål-komponent, använda koden som:

```
dependencies | summarize sum(itemCount) by target
```

tooassociate beroenden med relaterade begäranden, Använd en koppling:

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a>Tömmer data
Normalt skickar hello SDK data alltid valt toominimize hello inverkan på hello användare. Dock i vissa fall kanske du vill tooflush hello buffert – till exempel om du använder hello SDK i ett program som stängs av.

*C#*

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

Observera att hello-funktionen är asynkron för hello [server telemetri kanal](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).

## <a name="authenticated-users"></a>Autentiserade användare
I en webbapp identifieras användare (som standard) av cookies. En användare kan räknas mer än en gång om de har åtkomst till din app från en annan dator eller webbläsare, eller om de tar bort cookies.

Om användare loggar in tooyour app måste få du en mer korrekt antal genom att ange hello autentiserat användar-ID i hello webbläsare koden:

*JavaScript*

```JS
// Called when my app has identified hello user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

På en webbplats med ASP.NET MVC-program, till exempel:

*Razor*

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

Det är inte nödvändigt toouse hello användares faktiska inloggningsnamn. Har bara toobe ett ID som är unikt toothat användare. Namnet får inte innehålla blanksteg eller något av tecknen hello `,;=|`.

hello användar-ID är också i en sessionscookie och skickas toohello server. Om hello server SDK är installerat autentiserade hello användar-ID skickas som en del av hello kontextegenskaperna för både klient och server telemetri. Därefter kan du filtrera och söka på den.

Om din app-grupper användare till konton, du kan också skicka en identifierare för hello-konto (med hello tecken samma begränsningar).

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

I [Metrics Explorer](app-insights-metrics-explorer.md), du kan skapa ett diagram som räknar **autentiserade användare,**, och **användarkonton**.

Du kan också [Sök](app-insights-diagnostic-search.md) för klienten datapunkter med specifika användarnamn och konton.

## <a name="properties"></a>Filtrera, söka och segmentera dina data med hjälp av egenskaper
Du kan bifoga egenskaper och mätningar tooyour händelser (och även toometrics, sidvisningar, undantag och andra telemetridata).

*Egenskaper för* är strängvärden som du kan använda toofilter din telemetri i hello användningsrapporter. Till exempel om din app erbjuder flera spel, kan du koppla hello namnet på hello spel tooeach händelsen så att du kan se vilka spel är mer populära.

Det finns en gräns på 8192 på hello string-längden. (Om du vill toosend stora mängder data kan använda hello message-parameter för [TrackTrace](#track-trace).)

*Mått* är numeriska värden som kan presenteras grafiskt. Du kanske exempelvis vill toosee om det finns en stegvis ökning hello resultat som din spelare uppnå. hello diagram kan vara åtskilda med hello egenskaper som skickas med hello händelse så att du kan få avgränsa eller staplade diagram för olika spel.

De ska vara större än eller lika med too0 för måttvärden toobe visas korrekt.

Det finns några [begränsning hello antalet egenskaper och egenskapsvärden mått](#limits) som du kan använda.

*JavaScript*

    appInsights.trackEvent
      ("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

    appInsights.trackPageView
        ("page name", "http://fabrikam.com/pageurl.html",
          // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );


*C#*

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics);


*Visual Basic*

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics)


*Java*

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> Ta hand inte toolog personligt identifierbar information i egenskaperna.
>
>

*Om du använde mått*, öppna Metrics Explorer och välj hello mått hello **anpassad** grupp:

![Öppna Metrics Explorer, väljer hello diagrammet och välj hello mått](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> Om din mått inte visas, eller om hello **anpassad** rubrik finns inte där, Stäng hello markeringen bladet och försök igen senare. Mått kan ta en timme toobe samman via hello pipeline.

*Om du använde egenskaper och mått*, segmentera hello mått av hello-egenskapen:

![Ange gruppering och välj sedan hello egenskapen under Gruppera efter](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

*I diagnostiska sökningen*, kan du visa hello egenskaper och mått för enskilda förekomster av en händelse.

![Välj en instans och välj sedan ”...”](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

Använd hello **Sök** fältet toosee händelse förekomster som har ett visst egenskapsvärde.

![Skriv en term i sökrutan](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

[Mer information om sökuttryck](app-insights-diagnostic-search.md).

### <a name="alternative-way-tooset-properties-and-metrics"></a>Alternativt sätt tooset egenskaper och mått
Om det är enklare, du kan samla in hello parametrar för en händelse i ett separat objekt:

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> Inte återanvända hello samma telemetri objektet instans (`event` i det här exemplet) toocall Track*() flera gånger. Detta kan orsaka telemetri toobe skickas med felaktig konfiguration.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Anpassade mått och egenskaper i Analytics

I [Analytics](app-insights-analytics.md), anpassade mått och egenskaper visas i hello `customMeasurements` och `customDimensions` attribut för varje post telemetri.

Till exempel om du har lagt till en egenskap med namnet ”spel” tooyour begärandetelemetri frågan räknar hello förekomster av olika värden för ”spel” och visa hello medelvärdet av hello anpassade mått ”resultat”:

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

Observera att:

* När du extraherar ett värde från hello customDimensions eller customMeasurements JSON, den har dynamiska typ, och så måste du skicka den `tostring` eller `todouble`.
* tootake hänsyn hello möjligheten att [provtagning](app-insights-sampling.md), bör du använda `sum(itemCount)`, inte `count()`.



## <a name="timed"></a>Tidsinställning händelser
Ibland vill du toochart hur lång tid det tar tooperform en åtgärd. Du kanske exempelvis vill tooknow hur länge användare ta tooconsider val i spel. Du kan använda hello mätning parametern för denna.

*C#*

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform hello timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send hello event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <a name="defaults"></a>Standardegenskaperna för anpassad telemetri
Om du vill tooset egenskapen standardvärden för några av hello anpassade händelser som du skriver kan ange du dem i en TelemetryClient-instans. De är anslutna tooevery telemetri objekt som skickas från klienten.

*C#*

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame");

*Visual Basic*

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame")

*Java*

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



Enskilda telemetri anrop kan åsidosätta hello standardvärdena i sina egenskapen ordlistor.

*Web klienter för JavaScript*, [använder JavaScript telemetri initierare](#js-initializer).

*tooadd egenskaper tooall telemetri*, inklusive hello data från standard samling moduler [implementera `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>Provtagning, filtrering och bearbetar telemetri
Du kan skriva kod tooprocess din telemetri innan den skickas från hello SDK. hello bearbetning innehåller data som skickas från hello standard telemetri moduler, till exempel HTTP request-samlingen och beroende samling.

[Lägga till egenskaper](app-insights-api-filtering-sampling.md#add-properties) tootelemetry genom att implementera `ITelemetryInitializer`. Du kan till exempel lägga till versionsnummer eller värden som beräknats från andra egenskaper.

[Filtrering](app-insights-api-filtering-sampling.md#filtering) kan ändra eller ta bort telemetri innan det skickas från hello SDK genom att implementera `ITelemetryProcesor`. Du styr vilka skickas eller tas bort, men du har tooaccount för hello effekt på dina. Beroende på hur du ta bort objekt, kan du förlora hello möjlighet toonavigate mellan relaterade objekt.

[Provtagning](app-insights-api-filtering-sampling.md) är en lösning för paketerade tooreduce hello mängden data som skickas från din app toohello portal. Detta sker utan att påverka hello visas mått. Och detta sker utan att påverka din möjlighet toodiagnose problem genom att navigera mellan relaterade objekt som undantag, förfrågningar och sidvyer.

[Läs mer](app-insights-api-filtering-sampling.md).

## <a name="disabling-telemetry"></a>Inaktivera telemetri
för*dynamiskt stoppa och starta* hello insamling och vidarebefordran av telemetri:

*C#*

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

för*inaktivera valda standard insamlare*--exempelvis prestandaräknare, HTTP-begäranden eller beroenden – ta bort eller kommentera ut hello relevanta rader i [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Du kan göra detta, till exempel om du vill toosend TrackRequest data.

## <a name="debug"></a>Utvecklarläge
Vid felsökning, är det användbart toohave din telemetri expedierade via hello pipeline så att du kan se resultatet direkt. Du kan också hämta meddelandena som hjälper dig spåra eventuella problem med hello telemetri. Stänga av den i produktion, eftersom det kan påverka din app.

*C#*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

*Visual Basic*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <a name="ikey"></a>Ange hello instrumentation nyckel för valda anpassad telemetri
*C#*

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <a name="dynamic-ikey"></a>Dynamisk instrumentation nyckel
tooavoid blandas in telemetri från utvecklings-, test- och produktionsmiljöer, kan du [skapa separata Application Insights-resurser](app-insights-create-new-resource.md) och ändra deras nycklar, beroende på hello-miljö.

I stället för att hämta hello instrumentation nyckeln från hello konfigurationsfil kan du ange den i din kod. Ange hello nyckeln i en initieringsmetod, till exempel global.aspx.cs i en ASP.NET-tjänst:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

*JavaScript*

    appInsights.config.instrumentationKey = myKey;



Du kanske vill tooset i webbsidor, den från hello webbserverns tillstånd i stället kodning bokstavligt hello skript. Till exempel i en webbsida som skapats i en ASP.NET-app:

*JavaScript i Razor*

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a>TelemetryContext
TelemetryClient har en kontextegenskap som innehåller värden som skickas tillsammans med alla telemetridata. De normalt har angetts av hello standard telemetri moduler, men du kan också ange dem själv. Exempel:

    telemetry.Context.Operation.Name = "MyOperationName";

Om du anger dessa värden själv, ta bort hello relevanta raden från [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), så att dina värden och standardvärden som hello inte bli förvirrad.

* **Komponenten**: hello appen och dess version.
* **Enheten**: Data om hello enheten där hello appen körs. (I web apps är detta hello server eller klientenheten som hello telemetri skickas från.)
* **InstrumentationKey**: hello Application Insights-resurs i Azure var hello telemetri visas. Det hämtas vanligtvis från ApplicationInsights.config.
* **Plats**: hello hello enhetens geografiska plats.
* **Åtgärden**: I web apps hello aktuella HTTP-begäran. I andra apptyper du vill kan du ange den här toogroup händelser tillsammans.
  * **ID**: ett genererat värde som korreleras olika händelser så att när du granska alla händelser i diagnostiska sökning, kan du hitta relaterade objekt.
  * **Namnet**: ett ID, vanligtvis hello URL för hello HTTP-begäran.
  * **SyntheticSource**: om inte null eller tomt, en sträng som anger att hello datakälla för hello-begäran har identifierats som ett program eller webbplats test. Som standard är den inte beräkningar i Metrics Explorer.
* **Egenskaper för**: egenskaper som skickas med alla telemetridata. Den kan åsidosättas i enskilda spåra * anrop.
* **Sessionen**: hello användarens session. hello-ID har angetts tooa genereras-värde som ändras när hello användaren inte har active för en stund.
* **Användaren**: användarinformation.

## <a name="limits"></a>Begränsningar
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

tooavoid träffa hello-gräns för överföringshastigheten av data, Använd [provtagning](app-insights-sampling.md).

toodetermine hur länge data sparas finns [datalagring och sekretess](app-insights-data-retention-privacy.md).

## <a name="reference-docs"></a>Referensdokument
* [ASP.NET-referens](https://msdn.microsoft.com/library/dn817570.aspx)
* [Referens för Java](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [JavaScript-referens](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [Android SDK](https://github.com/Microsoft/ApplicationInsights-Android)
* [iOS SDK](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a>SDK-kod
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [Windows Server-paket](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)
* [Alla plattformar](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a>Frågor
* *Vilka undantag kan Track_() anrop utlösa?*

    Ingen. Du behöver inte toowrap dem i försök catch-satser. Om hello SDK påträffar problem, loggas meddelanden i hello debug konsolens utdata och – om hello få meddelanden via--i diagnostiska sökningen.
* *Finns det en REST API tooget data från hello portal?*

    Ja, hello [API för dataåtkomst](https://dev.applicationinsights.io/). Andra sätt tooextract data omfattar [exportera från Analytics tooPower BI](app-insights-export-power-bi.md) och [löpande export](app-insights-export-telemetry.md).

## <a name="next"></a>Nästa steg
* [Sök händelser och loggar](app-insights-diagnostic-search.md)

* [Felsökning](app-insights-troubleshoot-faq.md)


