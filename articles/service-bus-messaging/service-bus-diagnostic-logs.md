---
title: aaaAzure Service Bus diagnostikloggar | Microsoft Docs
description: "Lär dig hur tooset in diagnostikloggar för Service Bus i Azure."
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
ms.openlocfilehash: e48d6eaba6e865ae39f5b07ed6cd53d74c92e2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-diagnostic-logs"></a><span data-ttu-id="ceb95-103">Service Bus diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="ceb95-103">Service Bus diagnostic logs</span></span>

<span data-ttu-id="ceb95-104">Du kan visa två typer av loggar för Azure Service Bus:</span><span class="sxs-lookup"><span data-stu-id="ceb95-104">You can view two types of logs for Azure Service Bus:</span></span>
* <span data-ttu-id="ceb95-105">**[Aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="ceb95-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="ceb95-106">Dessa loggar innehåller information om åtgärder som utförs på ett annat jobb.</span><span class="sxs-lookup"><span data-stu-id="ceb95-106">These logs contain information about operations performed on a job.</span></span> <span data-ttu-id="ceb95-107">hello loggar är alltid aktiverat.</span><span class="sxs-lookup"><span data-stu-id="ceb95-107">hello logs are always enabled.</span></span>
* <span data-ttu-id="ceb95-108">**[Diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="ceb95-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="ceb95-109">Du kan konfigurera diagnostiska loggar för bättre information om allt som händer i ett jobb.</span><span class="sxs-lookup"><span data-stu-id="ceb95-109">You can configure diagnostic logs for richer information about everything that happens within a job.</span></span> <span data-ttu-id="ceb95-110">Diagnostikloggar omfattar aktiviteter från hello att hello jobb skapas tills hello jobbet tas bort, inklusive uppdateringar och aktiviteter som sker när hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="ceb95-110">Diagnostic logs cover activities from hello time hello job is created until hello job is deleted, including updates and activities that occur while hello job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="ceb95-111">Aktivera diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="ceb95-111">Turn on diagnostic logs</span></span>

<span data-ttu-id="ceb95-112">Diagnostik loggar är inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="ceb95-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="ceb95-113">tooenable diagnostikloggar, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="ceb95-113">tooenable diagnostic logs, perform hello following steps:</span></span>

1.  <span data-ttu-id="ceb95-114">I hello [Azure-portalen](https://portal.azure.com)under **övervakning + Management**, klickar du på **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="ceb95-114">In hello [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![bladet navigering toodiagnostic loggar](./media/service-bus-diagnostic-logs/image1.png)

2. <span data-ttu-id="ceb95-116">Klicka på hello resursen toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ceb95-116">Click hello resource you want toomonitor.</span></span>  

3.  <span data-ttu-id="ceb95-117">Klicka på **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="ceb95-117">Click **Turn on diagnostics**.</span></span>

    ![Aktivera diagnostikloggar](./media/service-bus-diagnostic-logs/image2.png)

4.  <span data-ttu-id="ceb95-119">För **Status**, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="ceb95-119">For **Status**, click **On**.</span></span>

    ![Ändra status diagnostikloggar](./media/service-bus-diagnostic-logs/image3.png)

5.  <span data-ttu-id="ceb95-121">Ange hello Arkiv mål som du vill; till exempel ett lagringskonto, en Händelsehubb eller Azure logganalys.</span><span class="sxs-lookup"><span data-stu-id="ceb95-121">Set hello archive target that you want; for example, a storage account, an Event Hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="ceb95-122">Spara hello nya diagnostikinställningar.</span><span class="sxs-lookup"><span data-stu-id="ceb95-122">Save hello new diagnostics settings.</span></span>

<span data-ttu-id="ceb95-123">Nya inställningar börjar gälla i cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="ceb95-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="ceb95-124">Efter det loggar visas i hello konfigurerats arkivering mål på hello **diagnostik loggar** bladet.</span><span class="sxs-lookup"><span data-stu-id="ceb95-124">After that, logs appear in hello configured archival target, on hello **Diagnostics logs** blade.</span></span>

<span data-ttu-id="ceb95-125">Mer information om hur du konfigurerar diagnostik finns hello [översikt över Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="ceb95-125">For more information about configuring diagnostics, see hello [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="ceb95-126">Diagnostikloggar schema</span><span class="sxs-lookup"><span data-stu-id="ceb95-126">Diagnostic logs schema</span></span>

<span data-ttu-id="ceb95-127">Alla loggar lagras i JavaScript Object Notation (JSON)-format.</span><span class="sxs-lookup"><span data-stu-id="ceb95-127">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="ceb95-128">Varje post innehåller strängfält som använder hello-format som beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="ceb95-128">Each entry has string fields that use hello format described in hello following section.</span></span>

## <a name="operational-logs-schema"></a><span data-ttu-id="ceb95-129">Operativa loggar schema</span><span class="sxs-lookup"><span data-stu-id="ceb95-129">Operational logs schema</span></span>

<span data-ttu-id="ceb95-130">Loggar in hello **OperationalLogs** kategori avbilda vad som händer under Service Bus-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ceb95-130">Logs in hello **OperationalLogs** category capture what happens during Service Bus operations.</span></span> <span data-ttu-id="ceb95-131">Mer specifikt loggarna avbilda hello åtgärdstyp, inklusive kön skapas, resurser som används, och hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ceb95-131">Specifically, these logs capture hello operation type, including queue creation, resources used, and hello status of hello operation.</span></span>

<span data-ttu-id="ceb95-132">Arbetsloggen JSON strängar innehålla element som anges i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="ceb95-132">Operational log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="ceb95-133">Namn</span><span class="sxs-lookup"><span data-stu-id="ceb95-133">Name</span></span> | <span data-ttu-id="ceb95-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ceb95-134">Description</span></span>
------- | -------
<span data-ttu-id="ceb95-135">ActivityId</span><span class="sxs-lookup"><span data-stu-id="ceb95-135">ActivityId</span></span> | <span data-ttu-id="ceb95-136">Internt ID som används för spårning</span><span class="sxs-lookup"><span data-stu-id="ceb95-136">Internal ID, used for tracking</span></span>
<span data-ttu-id="ceb95-137">EventName</span><span class="sxs-lookup"><span data-stu-id="ceb95-137">EventName</span></span> | <span data-ttu-id="ceb95-138">Åtgärdsnamn</span><span class="sxs-lookup"><span data-stu-id="ceb95-138">Operation name</span></span>           
<span data-ttu-id="ceb95-139">resourceId</span><span class="sxs-lookup"><span data-stu-id="ceb95-139">resourceId</span></span> | <span data-ttu-id="ceb95-140">Azure Resource Manager-resurs-ID</span><span class="sxs-lookup"><span data-stu-id="ceb95-140">Azure Resource Manager resource ID</span></span>
<span data-ttu-id="ceb95-141">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="ceb95-141">SubscriptionId</span></span> | <span data-ttu-id="ceb95-142">Prenumerations-ID:t</span><span class="sxs-lookup"><span data-stu-id="ceb95-142">Subscription ID</span></span>
<span data-ttu-id="ceb95-143">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="ceb95-143">EventTimeString</span></span> | <span data-ttu-id="ceb95-144">Åtgärden tid</span><span class="sxs-lookup"><span data-stu-id="ceb95-144">Operation time</span></span>
<span data-ttu-id="ceb95-145">EventProperties</span><span class="sxs-lookup"><span data-stu-id="ceb95-145">EventProperties</span></span> | <span data-ttu-id="ceb95-146">Åtgärden egenskaper</span><span class="sxs-lookup"><span data-stu-id="ceb95-146">Operation properties</span></span>
<span data-ttu-id="ceb95-147">Status</span><span class="sxs-lookup"><span data-stu-id="ceb95-147">Status</span></span> | <span data-ttu-id="ceb95-148">Åtgärdsstatus</span><span class="sxs-lookup"><span data-stu-id="ceb95-148">Operation status</span></span>
<span data-ttu-id="ceb95-149">Anropare</span><span class="sxs-lookup"><span data-stu-id="ceb95-149">Caller</span></span> | <span data-ttu-id="ceb95-150">Anroparen av åtgärden (Azure portal eller management-klienten)</span><span class="sxs-lookup"><span data-stu-id="ceb95-150">Caller of operation (Azure portal or management client)</span></span>
<span data-ttu-id="ceb95-151">category</span><span class="sxs-lookup"><span data-stu-id="ceb95-151">category</span></span> | <span data-ttu-id="ceb95-152">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="ceb95-152">OperationalLogs</span></span>

<span data-ttu-id="ceb95-153">Här är ett exempel på en arbetsloggen JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="ceb95-153">Here's an example of an operational log JSON string:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ceb95-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ceb95-154">Next steps</span></span>

<span data-ttu-id="ceb95-155">Finns följande länkar toolearn mer om Service Bus hello:</span><span class="sxs-lookup"><span data-stu-id="ceb95-155">Visit hello following links toolearn more about Service Bus:</span></span>

* [<span data-ttu-id="ceb95-156">Introduktion tooService Bus</span><span class="sxs-lookup"><span data-stu-id="ceb95-156">Introduction tooService Bus</span></span>](service-bus-messaging-overview.md)
* [<span data-ttu-id="ceb95-157">Kom igång med Service Bus</span><span class="sxs-lookup"><span data-stu-id="ceb95-157">Get started with Service Bus</span></span>](service-bus-dotnet-get-started-with-queues.md)
