---
title: "aaaEnable loggning för Batch - händelser i Azure | Microsoft Docs"
description: "Registrera och analysera diagnostiska logghändelser för Azure Batch-kontot resurser som pooler och uppgifter."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: e14e611d-12cd-4671-91dc-bc506dc853e5
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d03303a3e857e9303f40cc6de5c32b5a51d8f8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a><span data-ttu-id="c73e7-103">Logghändelser diagnostiska utvärdering och övervakning av Batch-lösningar</span><span class="sxs-lookup"><span data-stu-id="c73e7-103">Log events for diagnostic evaluation and monitoring of Batch solutions</span></span>

<span data-ttu-id="c73e7-104">Precis som med många Azure-tjänster, skickar hello Batch-tjänsten händelser för vissa resurser under hello livstid hello resurs.</span><span class="sxs-lookup"><span data-stu-id="c73e7-104">As with many Azure services, hello Batch service emits log events for certain resources during hello lifetime of hello resource.</span></span> <span data-ttu-id="c73e7-105">Du kan aktivera Azure Batch diagnostikloggar toorecord händelser för resurser som pooler och uppgifter och sedan använda hello-loggarna för diagnostiska utvärdering och övervakning.</span><span class="sxs-lookup"><span data-stu-id="c73e7-105">You can enable Azure Batch diagnostic logs toorecord events for resources like pools and tasks, and then use hello logs for diagnostic evaluation and monitoring.</span></span> <span data-ttu-id="c73e7-106">Händelser, t.ex. poolen skapa, ta bort poolen, uppgiften start, uppgift slutförd och andra ingår i Batch diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="c73e7-106">Events like pool create, pool delete, task start, task complete, and others are included in Batch diagnostic logs.</span></span>

> [!NOTE]
> <span data-ttu-id="c73e7-107">Den här artikeln beskriver loggning av händelser för Batch-kontot själva resurserna, inte jobbet och uppgift utdata.</span><span class="sxs-lookup"><span data-stu-id="c73e7-107">This article discusses logging events for Batch account resources themselves, not job and task output data.</span></span> <span data-ttu-id="c73e7-108">Mer information om lagra hello utdata för jobb och aktiviteter finns [kvarstår Azure Batch-jobb- och utdata](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="c73e7-108">For details on storing hello output data of your jobs and tasks, see [Persist Azure Batch job and task output](batch-task-output.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="c73e7-109">Krav</span><span class="sxs-lookup"><span data-stu-id="c73e7-109">Prerequisites</span></span>
* [<span data-ttu-id="c73e7-110">Azure Batch-kontot</span><span class="sxs-lookup"><span data-stu-id="c73e7-110">Azure Batch account</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="c73e7-111">Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="c73e7-111">Azure Storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  <span data-ttu-id="c73e7-112">toopersist Batch diagnostikloggar, måste du skapa ett Azure Storage-konto där Azure kommer att lagra hello loggar.</span><span class="sxs-lookup"><span data-stu-id="c73e7-112">toopersist Batch diagnostic logs, you must create an Azure Storage account where Azure will store hello logs.</span></span> <span data-ttu-id="c73e7-113">Du anger det här lagringskontot när du [aktivera diagnostikloggning](#enable-diagnostic-logging) för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="c73e7-113">You specify this Storage account when you [enable diagnostic logging](#enable-diagnostic-logging) for your Batch account.</span></span> <span data-ttu-id="c73e7-114">hello Storage-konto som du anger när du aktiverar Logginsamling är inte hello samma som en länkad lagring konto enligt tooin hello [programpaket](batch-application-packages.md) och [aktivitet utdata beständiga](batch-task-output.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="c73e7-114">hello Storage account you specify when you enable log collection is not hello same as a linked storage account referred tooin hello [application packages](batch-application-packages.md) and [task output persistence](batch-task-output.md) articles.</span></span>
  
  > [!WARNING]
  > <span data-ttu-id="c73e7-115">Du är **debiteras** för hello data som lagras i Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c73e7-115">You are **charged** for hello data stored in your Azure Storage account.</span></span> <span data-ttu-id="c73e7-116">Detta inkluderar hello diagnostikloggar som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c73e7-116">This includes hello diagnostic logs discussed in this article.</span></span> <span data-ttu-id="c73e7-117">Ha detta i åtanke när du utformar din [logga bevarandeprincip](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="c73e7-117">Keep this in mind when designing your [log retention policy](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span></span>
  > 
  > 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="c73e7-118">Aktivera diagnostikloggning</span><span class="sxs-lookup"><span data-stu-id="c73e7-118">Enable diagnostic logging</span></span>
<span data-ttu-id="c73e7-119">Diagnostikloggning är inte aktiverad som standard för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="c73e7-119">Diagnostic logging is not enabled by default for your Batch account.</span></span> <span data-ttu-id="c73e7-120">Du måste uttryckligen aktivera diagnostikloggning för varje Batch-kontot som du vill toomonitor:</span><span class="sxs-lookup"><span data-stu-id="c73e7-120">You must explicitly enable diagnostic logging for each Batch account you want toomonitor:</span></span>

[<span data-ttu-id="c73e7-121">Hur tooenable samling diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="c73e7-121">How tooenable collection of Diagnostic Logs</span></span>](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

<span data-ttu-id="c73e7-122">Vi rekommenderar att du läser hello fullständig [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel toogain förstå inte bara hur tooenable loggning, men hello logga kategorier som stöds av hello olika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c73e7-122">We recommend that you read hello full [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article toogain an understanding of not only how tooenable logging, but hello log categories supported by hello various Azure services.</span></span> <span data-ttu-id="c73e7-123">Till exempel Azure Batch för närvarande stöder en logg kategori: **tjänstloggar**.</span><span class="sxs-lookup"><span data-stu-id="c73e7-123">For example, Azure Batch currently supports one log category: **Service Logs**.</span></span>

## <a name="service-logs"></a><span data-ttu-id="c73e7-124">Tjänsten loggar</span><span class="sxs-lookup"><span data-stu-id="c73e7-124">Service Logs</span></span>
<span data-ttu-id="c73e7-125">Azure Batch-tjänstloggar innehålla händelser som sänds av hello Azure Batch-tjänsten under en Batch resurs, till exempel en pool eller aktivitet hello livstid.</span><span class="sxs-lookup"><span data-stu-id="c73e7-125">Azure Batch Service Logs contain events emitted by hello Azure Batch service during hello lifetime of a Batch resource like a pool or task.</span></span> <span data-ttu-id="c73e7-126">Varje händelse som orsakat av Batch lagras i hello angetts Storage-konto i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="c73e7-126">Each event emitted by Batch is stored in hello specified Storage account in JSON format.</span></span> <span data-ttu-id="c73e7-127">Detta är till exempel hello brödtexten i ett exempel på en **pool Skapa händelse**:</span><span class="sxs-lookup"><span data-stu-id="c73e7-127">For example, this is hello body of a sample **pool create event**:</span></span>

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

<span data-ttu-id="c73e7-128">Varje händelsemeddelandet finns i en JSON-fil i hello angetts Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c73e7-128">Each event body resides in a .json file in hello specified Azure Storage account.</span></span> <span data-ttu-id="c73e7-129">Om du vill tooaccess hello loggar direkt kan du tooreview hello [schemat för diagnostikloggar i hello lagringskonto](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span><span class="sxs-lookup"><span data-stu-id="c73e7-129">If you want tooaccess hello logs directly, you may wish tooreview hello [schema of Diagnostic Logs in hello storage account](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span></span>

## <a name="service-log-events"></a><span data-ttu-id="c73e7-130">Tjänsten logghändelser</span><span class="sxs-lookup"><span data-stu-id="c73e7-130">Service Log events</span></span>
<span data-ttu-id="c73e7-131">hello Batch-tjänsten skickar för närvarande hello följande Service logghändelser.</span><span class="sxs-lookup"><span data-stu-id="c73e7-131">hello Batch service currently emits hello following Service Log events.</span></span> <span data-ttu-id="c73e7-132">Den här listan får inte vara fullständig, eftersom ytterligare händelser kan ha lagts eftersom den här artikeln senast uppdaterades.</span><span class="sxs-lookup"><span data-stu-id="c73e7-132">This list may not be exhaustive, since additional events may have been added since this article was last updated.</span></span>

| <span data-ttu-id="c73e7-133">**Tjänsten logghändelser**</span><span class="sxs-lookup"><span data-stu-id="c73e7-133">**Service Log events**</span></span> |
| --- |
| <span data-ttu-id="c73e7-134">[Skapa poolen][pool_create]</span><span class="sxs-lookup"><span data-stu-id="c73e7-134">[Pool create][pool_create]</span></span> |
| <span data-ttu-id="c73e7-135">[Poolen delete start][pool_delete_start]</span><span class="sxs-lookup"><span data-stu-id="c73e7-135">[Pool delete start][pool_delete_start]</span></span> |
| <span data-ttu-id="c73e7-136">[Poolen ta bort klar][pool_delete_complete]</span><span class="sxs-lookup"><span data-stu-id="c73e7-136">[Pool delete complete][pool_delete_complete]</span></span> |
| <span data-ttu-id="c73e7-137">[Poolen storleksändring start][pool_resize_start]</span><span class="sxs-lookup"><span data-stu-id="c73e7-137">[Pool resize start][pool_resize_start]</span></span> |
| <span data-ttu-id="c73e7-138">[Poolen storleksändring slutförd][pool_resize_complete]</span><span class="sxs-lookup"><span data-stu-id="c73e7-138">[Pool resize complete][pool_resize_complete]</span></span> |
| <span data-ttu-id="c73e7-139">[Uppgiften start][task_start]</span><span class="sxs-lookup"><span data-stu-id="c73e7-139">[Task start][task_start]</span></span> |
| <span data-ttu-id="c73e7-140">[Uppgift slutförd][task_complete]</span><span class="sxs-lookup"><span data-stu-id="c73e7-140">[Task complete][task_complete]</span></span> |
| <span data-ttu-id="c73e7-141">[Aktiviteten misslyckas][task_fail]</span><span class="sxs-lookup"><span data-stu-id="c73e7-141">[Task fail][task_fail]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c73e7-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c73e7-142">Next steps</span></span>
<span data-ttu-id="c73e7-143">I tillägg toostoring diagnostiska logghändelser i ett Azure Storage-konto, kan du också strömma Batch-tjänsten logga händelser tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), och skicka dem för[Azure logganalys](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c73e7-143">In addition toostoring diagnostic log events in an Azure Storage account, you can also stream Batch Service Log events tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), and send them too[Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span>

* [<span data-ttu-id="c73e7-144">Dataströmmen Azure diagnostikloggar tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="c73e7-144">Stream Azure Diagnostic Logs tooEvent Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  <span data-ttu-id="c73e7-145">Strömma Batch diagnostiska händelser toohello mycket skalbar tjänst för dataingång, Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="c73e7-145">Stream Batch diagnostic events toohello highly scalable data ingress service, Event Hubs.</span></span> <span data-ttu-id="c73e7-146">Händelsehubbar kan mata in miljontals händelser per sekund, vilket du kan sedan omvandla och lagra med hjälp av en leverantör av realtidsanalys.</span><span class="sxs-lookup"><span data-stu-id="c73e7-146">Event Hubs can ingest millions of events per second, which you can then transform and store using any real-time analytics provider.</span></span>
* [<span data-ttu-id="c73e7-147">Analysera Azure diagnostikloggar med logganalys</span><span class="sxs-lookup"><span data-stu-id="c73e7-147">Analyze Azure diagnostic logs using Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
  
  <span data-ttu-id="c73e7-148">Skicka dina diagnostikloggar tooLog Analytics där du kan analysera dem i hello Operations Management Suite (OMS) portal eller exportera dem för analys i Power BI eller Excel.</span><span class="sxs-lookup"><span data-stu-id="c73e7-148">Send your diagnostic logs tooLog Analytics where you can analyze them in hello Operations Management Suite (OMS) portal, or export them for analysis in Power BI or Excel.</span></span>

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
