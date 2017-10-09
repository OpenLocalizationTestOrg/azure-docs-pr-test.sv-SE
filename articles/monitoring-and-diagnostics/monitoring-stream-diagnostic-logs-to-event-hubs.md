---
title: aaaStream Azure diagnostikloggar tooan Event Hubs Namespace | Microsoft Docs
description: "Lär dig hur toostream Azure diagnostiska loggar tooan Händelsehubbar namnområde."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 42bc4845-c564-4568-b72d-0614591ebd80
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: 00092ea8f3fe4fa1476e3a697bf1e8645dd21e6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-azure-diagnostic-logs-tooan-event-hubs-namespace"></a>Dataströmmen Azure diagnostikloggar tooan Event Hubs Namespace
**[Azure diagnostikloggar](monitoring-overview-of-diagnostic-logs.md)**  kan strömmas i nära realtid tooany program med hjälp av hello inbyggda ”exportera tooEvent NAV” alternativ i hello portalen eller genom att aktivera hello Service Bus regel-ID i diagnostikinställningen via hello Azure PowerShell Cmdlet: ar eller Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Vad du kan göra med diagnostik loggar och Händelsehubbar
Här följer några olika sätt som du kan använda hello strömning kapaciteten för diagnostikloggar:

* **Dataströmmen loggar too3rd loggning och telemetri part** – över tiden, Händelsehubbar strömning ska bli hello mekanism toopipe dina diagnostikloggar i toothird parts Siem och logga Analyslösningar.
* **Visa tjänstens hälsa med streaming ”varm path” data tooPowerBI** – med Händelsehubbar, Stream Analytics och PowerBI, du kan enkelt transformera diagnostikdata i toonear realtidsinsikter på Azure-tjänster. [Den här dokumentationen artikeln ger en översikt över hur tooset in Händelsehubbar, bearbeta data med Stream Analytics och använda PowerBI som utdata](../stream-analytics/stream-analytics-power-bi-dashboard.md). Här följer några tips om att hämta inställd med diagnostikloggar:
  
  * En händelsehubb för diagnostikloggar skapas automatiskt när du markerar alternativet hello hello-portalen eller aktivera via PowerShell, så du tooselect hello händelsehubb i hello namnområde med hello namn som börjar med **insights**.
  * hello följande SQL-kod är ett exempel Stream Analytics query som du kan använda tooparse loggdata för alla hello i tooa PowerBI-tabellen:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want tootrack]
    INTO
    [OutputSourceName – hello PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Skapa en anpassad telemetri och loggning plattform** – om du redan har en specialbyggt telemetri plattform eller är bara du tänker skapa en mycket skalbar hello publicera och prenumerera uppbyggnad Händelsehubbar kan du tooflexibly mata in diagnostik loggar. [Se Dan Rosanova guide toousing Händelsehubbar i en global skala telemetri platform här](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Strömning av diagnostiska loggar
Du kan aktivera strömning av diagnostikloggar programmässigt via hello-portalen eller med hjälp av hello [Azure övervakaren REST API: er](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). Oavsett hur du skapar en diagnostikinställningen där du kan ange ett namnområde för Händelsehubbar och hello loggen kategorier och mått som du vill toosend i toohello namnområde. En händelsehubb har skapats i hello namnområdet för varje logg kategori som du aktiverar. En diagnostik **loggen kategori** är en typ av logg som en resurs kan samla in.

> [!WARNING]
> Aktivera och strömning diagnostikloggar från beräkningsresurser (till exempel virtuella datorer eller Service Fabric) [kräver en annan uppsättning steg](../event-hubs/event-hubs-streaming-azure-diags-data.md).
> 
> 

hello Service Bus eller Händelsehubbar namnområdet inte har toobe i hello samma prenumeration som hello resurs avger loggar så länge hello användare som konfigurerar hello inställningen har lämplig RBAC åtkomst tooboth prenumerationer.

## <a name="stream-diagnostic-logs-using-hello-portal"></a>Dataströmmen diagnostikloggar med hello-portalen
1. Navigera tooAzure Övervakare i hello-portalen och klicka på **diagnostikinställningar**

    ![Avsnittet av Azure-Monitor övervakning](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. Du kan också hello-filterlista av resursgruppen eller resursen, och klicka sedan på hello resursen som du vill att tooset en diagnostikinställningen.

3. Om det finns inga inställningar på hello resursen som du har valt, kan du ange toocreate en inställning. Klicka på ”Aktivera diagnostik”.

   ![Lägg till diagnostikinställningen - inga befintliga inställningar](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   Om det finns befintliga inställningarna på hello resurs, visas en lista över inställningar som redan har konfigurerats på den här resursen. Klicka på ”Lägg till diagnostikinställningen”.

   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Ge ange ett namn och kryssrutan för hello **dataströmmen tooan händelsehubb**, välj sedan ett namnområde för Händelsehubbar.
   
   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   hello namnområde som valts kommer att där hello händelsehubb skapas (om det är första gången du strömning diagnostikloggar) eller direktuppspelas för (om det finns redan resurser som direktuppspelas loggen kategori toothis namnutrymmet), och hello principen definierar hello behörigheter som har hello strömmande mekanism. Idag kräver strömning tooan händelsehubb behörighet att hantera, skicka och lyssna. Du kan skapa eller ändra principer för Händelsehubbar namnområde delad åtkomst i hello portal under hello konfigurera fliken för namnområdet. tooupdate något av dessa diagnostikinställningar hello klienten måste ha behörighet för hello ListKey på hello Händelsehubbar auktoriseringsregeln.

4. Klicka på **Spara**.

Efter en liten stund hello nya inställningen visas i din lista över inställningar för den här resursen och diagnostiska loggar strömmas toothat storage-konto när nya händelsedata genereras.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
strömning via hello tooenable [Azure PowerShell-Cmdlets](insights-powershell-samples.md), kan du använda hello `Set-AzureRmDiagnosticSetting` med följande parametrar:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

hello regel-ID för Service Bus är en sträng med formatet: `{Service Bus resource ID}/authorizationrules/{key name}`, till exempel `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.

### <a name="via-azure-cli"></a>Via Azure CLI
strömning via hello tooenable [Azure CLI](insights-cli-samples.md), du kan använda hello `insights diagnostic set` kommandot så här:

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

Använd hello samma format för Service Bus regel-ID som förklaras för hello PowerShell-Cmdlet.

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a>Hur jag för att använda hello loggdata från Event Hubs?
Här är exempel utdata från Händelsehubbar:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Elementnamn | Beskrivning |
| --- | --- |
| Poster |En matris med alla händelser i den här nyttolasten. |
| time |Tiden då hello händelsen inträffade. |
| category |Loggen kategorin för den här händelsen. |
| resourceId |Resurs-ID för hello-resurs som genereras av den här händelsen. |
| operationName |Namnet på hello igen. |
| nivå |Valfri. Anger hello loggningsnivån för händelsen. |
| properties |Egenskaper för hello-händelse. |

Du kan visa en lista över alla resursproviders som har stöd för direktuppspelning tooEvent Hubs [här](monitoring-overview-of-diagnostic-logs.md).

## <a name="stream-data-from-compute-resources"></a>Dataströmmen data från beräkningsresurser
Du kan också strömma diagnostikloggar från beräkningsresurser med hello Windows Azure-diagnostik-agenten. [Finns den här artikeln](../event-hubs/event-hubs-streaming-azure-diags-data.md) för hur tooset som upp.

## <a name="next-steps"></a>Nästa steg
* [Läs mer om Azure diagnostikloggar](monitoring-overview-of-diagnostic-logs.md)
* [Kom igång med Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

