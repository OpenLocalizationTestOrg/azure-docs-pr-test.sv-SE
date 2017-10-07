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
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a>Logghändelser diagnostiska utvärdering och övervakning av Batch-lösningar

Precis som med många Azure-tjänster, skickar hello Batch-tjänsten händelser för vissa resurser under hello livstid hello resurs. Du kan aktivera Azure Batch diagnostikloggar toorecord händelser för resurser som pooler och uppgifter och sedan använda hello-loggarna för diagnostiska utvärdering och övervakning. Händelser, t.ex. poolen skapa, ta bort poolen, uppgiften start, uppgift slutförd och andra ingår i Batch diagnostikloggar.

> [!NOTE]
> Den här artikeln beskriver loggning av händelser för Batch-kontot själva resurserna, inte jobbet och uppgift utdata. Mer information om lagra hello utdata för jobb och aktiviteter finns [kvarstår Azure Batch-jobb- och utdata](batch-task-output.md).
> 
> 

## <a name="prerequisites"></a>Krav
* [Azure Batch-kontot](batch-account-create-portal.md)
* [Azure-lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  toopersist Batch diagnostikloggar, måste du skapa ett Azure Storage-konto där Azure kommer att lagra hello loggar. Du anger det här lagringskontot när du [aktivera diagnostikloggning](#enable-diagnostic-logging) för Batch-kontot. hello Storage-konto som du anger när du aktiverar Logginsamling är inte hello samma som en länkad lagring konto enligt tooin hello [programpaket](batch-application-packages.md) och [aktivitet utdata beständiga](batch-task-output.md) artiklar.
  
  > [!WARNING]
  > Du är **debiteras** för hello data som lagras i Azure Storage-konto. Detta inkluderar hello diagnostikloggar som beskrivs i den här artikeln. Ha detta i åtanke när du utformar din [logga bevarandeprincip](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).
  > 
  > 

## <a name="enable-diagnostic-logging"></a>Aktivera diagnostikloggning
Diagnostikloggning är inte aktiverad som standard för Batch-kontot. Du måste uttryckligen aktivera diagnostikloggning för varje Batch-kontot som du vill toomonitor:

[Hur tooenable samling diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

Vi rekommenderar att du läser hello fullständig [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel toogain förstå inte bara hur tooenable loggning, men hello logga kategorier som stöds av hello olika Azure-tjänster. Till exempel Azure Batch för närvarande stöder en logg kategori: **tjänstloggar**.

## <a name="service-logs"></a>Tjänsten loggar
Azure Batch-tjänstloggar innehålla händelser som sänds av hello Azure Batch-tjänsten under en Batch resurs, till exempel en pool eller aktivitet hello livstid. Varje händelse som orsakat av Batch lagras i hello angetts Storage-konto i JSON-format. Detta är till exempel hello brödtexten i ett exempel på en **pool Skapa händelse**:

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

Varje händelsemeddelandet finns i en JSON-fil i hello angetts Azure Storage-konto. Om du vill tooaccess hello loggar direkt kan du tooreview hello [schemat för diagnostikloggar i hello lagringskonto](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Tjänsten logghändelser
hello Batch-tjänsten skickar för närvarande hello följande Service logghändelser. Den här listan får inte vara fullständig, eftersom ytterligare händelser kan ha lagts eftersom den här artikeln senast uppdaterades.

| **Tjänsten logghändelser** |
| --- |
| [Skapa poolen][pool_create] |
| [Poolen delete start][pool_delete_start] |
| [Poolen ta bort klar][pool_delete_complete] |
| [Poolen storleksändring start][pool_resize_start] |
| [Poolen storleksändring slutförd][pool_resize_complete] |
| [Uppgiften start][task_start] |
| [Uppgift slutförd][task_complete] |
| [Aktiviteten misslyckas][task_fail] |

## <a name="next-steps"></a>Nästa steg
I tillägg toostoring diagnostiska logghändelser i ett Azure Storage-konto, kan du också strömma Batch-tjänsten logga händelser tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), och skicka dem för[Azure logganalys](../log-analytics/log-analytics-overview.md).

* [Dataströmmen Azure diagnostikloggar tooEvent hubbar](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  Strömma Batch diagnostiska händelser toohello mycket skalbar tjänst för dataingång, Händelsehubbar. Händelsehubbar kan mata in miljontals händelser per sekund, vilket du kan sedan omvandla och lagra med hjälp av en leverantör av realtidsanalys.
* [Analysera Azure diagnostikloggar med logganalys](../log-analytics/log-analytics-azure-storage.md)
  
  Skicka dina diagnostikloggar tooLog Analytics där du kan analysera dem i hello Operations Management Suite (OMS) portal eller exportera dem för analys i Power BI eller Excel.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
