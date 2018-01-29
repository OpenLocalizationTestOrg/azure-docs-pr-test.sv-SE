---
title: "Diagnostik i varaktiga funktioner – Azure"
description: "Lär dig att felsöka problem med filnamnstillägget varaktiga funktioner för Azure Functions."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 5ebab8660dfe21984e1a7f9a1cb925aea60de213
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="diagnostics-in-durable-functions-azure-functions"></a>Diagnostik i varaktiga funktioner (Azure-funktioner)

Det finns flera alternativ för att diagnostisera problem med [varaktiga funktioner](durable-functions-overview.md). Vissa av dessa alternativ är desamma för vanliga funktioner och några av dem är unika för beständig funktioner.

## <a name="application-insights"></a>Application Insights

[Application Insights](../application-insights/app-insights-overview.md) är det rekommenderade sättet att göra diagnostik- och övervakning i Azure Functions. Samma gäller varaktiga funktioner. En översikt över hur man utnyttjar Application Insights i funktionen appen finns [övervakaren Azure Functions](functions-monitoring.md).

Azure Functions varaktiga tillägget skickar också *spårningshändelser* som gör att du kan spåra körningen av en orchestration slutpunkt till slutpunkt. Dessa kan hittas och ställa frågor med hjälp av den [Application Insights Analytics](../application-insights/app-insights-analytics.md) verktyg i Azure-portalen.

### <a name="tracking-data"></a>Spårningsdata

Varje händelse livscykel orchestration instansen orsakar en spårning händelse skrivs till den **spårningar** samling i Application Insights. Den här händelsen innehåller en **customDimensions** nyttolasten med flera fält.  Fältnamn är inledd med `prop__`.

* **hubName**: namnet på din orkestreringarna kör uppgiften hubben.
* **appName**: namnet på funktionen appen. Detta är användbart när du har flera funktionen appar som delar samma Application Insights-instans.
* **slotName**: den [distributionsplatsen](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/) som den aktuella funktion appen körs. Detta är användbart när du använder distributionsplatser till version din orkestreringarna.
* **functionName**: namnet på funktionen orchestrator eller aktiviteten.
* **egenskaperna functionType**: typ av funktionen, t.ex **Orchestrator** eller **aktiviteten**.
* **instanceId**: unikt ID för orchestration-instans.
* **tillstånd**: Körningstillståndet livscykel för instansen. Giltiga värden är:
    * **Schemalagda**: funktionen har schemalagts för körning men har inte startats ännu.
    * **Starta**: funktionen har startats men har inte ännu inväntas eller slutförts.
    * **Inväntas**: orchestrator har schemalagt del arbete och väntar på att slutföra.
    * **Lyssning**: orchestrator lyssnar efter en extern händelseavisering.
    * **Slutföra**: funktionen har slutförts.
    * **Det gick inte**: funktionen misslyckades med ett fel.
* **Orsak**: ytterligare data som är associerade med spårning av händelsen. Om en instans väntar för en extern händelseavisering visar fältet namnet på den händelse som väntar. Om en funktion har misslyckats detta kommer att innehålla felinformation.
* **isReplay**: booleskt värde som anger om spårning av händelsen är för spelas körning.
* **extensionVersion**: version av beständiga Task-tillägget. Detta är särskilt viktiga data när reporting möjliga buggar i tillägget. Långvariga instanser kan rapportera flera versioner om en uppdatering sker när den körs. 

Detaljnivå för att spåra data som sänds till Application Insights kan konfigureras i den `logger` avsnitt i den `host.json` filen.

```json
{
    "logger": {
        "categoryFilter": {
            "categoryLevels": {
                "Host.Triggers.DurableTask": "Information"
            }
        }
    }
}
```

Som standard är alla spårning händelser orsakat. Du kan minska mängden data genom att ange `Host.Triggers.DurableTask` till `"Warning"` eller `"Error"` då spårningshändelser kommer endast att orsakat för undantagsfall.

> [!WARNING]
> Som standard prov Application Insights telemetri av Azure Functions-runtime att undvika avger data för ofta. Detta kan orsaka spårningsinformation går förlorade vid många livscykel händelser under en kort tidsperiod. Den [Azure Functions-övervakning artikel](functions-monitoring.md#configure-sampling) förklarar hur du konfigurerar det här beteendet.

### <a name="single-instance-query"></a>Instans-fråga

Följande fråga visar av historiska spårningsdata för en enda instans av den [Hello sekvens](durable-functions-sequence.md) fungerar orchestration. De skrivs med hjälp av den [Application Insights Query Language (AIQL)](https://docs.loganalytics.io/docs/Language-Reference). Filtreras bort replay körning så att endast den *logiska* körning av sökväg visas.

```AIQL
let targetInstanceId = "bf71335b26564016a93860491aa50c7f";
let start = datetime(2017-09-29T00:00:00);
traces
| where timestamp > start and timestamp < start + 30m
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = customDimensions["prop__functionName"]
| extend instanceId = customDimensions["prop__instanceId"]
| extend state = customDimensions["prop__state"]
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| where isReplay == false
| where instanceId == targetInstanceId
| project timestamp, functionName, state, instanceId, appName = cloud_RoleName
```
Resultatet är en lista över spåra händelser som visar sökvägen för körningen av orchestration, inklusive eventuella aktivitet funktioner.

![Application Insights-fråga](media/durable-functions-diagnostics/app-insights-single-instance-query.png)

> [!NOTE]
> Vissa av dessa spårningshändelser kan vara i fel ordning på grund av bristande precision i den `timestamp` kolumn. Detta spåras i GitHub som [utfärda #71](https://github.com/Azure/azure-functions-durable-extension/issues/71).

### <a name="instance-summary-query"></a>Översikt över instans-fråga

Följande fråga visar status för alla orchestration-instanser som körs i ett angivet tidsintervall.

```AIQL
let start = datetime(2017-09-30T04:30:00);
traces
| where timestamp > start and timestamp < start + 1h
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = tostring(customDimensions["prop__functionName"])
| extend instanceId = tostring(customDimensions["prop__instanceId"])
| extend state = tostring(customDimensions["prop__state"])
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend output = tostring(customDimensions["prop__output"])
| where isReplay == false
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```
Resultatet är en lista över instans-ID: N och deras aktuella Körningsstatus.

![Application Insights-fråga](media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Loggning

Det är viktigt att Tänk orchestrator replay beteende när du skriver loggarna direkt från en orchestrator-funktion. Tänk dig följande orchestrator-funktion:

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```

Den resulterande loggdata ska se ut ungefär så här:

```txt
Calling F1.
Calling F1.
Calling F2.
Calling F1.
Calling F2.
Calling F3.
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> Kom ihåg att när loggarna anspråk att anropa F1 och F2 F3, dessa funktioner är endast *faktiskt* kallas första gången de påträffas. Efterföljande anrop under replay hoppas över och utdata spelas upp till orchestrator-logik.

Om du vill bara logga in icke replay-körningen kan du skriva villkorsuttryck till loggen om `IsReplaying` är `false`. Överväg att exemplet ovan, men nu med replay-kontroller.

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    if (!ctx.IsReplaying) log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    if (!ctx.IsReplaying) log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    if (!ctx.IsReplaying) log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```
Med den här ändringen är loggutdata följande:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

## <a name="debugging"></a>Felsökning

Azure Functions stöder felsökning Funktionskoden direkt och som har stöd för samma följer vidarebefordra till varaktiga funktion, oavsett om de körs i Azure eller lokalt. Det finns dock några beteenden vara medveten om när du felsöker:

* **Spela upp**: Orchestrator-funktioner regelbundet spelas upp när nya indata tas emot. Detta innebär en enda *logiska* körning av en orchestrator-funktion kan resultera i att träffa samma brytpunkt flera gånger, särskilt om det angetts tidigt i funktionskoden.
* **Await**: när en `await` har uppstått, den ger kontroll tillbaka till den beständiga aktiviteten Framework dispatcher. Om det här är första gången en viss `await` har påträffade, associerade aktiviteten är *aldrig* återupptas. Eftersom aktiviteten fortsätter aldrig, stega igenom *över* await (F10 i Visual Studio) går inte att. Gå över fungerar bara när en aktivitet som spelas.
* **Messaging tidsgränser**: varaktiga funktioner använder internt Kömeddelanden till enheten körning av både orchestrator-funktioner och funktioner för aktiviteten. I en miljö med flera Virtuella leda dela i Felsökning för längre tid till en annan virtuell dator att hämta meddelandet, vilket resulterar i dubbla körning. Det här beteendet finns för vanlig kö-utlösaren-funktioner, men är viktigt att peka i den här kontexten eftersom köerna är en implementering detaljer.

> [!TIP]
> När inställningen brytpunkter, om du vill bryta bara på icke-repetitionsattacker körning, kan du ange en villkorlig brytpunkt att endast om radbrytningar `IsReplaying` är `false`.

## <a name="storage"></a>Lagring

Som standard lagras varaktigt funktioner tillstånd i Azure Storage. Det innebär att du kan kontrollera statusen för din orkestreringarna med hjälp av verktyg som [Microsoft Azure Lagringsutforskaren](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Azure Lagringsutforskaren skärmbild](media/durable-functions-diagnostics/storage-explorer.png)

Detta är användbart för felsökning eftersom du se exakt vilka tillstånd som en orchestration kanske är i. Meddelanden i köer kan också undersökas om du vill veta vilka arbete väntar (eller fastnat i vissa fall).

> [!WARNING]
> Det är praktiskt att se körningstiden i table storage, Undvik att ta med eventuella beroende på den här tabellen. Den kan ändras när tillägget varaktiga funktioner utvecklas.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Lär dig hur du använder beständiga timers](durable-functions-timers.md)