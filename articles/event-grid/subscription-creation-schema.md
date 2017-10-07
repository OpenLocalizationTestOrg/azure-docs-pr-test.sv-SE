---
title: "aaaAzure händelse rutnätet prenumeration schema"
description: "Beskriver hello egenskaper för prenumerationsapp tooan händelse med Azure händelse rutnätet."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/17/2017
ms.author: babanisa
ms.openlocfilehash: 6a96d67975a5a733c5ea3c56ea54501f94ea4cd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-subscription-schema"></a><span data-ttu-id="1524c-103">Händelseschema rutnätet prenumeration</span><span class="sxs-lookup"><span data-stu-id="1524c-103">Event Grid subscription schema</span></span>

<span data-ttu-id="1524c-104">toocreate en händelse rutnätet prenumeration du skicka en begäran toohello Skapa händelse prenumerationen igen.</span><span class="sxs-lookup"><span data-stu-id="1524c-104">toocreate an Event Grid subscription, you send a request toohello Create Event subscription operation.</span></span> <span data-ttu-id="1524c-105">Använd hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1524c-105">Use hello following format:</span></span>

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

<span data-ttu-id="1524c-106">Till exempel toocreate en händelseprenumerationen för ett lagringskonto med namnet `examplestorage` i en resursgrupp med namnet `examplegroup`, Använd hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1524c-106">For example, toocreate an event subscription for a storage account named `examplestorage` in a resource group named `examplegroup`, use hello following format:</span></span>

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

<span data-ttu-id="1524c-107">hello beskrivs hello egenskaper och schemat för hello hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="1524c-107">hello article describes hello properties and schema for hello body of hello request.</span></span>
 
## <a name="event-subscription-properties"></a><span data-ttu-id="1524c-108">Händelseegenskaper för prenumeration</span><span class="sxs-lookup"><span data-stu-id="1524c-108">Event subscription properties</span></span>

| <span data-ttu-id="1524c-109">Egenskap</span><span class="sxs-lookup"><span data-stu-id="1524c-109">Property</span></span> | <span data-ttu-id="1524c-110">Typ</span><span class="sxs-lookup"><span data-stu-id="1524c-110">Type</span></span> | <span data-ttu-id="1524c-111">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1524c-111">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="1524c-112">Mål</span><span class="sxs-lookup"><span data-stu-id="1524c-112">destination</span></span> | <span data-ttu-id="1524c-113">Objektet</span><span class="sxs-lookup"><span data-stu-id="1524c-113">object</span></span> | <span data-ttu-id="1524c-114">hello-objekt som definierar hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="1524c-114">hello object that defines hello endpoint.</span></span> |
| <span data-ttu-id="1524c-115">Filter</span><span class="sxs-lookup"><span data-stu-id="1524c-115">filter</span></span> | <span data-ttu-id="1524c-116">Objektet</span><span class="sxs-lookup"><span data-stu-id="1524c-116">object</span></span> | <span data-ttu-id="1524c-117">Ett valfritt fält för att filtrera hello typer av händelser.</span><span class="sxs-lookup"><span data-stu-id="1524c-117">An optional field for filtering hello types of events.</span></span> |

### <a name="destination-object"></a><span data-ttu-id="1524c-118">Målobjekt</span><span class="sxs-lookup"><span data-stu-id="1524c-118">destination object</span></span>

| <span data-ttu-id="1524c-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="1524c-119">Property</span></span> | <span data-ttu-id="1524c-120">Typ</span><span class="sxs-lookup"><span data-stu-id="1524c-120">Type</span></span> | <span data-ttu-id="1524c-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1524c-121">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="1524c-122">endpointType</span><span class="sxs-lookup"><span data-stu-id="1524c-122">endpointType</span></span> | <span data-ttu-id="1524c-123">Sträng</span><span class="sxs-lookup"><span data-stu-id="1524c-123">string</span></span> | <span data-ttu-id="1524c-124">hello typ av slutpunkt för hello prenumeration (webhook/HTTP, Event Hub eller kön).</span><span class="sxs-lookup"><span data-stu-id="1524c-124">hello type of endpoint for hello subscription (webhook/HTTP, Event Hub, or queue).</span></span> | 
| <span data-ttu-id="1524c-125">endpointUrl</span><span class="sxs-lookup"><span data-stu-id="1524c-125">endpointUrl</span></span> | <span data-ttu-id="1524c-126">Sträng</span><span class="sxs-lookup"><span data-stu-id="1524c-126">string</span></span> |  | 

### <a name="filter-object"></a><span data-ttu-id="1524c-127">filtreringsobjekt</span><span class="sxs-lookup"><span data-stu-id="1524c-127">filter object</span></span>

| <span data-ttu-id="1524c-128">Egenskap</span><span class="sxs-lookup"><span data-stu-id="1524c-128">Property</span></span> | <span data-ttu-id="1524c-129">Typ</span><span class="sxs-lookup"><span data-stu-id="1524c-129">Type</span></span> | <span data-ttu-id="1524c-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1524c-130">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="1524c-131">includedEventTypes</span><span class="sxs-lookup"><span data-stu-id="1524c-131">includedEventTypes</span></span> | <span data-ttu-id="1524c-132">matris</span><span class="sxs-lookup"><span data-stu-id="1524c-132">array</span></span> | <span data-ttu-id="1524c-133">Matcha när hello händelsetyp i hello händelsemeddelandet är en exakt matchning tooone av dessa namn för typen av händelse.</span><span class="sxs-lookup"><span data-stu-id="1524c-133">Match when hello event type in hello event message is an exact match tooone of these event type names.</span></span> <span data-ttu-id="1524c-134">Genererar ett fel när händelsenamn inte matchar hello registrerade händelse typnamnen för hello händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="1524c-134">Raises an error when event name does not match hello registered event type names for hello event source.</span></span> <span data-ttu-id="1524c-135">Standard matchar alla händelsetyper.</span><span class="sxs-lookup"><span data-stu-id="1524c-135">Default matches all event types.</span></span> |
| <span data-ttu-id="1524c-136">subjectBeginsWith</span><span class="sxs-lookup"><span data-stu-id="1524c-136">subjectBeginsWith</span></span> | <span data-ttu-id="1524c-137">Sträng</span><span class="sxs-lookup"><span data-stu-id="1524c-137">string</span></span> | <span data-ttu-id="1524c-138">Prefix-matchning filter toohello ämnesfältet i hello händelsemeddelandet.</span><span class="sxs-lookup"><span data-stu-id="1524c-138">A prefix-match filter toohello subject field in hello event message.</span></span> <span data-ttu-id="1524c-139">matchar alla hello standard eller en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="1524c-139">hello default or empty string matches all.</span></span> | 
| <span data-ttu-id="1524c-140">subjectEndsWith</span><span class="sxs-lookup"><span data-stu-id="1524c-140">subjectEndsWith</span></span> | <span data-ttu-id="1524c-141">Sträng</span><span class="sxs-lookup"><span data-stu-id="1524c-141">string</span></span> | <span data-ttu-id="1524c-142">Suffix matchar filtret toohello ämnesfältet i hello händelsemeddelandet.</span><span class="sxs-lookup"><span data-stu-id="1524c-142">A suffix-match filter toohello subject field in hello event message.</span></span> <span data-ttu-id="1524c-143">matchar alla hello standard eller en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="1524c-143">hello default or empty string matches all.</span></span> |
| <span data-ttu-id="1524c-144">subjectIsCaseSensitive</span><span class="sxs-lookup"><span data-stu-id="1524c-144">subjectIsCaseSensitive</span></span> | <span data-ttu-id="1524c-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="1524c-145">string</span></span> | <span data-ttu-id="1524c-146">Kontroller skiftlägeskänsliga matchningen för filter.</span><span class="sxs-lookup"><span data-stu-id="1524c-146">Controls case-sensitive matching for filters.</span></span> |


## <a name="example-subscription-schema"></a><span data-ttu-id="1524c-147">Exempel prenumeration schema</span><span class="sxs-lookup"><span data-stu-id="1524c-147">Example subscription schema</span></span>

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "blobCreated", "blobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="1524c-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1524c-148">Next steps</span></span>

* <span data-ttu-id="1524c-149">En introduktion tooEvent rutnätet finns [vad är händelsen rutnätet?](overview.md)</span><span class="sxs-lookup"><span data-stu-id="1524c-149">For an introduction tooEvent Grid, see [What is Event Grid?](overview.md)</span></span>
