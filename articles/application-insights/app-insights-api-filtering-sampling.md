---
title: "aaaFiltering och förbearbetning i hello Azure Application Insights SDK | Microsoft Docs"
description: "Skriva telemetri processorer och telemetri initierare för hello SDK toofilter eller Lägg till egenskaper toohello data innan hello telemetri skickas toohello Application Insights-portalen."
services: application-insights
documentationcenter: 
author: beckylino
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 51b9db69b2375b8799718f1b0e1af77620dc2692
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-hello-application-insights-sdk"></a>Filtrering och förbearbetning telemetri i hello Application Insights SDK


Du kan skriva och konfigurera plugin-program för hello Application Insights SDK toocustomize hur telemetri fångas och bearbetas innan den skickas toohello Application Insights-tjänsten.

* [Provtagning](app-insights-sampling.md) minskar hello telemetrivolym utan att påverka din statistik. Den bevarar tillsammans relaterade datapunkter så att du kan navigera mellan dem när de undersöker ett problem. Hello-portalen är hello Totalt antal multiplicerade toocompensate för hello provtagning.
* Filtrering med telemetri processorer [för ASP.NET](#filtering) eller [Java](app-insights-java-filter-telemetry.md) kan du välja eller ändra telemetri i hello SDK innan den skickas toohello server. Exempelvis kan du minska hello telemetrivolym exkluderar begäranden från robotar. Men filtrering är en grundläggande tillvägagångssätt tooreducing trafik än samplingsfrekvensen. Det gör att du mer kontroll över vad skickas men du har toobe medveten om att den påverkar din statistik – till exempel om du filtrerar ut alla lyckade begäranden.
* [Telemetri initierare lägga till egenskaper](#add-properties) tooany telemetri som skickas från din app, inklusive telemetri från hello standard moduler. Exempelvis kan du lägga till beräknade värden. eller versionsnummer genom vilka toofilter hello data i hello-portalen.
* [hello SDK API](app-insights-api-custom-events-metrics.md) är används toosend anpassade händelser och mått.

Innan du börjar:

* Installera hello Application Insights [SDK för ASP.NET](app-insights-asp-net.md) eller [SDK för Java](app-insights-java-get-started.md) i din app.

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a>Filtrering: ITelemetryProcessor
Den här metoden ger mer kontroll över vad inkluderas eller uteslutas från hello telemetri dataström. Du kan använda tillsammans med provtagning, eller separat.

toofilter telemetri som du skriver en telemetri processor och registrera den med hello SDK. All telemetri passerar processorn och du kan välja toodrop från hello strömma eller lägga till egenskaper. Detta inkluderar telemetri från hello standard moduler, till exempel hello HTTP-begäran insamlaren och hello beroende insamlaren samt telemetri som du har skrivit själv. Du kan till exempel filtrera ut telemetri om förfrågningar från robotar eller lyckade beroendeanrop.

> [!WARNING]
> Filtrering hello telemetri som skickas från hello SDK-processorer kan ge skeva hello statistik du finns i hello-portalen och gör det svårt toofollow relaterade objekt.
>
> I stället använda [provtagning](app-insights-sampling.md).
>
>

### <a name="create-a-telemetry-processor-c"></a>Skapa en telemetri-processor (C#)
1. Kontrollera att hello Application Insights SDK i projektet är version 2.0.0 eller senare. Högerklicka på projektet i Visual Studio Solution Explorer och välja hantera NuGet-paket. Kontrollera Microsoft.ApplicationInsights.Web i NuGet-Pakethanteraren.
2. toocreate ett filter, implementera ITelemetryProcessor. Det här är en annan utökningspunkt som telemetri modul, telemetri initieraren och telemetri kanal.

    Observera att telemetri processorer konstruera en kedja av bearbetning. När du initierar en telemetri processor, kan du skicka en länk toohello nästa processor i hello kedja. När toohello Process-metoden skickas en datapunkt telemetri sker arbetet och sedan anrop hello nästa telemetri Processor i hello kedja.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors tooeach other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // toofilter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify hello item if required
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }

    ```
1. Infoga det i ApplicationInsights.config:

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Detta är hello samma avsnitt där du vill initiera ett filter för provtagning.)

Du kan överföra strängvärden från hello .config-fil genom att tillhandahålla offentliga namngivna egenskaper i klassen.

> [!WARNING]
> Ta hand toomatch hello typnamn och eventuella egenskapsnamn i hello .config-fil toohello klass och egenskapsnamn i hello kod. Om hello .config-filen refererar till en obefintlig eller egenskapen, misslyckas hello SDK tyst toosend alla telemetri.
>
>

**Du kan också** du kan initiera hello filter i koden. I en lämplig initiering – till exempel AppStart i Global.asax.cs - Infoga processorn i kedjan hello:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients som skapats efter den här punkten kommer att använda din processorer.

### <a name="example-filters"></a>Exempel filter
#### <a name="synthetic-requests"></a>Syntetiska begäranden
Filtrera ut robotar och web tester. Även om Metrics Explorer ger du Hej alternativet toofilter ut syntetiska källor, det här alternativet minskar trafiken genom att filtrera dem på hello SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Misslyckad verifiering
Filtrera ut begäranden med svaret ”401”.

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // toofilter out an item, just terminate hello chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Filtrera bort snabb remote beroendeanrop
Om du bara vill toodiagnose anrop som är långsamma filtrera bort hello fast som.

> [!NOTE]
> Detta kan ge skeva hello statistik som du ser på hello-portalen. hello beroende diagram ser ut som om hello beroendeanrop är alla fel.
>
>

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Diagnostisera beroendeproblem
[Den här bloggen](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) beskriver ett projekt toodiagnose beroende problem genom att automatiskt skicka regelbunden ping toodependencies.


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a>Lägga till egenskaper: ITelemetryInitializer
Använda telemetri initierare toodefine globala egenskaper som skickas med all telemetri; och toooverride valda beteendet för hello standard telemetri moduler.

Till exempel hello Application Insights för webbpaketet samlar in telemetri om HTTP-förfrågningar. Som standard flaggor som misslyckad varje begäran med en svarskoden > = 400. Men om du vill tootreat 400 som lyckas, kan du ange en telemetri initieraren som anger hello lyckade egenskapen.

Om du anger en telemetri initieraren kallas när någon av hello Track*() metoder anropas. Detta inkluderar metoder anropas av hello standard telemetri moduler. Dessa moduler ange konventionen är inte en egenskap som redan har ställts in av en initierare.

**Definiera din initieraren**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides hello default SDK
       * behavior of treating response codes >= 400 as failed requests
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set hello Success property, hello SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us toofilter these requests in hello portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave hello SDK tooset hello Success property      
        }
      }
    }
```

**Läsa in din initieraren**

I ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Du kan också* du kan initiera hello initierare i koden, till exempel i Global.aspx.cs:

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Se mer av det här exemplet.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a>JavaScript telemetri initierare
*JavaScript*

Infoga en telemetri initieraren omedelbart efter hello initieringskod som du fick från portalen hello:

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // toocheck hello telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // tooset custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // tooset custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

En sammanfattning av hello icke anpassade egenskaper som är tillgängliga på hello telemetryItem finns [exportera datamodellen i Application Insights](app-insights-export-data-model.md).

Du kan lägga till så många initierare som du vill.

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor och ITelemetryInitializer
Vad är hello skillnaden mellan telemetri processorer och telemetri initierare?

* Det finns vissa överlappningar i vad du kan göra med dem: kan vara används tooadd egenskaper tootelemetry.
* TelemetryInitializers körs alltid innan TelemetryProcessors.
* TelemetryProcessors Tillåt toocompletely ersätta eller ta bort ett telemetri-objekt.
* TelemetryProcessors bearbeta inte prestandaräknaren telemetri.


## <a name="reference-docs"></a>Referensdokument
* [API-översikt](app-insights-api-custom-events-metrics.md)
* [ASP.NET-referens](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK-kod
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Nästa steg
* [Sök händelser och loggar](app-insights-diagnostic-search.md)
* [Sampling](app-insights-sampling.md)
* [Felsökning](app-insights-troubleshoot-faq.md)
