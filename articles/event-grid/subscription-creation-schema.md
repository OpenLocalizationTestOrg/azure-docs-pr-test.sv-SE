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
# <a name="event-grid-subscription-schema"></a>Händelseschema rutnätet prenumeration

toocreate en händelse rutnätet prenumeration du skicka en begäran toohello Skapa händelse prenumerationen igen. Använd hello följande format:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Till exempel toocreate en händelseprenumerationen för ett lagringskonto med namnet `examplestorage` i en resursgrupp med namnet `examplegroup`, Använd hello följande format:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

hello beskrivs hello egenskaper och schemat för hello hello-begäran.
 
## <a name="event-subscription-properties"></a>Händelseegenskaper för prenumeration

| Egenskap | Typ | Beskrivning |
| -------- | ---- | ----------- |
| Mål | Objektet | hello-objekt som definierar hello slutpunkt. |
| Filter | Objektet | Ett valfritt fält för att filtrera hello typer av händelser. |

### <a name="destination-object"></a>Målobjekt

| Egenskap | Typ | Beskrivning |
| -------- | ---- | ----------- |
| endpointType | Sträng | hello typ av slutpunkt för hello prenumeration (webhook/HTTP, Event Hub eller kön). | 
| endpointUrl | Sträng |  | 

### <a name="filter-object"></a>filtreringsobjekt

| Egenskap | Typ | Beskrivning |
| -------- | ---- | ----------- |
| includedEventTypes | matris | Matcha när hello händelsetyp i hello händelsemeddelandet är en exakt matchning tooone av dessa namn för typen av händelse. Genererar ett fel när händelsenamn inte matchar hello registrerade händelse typnamnen för hello händelsekälla. Standard matchar alla händelsetyper. |
| subjectBeginsWith | Sträng | Prefix-matchning filter toohello ämnesfältet i hello händelsemeddelandet. matchar alla hello standard eller en tom sträng. | 
| subjectEndsWith | Sträng | Suffix matchar filtret toohello ämnesfältet i hello händelsemeddelandet. matchar alla hello standard eller en tom sträng. |
| subjectIsCaseSensitive | Sträng | Kontroller skiftlägeskänsliga matchningen för filter. |


## <a name="example-subscription-schema"></a>Exempel prenumeration schema

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

## <a name="next-steps"></a>Nästa steg

* En introduktion tooEvent rutnätet finns [vad är händelsen rutnätet?](overview.md)
