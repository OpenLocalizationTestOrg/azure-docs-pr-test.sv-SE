---
title: "aaaAzure Händelsehubbar diagnostikloggar | Microsoft Docs"
description: "Lär dig hur tooset in diagnostikloggar för händelsehubbar i Azure."
keywords: 
documentationcenter: 
services: event-hubs
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: sethm;babanisa
ms.openlocfilehash: d2054e2e444e715e5077fe2608fe1e009e6c1d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-diagnostic-logs"></a><span data-ttu-id="715f1-103">Event Hubs diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="715f1-103">Event Hubs diagnostic logs</span></span>

<span data-ttu-id="715f1-104">Du kan visa två typer av loggar för Händelsehubbar i Azure:</span><span class="sxs-lookup"><span data-stu-id="715f1-104">You can view two types of logs for Azure Event Hubs:</span></span>
* <span data-ttu-id="715f1-105">**[Aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="715f1-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="715f1-106">Dessa loggar har information om åtgärder som utförs på ett annat jobb.</span><span class="sxs-lookup"><span data-stu-id="715f1-106">These logs have information about operations performed on a job.</span></span> <span data-ttu-id="715f1-107">hello loggar är alltid aktiverat.</span><span class="sxs-lookup"><span data-stu-id="715f1-107">hello logs are always enabled.</span></span>
* <span data-ttu-id="715f1-108">**[Diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="715f1-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="715f1-109">Du kan konfigurera diagnostikloggar för en heltäckande vy av allt som händer med ett jobb.</span><span class="sxs-lookup"><span data-stu-id="715f1-109">You can configure diagnostic logs for a richer view of everything that happens with a job.</span></span> <span data-ttu-id="715f1-110">Diagnostikloggar omfattar aktiviteter från hello att hello jobb skapas tills hello jobbet tas bort, inklusive uppdateringar och aktiviteter som sker när hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="715f1-110">Diagnostic logs cover activities from hello time hello job is created until hello job is deleted, including updates and activities that occur while hello job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="715f1-111">Aktivera diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="715f1-111">Turn on diagnostic logs</span></span>
<span data-ttu-id="715f1-112">Diagnostik loggar är inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="715f1-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="715f1-113">tooenable diagnostikloggar:</span><span class="sxs-lookup"><span data-stu-id="715f1-113">tooenable diagnostic logs:</span></span>

1.  <span data-ttu-id="715f1-114">I hello [Azure-portalen](https://portal.azure.com)under **övervakning + Management**, klickar du på **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="715f1-114">In hello [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![bladet navigering toodiagnostic loggar](./media/event-hubs-diagnostic-logs/image1.png)

2.  <span data-ttu-id="715f1-116">Klicka på hello resursen toomonitor.</span><span class="sxs-lookup"><span data-stu-id="715f1-116">Click hello resource you want toomonitor.</span></span>

3.  <span data-ttu-id="715f1-117">Klicka på **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="715f1-117">Click **Turn on diagnostics**.</span></span>

    ![Aktivera diagnostikloggar](./media/event-hubs-diagnostic-logs/image2.png)

4.  <span data-ttu-id="715f1-119">För **Status**, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="715f1-119">For **Status**, click **On**.</span></span>

    ![Ändra hello status för diagnostikloggar](./media/event-hubs-diagnostic-logs/image3.png)

5.  <span data-ttu-id="715f1-121">Ange hello Arkiv mål som du vill; ett lagringskonto, en händelsehubb eller Azure logganalys.</span><span class="sxs-lookup"><span data-stu-id="715f1-121">Set hello archive target that you want; for example, a storage account, an event hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="715f1-122">Spara hello nya diagnostikinställningar.</span><span class="sxs-lookup"><span data-stu-id="715f1-122">Save hello new diagnostics settings.</span></span>

<span data-ttu-id="715f1-123">Nya inställningar börjar gälla i cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="715f1-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="715f1-124">Efter det loggar visas i hello konfigurerats arkivering mål på hello **diagnostik loggar** bladet.</span><span class="sxs-lookup"><span data-stu-id="715f1-124">After that, logs appear in hello configured archival target, on hello **Diagnostics logs** blade.</span></span>

<span data-ttu-id="715f1-125">Mer information om hur du konfigurerar diagnostik finns hello [översikt över Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="715f1-125">For more information about configuring diagnostics, see hello [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-categories"></a><span data-ttu-id="715f1-126">Diagnostikloggar kategorier</span><span class="sxs-lookup"><span data-stu-id="715f1-126">Diagnostic logs categories</span></span>
<span data-ttu-id="715f1-127">Händelsehubbar samlar in diagnostikloggar två kategorier:</span><span class="sxs-lookup"><span data-stu-id="715f1-127">Event Hubs captures diagnostic logs for two categories:</span></span>

* <span data-ttu-id="715f1-128">**ArchiveLogs**: loggar relaterade tooEvent hubbar Arkiv specifikt loggar relaterade tooarchive fel.</span><span class="sxs-lookup"><span data-stu-id="715f1-128">**ArchiveLogs**: logs related tooEvent Hubs archives, specifically, logs related tooarchive errors.</span></span>
* <span data-ttu-id="715f1-129">**OperationalLogs**: information om vad som händer under Händelsehubbar åtgärder, särskilt hello åtgärdstyp, inklusive event hub skapas, resurser som används, och hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="715f1-129">**OperationalLogs**: information about what is happening during Event Hubs operations, specifically, hello operation type, including event hub creation, resources used, and hello status of hello operation.</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="715f1-130">Diagnostikloggar schema</span><span class="sxs-lookup"><span data-stu-id="715f1-130">Diagnostic logs schema</span></span>
<span data-ttu-id="715f1-131">Alla loggar lagras i JavaScript Object Notation (JSON)-format.</span><span class="sxs-lookup"><span data-stu-id="715f1-131">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="715f1-132">Varje post innehåller strängfält som använder hello-format som beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="715f1-132">Each entry has string fields that use hello format described in hello following sections.</span></span>

### <a name="archive-logs-schema"></a><span data-ttu-id="715f1-133">Arkivera loggar schema</span><span class="sxs-lookup"><span data-stu-id="715f1-133">Archive logs schema</span></span>

<span data-ttu-id="715f1-134">Arkivera loggen JSON strängar innehålla element som anges i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="715f1-134">Archive log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="715f1-135">Namn</span><span class="sxs-lookup"><span data-stu-id="715f1-135">Name</span></span> | <span data-ttu-id="715f1-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="715f1-136">Description</span></span>
------- | -------
<span data-ttu-id="715f1-137">Aktivitetsnamn</span><span class="sxs-lookup"><span data-stu-id="715f1-137">TaskName</span></span> | <span data-ttu-id="715f1-138">Beskrivning av hello-aktivitet som misslyckades.</span><span class="sxs-lookup"><span data-stu-id="715f1-138">Description of hello task that failed.</span></span>
<span data-ttu-id="715f1-139">ActivityId</span><span class="sxs-lookup"><span data-stu-id="715f1-139">ActivityId</span></span> | <span data-ttu-id="715f1-140">Internt ID som används för spårning.</span><span class="sxs-lookup"><span data-stu-id="715f1-140">Internal ID, used for tracking.</span></span>
<span data-ttu-id="715f1-141">trackingId</span><span class="sxs-lookup"><span data-stu-id="715f1-141">trackingId</span></span> | <span data-ttu-id="715f1-142">Internt ID som används för spårning.</span><span class="sxs-lookup"><span data-stu-id="715f1-142">Internal ID, used for tracking.</span></span>
<span data-ttu-id="715f1-143">resourceId</span><span class="sxs-lookup"><span data-stu-id="715f1-143">resourceId</span></span> | <span data-ttu-id="715f1-144">Azure Resource Manager-resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="715f1-144">Azure Resource Manager resource ID.</span></span>
<span data-ttu-id="715f1-145">EventHub</span><span class="sxs-lookup"><span data-stu-id="715f1-145">eventHub</span></span> | <span data-ttu-id="715f1-146">Händelsehubb fullständigt namn (inklusive namnområdesnamnet).</span><span class="sxs-lookup"><span data-stu-id="715f1-146">Event hub full name (includes namespace name).</span></span>
<span data-ttu-id="715f1-147">partitions-ID</span><span class="sxs-lookup"><span data-stu-id="715f1-147">partitionId</span></span> | <span data-ttu-id="715f1-148">Event Hub partition skrivs till.</span><span class="sxs-lookup"><span data-stu-id="715f1-148">Event Hub partition being written to.</span></span>
<span data-ttu-id="715f1-149">archiveStep</span><span class="sxs-lookup"><span data-stu-id="715f1-149">archiveStep</span></span> | <span data-ttu-id="715f1-150">ArchiveFlushWriter</span><span class="sxs-lookup"><span data-stu-id="715f1-150">ArchiveFlushWriter</span></span>
<span data-ttu-id="715f1-151">startTime</span><span class="sxs-lookup"><span data-stu-id="715f1-151">startTime</span></span> | <span data-ttu-id="715f1-152">Fel starttid.</span><span class="sxs-lookup"><span data-stu-id="715f1-152">Failure start time.</span></span>
<span data-ttu-id="715f1-153">fel</span><span class="sxs-lookup"><span data-stu-id="715f1-153">failures</span></span> | <span data-ttu-id="715f1-154">Antal gånger som fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="715f1-154">Number of times failure occurred.</span></span>
<span data-ttu-id="715f1-155">durationInSeconds</span><span class="sxs-lookup"><span data-stu-id="715f1-155">durationInSeconds</span></span> | <span data-ttu-id="715f1-156">Varaktighet för felet.</span><span class="sxs-lookup"><span data-stu-id="715f1-156">Duration of failure.</span></span>
<span data-ttu-id="715f1-157">Meddelande</span><span class="sxs-lookup"><span data-stu-id="715f1-157">message</span></span> | <span data-ttu-id="715f1-158">Felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="715f1-158">Error message.</span></span>
<span data-ttu-id="715f1-159">category</span><span class="sxs-lookup"><span data-stu-id="715f1-159">category</span></span> | <span data-ttu-id="715f1-160">ArchiveLogs</span><span class="sxs-lookup"><span data-stu-id="715f1-160">ArchiveLogs</span></span>

<span data-ttu-id="715f1-161">hello är följande kod ett exempel på en arkivera loggen JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="715f1-161">hello following code is an example of an archive log JSON string:</span></span>

```json
{
     "TaskName": "EventHubArchiveUserError",
     "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
     "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
     "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
     "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
     "partitionId": "1",
     "archiveStep": "ArchiveFlushWriter",
     "startTime": "9/22/2016 5:11:21 AM",
     "failures": 3,
     "durationInSeconds": 360,
     "message": "Microsoft.WindowsAzure.Storage.StorageException: hello remote server returned an error: (404) Not Found. ---> System.Net.WebException: hello remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
     "category": "ArchiveLogs"
}
```

### <a name="operational-logs-schema"></a><span data-ttu-id="715f1-162">Operativa loggar schema</span><span class="sxs-lookup"><span data-stu-id="715f1-162">Operational logs schema</span></span>

<span data-ttu-id="715f1-163">Arbetsloggen JSON strängar innehålla element som anges i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="715f1-163">Operational log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="715f1-164">Namn</span><span class="sxs-lookup"><span data-stu-id="715f1-164">Name</span></span> | <span data-ttu-id="715f1-165">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="715f1-165">Description</span></span>
------- | -------
<span data-ttu-id="715f1-166">ActivityId</span><span class="sxs-lookup"><span data-stu-id="715f1-166">ActivityId</span></span> | <span data-ttu-id="715f1-167">Internt ID används tootrack syfte.</span><span class="sxs-lookup"><span data-stu-id="715f1-167">Internal ID, used tootrack purpose.</span></span>
<span data-ttu-id="715f1-168">EventName</span><span class="sxs-lookup"><span data-stu-id="715f1-168">EventName</span></span> | <span data-ttu-id="715f1-169">Åtgärdsnamnet.</span><span class="sxs-lookup"><span data-stu-id="715f1-169">Operation name.</span></span>  
<span data-ttu-id="715f1-170">resourceId</span><span class="sxs-lookup"><span data-stu-id="715f1-170">resourceId</span></span> | <span data-ttu-id="715f1-171">Azure Resource Manager-resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="715f1-171">Azure Resource Manager resource ID.</span></span>
<span data-ttu-id="715f1-172">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="715f1-172">SubscriptionId</span></span> | <span data-ttu-id="715f1-173">Prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="715f1-173">Subscription ID.</span></span>
<span data-ttu-id="715f1-174">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="715f1-174">EventTimeString</span></span> | <span data-ttu-id="715f1-175">Tid för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="715f1-175">Operation time.</span></span>
<span data-ttu-id="715f1-176">EventProperties</span><span class="sxs-lookup"><span data-stu-id="715f1-176">EventProperties</span></span> | <span data-ttu-id="715f1-177">Åtgärden egenskaper.</span><span class="sxs-lookup"><span data-stu-id="715f1-177">Operation properties.</span></span>
<span data-ttu-id="715f1-178">Status</span><span class="sxs-lookup"><span data-stu-id="715f1-178">Status</span></span> | <span data-ttu-id="715f1-179">Åtgärdsstatus för.</span><span class="sxs-lookup"><span data-stu-id="715f1-179">Operation status.</span></span>
<span data-ttu-id="715f1-180">Anropare</span><span class="sxs-lookup"><span data-stu-id="715f1-180">Caller</span></span> | <span data-ttu-id="715f1-181">Anroparen av åtgärden (Azure portal eller management-klienten).</span><span class="sxs-lookup"><span data-stu-id="715f1-181">Caller of operation (Azure portal or management client).</span></span>
<span data-ttu-id="715f1-182">category</span><span class="sxs-lookup"><span data-stu-id="715f1-182">category</span></span> | <span data-ttu-id="715f1-183">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="715f1-183">OperationalLogs</span></span>

<span data-ttu-id="715f1-184">hello är följande kod ett exempel på en arbetsloggen JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="715f1-184">hello following code is an example of an operational log JSON string:</span></span>

```json
Example:
{
     "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
     "EventName": "Create EventHub",
     "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
     "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
     "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
     "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
     "Status": "Succeeded",
     "Caller": "ServiceBus Client",
     "category": "OperationalLogs"
}
```

## <a name="next-steps"></a><span data-ttu-id="715f1-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="715f1-185">Next steps</span></span>
* [<span data-ttu-id="715f1-186">Introduktion tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="715f1-186">Introduction tooEvent Hubs</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="715f1-187">Event Hubs API-översikt</span><span class="sxs-lookup"><span data-stu-id="715f1-187">Event Hubs API overview</span></span>](event-hubs-api-overview.md)
* [<span data-ttu-id="715f1-188">Kom igång med Event Hubs</span><span class="sxs-lookup"><span data-stu-id="715f1-188">Get started with Event Hubs</span></span>](event-hubs-csharp-ephcs-getstarted.md)
