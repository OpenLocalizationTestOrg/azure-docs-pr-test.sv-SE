---
title: "aaaAzure övervakaren Autoskala vanliga mått | Microsoft Docs"
description: "Information om vilka mått används ofta för autoskalning dina molntjänster, virtuella datorer och Web Apps."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 189b2a13-01c8-4aca-afd5-90711903ca59
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/6/2016
ms.author: ancav
ms.openlocfilehash: 372a40d72d7a6c22c5ff854b1460ec8a3b7ed1d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure övervakaren autoskalning vanliga mått
Azure övervakaren autoskalning kan du tooscale hello antal instanser uppåt eller nedåt enligt telemetridata (mått). Det här dokumentet beskriver vanliga mått som du kanske vill toouse. Du kan välja hello mått för hello resurs tooscale av i hello Azure-portalen för molntjänster och servergrupper. Du kan också välja alla mått från en annan resurs tooscale av.

Azure övervakaren Autoskala gäller endast för[Skalningsuppsättningar i virtuella](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [molntjänster](https://azure.microsoft.com/services/cloud-services/), och [App Service - Webbappar](https://azure.microsoft.com/services/app-service/web/). Andra Azure-tjänster använda olika metoder för skalning.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Beräkna mått för Resource Manager-baserade virtuella datorer
Som standard genererar grundläggande (värdnivå) mått av Resource Manager-baserade virtuella datorer och virtuella datorer. Dessutom, när du konfigurerar diagnostik datainsamling för en virtuell dator i Azure och VMSS avger hello Azure diagnostisk tillägget också gäst-OS prestandaräknare (kallas ofta ”gäst-OS mått”).  Du kan använda de här måtten i Autoskala regler.

Du kan använda hello `Get MetricDefinitions` CLI-API/PoSH tooview hello tillgängliga mått för din VMSS resurs.

Om du använder skalningsuppsättningar och du inte ser ett viss mått i listan och sedan är förmodligen *inaktiverat* i diagnostik-tillägget.

Om ett viss mått inte används provade eller överförda på hello ofta, kan du uppdatera hello diagnostik konfiguration.

Om antingen föregående fall är true, granska [Använd PowerShell tooenable Azure-diagnostik i en virtuell dator som kör Windows](../virtual-machines/windows/ps-extensions-diagnostics.md) om PowerShell tooconfigure och uppdatera din Azure-diagnostik för VM-tillägget tooenable hello mått. Artikeln innehåller också en exempelfil diagnostik konfiguration.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Värden mått för Resource Manager-baserade Windows och Linux virtuella datorer
hello följande värdnivå mått släpps som standard för virtuella Azure-datorn och VMSS i Windows- och Linux-instanser. De här måtten beskriver Azure VM, men har samlats in från hello Azure VM-värden i stället för via agent installeras på hello gäst-VM. Du kan använda de här måtten i autoskalning regler.

- [Värden mått för Resource Manager-baserade Windows och Linux virtuella datorer](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)
- [Värden mått för Resource Manager-baserade Windows och Linux-Skalningsuppsättningar](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Gästoperativsystem mått Resource Manager-baserade virtuella Windows-datorer
När du skapar en virtuell dator i Azure är diagnostics aktiverat genom att använda hello diagnostik tillägget. hello diagnostik tillägget skickar en uppsättning mått som hämtats från inuti hello VM. Detta innebär att du kan Autoskala från mått som inte har orsakat som standard.

Du kan generera en lista över hello mått med hjälp av följande kommando i PowerShell hello.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Du kan skapa en avisering om hello följande mått:

| Måttnamnet | Enhet |
| --- | --- |
| \Processor(_Total)\% processortid |Procent |
| \Processor(_Total)\% privilegierad tid |Procent |
| \Processor(_Total)\% användartid |Procent |
| \Processor information (_Total) \Processor frekvens |Antal |
| \System\Processes |Antal |
| \Process (_Total) \Thread antal |Antal |
| \Process (_Total) \Handle antal |Antal |
| \Memory\% använda dedikerade byte |Procent |
| \Memory\Tillgängliga byte |Byte |
| \Memory\Committed byte |Byte |
| \Memory\Commit gräns |Byte |
| \Memory\Pool systemminne-byte |Byte |
| \Memory\Pool växlingsbart systemminne-byte |Byte |
| \PhysicalDisk(_Total)\% disk tid |Procent |
| \PhysicalDisk(_Total)\% disk Lästid |Procent |
| \PhysicalDisk(_Total)\% disk skrivåtgärder tid |Procent |
| \PhysicalDisk (_Total) \Disk disköverföringar/sek |CountPerSecond |
| \PhysicalDisk (_Total) \Disk Diskläsningar/sek |CountPerSecond |
| \PhysicalDisk (_Total) \Disk Diskskrivningar/sek |CountPerSecond |
| \PhysicalDisk (_Total) \Disk byte/sek |BytesPerSecond |
| \PhysicalDisk (_Total) \Disk-lästa byte/s |BytesPerSecond |
| \PhysicalDisk (_Total) \Disk skrivna byte/s |BytesPerSecond |
| \Avg \PhysicalDisk (_Total). Diskkölängd |Antal |
| \Avg \PhysicalDisk (_Total). Läs diskkölängd |Antal |
| \Avg \PhysicalDisk (_Total). Diskkölängd för skrivning |Antal |
| \LogicalDisk(_Total)\% ledigt utrymme |Procent |
| \LogicalDisk (_Total) \Free megabyte |Antal |

### <a name="guest-os-metrics-linux-vms"></a>Gästoperativsystem mått virtuella Linux-datorer
När du skapar en virtuell dator i Azure är diagnostics aktiverad som standard med hjälp av diagnostik-tillägget.

Du kan generera en lista över hello mått med hjälp av följande kommando i PowerShell hello.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Du kan skapa en avisering om hello följande mått:

| Måttnamnet | Enhet |
| --- | --- |
| \Memory\AvailableMemory |Byte |
| \Memory\PercentAvailableMemory |Procent |
| \Memory\UsedMemory |Byte |
| \Memory\PercentUsedMemory |Procent |
| \Memory\PercentUsedByCache |Procent |
| \Memory\PagesPerSec |CountPerSecond |
| \Memory\PagesReadPerSec |CountPerSecond |
| \Memory\PagesWrittenPerSec |CountPerSecond |
| \Memory\AvailableSwap |Byte |
| \Memory\PercentAvailableSwap |Procent |
| \Memory\UsedSwap |Byte |
| \Memory\PercentUsedSwap |Procent |
| \Processor\PercentIdleTime |Procent |
| \Processor\PercentUserTime |Procent |
| \Processor\PercentNiceTime |Procent |
| \Processor\PercentPrivilegedTime |Procent |
| \Processor\PercentInterruptTime |Procent |
| \Processor\PercentDPCTime |Procent |
| \Processor\PercentProcessorTime |Procent |
| \Processor\PercentIOWaitTime |Procent |
| \PhysicalDisk\BytesPerSecond |BytesPerSecond |
| \PhysicalDisk\ReadBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\WriteBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\TransfersPerSecond |CountPerSecond |
| \PhysicalDisk\ReadsPerSecond |CountPerSecond |
| \PhysicalDisk\WritesPerSecond |CountPerSecond |
| \PhysicalDisk\AverageReadTime |Sekunder |
| \PhysicalDisk\AverageWriteTime |Sekunder |
| \PhysicalDisk\AverageTransferTime |Sekunder |
| \PhysicalDisk\AverageDiskQueueLength |Antal |
| \NetworkInterface\BytesTransmitted |Byte |
| \NetworkInterface\BytesReceived |Byte |
| \NetworkInterface\PacketsTransmitted |Antal |
| \NetworkInterface\PacketsReceived |Antal |
| \NetworkInterface\BytesTotal |Byte |
| \NetworkInterface\TotalRxErrors |Antal |
| \NetworkInterface\TotalTxErrors |Antal |
| \NetworkInterface\TotalCollisions |Antal |

## <a name="commonly-used-web-server-farm-metrics"></a>Vanliga mätvärden från webben (servergruppen)
Du kan också utföra Autoskala baserat på gemensamma web-serverstatistik som hello Kölängd för Http. Tjänstmåttets namn är **HttpQueueLength**.  följande avsnitt visar tillgängliga servergruppen (webbprogram) serverstatistik hello.

### <a name="web-apps-metrics"></a>Web Apps mått
Du kan generera en lista över hello Web Apps mått med hjälp av följande kommando i PowerShell hello.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Du kan varning i eller skala av de här måtten.

| Måttnamnet | Enhet |
| --- | --- |
| CpuPercentage |Procent |
| MemoryPercentage |Procent |
| DiskQueueLength |Antal |
| HttpQueueLength |Antal |
| BytesReceived |Byte |
| BytesSent |Byte |

## <a name="commonly-used-storage-metrics"></a>Vanliga Storage-mätvärden
Du kan skala genom Kölängd för lagring som är hello antal meddelanden i kö för hello lagring. Kölängd för lagring är en särskild mått och hello tröskelvärdet är hello antal meddelanden per instans. Till exempel inträffar om det finns två instanser och hello tröskelvärde anges too100, skalning när hello Totalt antal meddelanden i kö för hello 200. Som kan vara 100 meddelanden per instans, 120 och 80, eller någon annan kombination som lägger till too200 eller mer.

Konfigurera den här inställningen i hello Azure-portalen i hello **inställningar** bladet. För skalningsuppsättningar, kan du uppdatera hello autoskalningsinställning i hello Resource Manager-mall toouse *metricName* som *ApproximateMessageCount* och skicka hello-ID för hello lagring kö som  *metricResourceUri*.

Till exempel, med en klassiska Lagringskonto hello skulle Autoskala inställningen metricTrigger omfattar:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

För ett lagringskonto med (icke-klassisk) omfattar hello metricTrigger:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Vanliga Service Bus-mått
Du kan skala av Service Bus Kölängd, vilket är hello antal meddelanden i hello Service Bus-kö. Kölängd för Service Bus är en särskild mått och hello tröskelvärdet är hello antal meddelanden per instans. Till exempel inträffar om det finns två instanser och hello tröskelvärde anges too100, skalning när hello Totalt antal meddelanden i kö för hello 200. Som kan vara 100 meddelanden per instans, 120 och 80, eller någon annan kombination som lägger till too200 eller mer.

För skalningsuppsättningar, kan du uppdatera hello autoskalningsinställning i hello Resource Manager-mall toouse *metricName* som *ApproximateMessageCount* och skicka hello-ID för hello lagring kö som  *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Hello resurs grupp konceptet finns inte för Service Bus, men Azure Resource Manager skapar en standard resursgrupp per region. hello resursgruppen har vanligtvis hello 'Default - ServiceBus-[region]'-format. Till exempel 'Standard-ServiceBus-EastUS', 'Standard-ServiceBus-WestUS', 'Standard-ServiceBus-AustraliaEast ”osv.
>
>
