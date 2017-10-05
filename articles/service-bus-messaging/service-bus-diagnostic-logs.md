---
title: Azure Service Bus-diagnostikloggar | Microsoft Docs
description: "Lär dig hur du ställer in diagnostikloggar för Service Bus i Azure."
keywords: 
documentationcenter: .net
services: service-bus-messaging
author: banisadr
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: babanisa;sethm
ms.openlocfilehash: 72e18444c83b84c5191a0aab3dc6983517167dd1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="service-bus-diagnostic-logs"></a><span data-ttu-id="cae9b-103">Service Bus diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="cae9b-103">Service Bus diagnostic logs</span></span>

<span data-ttu-id="cae9b-104">Du kan visa två typer av loggar för Azure Service Bus:</span><span class="sxs-lookup"><span data-stu-id="cae9b-104">You can view two types of logs for Azure Service Bus:</span></span>
* <span data-ttu-id="cae9b-105">**[Aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="cae9b-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="cae9b-106">Dessa loggar innehåller information om åtgärder som utförs på ett annat jobb.</span><span class="sxs-lookup"><span data-stu-id="cae9b-106">These logs contain information about operations performed on a job.</span></span> <span data-ttu-id="cae9b-107">Loggarna är alltid aktiverat.</span><span class="sxs-lookup"><span data-stu-id="cae9b-107">The logs are always enabled.</span></span>
* <span data-ttu-id="cae9b-108">**[Diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="cae9b-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="cae9b-109">Du kan konfigurera diagnostiska loggar för bättre information om allt som händer i ett jobb.</span><span class="sxs-lookup"><span data-stu-id="cae9b-109">You can configure diagnostic logs for richer information about everything that happens within a job.</span></span> <span data-ttu-id="cae9b-110">Diagnostikloggar omfattar aktiviteter från den tidpunkt då jobbet skapades tills jobbet tas bort, inklusive uppdateringar och aktiviteter som inträffar när jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="cae9b-110">Diagnostic logs cover activities from the time the job is created until the job is deleted, including updates and activities that occur while the job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="cae9b-111">Aktivera diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="cae9b-111">Turn on diagnostic logs</span></span>

<span data-ttu-id="cae9b-112">Diagnostik loggar är inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="cae9b-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="cae9b-113">Utför följande steg om du vill aktivera diagnostikloggar:</span><span class="sxs-lookup"><span data-stu-id="cae9b-113">To enable diagnostic logs, perform the following steps:</span></span>

1.  <span data-ttu-id="cae9b-114">I den [Azure-portalen](https://portal.azure.com)under **övervakning + Management**, klickar du på **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="cae9b-114">In the [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![Bladet navigering till diagnostikloggar](./media/service-bus-diagnostic-logs/image1.png)

2. <span data-ttu-id="cae9b-116">Klicka på resursen som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="cae9b-116">Click the resource you want to monitor.</span></span>  

3.  <span data-ttu-id="cae9b-117">Klicka på **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="cae9b-117">Click **Turn on diagnostics**.</span></span>

    ![Aktivera diagnostikloggar](./media/service-bus-diagnostic-logs/image2.png)

4.  <span data-ttu-id="cae9b-119">För **Status**, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="cae9b-119">For **Status**, click **On**.</span></span>

    ![Ändra status diagnostikloggar](./media/service-bus-diagnostic-logs/image3.png)

5.  <span data-ttu-id="cae9b-121">Ange Arkiv-mål som du vill. till exempel ett lagringskonto, en Händelsehubb eller Azure logganalys.</span><span class="sxs-lookup"><span data-stu-id="cae9b-121">Set the archive target that you want; for example, a storage account, an Event Hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="cae9b-122">Spara de nya diagnostikinställningarna för.</span><span class="sxs-lookup"><span data-stu-id="cae9b-122">Save the new diagnostics settings.</span></span>

<span data-ttu-id="cae9b-123">Nya inställningar börjar gälla i cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="cae9b-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="cae9b-124">Efter det loggarna visas i det konfigurerade arkivering målet på den **diagnostik loggar** bladet.</span><span class="sxs-lookup"><span data-stu-id="cae9b-124">After that, logs appear in the configured archival target, on the **Diagnostics logs** blade.</span></span>

<span data-ttu-id="cae9b-125">Mer information om hur du konfigurerar diagnostik finns i [översikt över Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="cae9b-125">For more information about configuring diagnostics, see the [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="cae9b-126">Diagnostikloggar schema</span><span class="sxs-lookup"><span data-stu-id="cae9b-126">Diagnostic logs schema</span></span>

<span data-ttu-id="cae9b-127">Alla loggar lagras i JavaScript Object Notation (JSON)-format.</span><span class="sxs-lookup"><span data-stu-id="cae9b-127">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="cae9b-128">Varje post innehåller strängfält som använder det format som beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cae9b-128">Each entry has string fields that use the format described in the following section.</span></span>

## <a name="operational-logs-schema"></a><span data-ttu-id="cae9b-129">Operativa loggar schema</span><span class="sxs-lookup"><span data-stu-id="cae9b-129">Operational logs schema</span></span>

<span data-ttu-id="cae9b-130">Loggar in på **OperationalLogs** kategori avbilda vad som händer under Service Bus-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cae9b-130">Logs in the **OperationalLogs** category capture what happens during Service Bus operations.</span></span> <span data-ttu-id="cae9b-131">Dessa loggar avbilda specifikt typ av åtgärd, inklusive kön skapas, resurser och status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="cae9b-131">Specifically, these logs capture the operation type, including queue creation, resources used, and the status of the operation.</span></span>

<span data-ttu-id="cae9b-132">Arbetsloggen JSON strängar innehålla element som visas i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="cae9b-132">Operational log JSON strings include elements listed in the following table:</span></span>

<span data-ttu-id="cae9b-133">Namn</span><span class="sxs-lookup"><span data-stu-id="cae9b-133">Name</span></span> | <span data-ttu-id="cae9b-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cae9b-134">Description</span></span>
------- | -------
<span data-ttu-id="cae9b-135">ActivityId</span><span class="sxs-lookup"><span data-stu-id="cae9b-135">ActivityId</span></span> | <span data-ttu-id="cae9b-136">Internt ID som används för spårning</span><span class="sxs-lookup"><span data-stu-id="cae9b-136">Internal ID, used for tracking</span></span>
<span data-ttu-id="cae9b-137">EventName</span><span class="sxs-lookup"><span data-stu-id="cae9b-137">EventName</span></span> | <span data-ttu-id="cae9b-138">Åtgärdsnamn</span><span class="sxs-lookup"><span data-stu-id="cae9b-138">Operation name</span></span>           
<span data-ttu-id="cae9b-139">resourceId</span><span class="sxs-lookup"><span data-stu-id="cae9b-139">resourceId</span></span> | <span data-ttu-id="cae9b-140">Azure Resource Manager-resurs-ID</span><span class="sxs-lookup"><span data-stu-id="cae9b-140">Azure Resource Manager resource ID</span></span>
<span data-ttu-id="cae9b-141">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="cae9b-141">SubscriptionId</span></span> | <span data-ttu-id="cae9b-142">Prenumerations-ID:t</span><span class="sxs-lookup"><span data-stu-id="cae9b-142">Subscription ID</span></span>
<span data-ttu-id="cae9b-143">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="cae9b-143">EventTimeString</span></span> | <span data-ttu-id="cae9b-144">Åtgärden tid</span><span class="sxs-lookup"><span data-stu-id="cae9b-144">Operation time</span></span>
<span data-ttu-id="cae9b-145">EventProperties</span><span class="sxs-lookup"><span data-stu-id="cae9b-145">EventProperties</span></span> | <span data-ttu-id="cae9b-146">Åtgärden egenskaper</span><span class="sxs-lookup"><span data-stu-id="cae9b-146">Operation properties</span></span>
<span data-ttu-id="cae9b-147">Status</span><span class="sxs-lookup"><span data-stu-id="cae9b-147">Status</span></span> | <span data-ttu-id="cae9b-148">Åtgärdsstatus</span><span class="sxs-lookup"><span data-stu-id="cae9b-148">Operation status</span></span>
<span data-ttu-id="cae9b-149">Anropare</span><span class="sxs-lookup"><span data-stu-id="cae9b-149">Caller</span></span> | <span data-ttu-id="cae9b-150">Anroparen av åtgärden (Azure portal eller management-klienten)</span><span class="sxs-lookup"><span data-stu-id="cae9b-150">Caller of operation (Azure portal or management client)</span></span>
<span data-ttu-id="cae9b-151">category</span><span class="sxs-lookup"><span data-stu-id="cae9b-151">category</span></span> | <span data-ttu-id="cae9b-152">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="cae9b-152">OperationalLogs</span></span>

<span data-ttu-id="cae9b-153">Här är ett exempel på en arbetsloggen JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="cae9b-153">Here's an example of an operational log JSON string:</span></span>

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a><span data-ttu-id="cae9b-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cae9b-154">Next steps</span></span>

<span data-ttu-id="cae9b-155">Finns följande länkar för att lära dig mer om Service Bus:</span><span class="sxs-lookup"><span data-stu-id="cae9b-155">Visit the following links to learn more about Service Bus:</span></span>

* [<span data-ttu-id="cae9b-156">Introduktion till Service Bus</span><span class="sxs-lookup"><span data-stu-id="cae9b-156">Introduction to Service Bus</span></span>](service-bus-messaging-overview.md)
* [<span data-ttu-id="cae9b-157">Kom igång med Service Bus</span><span class="sxs-lookup"><span data-stu-id="cae9b-157">Get started with Service Bus</span></span>](service-bus-dotnet-get-started-with-queues.md)
