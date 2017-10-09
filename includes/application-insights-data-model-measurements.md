<span data-ttu-id="7eb44-101">Samling av anpassade mått.</span><span class="sxs-lookup"><span data-stu-id="7eb44-101">Collection of custom measurements.</span></span> <span data-ttu-id="7eb44-102">Använd den här samlingen tooreport med namnet mått som associeras med hello telemetri objekt.</span><span class="sxs-lookup"><span data-stu-id="7eb44-102">Use this collection tooreport named measurement associated with hello telemetry item.</span></span> <span data-ttu-id="7eb44-103">Vanliga användningsområden är:</span><span class="sxs-lookup"><span data-stu-id="7eb44-103">Typical use cases are:</span></span>
- <span data-ttu-id="7eb44-104">hello storleken på Beroendetelemetri nyttolast</span><span class="sxs-lookup"><span data-stu-id="7eb44-104">hello size of Dependency Telemetry payload</span></span>
- <span data-ttu-id="7eb44-105">hello antalet objekt i kö bearbetas av begäran telemetri</span><span class="sxs-lookup"><span data-stu-id="7eb44-105">hello number of queue items processed by Request Telemetry</span></span>
- <span data-ttu-id="7eb44-106">tid kunden tog toocomplete hello steg i guiden steg slutförande händelse telemetri.</span><span class="sxs-lookup"><span data-stu-id="7eb44-106">time that customer took toocomplete hello step in wizard step completion Event Telemetry.</span></span>

<span data-ttu-id="7eb44-107">Du kan fråga [anpassade mått](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) i Application Analytics:</span><span class="sxs-lookup"><span data-stu-id="7eb44-107">You can query [custom measurements](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) in Application Analytics:</span></span>

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > <span data-ttu-id="7eb44-108">Anpassade mått är associerade med hello telemetri objekt som de tillhör.</span><span class="sxs-lookup"><span data-stu-id="7eb44-108">Custom measurements are associated with hello telemetry item they belong to.</span></span> <span data-ttu-id="7eb44-109">De är ämne toosampling med hello telemetri-objekt som innehåller dessa mått.</span><span class="sxs-lookup"><span data-stu-id="7eb44-109">They are subject toosampling with hello telemetry item containing those measurements.</span></span> <span data-ttu-id="7eb44-110">tootrack ett mått som har ett värde som är oberoende av andra telemetri typer, Använd [mått telemetri](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="7eb44-110">tootrack a measurement that has a value independent from other telemetry types, use [Metric telemetry](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="7eb44-111">Maximal nyckellängd: 150</span><span class="sxs-lookup"><span data-stu-id="7eb44-111">Max key length: 150</span></span>
