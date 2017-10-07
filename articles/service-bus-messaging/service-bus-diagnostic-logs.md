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
# <a name="service-bus-diagnostic-logs"></a>Service Bus diagnostikloggar

Du kan visa två typer av loggar för Azure Service Bus:
* **[Aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Dessa loggar innehåller information om åtgärder som utförs på ett annat jobb. hello loggar är alltid aktiverat.
* **[Diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. Du kan konfigurera diagnostiska loggar för bättre information om allt som händer i ett jobb. Diagnostikloggar omfattar aktiviteter från hello att hello jobb skapas tills hello jobbet tas bort, inklusive uppdateringar och aktiviteter som sker när hello jobbet körs.

## <a name="turn-on-diagnostic-logs"></a>Aktivera diagnostikloggar

Diagnostik loggar är inaktiverade som standard. tooenable diagnostikloggar, utför följande steg hello:

1.  I hello [Azure-portalen](https://portal.azure.com)under **övervakning + Management**, klickar du på **diagnostik loggar**.

    ![bladet navigering toodiagnostic loggar](./media/service-bus-diagnostic-logs/image1.png)

2. Klicka på hello resursen toomonitor.  

3.  Klicka på **aktivera diagnostiken**.

    ![Aktivera diagnostikloggar](./media/service-bus-diagnostic-logs/image2.png)

4.  För **Status**, klickar du på **på**.

    ![Ändra status diagnostikloggar](./media/service-bus-diagnostic-logs/image3.png)

5.  Ange hello Arkiv mål som du vill; till exempel ett lagringskonto, en Händelsehubb eller Azure logganalys.

6.  Spara hello nya diagnostikinställningar.

Nya inställningar börjar gälla i cirka 10 minuter. Efter det loggar visas i hello konfigurerats arkivering mål på hello **diagnostik loggar** bladet.

Mer information om hur du konfigurerar diagnostik finns hello [översikt över Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-schema"></a>Diagnostikloggar schema

Alla loggar lagras i JavaScript Object Notation (JSON)-format. Varje post innehåller strängfält som använder hello-format som beskrivs i följande avsnitt hello.

## <a name="operational-logs-schema"></a>Operativa loggar schema

Loggar in hello **OperationalLogs** kategori avbilda vad som händer under Service Bus-åtgärder. Mer specifikt loggarna avbilda hello åtgärdstyp, inklusive kön skapas, resurser som används, och hello status för hello-åtgärd.

Arbetsloggen JSON strängar innehålla element som anges i följande tabell hello:

Namn | Beskrivning
------- | -------
ActivityId | Internt ID som används för spårning
EventName | Åtgärdsnamn           
resourceId | Azure Resource Manager-resurs-ID
SubscriptionId | Prenumerations-ID:t
EventTimeString | Åtgärden tid
EventProperties | Åtgärden egenskaper
Status | Åtgärdsstatus
Anropare | Anroparen av åtgärden (Azure portal eller management-klienten)
category | OperationalLogs

Här är ett exempel på en arbetsloggen JSON-sträng:

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

## <a name="next-steps"></a>Nästa steg

Finns följande länkar toolearn mer om Service Bus hello:

* [Introduktion tooService Bus](service-bus-messaging-overview.md)
* [Kom igång med Service Bus](service-bus-dotnet-get-started-with-queues.md)
