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
# <a name="event-grid-event-schema"></a><span data-ttu-id="fb5da-103">Händelseschema rutnätet händelse</span><span class="sxs-lookup"><span data-stu-id="fb5da-103">Event Grid event schema</span></span>

<span data-ttu-id="fb5da-104">Den här artikeln innehåller hello egenskaper och schemat för händelser.</span><span class="sxs-lookup"><span data-stu-id="fb5da-104">This article provides hello properties and schema for events.</span></span> <span data-ttu-id="fb5da-105">Händelser som består av en uppsättning med fem krävs strängegenskaper och en nödvändig **data** objekt.</span><span class="sxs-lookup"><span data-stu-id="fb5da-105">Events consist of a set of five required string properties and a required **data** object.</span></span> <span data-ttu-id="fb5da-106">hello egenskaper är gemensamma tooall händelser från alla utgivare.</span><span class="sxs-lookup"><span data-stu-id="fb5da-106">hello properties are common tooall events from any publisher.</span></span> <span data-ttu-id="fb5da-107">Hej **data** objektet innehåller de egenskaper som är specifika tooeach utgivare.</span><span class="sxs-lookup"><span data-stu-id="fb5da-107">hello **data** object contains properties that are specific tooeach publisher.</span></span> <span data-ttu-id="fb5da-108">För system-avsnitt är dessa egenskaper specifika toohello resursleverantör, till exempel lagring eller Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="fb5da-108">For system topics, these properties are specific toohello resource provider, such as Storage or Event Hubs.</span></span>

<span data-ttu-id="fb5da-109">Händelser skickas tooAzure händelse rutnät i en matris som kan innehålla flera händelseobjekt.</span><span class="sxs-lookup"><span data-stu-id="fb5da-109">Events are sent tooAzure Event Grid in an array, which can contain multiple event objects.</span></span> <span data-ttu-id="fb5da-110">Om det finns endast en enskild händelse, har hello matris en längd på 1.</span><span class="sxs-lookup"><span data-stu-id="fb5da-110">If there is only a single event, hello array has a length of 1.</span></span> 
 
## <a name="event-properties"></a><span data-ttu-id="fb5da-111">Egenskaper för händelse</span><span class="sxs-lookup"><span data-stu-id="fb5da-111">Event properties</span></span>

<span data-ttu-id="fb5da-112">Alla händelser kommer att innehålla hello samma följande data på översta nivån.</span><span class="sxs-lookup"><span data-stu-id="fb5da-112">All events will contain hello same following top level data.</span></span>

| <span data-ttu-id="fb5da-113">Egenskap</span><span class="sxs-lookup"><span data-stu-id="fb5da-113">Property</span></span> | <span data-ttu-id="fb5da-114">Typ</span><span class="sxs-lookup"><span data-stu-id="fb5da-114">Type</span></span> | <span data-ttu-id="fb5da-115">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fb5da-115">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="fb5da-116">Avsnittet</span><span class="sxs-lookup"><span data-stu-id="fb5da-116">topic</span></span> | <span data-ttu-id="fb5da-117">Sträng</span><span class="sxs-lookup"><span data-stu-id="fb5da-117">string</span></span> | <span data-ttu-id="fb5da-118">Fullständigt labbresurs sökväg toohello händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="fb5da-118">Full resource path toohello event source.</span></span> <span data-ttu-id="fb5da-119">Det här fältet är skrivskyddat.</span><span class="sxs-lookup"><span data-stu-id="fb5da-119">This field is not writeable.</span></span> |
| <span data-ttu-id="fb5da-120">Ämne</span><span class="sxs-lookup"><span data-stu-id="fb5da-120">subject</span></span> | <span data-ttu-id="fb5da-121">Sträng</span><span class="sxs-lookup"><span data-stu-id="fb5da-121">string</span></span> | <span data-ttu-id="fb5da-122">Publisher definierade sökvägen toohello händelse ämne.</span><span class="sxs-lookup"><span data-stu-id="fb5da-122">Publisher defined path toohello event subject.</span></span> |
| <span data-ttu-id="fb5da-123">Händelsetyp</span><span class="sxs-lookup"><span data-stu-id="fb5da-123">eventType</span></span> | <span data-ttu-id="fb5da-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="fb5da-124">string</span></span> | <span data-ttu-id="fb5da-125">Ett av hello registrerat händelsetyper för denna källa.</span><span class="sxs-lookup"><span data-stu-id="fb5da-125">One of hello registered event types for this event source.</span></span> |
| <span data-ttu-id="fb5da-126">EventTime</span><span class="sxs-lookup"><span data-stu-id="fb5da-126">eventTime</span></span> | <span data-ttu-id="fb5da-127">Sträng</span><span class="sxs-lookup"><span data-stu-id="fb5da-127">string</span></span> | <span data-ttu-id="fb5da-128">hello tid hello händelse genereras baserat på UTC-tid för hello-providern.</span><span class="sxs-lookup"><span data-stu-id="fb5da-128">hello time hello event is generated based on hello provider's UTC time.</span></span> |
| <span data-ttu-id="fb5da-129">id</span><span class="sxs-lookup"><span data-stu-id="fb5da-129">id</span></span> | <span data-ttu-id="fb5da-130">Sträng</span><span class="sxs-lookup"><span data-stu-id="fb5da-130">string</span></span> | <span data-ttu-id="fb5da-131">Unik identifierare för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="fb5da-131">Unique identifier for hello event.</span></span> |
| <span data-ttu-id="fb5da-132">Data</span><span class="sxs-lookup"><span data-stu-id="fb5da-132">data</span></span> | <span data-ttu-id="fb5da-133">Objektet</span><span class="sxs-lookup"><span data-stu-id="fb5da-133">object</span></span> | <span data-ttu-id="fb5da-134">Specifika toohello resource provider för händelsedata.</span><span class="sxs-lookup"><span data-stu-id="fb5da-134">Event data specific toohello resource provider.</span></span> |

## <a name="available-event-sources"></a><span data-ttu-id="fb5da-135">Tillgängliga händelsekällor</span><span class="sxs-lookup"><span data-stu-id="fb5da-135">Available event sources</span></span>

<span data-ttu-id="fb5da-136">hello följande händelsekällor publicera händelser för förbrukning via händelsen rutnätet:</span><span class="sxs-lookup"><span data-stu-id="fb5da-136">hello following event sources publish events for consumption via Event Grid:</span></span>

* <span data-ttu-id="fb5da-137">Resursgrupper (hanteringsåtgärder)</span><span class="sxs-lookup"><span data-stu-id="fb5da-137">Resource Groups (management operations)</span></span>
* <span data-ttu-id="fb5da-138">Azure-prenumerationer (hanteringsåtgärder)</span><span class="sxs-lookup"><span data-stu-id="fb5da-138">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="fb5da-139">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="fb5da-139">Event Hubs</span></span>
* <span data-ttu-id="fb5da-140">Anpassade avsnitt</span><span class="sxs-lookup"><span data-stu-id="fb5da-140">Custom Topics</span></span>

## <a name="azure-subscriptions"></a><span data-ttu-id="fb5da-141">Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="fb5da-141">Azure Subscriptions</span></span>

<span data-ttu-id="fb5da-142">Azure-prenumerationer kan nu skapa management händelser från Azure Resource Manager, t.ex när en virtuell dator skapas eller ett lagringskonto tas bort.</span><span class="sxs-lookup"><span data-stu-id="fb5da-142">Azure subscriptions can now emit management events from Azure Resource Manager such as when a VM is created or a storage account is deleted.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="fb5da-143">Tillgängliga händelsetyper</span><span class="sxs-lookup"><span data-stu-id="fb5da-143">Available event types</span></span>

- <span data-ttu-id="fb5da-144">**Microsoft.Resources.ResourceWriteSuccess**: aktiveras när en resurs skapa eller uppdatera åtgärden lyckas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-144">**Microsoft.Resources.ResourceWriteSuccess**: Raised when a resource create or update operation succeeds.</span></span>  
- <span data-ttu-id="fb5da-145">**Microsoft.Resources.ResourceWriteFailure**: aktiveras när en resurs skapa eller uppdateringsåtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-145">**Microsoft.Resources.ResourceWriteFailure**: Raised when a resource create or update operation fails.</span></span>  
- <span data-ttu-id="fb5da-146">**Microsoft.Resources.ResourceWriteCancel**: aktiveras när en resurs skapa eller uppdatera åtgärden har avbrutits.</span><span class="sxs-lookup"><span data-stu-id="fb5da-146">**Microsoft.Resources.ResourceWriteCancel**: Raised when a resource create or update operation is cancelled.</span></span>  
- <span data-ttu-id="fb5da-147">**Microsoft.Resources.ResourceDeleteSuccess**: aktiveras när en resurs borttagningsåtgärd lyckas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-147">**Microsoft.Resources.ResourceDeleteSuccess**: Raised when a resource deletion operation succeeds.</span></span>  
- <span data-ttu-id="fb5da-148">**Microsoft.Resources.ResourceDeleteFailure**: aktiveras när en resurs borttagningsåtgärd misslyckas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-148">**Microsoft.Resources.ResourceDeleteFailure**: Raised when a resource delete operation fails.</span></span>  
- <span data-ttu-id="fb5da-149">**Microsoft.Resources.ResourceDeleteCancel**: ”utlöses när en resurs delete avbryts.</span><span class="sxs-lookup"><span data-stu-id="fb5da-149">**Microsoft.Resources.ResourceDeleteCancel**: "Raised when a resource delete is cancelled.</span></span> <span data-ttu-id="fb5da-150">Detta händer när mallen distribueras avbryts.</span><span class="sxs-lookup"><span data-stu-id="fb5da-150">This happens when template deployment is cancelled.</span></span>

### <a name="example-event-schema"></a><span data-ttu-id="fb5da-151">Exempel Händelseschema</span><span class="sxs-lookup"><span data-stu-id="fb5da-151">Example event schema</span></span>

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



## <a name="resource-groups"></a><span data-ttu-id="fb5da-152">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="fb5da-152">Resource Groups</span></span>

<span data-ttu-id="fb5da-153">Resursgrupper kan nu skapa management händelser från Azure Resource Manager, t.ex när en virtuell dator skapas eller ett lagringskonto tas bort.</span><span class="sxs-lookup"><span data-stu-id="fb5da-153">Resource Groups can now emit management events from Azure Resource Manager such as when a VM is created or a storage account is deleted.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="fb5da-154">Tillgängliga händelsetyper</span><span class="sxs-lookup"><span data-stu-id="fb5da-154">Available event types</span></span>

- <span data-ttu-id="fb5da-155">**Microsoft.Resources.ResourceWriteSuccess**: aktiveras när en resurs skapa eller uppdatera åtgärden lyckas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-155">**Microsoft.Resources.ResourceWriteSuccess**: Raised when a resource create or update operation succeeds.</span></span>  
- <span data-ttu-id="fb5da-156">**Microsoft.Resources.ResourceWriteFailure**: aktiveras när en resurs skapa eller uppdateringsåtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-156">**Microsoft.Resources.ResourceWriteFailure**: Raised when a resource create or update operation fails.</span></span>  
- <span data-ttu-id="fb5da-157">**Microsoft.Resources.ResourceWriteCancel**: aktiveras när en resurs skapa eller uppdatera åtgärden har avbrutits.</span><span class="sxs-lookup"><span data-stu-id="fb5da-157">**Microsoft.Resources.ResourceWriteCancel**: Raised when a resource create or update operation is cancelled.</span></span>  
- <span data-ttu-id="fb5da-158">**Microsoft.Resources.ResourceDeleteSuccess**: aktiveras när en resurs borttagningsåtgärd lyckas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-158">**Microsoft.Resources.ResourceDeleteSuccess**: Raised when a resource deletion operation succeeds.</span></span>  
- <span data-ttu-id="fb5da-159">**Microsoft.Resources.ResourceDeleteFailure**: aktiveras när en resurs borttagningsåtgärd misslyckas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-159">**Microsoft.Resources.ResourceDeleteFailure**: Raised when a resource delete operation fails.</span></span>  
- <span data-ttu-id="fb5da-160">**Microsoft.Resources.ResourceDeleteCancel**: ”utlöses när en resurs delete avbryts.</span><span class="sxs-lookup"><span data-stu-id="fb5da-160">**Microsoft.Resources.ResourceDeleteCancel**: "Raised when a resource delete is cancelled.</span></span> <span data-ttu-id="fb5da-161">Detta händer när mallen distribueras avbryts.</span><span class="sxs-lookup"><span data-stu-id="fb5da-161">This happens when template deployment is cancelled.</span></span>

### <a name="example-event"></a><span data-ttu-id="fb5da-162">Exempel-händelse</span><span class="sxs-lookup"><span data-stu-id="fb5da-162">Example event</span></span>

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



## <a name="event-hubs"></a><span data-ttu-id="fb5da-163">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="fb5da-163">Event Hubs</span></span>

<span data-ttu-id="fb5da-164">Event Hubs händelser är för närvarande endast orsakat när en fil skickas automatiskt toostorage hello avbilda funktionen.</span><span class="sxs-lookup"><span data-stu-id="fb5da-164">Event Hubs events are currently only emitted when a file is automatically sent toostorage using hello Capture feature.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="fb5da-165">Tillgängliga händelsetyper</span><span class="sxs-lookup"><span data-stu-id="fb5da-165">Available event types</span></span>

- <span data-ttu-id="fb5da-166">**Microsoft.EventHub.CaptureFileCreated**: aktiveras när en avbildning fil skapas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-166">**Microsoft.EventHub.CaptureFileCreated**: Raised when a capture file is created.</span></span>

### <a name="example-event"></a><span data-ttu-id="fb5da-167">Exempel-händelse</span><span class="sxs-lookup"><span data-stu-id="fb5da-167">Example event</span></span>

<span data-ttu-id="fb5da-168">Den här händelsen i exemplet visar hello schemat för en Händelsehubbar händelse som aktiveras när en fil lagras i avbildningen.</span><span class="sxs-lookup"><span data-stu-id="fb5da-168">This sample event shows hello schema of an Event Hubs event raised when Capture stores a file.</span></span> 

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



## <a name="azure-blob-storage"></a><span data-ttu-id="fb5da-169">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="fb5da-169">Azure Blob Storage</span></span>

<span data-ttu-id="fb5da-170">Azure Blob Storage i privat förhandsvisning med registreringen för integrering med händelsen rutnätet.</span><span class="sxs-lookup"><span data-stu-id="fb5da-170">Azure Blob Storage in private preview with sign-up for integration with Event Grid.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="fb5da-171">Tillgängliga händelsetyper</span><span class="sxs-lookup"><span data-stu-id="fb5da-171">Available event types</span></span>

- <span data-ttu-id="fb5da-172">**Microsoft.Storage.BlobCreated**: aktiveras när en blob har skapats.</span><span class="sxs-lookup"><span data-stu-id="fb5da-172">**Microsoft.Storage.BlobCreated**: Raised when a blob is created.</span></span>
- <span data-ttu-id="fb5da-173">**Microsoft.Storage.BlobDeleted**: aktiveras när en blob tas bort.</span><span class="sxs-lookup"><span data-stu-id="fb5da-173">**Microsoft.Storage.BlobDeleted**: Raised when a blob is deleted.</span></span>

### <a name="example-event"></a><span data-ttu-id="fb5da-174">Exempel-händelse</span><span class="sxs-lookup"><span data-stu-id="fb5da-174">Example event</span></span>

<span data-ttu-id="fb5da-175">Den här händelsen i exemplet visar hello schemat för en lagring händelse som aktiveras när en blob skapas.</span><span class="sxs-lookup"><span data-stu-id="fb5da-175">This sample event shows hello schema of a storage event raised when a blob is created.</span></span> 

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




## <a name="custom-topics"></a><span data-ttu-id="fb5da-176">Anpassade avsnitt</span><span class="sxs-lookup"><span data-stu-id="fb5da-176">Custom Topics</span></span>

<span data-ttu-id="fb5da-177">hello-nyttolast av din anpassade händelser definieras av du och kan vara en välformad JSON.</span><span class="sxs-lookup"><span data-stu-id="fb5da-177">hello data payload of your custom events is defined by you and can be any well formated JSON.</span></span> <span data-ttu-id="fb5da-178">hello data på översta nivån ska innehålla hello samma fält som standard resursen definieras händelser.</span><span class="sxs-lookup"><span data-stu-id="fb5da-178">hello top level data should contain hello same fields as standard resource defined events.</span></span> <span data-ttu-id="fb5da-179">När du publicerar händelser toocustom avsnitt bör du modellera hello ämnet för din tooaid händelser i Routning och filtrering.</span><span class="sxs-lookup"><span data-stu-id="fb5da-179">When publishing events toocustom topics you should consider modeling hello subject of your events tooaid in routing and filtering.</span></span>

### <a name="example-event"></a><span data-ttu-id="fb5da-180">Exempel-händelse</span><span class="sxs-lookup"><span data-stu-id="fb5da-180">Example event</span></span>

<span data-ttu-id="fb5da-181">hello som följande exempel visar en händelse för en anpassad avsnittet:</span><span class="sxs-lookup"><span data-stu-id="fb5da-181">hello following example shows an event for a custom topic:</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="fb5da-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb5da-182">Next steps</span></span>

* <span data-ttu-id="fb5da-183">En introduktion tooEvent rutnätet finns [vad är händelsen rutnätet?](overview.md)</span><span class="sxs-lookup"><span data-stu-id="fb5da-183">For an introduction tooEvent Grid, see [What is Event Grid?](overview.md)</span></span>
* <span data-ttu-id="fb5da-184">toolearn om hur du skapar en händelse rutnätet prenumeration finns [händelse rutnätet prenumeration schemat](subscription-creation-schema.md).</span><span class="sxs-lookup"><span data-stu-id="fb5da-184">toolearn about creating an Event Grid subscription, see [Event Grid subscription schema](subscription-creation-schema.md).</span></span>
