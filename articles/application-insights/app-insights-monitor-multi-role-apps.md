---
title: "aaaAzure Application Insights stöd för flera komponenter, mikrotjänster och behållare | Microsoft Docs"
description: "Övervakning av appar som består av flera komponenter eller roller för prestanda och användning."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: 6185eedf32ec450d7541603b94de6c3dcdf64a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>Övervaka flera komponenten program med Application Insights (förhandsversion)

Du kan övervaka appar som består av flera server-komponenter, roller eller -tjänster med [Azure Application Insights](app-insights-overview.md). hello hälsotillståndet för hello komponenter och hello relationer mellan dem visas på en enda programavbildningen. Du kan spåra enskilda åtgärder med hjälp av flera komponenter med automatisk HTTP korrelation. Behållaren diagnostik integrering och samverkar med programtelemetri. Använda en enda Application Insights-resurs för alla hello komponenter för programmet. 

![Flera komponenten programavbildningen](./media/app-insights-monitor-multi-role-apps/app-map.png)

Vi använder ”komponent' här toomean någon fungerande del av ett omfattande program. Till exempel en typisk affärsprogram kan bestå av klientkod som körs i webbläsare, prata tooone eller mer app webbtjänster, vilka i sin tur använder tillbaka avsluta tjänster. Server-komponenter kan vara lokalt på i hello moln, eller kanske Azure webb- och arbetsroller roller eller kan köras i behållare, till exempel Docker eller Service Fabric. 

### <a name="sharing-a-single-application-insights-resource"></a>Dela en enda Application Insights-resurs 

hello viktiga tekniken här är toosend telemetri från varje komponent i ditt program toohello samma Application Insights-resurs, men Använd hello `cloud_RoleName` egenskapen toodistinguish komponenter vid behov. hello Application Insights SDK lägger till hello `cloud_RoleName` egenskapen toohello telemetri komponenter genererar. Till exempel hello SDK kommer att lägga till en webbplatsens namn eller tjänsten rollen namn toohello `cloud_RoleName` egenskapen. Du kan åsidosätta detta värde med en telemetryinitializer. hello programavbildningen använder hello `cloud_RoleName` egenskapen tooidentify hello komponenter på hello karta.

Mer information om hur Åsidosätt hello `cloud_RoleName` egenskapen Se [lägga till egenskaper: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

I vissa fall kan detta kanske inte är rätt och du kanske föredrar toouse separata resurser för olika grupper av komponenter. Till exempel behöva toouse olika resurser för att hantera fakturering. Med hjälp av separata resurser innebär att du inte ser alla hello-komponenter som visas på en enda programavbildningen; och att du kan inte fråga på komponenter i [Analytics](app-insights-analytics.md). Du kan också ha tooset hello separata resurser.

Med den begränsning antar vi i hello resten av det här dokumentet som du vill toosend data från flera komponenter tooone Application Insights-resurs.

## <a name="configure-multi-component-applications"></a>Konfigurera flera komponenten program

Mappa om tooget flera komponenten program måste du tooachieve dessa mål:

* **Installera hello senaste förhandsversionen** Application Insights-paketet i varje komponent av programmet hello. 
* **Dela en enda Application Insights-resurs** för alla hello serverkomponenter i ditt program.
* **Aktivera flera rollen programavbildningen** hello förhandsgranskningar-bladet.

Konfigurera varje komponent i ditt program med hello lämplig metod för typen. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)

### <a name="1-install-hello-latest-pre-release-package"></a>1. Installera hello senaste förhands-paket

Uppdatera eller installera hello program insikter paket i hello projekt för varje serverkomponent. Om du använder Visual Studio:

1. Högerklicka på projektet och välj **hantera NuGet-paket**. 
2. Välj **inkludera förhandsversion**.
3. Om Application Insights visas paketen i uppdateringar, markera dem. 

    Annars kan söka efter och installera hello lämpligt paket:
    
    * Microsoft.ApplicationInsights.WindowsServer
    * Microsoft.ApplicationInsights.ServiceFabric - komponenter som körs som gäst körbara filer och Docker-behållare som kör ett i Service Fabric-program
    * Microsoft.ApplicationInsights.ServiceFabric.Native - för tillförlitlig tjänster i ServiceFabric program
    * Microsoft.ApplicationInsights.Kubernetes för komponenter som körs i Docker på Kubernetes

### <a name="2-share-a-single-application-insights-resource"></a>2. Dela en enda Application Insights-resurs

* Högerklicka på ett projekt i Visual Studio och välj **konfigurera Application Insights**, eller **Programinsikter > Konfigurera**. Använd hello guiden toocreate Application Insights-resurs för första hello-projektet. För efterföljande projekt hello väljer du samma resurs.
* Om det finns inga Application Insights-menyn, konfigurera manuellt:

   1. I [Azure-portalen](https://portal,azure.com), öppna hello Application Insights-resurs som du redan har skapats för en annan komponent.
   2. I hello översikt bladet och öppna hello Essentials nedrullningsbara fliken kopia hello **Instrumentation nyckel.**
   3. Öppna ApplicationInsights.config och infoga i projektet:`<InstrumentationKey>your copied key</InstrumentationKey>`

![Kopiera hello instrumentation viktiga toohello .config-fil](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a>3. Aktivera flera rollen programavbildningen

Öppna hello resurs för ditt program i hello Azure-portalen. Aktivera i hello förhandsgranskningar bladet *flera rollen programavbildningen*.

### <a name="4-enable-docker-metrics-optional"></a>4. Aktivera Docker mätvärden (valfritt) 

Om en komponent som körs i en Docker som finns på en Windows Azure-dator, kan du samla in ytterligare mått från hello behållare. Infoga det i ditt [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurationsfil:

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-tooseparate-components"></a>Använda cloud_RoleName tooseparate komponenter

Hej `cloud_RoleName` egenskapen är anslutna tooall telemetri. Den identifierar hello komponent - hello roll eller tjänst - kommer hello telemetri. (Det är inte hello samma som cloud_RoleInstance som separerar identiska roller som körs parallellt på flera serverprocesser eller datorer.)

Du kan filtrera eller segmentera dina telemetri som använder den här egenskapen i hello-portalen. I det här exemplet är hello fel bladet filtrerade tooshow bara information från hello frontwebbservern tjänsten, filtrera fel från hello CRM API-serverdel:

![Mått diagram åtskilda med rollen Molnnamn](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Spåra åtgärder mellan komponenter

Du kan spåra från en komponent tooanother, hello-anrop som görs under bearbetning av en enskild åtgärd.


![Visa telemetri för åtgärden](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Klicka dig igenom tooa korrelerade lista över telemetri för den här åtgärden över hello frontwebbservern och hello backend-API:

![Söka i komponenter](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Nästa steg

* [Separata telemetri från utveckling, Test och produktion](app-insights-separate-resources.md)
