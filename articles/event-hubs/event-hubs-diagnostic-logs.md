---
title: Azure Event Hubs diagnostikloggar | Microsoft Docs
description: "Lär dig hur du ställer in diagnostikloggar för händelsehubbar i Azure."
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
ms.openlocfilehash: 09bc62f4918635419d74ef3ae400a41d4ce58b5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="event-hubs-diagnostic-logs"></a><span data-ttu-id="dbae4-103">Event Hubs diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="dbae4-103">Event Hubs diagnostic logs</span></span>

<span data-ttu-id="dbae4-104">Du kan visa två typer av loggar för Händelsehubbar i Azure:</span><span class="sxs-lookup"><span data-stu-id="dbae4-104">You can view two types of logs for Azure Event Hubs:</span></span>
* <span data-ttu-id="dbae4-105">**[Aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="dbae4-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="dbae4-106">Dessa loggar har information om åtgärder som utförs på ett annat jobb.</span><span class="sxs-lookup"><span data-stu-id="dbae4-106">These logs have information about operations performed on a job.</span></span> <span data-ttu-id="dbae4-107">Loggarna är alltid aktiverat.</span><span class="sxs-lookup"><span data-stu-id="dbae4-107">The logs are always enabled.</span></span>
* <span data-ttu-id="dbae4-108">**[Diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="dbae4-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="dbae4-109">Du kan konfigurera diagnostikloggar för en heltäckande vy av allt som händer med ett jobb.</span><span class="sxs-lookup"><span data-stu-id="dbae4-109">You can configure diagnostic logs for a richer view of everything that happens with a job.</span></span> <span data-ttu-id="dbae4-110">Diagnostikloggar omfattar aktiviteter från den tidpunkt då jobbet skapades tills jobbet tas bort, inklusive uppdateringar och aktiviteter som inträffar när jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="dbae4-110">Diagnostic logs cover activities from the time the job is created until the job is deleted, including updates and activities that occur while the job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="dbae4-111">Aktivera diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="dbae4-111">Turn on diagnostic logs</span></span>
<span data-ttu-id="dbae4-112">Diagnostik loggar är inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="dbae4-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="dbae4-113">Aktivera diagnostikloggar:</span><span class="sxs-lookup"><span data-stu-id="dbae4-113">To enable diagnostic logs:</span></span>

1.  <span data-ttu-id="dbae4-114">I den [Azure-portalen](https://portal.azure.com)under **övervakning + Management**, klickar du på **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="dbae4-114">In the [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![Bladet navigering till diagnostikloggar](./media/event-hubs-diagnostic-logs/image1.png)

2.  <span data-ttu-id="dbae4-116">Klicka på resursen som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="dbae4-116">Click the resource you want to monitor.</span></span>

3.  <span data-ttu-id="dbae4-117">Klicka på **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="dbae4-117">Click **Turn on diagnostics**.</span></span>

    ![Aktivera diagnostikloggar](./media/event-hubs-diagnostic-logs/image2.png)

4.  <span data-ttu-id="dbae4-119">För **Status**, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="dbae4-119">For **Status**, click **On**.</span></span>

    ![Ändra status för diagnostikloggar](./media/event-hubs-diagnostic-logs/image3.png)

5.  <span data-ttu-id="dbae4-121">Ange Arkiv-mål som du vill. ett lagringskonto, en händelsehubb eller Azure logganalys.</span><span class="sxs-lookup"><span data-stu-id="dbae4-121">Set the archive target that you want; for example, a storage account, an event hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="dbae4-122">Spara de nya diagnostikinställningarna för.</span><span class="sxs-lookup"><span data-stu-id="dbae4-122">Save the new diagnostics settings.</span></span>

<span data-ttu-id="dbae4-123">Nya inställningar börjar gälla i cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="dbae4-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="dbae4-124">Efter det loggarna visas i det konfigurerade arkivering målet på den **diagnostik loggar** bladet.</span><span class="sxs-lookup"><span data-stu-id="dbae4-124">After that, logs appear in the configured archival target, on the **Diagnostics logs** blade.</span></span>

<span data-ttu-id="dbae4-125">Mer information om hur du konfigurerar diagnostik finns i [översikt över Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="dbae4-125">For more information about configuring diagnostics, see the [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-categories"></a><span data-ttu-id="dbae4-126">Diagnostikloggar kategorier</span><span class="sxs-lookup"><span data-stu-id="dbae4-126">Diagnostic logs categories</span></span>
<span data-ttu-id="dbae4-127">Händelsehubbar samlar in diagnostikloggar två kategorier:</span><span class="sxs-lookup"><span data-stu-id="dbae4-127">Event Hubs captures diagnostic logs for two categories:</span></span>

* <span data-ttu-id="dbae4-128">**ArchiveLogs**: relaterade till Händelsehubbar Arkiv specifikt, relaterade till Arkivera fel.</span><span class="sxs-lookup"><span data-stu-id="dbae4-128">**ArchiveLogs**: logs related to Event Hubs archives, specifically, logs related to archive errors.</span></span>
* <span data-ttu-id="dbae4-129">**OperationalLogs**: information om vad som händer under Händelsehubbar åtgärder, särskilt åtgärden skriver, inklusive event hub skapas, resurser som används och status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="dbae4-129">**OperationalLogs**: information about what is happening during Event Hubs operations, specifically, the operation type, including event hub creation, resources used, and the status of the operation.</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="dbae4-130">Diagnostikloggar schema</span><span class="sxs-lookup"><span data-stu-id="dbae4-130">Diagnostic logs schema</span></span>
<span data-ttu-id="dbae4-131">Alla loggar lagras i JavaScript Object Notation (JSON)-format.</span><span class="sxs-lookup"><span data-stu-id="dbae4-131">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="dbae4-132">Varje post innehåller strängfält som använder det format som beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dbae4-132">Each entry has string fields that use the format described in the following sections.</span></span>

### <a name="archive-logs-schema"></a><span data-ttu-id="dbae4-133">Arkivera loggar schema</span><span class="sxs-lookup"><span data-stu-id="dbae4-133">Archive logs schema</span></span>

<span data-ttu-id="dbae4-134">Archive log JSON strängar innehålla element som visas i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="dbae4-134">Archive log JSON strings include elements listed in the following table:</span></span>

<span data-ttu-id="dbae4-135">Namn</span><span class="sxs-lookup"><span data-stu-id="dbae4-135">Name</span></span> | <span data-ttu-id="dbae4-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dbae4-136">Description</span></span>
------- | -------
<span data-ttu-id="dbae4-137">Aktivitetsnamn</span><span class="sxs-lookup"><span data-stu-id="dbae4-137">TaskName</span></span> | <span data-ttu-id="dbae4-138">Beskrivning av uppgiften som misslyckades.</span><span class="sxs-lookup"><span data-stu-id="dbae4-138">Description of the task that failed.</span></span>
<span data-ttu-id="dbae4-139">ActivityId</span><span class="sxs-lookup"><span data-stu-id="dbae4-139">ActivityId</span></span> | <span data-ttu-id="dbae4-140">Internt ID som används för spårning.</span><span class="sxs-lookup"><span data-stu-id="dbae4-140">Internal ID, used for tracking.</span></span>
<span data-ttu-id="dbae4-141">trackingId</span><span class="sxs-lookup"><span data-stu-id="dbae4-141">trackingId</span></span> | <span data-ttu-id="dbae4-142">Internt ID som används för spårning.</span><span class="sxs-lookup"><span data-stu-id="dbae4-142">Internal ID, used for tracking.</span></span>
<span data-ttu-id="dbae4-143">resourceId</span><span class="sxs-lookup"><span data-stu-id="dbae4-143">resourceId</span></span> | <span data-ttu-id="dbae4-144">Azure Resource Manager-resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="dbae4-144">Azure Resource Manager resource ID.</span></span>
<span data-ttu-id="dbae4-145">EventHub</span><span class="sxs-lookup"><span data-stu-id="dbae4-145">eventHub</span></span> | <span data-ttu-id="dbae4-146">Händelsehubb fullständigt namn (inklusive namnområdesnamnet).</span><span class="sxs-lookup"><span data-stu-id="dbae4-146">Event hub full name (includes namespace name).</span></span>
<span data-ttu-id="dbae4-147">partitions-ID</span><span class="sxs-lookup"><span data-stu-id="dbae4-147">partitionId</span></span> | <span data-ttu-id="dbae4-148">Event Hub partition skrivs till.</span><span class="sxs-lookup"><span data-stu-id="dbae4-148">Event Hub partition being written to.</span></span>
<span data-ttu-id="dbae4-149">archiveStep</span><span class="sxs-lookup"><span data-stu-id="dbae4-149">archiveStep</span></span> | <span data-ttu-id="dbae4-150">ArchiveFlushWriter</span><span class="sxs-lookup"><span data-stu-id="dbae4-150">ArchiveFlushWriter</span></span>
<span data-ttu-id="dbae4-151">startTime</span><span class="sxs-lookup"><span data-stu-id="dbae4-151">startTime</span></span> | <span data-ttu-id="dbae4-152">Fel starttid.</span><span class="sxs-lookup"><span data-stu-id="dbae4-152">Failure start time.</span></span>
<span data-ttu-id="dbae4-153">fel</span><span class="sxs-lookup"><span data-stu-id="dbae4-153">failures</span></span> | <span data-ttu-id="dbae4-154">Antal gånger som fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="dbae4-154">Number of times failure occurred.</span></span>
<span data-ttu-id="dbae4-155">durationInSeconds</span><span class="sxs-lookup"><span data-stu-id="dbae4-155">durationInSeconds</span></span> | <span data-ttu-id="dbae4-156">Varaktighet för felet.</span><span class="sxs-lookup"><span data-stu-id="dbae4-156">Duration of failure.</span></span>
<span data-ttu-id="dbae4-157">Meddelande</span><span class="sxs-lookup"><span data-stu-id="dbae4-157">message</span></span> | <span data-ttu-id="dbae4-158">Felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="dbae4-158">Error message.</span></span>
<span data-ttu-id="dbae4-159">category</span><span class="sxs-lookup"><span data-stu-id="dbae4-159">category</span></span> | <span data-ttu-id="dbae4-160">ArchiveLogs</span><span class="sxs-lookup"><span data-stu-id="dbae4-160">ArchiveLogs</span></span>

<span data-ttu-id="dbae4-161">Följande kod är ett exempel på en arkivera loggen JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="dbae4-161">The following code is an example of an archive log JSON string:</span></span>

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
     "message": "Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (404) Not Found. ---> System.Net.WebException: The remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
     "category": "ArchiveLogs"
}
```

### <a name="operational-logs-schema"></a><span data-ttu-id="dbae4-162">Operativa loggar schema</span><span class="sxs-lookup"><span data-stu-id="dbae4-162">Operational logs schema</span></span>

<span data-ttu-id="dbae4-163">Arbetsloggen JSON strängar innehålla element som visas i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="dbae4-163">Operational log JSON strings include elements listed in the following table:</span></span>

<span data-ttu-id="dbae4-164">Namn</span><span class="sxs-lookup"><span data-stu-id="dbae4-164">Name</span></span> | <span data-ttu-id="dbae4-165">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dbae4-165">Description</span></span>
------- | -------
<span data-ttu-id="dbae4-166">ActivityId</span><span class="sxs-lookup"><span data-stu-id="dbae4-166">ActivityId</span></span> | <span data-ttu-id="dbae4-167">Internt ID som används för att spåra syfte.</span><span class="sxs-lookup"><span data-stu-id="dbae4-167">Internal ID, used to track purpose.</span></span>
<span data-ttu-id="dbae4-168">EventName</span><span class="sxs-lookup"><span data-stu-id="dbae4-168">EventName</span></span> | <span data-ttu-id="dbae4-169">Åtgärdsnamnet.</span><span class="sxs-lookup"><span data-stu-id="dbae4-169">Operation name.</span></span>  
<span data-ttu-id="dbae4-170">resourceId</span><span class="sxs-lookup"><span data-stu-id="dbae4-170">resourceId</span></span> | <span data-ttu-id="dbae4-171">Azure Resource Manager-resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="dbae4-171">Azure Resource Manager resource ID.</span></span>
<span data-ttu-id="dbae4-172">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="dbae4-172">SubscriptionId</span></span> | <span data-ttu-id="dbae4-173">Prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="dbae4-173">Subscription ID.</span></span>
<span data-ttu-id="dbae4-174">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="dbae4-174">EventTimeString</span></span> | <span data-ttu-id="dbae4-175">Tid för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="dbae4-175">Operation time.</span></span>
<span data-ttu-id="dbae4-176">EventProperties</span><span class="sxs-lookup"><span data-stu-id="dbae4-176">EventProperties</span></span> | <span data-ttu-id="dbae4-177">Åtgärden egenskaper.</span><span class="sxs-lookup"><span data-stu-id="dbae4-177">Operation properties.</span></span>
<span data-ttu-id="dbae4-178">Status</span><span class="sxs-lookup"><span data-stu-id="dbae4-178">Status</span></span> | <span data-ttu-id="dbae4-179">Åtgärdsstatus för.</span><span class="sxs-lookup"><span data-stu-id="dbae4-179">Operation status.</span></span>
<span data-ttu-id="dbae4-180">Anropare</span><span class="sxs-lookup"><span data-stu-id="dbae4-180">Caller</span></span> | <span data-ttu-id="dbae4-181">Anroparen av åtgärden (Azure portal eller management-klienten).</span><span class="sxs-lookup"><span data-stu-id="dbae4-181">Caller of operation (Azure portal or management client).</span></span>
<span data-ttu-id="dbae4-182">category</span><span class="sxs-lookup"><span data-stu-id="dbae4-182">category</span></span> | <span data-ttu-id="dbae4-183">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="dbae4-183">OperationalLogs</span></span>

<span data-ttu-id="dbae4-184">Följande kod är ett exempel på en arbetsloggen JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="dbae4-184">The following code is an example of an operational log JSON string:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="dbae4-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dbae4-185">Next steps</span></span>
* [<span data-ttu-id="dbae4-186">Introduktion till Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="dbae4-186">Introduction to Event Hubs</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="dbae4-187">Event Hubs API-översikt</span><span class="sxs-lookup"><span data-stu-id="dbae4-187">Event Hubs API overview</span></span>](event-hubs-api-overview.md)
* [<span data-ttu-id="dbae4-188">Kom igång med Event Hubs</span><span class="sxs-lookup"><span data-stu-id="dbae4-188">Get started with Event Hubs</span></span>](event-hubs-csharp-ephcs-getstarted.md)
