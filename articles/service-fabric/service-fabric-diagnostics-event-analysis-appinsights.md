---
title: "aaaAzure Service Fabric-Händelseanalys med Application Insights | Microsoft Docs"
description: "Läs mer om visualisera och analysera händelser med hjälp av Application Insights för övervakning och diagnostik av Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 59bb5a409f2951e5b2034049e782dd0da67f933c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Händelseanalys och visualisering med Application Insights

Azure Application Insights är en utökningsbar plattform för övervakning av program och diagnostik. Den innehåller en kraftfull analytics och fråga efter verktyget, anpassningsbar instrumentpanel och visualiseringar och ytterligare alternativ, inklusive automatiserade aviseringar. Det är hello bör plattform för övervakning och diagnostik för Service Fabric-program och tjänster.

## <a name="setting-up-application-insights"></a>Konfigurera Application Insights

### <a name="creating-an-ai-resource"></a>Skapa en AI-resurs

toocreate en AI resurs, head över toohello Azure Marketplace och Sök efter ”Application Insights”. Det ska visas som första hello-lösning (det är under kategori ”webb + mobil”). Klicka på **skapa** när du tittar på hello rätt resurs (bekräfta att din sökväg matchar hello bilden nedan).

![Ny Application Insights-resurs](media/service-fabric-diagnostics-event-analysis-appinsights/create-new-ai-resource.png)

Du behöver toofill ut information tooprovision hello resurser på rätt sätt. I hello *programtyp* fält används ”ASP.NET-webbprogram” om du kommer att använda någon av Service Fabric programmeringsmodeller eller publicera ett .NET-program toohello kluster. Använd ”Allmänt” om du ska distribuera gäst körbara filer och behållare. I allmänhet standard toousing ”ASP.NET-webbprogram” tookeep alternativen öppna hello framtida. hello heter in tooyour inställningar och både hello resursgruppen och prenumeration är kan ändras efter distributionen av hello resurs. Vi rekommenderar att AI-resurs i hello samma resursgrupp som klustret. Om du behöver mer information, se [skapa Application Insights-resurs](../application-insights/app-insights-create-new-resource.md)

Du behöver hello AI Instrumentation nyckeln tooconfigure AI med verktyget du händelsen aggregering. När AI-resursen har konfigurerats (tar några minuter efter hello distribution verifieras), navigera tooit och hitta hello **egenskaper** avsnittet hello vänstra navigeringsfältet. Ett nytt blad öppnas som visar ett *INSTRUMENTATION NYCKELN*. Om du behöver toochange hello prenumeration eller resursgrupp hello resurs, kan det göras här samt.

### <a name="configuring-ai-with-wad"></a>Konfigurera AI med BOMULLSTUSS

Det finns två sätt toosend data från BOMULLSTUSS tooAzure AI, som uppnås genom att lägga till en AI sink toohello BOMULLSTUSS konfiguration enligt anvisningarna i [i den här artikeln](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Lägg till en AI Instrumentation nyckel när du skapar ett kluster i Azure-portalen

![Lägga till en AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

När du skapar ett kluster, om diagnostik är aktiverat ””, ett valfritt fält tooenter en insikter Programinstrumentering nyckel visas. Om du klistrar in din AI IKey här hello AI sink blir automatiskt konfigurerade för dig i hello Resource Manager-mall som används toodeploy klustret.

#### <a name="add-hello-ai-sink-toohello-resource-manager-template"></a>Lägg till hello AI Sink toohello Resource Manager-mall

Lägg till en ”Sink” genom att inkludera hello följande två ändringar i hello ”WadCfg” av hello Resource Manager-mall:

1. Lägg till hello sink konfiguration:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Inkludera hello Sink i hello DiagnosticMonitorConfiguration genom att lägga till följande rad i ”DiagnosticMonitorConfiguration” hello av hello ”WadCfg”:

    ```json
    "sinks": "applicationInsights"
    ```

I båda hello kodstycken ovan hello var namnet ”applicationInsights” används toodescribe hello mottagare. Detta är inte ett krav och så länge hello namnet på hello sink ingår i ”sänkor”, du kan ange hello tooany sträng.

För närvarande visas loggar från hello kluster som spåren i AIS Loggvisaren. Eftersom de flesta av hello spår som kommer från hello plattform för nivå ”information” bör överväga du att ändra hello sink configuration tooonly skicka loggar av typen ”kritiska” eller ”Error”. Detta kan göras genom att lägga till ”kanaler” tooyour mottagare som visas i [i den här artikeln](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Om du använder en felaktig AI IKey i portalen eller i mallen för hanteraren för filserverresurser, har du toomanually ändra hello nyckel och uppdatera klustret hello / distribuera den. 

### <a name="configuring-ai-with-eventflow"></a>Konfigurera AI med EventFlow

Om du använder EventFlow tooaggregate händelser, se till att tooimport hello `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet-paketet. hello följande har toobe som ingår i hello *matar ut* avsnitt i hello *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        // (replace hello following value with your AI resource's instrumentation key)
        "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
]
```

Gör att toomake hello krävs ändringar i filter och även innehåller andra indata (tillsammans med deras respektive NuGet-paket).

## <a name="aisdk"></a>AI. SDK

Det rekommenderas vanligtvis toouse EventFlow och BOMULLSTUSS som aggregering lösningar, eftersom de tillåter för en mer modulära metoden toodiagnostics och övervakning, dvs. Om du vill toochange dina utdata från EventFlow, krävs ingen ändring tooyour faktiska Instrumentation bara en enkel ändring tooyour konfigurationsfil. Om dock bestämma tooinvest i med hjälp av Application Insights och är inte troligt toochange tooa olika plattformar, bör du fundera på med AIS nya SDK för att skicka dem tooAI och samla händelser. Det innebär att du inte längre har tooconfigure EventFlow toosend tooAI dina data, men i stället installeras hello ApplicationInsight Service Fabric-NuGet-paketet. Information om hello paketet finns [här](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Application Insights-stöd för Mikrotjänster och behållare](https://azure.microsoft.com/app-insights-microservices/) visar några hello nya funktioner som arbete utförs i (för närvarande i beta), vilket gör att du toohave bättre out box övervakningsalternativ med AI. Dessa omfattar beroende spårning (används för att bygga en AppMap av alla tjänster och program i ett kluster och hello kommunikation mellan dem) och bättre korrelation av spår som kommer från dina tjänster (hjälper i bättre lokalisera ett problem i hello arbetsflöde för en app eller tjänst).

Om du utvecklar i .NET och kommer troligen att använder vissa av Service Fabric programmeringsmodeller och är villigt toouse AI som din plattform för att visualisera och analysera händelse och loggar data rekommenderar vi att du går via hello AI SDK väg som övervakningen och diagnostik för arbetsflödet. Läs [detta](../application-insights/app-insights-asp-net-more.md) och [detta](../application-insights/app-insights-asp-net-trace-logs.md) tooget igång med att använda AI toocollect och visa loggar.

## <a name="navigating-hello-ai-resource-in-azure-portal"></a>Navigera hello AI-resurs i Azure-portalen

När du har konfigurerat AI som utdata för händelser och loggar, ska information starta tooshow i AI-resursen på några minuter. Navigera toohello AI-resurs som tar du toohello AI resurs instrumentpanelen. Klicka på **Sök** i hello AI Aktivitetsfältet toosee hello senaste spårningar som har tagit emot och toobe kan toofilter igenom dem.

*Metrics Explorer* är användbart för att skapa anpassade instrumentpaneler baserat på mått som ditt program, tjänster och klustret kan reporting. Se [utforska mått i Application Insights](../application-insights/app-insights-metrics-explorer.md) tooset in några diagram själv baserat på hello data du samlar in.

Klicka på **Analytics** tar dig toohello Application Insights Analytics-portalen där du kan fråga efter händelser och spårningar med större scope och -alternativ. Läs mer om detta i [analyser i Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Nästa steg

* [Konfigurera aviseringar i AI](../application-insights/app-insights-alerts.md) toobe meddelas om ändringar i prestanda eller användning
* [Identifiering i Application Insights för smartkort](../application-insights/app-insights-proactive-diagnostics.md) utför en proaktiv analys av hello telemetri skickas tooAI toowarn dig om potentiella problem med prestanda
