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
# <a name="stream-azure-diagnostic-logs-tooan-event-hubs-namespace"></a><span data-ttu-id="744a1-103">Dataströmmen Azure diagnostikloggar tooan Event Hubs Namespace</span><span class="sxs-lookup"><span data-stu-id="744a1-103">Stream Azure Diagnostic Logs tooan Event Hubs Namespace</span></span>
<span data-ttu-id="744a1-104">**[Azure diagnostikloggar](monitoring-overview-of-diagnostic-logs.md)**  kan strömmas i nära realtid tooany program med hjälp av hello inbyggda ”exportera tooEvent NAV” alternativ i hello portalen eller genom att aktivera hello Service Bus regel-ID i diagnostikinställningen via hello Azure PowerShell Cmdlet: ar eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="744a1-104">**[Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md)** can be streamed in near real time tooany application using hello built-in “Export tooEvent Hubs” option in hello Portal, or by enabling hello Service Bus Rule ID in a diagnostic setting via hello Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a><span data-ttu-id="744a1-105">Vad du kan göra med diagnostik loggar och Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="744a1-105">What you can do with diagnostics logs and Event Hubs</span></span>
<span data-ttu-id="744a1-106">Här följer några olika sätt som du kan använda hello strömning kapaciteten för diagnostikloggar:</span><span class="sxs-lookup"><span data-stu-id="744a1-106">Here are just a few ways you might use hello streaming capability for Diagnostic Logs:</span></span>

* <span data-ttu-id="744a1-107">**Dataströmmen loggar too3rd loggning och telemetri part** – över tiden, Händelsehubbar strömning ska bli hello mekanism toopipe dina diagnostikloggar i toothird parts Siem och logga Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="744a1-107">**Stream logs too3rd party logging and telemetry systems** – Over time, Event Hubs streaming will become hello mechanism toopipe your diagnostic logs in toothird-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="744a1-108">**Visa tjänstens hälsa med streaming ”varm path” data tooPowerBI** – med Händelsehubbar, Stream Analytics och PowerBI, du kan enkelt transformera diagnostikdata i toonear realtidsinsikter på Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="744a1-108">**View service health by streaming “hot path” data tooPowerBI** – Using Event Hubs, Stream Analytics, and PowerBI, you can easily transform your diagnostics data in toonear real-time insights on your Azure services.</span></span> <span data-ttu-id="744a1-109">[Den här dokumentationen artikeln ger en översikt över hur tooset in Händelsehubbar, bearbeta data med Stream Analytics och använda PowerBI som utdata](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="744a1-109">[This documentation article gives a great overview of how tooset up Event Hubs, process data with Stream Analytics, and use PowerBI as an output](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span></span> <span data-ttu-id="744a1-110">Här följer några tips om att hämta inställd med diagnostikloggar:</span><span class="sxs-lookup"><span data-stu-id="744a1-110">Here are a few tips for getting set up with diagnostic logs:</span></span>
  
  * <span data-ttu-id="744a1-111">En händelsehubb för diagnostikloggar skapas automatiskt när du markerar alternativet hello hello-portalen eller aktivera via PowerShell, så du tooselect hello händelsehubb i hello namnområde med hello namn som börjar med **insights**.</span><span class="sxs-lookup"><span data-stu-id="744a1-111">An event hub for a category of diagnostic logs is created automatically when you check hello option in hello portal or enable it through PowerShell, so you want tooselect hello event hub in hello namespace with hello name that starts with **insights-**.</span></span>
  * <span data-ttu-id="744a1-112">hello följande SQL-kod är ett exempel Stream Analytics query som du kan använda tooparse loggdata för alla hello i tooa PowerBI-tabellen:</span><span class="sxs-lookup"><span data-stu-id="744a1-112">hello following SQL code is a sample Stream Analytics query that you can use tooparse all hello log data in tooa PowerBI table:</span></span>

    ```sql
    SELECT
    records.ArrayValue.[Properties you want tootrack]
    INTO
    [OutputSourceName – hello PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* <span data-ttu-id="744a1-113">**Skapa en anpassad telemetri och loggning plattform** – om du redan har en specialbyggt telemetri plattform eller är bara du tänker skapa en mycket skalbar hello publicera och prenumerera uppbyggnad Händelsehubbar kan du tooflexibly mata in diagnostik loggar.</span><span class="sxs-lookup"><span data-stu-id="744a1-113">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, hello highly scalable publish-subscribe nature of Event Hubs allows you tooflexibly ingest diagnostic logs.</span></span> <span data-ttu-id="744a1-114">[Se Dan Rosanova guide toousing Händelsehubbar i en global skala telemetri platform här](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span><span class="sxs-lookup"><span data-stu-id="744a1-114">[See Dan Rosanova’s guide toousing Event Hubs in a global scale telemetry platform here](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span></span>

## <a name="enable-streaming-of-diagnostic-logs"></a><span data-ttu-id="744a1-115">Strömning av diagnostiska loggar</span><span class="sxs-lookup"><span data-stu-id="744a1-115">Enable streaming of diagnostic logs</span></span>
<span data-ttu-id="744a1-116">Du kan aktivera strömning av diagnostikloggar programmässigt via hello-portalen eller med hjälp av hello [Azure övervakaren REST API: er](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span><span class="sxs-lookup"><span data-stu-id="744a1-116">You can enable streaming of diagnostic logs programmatically, via hello portal, or using hello [Azure Monitor REST APIs](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span></span> <span data-ttu-id="744a1-117">Oavsett hur du skapar en diagnostikinställningen där du kan ange ett namnområde för Händelsehubbar och hello loggen kategorier och mått som du vill toosend i toohello namnområde.</span><span class="sxs-lookup"><span data-stu-id="744a1-117">Either way, you create a diagnostic setting in which you specify an Event Hubs namespace and hello log categories and metrics you want toosend in toohello namespace.</span></span> <span data-ttu-id="744a1-118">En händelsehubb har skapats i hello namnområdet för varje logg kategori som du aktiverar.</span><span class="sxs-lookup"><span data-stu-id="744a1-118">An event hub is created in hello namespace for each log category you enable.</span></span> <span data-ttu-id="744a1-119">En diagnostik **loggen kategori** är en typ av logg som en resurs kan samla in.</span><span class="sxs-lookup"><span data-stu-id="744a1-119">A diagnostic **log category** is a type of log that a resource may collect.</span></span>

> [!WARNING]
> <span data-ttu-id="744a1-120">Aktivera och strömning diagnostikloggar från beräkningsresurser (till exempel virtuella datorer eller Service Fabric) [kräver en annan uppsättning steg](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span><span class="sxs-lookup"><span data-stu-id="744a1-120">Enabling and streaming diagnostic logs from Compute resources (for example, VMs or Service Fabric) [requires a different set of steps](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span></span>
> 
> 

<span data-ttu-id="744a1-121">hello Service Bus eller Händelsehubbar namnområdet inte har toobe i hello samma prenumeration som hello resurs avger loggar så länge hello användare som konfigurerar hello inställningen har lämplig RBAC åtkomst tooboth prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="744a1-121">hello Service Bus or Event Hubs namespace does not have toobe in hello same subscription as hello resource emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="stream-diagnostic-logs-using-hello-portal"></a><span data-ttu-id="744a1-122">Dataströmmen diagnostikloggar med hello-portalen</span><span class="sxs-lookup"><span data-stu-id="744a1-122">Stream diagnostic logs using hello portal</span></span>
1. <span data-ttu-id="744a1-123">Navigera tooAzure Övervakare i hello-portalen och klicka på **diagnostikinställningar**</span><span class="sxs-lookup"><span data-stu-id="744a1-123">In hello portal, navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Avsnittet av Azure-Monitor övervakning](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. <span data-ttu-id="744a1-125">Du kan också hello-filterlista av resursgruppen eller resursen, och klicka sedan på hello resursen som du vill att tooset en diagnostikinställningen.</span><span class="sxs-lookup"><span data-stu-id="744a1-125">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="744a1-126">Om det finns inga inställningar på hello resursen som du har valt, kan du ange toocreate en inställning.</span><span class="sxs-lookup"><span data-stu-id="744a1-126">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="744a1-127">Klicka på ”Aktivera diagnostik”.</span><span class="sxs-lookup"><span data-stu-id="744a1-127">Click "Turn on diagnostics."</span></span>

   ![Lägg till diagnostikinställningen - inga befintliga inställningar](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   <span data-ttu-id="744a1-129">Om det finns befintliga inställningarna på hello resurs, visas en lista över inställningar som redan har konfigurerats på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="744a1-129">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="744a1-130">Klicka på ”Lägg till diagnostikinställningen”.</span><span class="sxs-lookup"><span data-stu-id="744a1-130">Click "Add diagnostic setting."</span></span>

   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="744a1-132">Ge ange ett namn och kryssrutan för hello **dataströmmen tooan händelsehubb**, välj sedan ett namnområde för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="744a1-132">Give your setting a name and check hello box for **Stream tooan event hub**, then select an Event Hubs namespace.</span></span>
   
   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   <span data-ttu-id="744a1-134">hello namnområde som valts kommer att där hello händelsehubb skapas (om det är första gången du strömning diagnostikloggar) eller direktuppspelas för (om det finns redan resurser som direktuppspelas loggen kategori toothis namnutrymmet), och hello principen definierar hello behörigheter som har hello strömmande mekanism.</span><span class="sxs-lookup"><span data-stu-id="744a1-134">hello namespace selected will be where hello event hub is created (if this is your first time streaming diagnostic logs) or streamed too(if there are already resources that are streaming that log category toothis namespace), and hello policy defines hello permissions that hello streaming mechanism has.</span></span> <span data-ttu-id="744a1-135">Idag kräver strömning tooan händelsehubb behörighet att hantera, skicka och lyssna.</span><span class="sxs-lookup"><span data-stu-id="744a1-135">Today, streaming tooan event hub requires Manage, Send, and Listen permissions.</span></span> <span data-ttu-id="744a1-136">Du kan skapa eller ändra principer för Händelsehubbar namnområde delad åtkomst i hello portal under hello konfigurera fliken för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="744a1-136">You can create or modify Event Hubs namespace shared access policies in hello portal under hello Configure tab for your namespace.</span></span> <span data-ttu-id="744a1-137">tooupdate något av dessa diagnostikinställningar hello klienten måste ha behörighet för hello ListKey på hello Händelsehubbar auktoriseringsregeln.</span><span class="sxs-lookup"><span data-stu-id="744a1-137">tooupdate one of these diagnostic settings, hello client must have hello ListKey permission on hello Event Hubs authorization rule.</span></span>

4. <span data-ttu-id="744a1-138">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="744a1-138">Click **Save**.</span></span>

<span data-ttu-id="744a1-139">Efter en liten stund hello nya inställningen visas i din lista över inställningar för den här resursen och diagnostiska loggar strömmas toothat storage-konto när nya händelsedata genereras.</span><span class="sxs-lookup"><span data-stu-id="744a1-139">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are streamed toothat storage account as soon as new event data is generated.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="744a1-140">Via PowerShell-Cmdlets</span><span class="sxs-lookup"><span data-stu-id="744a1-140">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="744a1-141">strömning via hello tooenable [Azure PowerShell-Cmdlets](insights-powershell-samples.md), kan du använda hello `Set-AzureRmDiagnosticSetting` med följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="744a1-141">tooenable streaming via hello [Azure PowerShell Cmdlets](insights-powershell-samples.md), you can use hello `Set-AzureRmDiagnosticSetting` cmdlet with these parameters:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

<span data-ttu-id="744a1-142">hello regel-ID för Service Bus är en sträng med formatet: `{Service Bus resource ID}/authorizationrules/{key name}`, till exempel `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="744a1-142">hello Service Bus Rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`, for example, `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span></span>

### <a name="via-azure-cli"></a><span data-ttu-id="744a1-143">Via Azure CLI</span><span class="sxs-lookup"><span data-stu-id="744a1-143">Via Azure CLI</span></span>
<span data-ttu-id="744a1-144">strömning via hello tooenable [Azure CLI](insights-cli-samples.md), du kan använda hello `insights diagnostic set` kommandot så här:</span><span class="sxs-lookup"><span data-stu-id="744a1-144">tooenable streaming via hello [Azure CLI](insights-cli-samples.md), you can use hello `insights diagnostic set` command like this:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

<span data-ttu-id="744a1-145">Använd hello samma format för Service Bus regel-ID som förklaras för hello PowerShell-Cmdlet.</span><span class="sxs-lookup"><span data-stu-id="744a1-145">Use hello same format for Service Bus Rule ID as explained for hello PowerShell Cmdlet.</span></span>

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a><span data-ttu-id="744a1-146">Hur jag för att använda hello loggdata från Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="744a1-146">How do I consume hello log data from Event Hubs?</span></span>
<span data-ttu-id="744a1-147">Här är exempel utdata från Händelsehubbar:</span><span class="sxs-lookup"><span data-stu-id="744a1-147">Here is sample output data from Event Hubs:</span></span>

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

| <span data-ttu-id="744a1-148">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="744a1-148">Element Name</span></span> | <span data-ttu-id="744a1-149">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="744a1-149">Description</span></span> |
| --- | --- |
| <span data-ttu-id="744a1-150">Poster</span><span class="sxs-lookup"><span data-stu-id="744a1-150">records</span></span> |<span data-ttu-id="744a1-151">En matris med alla händelser i den här nyttolasten.</span><span class="sxs-lookup"><span data-stu-id="744a1-151">An array of all log events in this payload.</span></span> |
| <span data-ttu-id="744a1-152">time</span><span class="sxs-lookup"><span data-stu-id="744a1-152">time</span></span> |<span data-ttu-id="744a1-153">Tiden då hello händelsen inträffade.</span><span class="sxs-lookup"><span data-stu-id="744a1-153">Time at which hello event occurred.</span></span> |
| <span data-ttu-id="744a1-154">category</span><span class="sxs-lookup"><span data-stu-id="744a1-154">category</span></span> |<span data-ttu-id="744a1-155">Loggen kategorin för den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="744a1-155">Log category for this event.</span></span> |
| <span data-ttu-id="744a1-156">resourceId</span><span class="sxs-lookup"><span data-stu-id="744a1-156">resourceId</span></span> |<span data-ttu-id="744a1-157">Resurs-ID för hello-resurs som genereras av den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="744a1-157">Resource ID of hello resource that generated this event.</span></span> |
| <span data-ttu-id="744a1-158">operationName</span><span class="sxs-lookup"><span data-stu-id="744a1-158">operationName</span></span> |<span data-ttu-id="744a1-159">Namnet på hello igen.</span><span class="sxs-lookup"><span data-stu-id="744a1-159">Name of hello operation.</span></span> |
| <span data-ttu-id="744a1-160">nivå</span><span class="sxs-lookup"><span data-stu-id="744a1-160">level</span></span> |<span data-ttu-id="744a1-161">Valfri.</span><span class="sxs-lookup"><span data-stu-id="744a1-161">Optional.</span></span> <span data-ttu-id="744a1-162">Anger hello loggningsnivån för händelsen.</span><span class="sxs-lookup"><span data-stu-id="744a1-162">Indicates hello log event level.</span></span> |
| <span data-ttu-id="744a1-163">properties</span><span class="sxs-lookup"><span data-stu-id="744a1-163">properties</span></span> |<span data-ttu-id="744a1-164">Egenskaper för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="744a1-164">Properties of hello event.</span></span> |

<span data-ttu-id="744a1-165">Du kan visa en lista över alla resursproviders som har stöd för direktuppspelning tooEvent Hubs [här](monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="744a1-165">You can view a list of all resource providers that support streaming tooEvent Hubs [here](monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="stream-data-from-compute-resources"></a><span data-ttu-id="744a1-166">Dataströmmen data från beräkningsresurser</span><span class="sxs-lookup"><span data-stu-id="744a1-166">Stream data from Compute resources</span></span>
<span data-ttu-id="744a1-167">Du kan också strömma diagnostikloggar från beräkningsresurser med hello Windows Azure-diagnostik-agenten.</span><span class="sxs-lookup"><span data-stu-id="744a1-167">You can also stream diagnostic logs from Compute resources using hello Windows Azure Diagnostics agent.</span></span> <span data-ttu-id="744a1-168">[Finns den här artikeln](../event-hubs/event-hubs-streaming-azure-diags-data.md) för hur tooset som upp.</span><span class="sxs-lookup"><span data-stu-id="744a1-168">[See this article](../event-hubs/event-hubs-streaming-azure-diags-data.md) for how tooset that up.</span></span>

## <a name="next-steps"></a><span data-ttu-id="744a1-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="744a1-169">Next steps</span></span>
* [<span data-ttu-id="744a1-170">Läs mer om Azure diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="744a1-170">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="744a1-171">Kom igång med Event Hubs</span><span class="sxs-lookup"><span data-stu-id="744a1-171">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

