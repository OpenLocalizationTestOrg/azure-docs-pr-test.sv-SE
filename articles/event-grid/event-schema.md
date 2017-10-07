---
title: "aaaAzure händelse rutnätet Händelseschema"
description: "Beskriver hello egenskaper som har angetts för händelser med Azure händelse rutnätet."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/15/2017
ms.author: babanisa
ms.openlocfilehash: 37178a5650b93fd9072d9cff3333aae14b2a2ba7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-event-schema"></a>Händelseschema rutnätet händelse

Den här artikeln innehåller hello egenskaper och schemat för händelser. Händelser som består av en uppsättning med fem krävs strängegenskaper och en nödvändig **data** objekt. hello egenskaper är gemensamma tooall händelser från alla utgivare. Hej **data** objektet innehåller de egenskaper som är specifika tooeach utgivare. För system-avsnitt är dessa egenskaper specifika toohello resursleverantör, till exempel lagring eller Händelsehubbar.

Händelser skickas tooAzure händelse rutnät i en matris som kan innehålla flera händelseobjekt. Om det finns endast en enskild händelse, har hello matris en längd på 1. 
 
## <a name="event-properties"></a>Egenskaper för händelse

Alla händelser kommer att innehålla hello samma följande data på översta nivån.

| Egenskap | Typ | Beskrivning |
| -------- | ---- | ----------- |
| Avsnittet | Sträng | Fullständigt labbresurs sökväg toohello händelsekälla. Det här fältet är skrivskyddat. |
| Ämne | Sträng | Publisher definierade sökvägen toohello händelse ämne. |
| Händelsetyp | Sträng | Ett av hello registrerat händelsetyper för denna källa. |
| EventTime | Sträng | hello tid hello händelse genereras baserat på UTC-tid för hello-providern. |
| id | Sträng | Unik identifierare för hello-händelse. |
| Data | Objektet | Specifika toohello resource provider för händelsedata. |

## <a name="available-event-sources"></a>Tillgängliga händelsekällor

hello följande händelsekällor publicera händelser för förbrukning via händelsen rutnätet:

* Resursgrupper (hanteringsåtgärder)
* Azure-prenumerationer (hanteringsåtgärder)
* Händelsehubbar
* Anpassade avsnitt

## <a name="azure-subscriptions"></a>Azure-prenumerationer

Azure-prenumerationer kan nu skapa management händelser från Azure Resource Manager, t.ex när en virtuell dator skapas eller ett lagringskonto tas bort.

### <a name="available-event-types"></a>Tillgängliga händelsetyper

- **Microsoft.Resources.ResourceWriteSuccess**: aktiveras när en resurs skapa eller uppdatera åtgärden lyckas.  
- **Microsoft.Resources.ResourceWriteFailure**: aktiveras när en resurs skapa eller uppdateringsåtgärden misslyckas.  
- **Microsoft.Resources.ResourceWriteCancel**: aktiveras när en resurs skapa eller uppdatera åtgärden har avbrutits.  
- **Microsoft.Resources.ResourceDeleteSuccess**: aktiveras när en resurs borttagningsåtgärd lyckas.  
- **Microsoft.Resources.ResourceDeleteFailure**: aktiveras när en resurs borttagningsåtgärd misslyckas.  
- **Microsoft.Resources.ResourceDeleteCancel**: ”utlöses när en resurs delete avbryts. Detta händer när mallen distribueras avbryts.

### <a name="example-event-schema"></a>Exempel Händelseschema

```json
[
    {
    "topic":"/subscriptions/{subscription-id}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="resource-groups"></a>Resursgrupper

Resursgrupper kan nu skapa management händelser från Azure Resource Manager, t.ex när en virtuell dator skapas eller ett lagringskonto tas bort.

### <a name="available-event-types"></a>Tillgängliga händelsetyper

- **Microsoft.Resources.ResourceWriteSuccess**: aktiveras när en resurs skapa eller uppdatera åtgärden lyckas.  
- **Microsoft.Resources.ResourceWriteFailure**: aktiveras när en resurs skapa eller uppdateringsåtgärden misslyckas.  
- **Microsoft.Resources.ResourceWriteCancel**: aktiveras när en resurs skapa eller uppdatera åtgärden har avbrutits.  
- **Microsoft.Resources.ResourceDeleteSuccess**: aktiveras när en resurs borttagningsåtgärd lyckas.  
- **Microsoft.Resources.ResourceDeleteFailure**: aktiveras när en resurs borttagningsåtgärd misslyckas.  
- **Microsoft.Resources.ResourceDeleteCancel**: ”utlöses när en resurs delete avbryts. Detta händer när mallen distribueras avbryts.

### <a name="example-event"></a>Exempel-händelse

```json
[
    {
    "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="event-hubs"></a>Händelsehubbar

Event Hubs händelser är för närvarande endast orsakat när en fil skickas automatiskt toostorage hello avbilda funktionen.

### <a name="available-event-types"></a>Tillgängliga händelsetyper

- **Microsoft.EventHub.CaptureFileCreated**: aktiveras när en avbildning fil skapas.

### <a name="example-event"></a>Exempel-händelse

Den här händelsen i exemplet visar hello schemat för en Händelsehubbar händelse som aktiveras när en fil lagras i avbildningen. 

```json
[
    {
        "topic": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{event-hubs-ns}",
        "subject": "eventhubs/eh1",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-07-11T00:55:55.0120485Z",
        "id": "bd440490-a65e-4c97-8298-ef1eb325673c",
        "data": {
            "fileUrl": "https://gridtest1.blob.core.windows.net/acontainer/eventgridtest1/eh1/1/2017/07/11/00/54/54.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 0,
            "eventCount": 0,
            "firstSequenceNumber": -1,
            "lastSequenceNumber": -1,
            "firstEnqueueTime": "0001-01-01T00:00:00",
            "lastEnqueueTime": "0001-01-01T00:00:00"
        },
    }
]

```



## <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Blob Storage i privat förhandsvisning med registreringen för integrering med händelsen rutnätet.

### <a name="available-event-types"></a>Tillgängliga händelsetyper

- **Microsoft.Storage.BlobCreated**: aktiveras när en blob har skapats.
- **Microsoft.Storage.BlobDeleted**: aktiveras när en blob tas bort.

### <a name="example-event"></a>Exempel-händelse

Den här händelsen i exemplet visar hello schemat för en lagring händelse som aktiveras när en blob skapas. 

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
  }
]
```




## <a name="custom-topics"></a>Anpassade avsnitt

hello-nyttolast av din anpassade händelser definieras av du och kan vara en välformad JSON. hello data på översta nivån ska innehålla hello samma fält som standard resursen definieras händelser. När du publicerar händelser toocustom avsnitt bör du modellera hello ämnet för din tooaid händelser i Routning och filtrering.

### <a name="example-event"></a>Exempel-händelse

hello som följande exempel visar en händelse för en anpassad avsnittet:
````json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.EventGrid/topics/myeventgridtopic",
    "subject": "/myapp/vehicles/motorcycles",    
    "id": "b68529f3-68cd-4744-baa4-3c0498ec19e2",
    "eventType": "recordInserted",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "data":{
      "make": "Ducati",
      "model": "Monster"
    }
  }
]

````

## <a name="next-steps"></a>Nästa steg

* En introduktion tooEvent rutnätet finns [vad är händelsen rutnätet?](overview.md)
* toolearn om hur du skapar en händelse rutnätet prenumeration finns [händelse rutnätet prenumeration schemat](subscription-creation-schema.md).
