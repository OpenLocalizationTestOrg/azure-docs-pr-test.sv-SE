<span data-ttu-id="cb62b-101">Samling av anpassade mått.</span><span class="sxs-lookup"><span data-stu-id="cb62b-101">Collection of custom measurements.</span></span> <span data-ttu-id="cb62b-102">Använd den här samlingen i rapporten med namnet mått som är associerade med objektet telemetri.</span><span class="sxs-lookup"><span data-stu-id="cb62b-102">Use this collection to report named measurement associated with the telemetry item.</span></span> <span data-ttu-id="cb62b-103">Vanliga användningsområden är:</span><span class="sxs-lookup"><span data-stu-id="cb62b-103">Typical use cases are:</span></span>
- <span data-ttu-id="cb62b-104">storleken på Beroendetelemetri nyttolast</span><span class="sxs-lookup"><span data-stu-id="cb62b-104">the size of Dependency Telemetry payload</span></span>
- <span data-ttu-id="cb62b-105">Antal objekt i kö bearbetas av begäran telemetri</span><span class="sxs-lookup"><span data-stu-id="cb62b-105">the number of queue items processed by Request Telemetry</span></span>
- <span data-ttu-id="cb62b-106">tid kunden tog för att slutföra steg i guiden steg slutförande händelse telemetri.</span><span class="sxs-lookup"><span data-stu-id="cb62b-106">time that customer took to complete the step in wizard step completion Event Telemetry.</span></span>

<span data-ttu-id="cb62b-107">Du kan fråga [anpassade mått](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) i Application Analytics:</span><span class="sxs-lookup"><span data-stu-id="cb62b-107">You can query [custom measurements](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) in Application Analytics:</span></span>

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > <span data-ttu-id="cb62b-108">Anpassade mått är kopplade till objektet telemetri som de tillhör.</span><span class="sxs-lookup"><span data-stu-id="cb62b-108">Custom measurements are associated with the telemetry item they belong to.</span></span> <span data-ttu-id="cb62b-109">De regleras provtagning telemetri-objektet som innehåller dessa mått.</span><span class="sxs-lookup"><span data-stu-id="cb62b-109">They are subject to sampling with the telemetry item containing those measurements.</span></span> <span data-ttu-id="cb62b-110">Om du vill spåra ett mått som har ett värde som är oberoende av andra typer av telemetri använda [mått telemetri](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="cb62b-110">To track a measurement that has a value independent from other telemetry types, use [Metric telemetry](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="cb62b-111">Maximal nyckellängd: 150</span><span class="sxs-lookup"><span data-stu-id="cb62b-111">Max key length: 150</span></span>
