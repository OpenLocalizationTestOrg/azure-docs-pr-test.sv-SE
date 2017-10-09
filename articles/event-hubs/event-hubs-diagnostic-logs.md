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
# <a name="event-hubs-diagnostic-logs"></a>Event Hubs diagnostikloggar

Du kan visa två typer av loggar för Händelsehubbar i Azure:
* **[Aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Dessa loggar har information om åtgärder som utförs på ett annat jobb. hello loggar är alltid aktiverat.
* **[Diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. Du kan konfigurera diagnostikloggar för en heltäckande vy av allt som händer med ett jobb. Diagnostikloggar omfattar aktiviteter från hello att hello jobb skapas tills hello jobbet tas bort, inklusive uppdateringar och aktiviteter som sker när hello jobbet körs.

## <a name="turn-on-diagnostic-logs"></a>Aktivera diagnostikloggar
Diagnostik loggar är inaktiverade som standard. tooenable diagnostikloggar:

1.  I hello [Azure-portalen](https://portal.azure.com)under **övervakning + Management**, klickar du på **diagnostik loggar**.

    ![bladet navigering toodiagnostic loggar](./media/event-hubs-diagnostic-logs/image1.png)

2.  Klicka på hello resursen toomonitor.

3.  Klicka på **aktivera diagnostiken**.

    ![Aktivera diagnostikloggar](./media/event-hubs-diagnostic-logs/image2.png)

4.  För **Status**, klickar du på **på**.

    ![Ändra hello status för diagnostikloggar](./media/event-hubs-diagnostic-logs/image3.png)

5.  Ange hello Arkiv mål som du vill; ett lagringskonto, en händelsehubb eller Azure logganalys.

6.  Spara hello nya diagnostikinställningar.

Nya inställningar börjar gälla i cirka 10 minuter. Efter det loggar visas i hello konfigurerats arkivering mål på hello **diagnostik loggar** bladet.

Mer information om hur du konfigurerar diagnostik finns hello [översikt över Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-categories"></a>Diagnostikloggar kategorier
Händelsehubbar samlar in diagnostikloggar två kategorier:

* **ArchiveLogs**: loggar relaterade tooEvent hubbar Arkiv specifikt loggar relaterade tooarchive fel.
* **OperationalLogs**: information om vad som händer under Händelsehubbar åtgärder, särskilt hello åtgärdstyp, inklusive event hub skapas, resurser som används, och hello status för hello-åtgärd.

## <a name="diagnostic-logs-schema"></a>Diagnostikloggar schema
Alla loggar lagras i JavaScript Object Notation (JSON)-format. Varje post innehåller strängfält som använder hello-format som beskrivs i följande avsnitt hello.

### <a name="archive-logs-schema"></a>Arkivera loggar schema

Arkivera loggen JSON strängar innehålla element som anges i följande tabell hello:

Namn | Beskrivning
------- | -------
Aktivitetsnamn | Beskrivning av hello-aktivitet som misslyckades.
ActivityId | Internt ID som används för spårning.
trackingId | Internt ID som används för spårning.
resourceId | Azure Resource Manager-resurs-ID.
EventHub | Händelsehubb fullständigt namn (inklusive namnområdesnamnet).
partitions-ID | Event Hub partition skrivs till.
archiveStep | ArchiveFlushWriter
startTime | Fel starttid.
fel | Antal gånger som fel uppstod.
durationInSeconds | Varaktighet för felet.
Meddelande | Felmeddelande.
category | ArchiveLogs

hello är följande kod ett exempel på en arkivera loggen JSON-sträng:

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

### <a name="operational-logs-schema"></a>Operativa loggar schema

Arbetsloggen JSON strängar innehålla element som anges i följande tabell hello:

Namn | Beskrivning
------- | -------
ActivityId | Internt ID används tootrack syfte.
EventName | Åtgärdsnamnet.  
resourceId | Azure Resource Manager-resurs-ID.
SubscriptionId | Prenumerations-ID.
EventTimeString | Tid för åtgärden.
EventProperties | Åtgärden egenskaper.
Status | Åtgärdsstatus för.
Anropare | Anroparen av åtgärden (Azure portal eller management-klienten).
category | OperationalLogs

hello är följande kod ett exempel på en arbetsloggen JSON-sträng:

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

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooEvent hubbar](event-hubs-what-is-event-hubs.md)
* [Event Hubs API-översikt](event-hubs-api-overview.md)
* [Kom igång med Event Hubs](event-hubs-csharp-ephcs-getstarted.md)
