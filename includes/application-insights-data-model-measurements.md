Samling av anpassade mått. Använd den här samlingen tooreport med namnet mått som associeras med hello telemetri objekt. Vanliga användningsområden är:
- hello storleken på Beroendetelemetri nyttolast
- hello antalet objekt i kö bearbetas av begäran telemetri
- tid kunden tog toocomplete hello steg i guiden steg slutförande händelse telemetri.

Du kan fråga [anpassade mått](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) i Application Analytics:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Anpassade mått är associerade med hello telemetri objekt som de tillhör. De är ämne toosampling med hello telemetri-objekt som innehåller dessa mått. tootrack ett mått som har ett värde som är oberoende av andra telemetri typer, Använd [mått telemetri](../articles/application-insights/app-insights-api-custom-events-metrics.md).

Maximal nyckellängd: 150
