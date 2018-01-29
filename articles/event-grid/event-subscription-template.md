---
title: "Azure rutnätet händelseprenumerationen med mallen"
description: "Skapa ett rutnät händelseprenumerationen med Resource Manager-mall."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2b9f55f8e944d688b622e30a773e1a34698f22ec
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/05/2018
---
# <a name="use-resource-manager-template-for-event-grid-subscription"></a>Använd Resource Manager-mall för händelsen rutnätet prenumeration

Den här artikeln visar hur du använder en Azure Resource Manager-mall för att skapa händelsen rutnätet prenumerationer. Det format du använder skiljer sig åt beroende på om du prenumerera på resursen gruppera händelser eller händelser för en viss resurstyp. Båda formaten visas i den här artikeln.

## <a name="subscribe-to-resource-group-events"></a>Prenumerera på resursen gruppera händelser

När du prenumererar på resursen gruppera händelser, Använd `Microsoft.EventGrid/eventSubscriptions` för resurstypen. Peka typen för händelsen kan använda antingen `WebHook` eller `EventHub`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.EventGrid/eventSubscriptions",
            "name": "mySubscription",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false,
                    "includedEventTypes": ["All"]
                }
            }
        }
    ]
}
```

När du distribuerar den här mallen till en resursgrupp kan prenumerera du på händelser för resursgruppen.

## <a name="subscribe-to-resource-events"></a>Prenumerera på resursen händelser

När du prenumererar på resursen händelser kan du associera prenumerationen på rätt resurs genom att inkludera resurstyp och namn i Prenumerationsdefinitionen. Resurstyp, Använd `<provider-namespace>/<resource-type>/providers/eventSubscriptions`. Namn, Använd `<resource-name>/Microsoft.EventGrid/<subscription-name>`. Peka typen för händelsen kan använda antingen `WebHook` eller `EventHub`.

I följande exempel visas hur du prenumererar på Blob storage-händelser.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
            "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/myStorageSubscription')]",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Nästa steg

* En introduktion till händelse rutnätet finns [om händelsen rutnätet](overview.md).
* En introduktion till Resource Manager finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
* Om du vill komma igång med händelsen rutnätet, se [skapa och flöde anpassade händelser med Azure händelse rutnätet](custom-event-quickstart.md).