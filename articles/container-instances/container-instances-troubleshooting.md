---
title: "aaaTroubleshooting Behållarinstanser som Azure"
description: "Lär dig hur tootroubleshoot problem med Azure Container instanser"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/03/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: dfec636a0a174c74a6f2e9d9c4da6e871f8d2fda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Felsöka distributionsproblem med Azure Container instanser

Den här artikeln visar hur tootroubleshoot problem när du distribuerar behållare tooAzure Behållarinstanser. Här beskrivs också några hello vanliga problem kan du stöta på.

## <a name="getting-diagnostic-events"></a>Hämtning av diagnostiska händelser

tooview loggar från din programkod i en behållare, kan du använda hello [az behållaren loggar](/cli/azure/container#logs) kommando. Men om din behållaren inte distribuerar har du tooreview hello diagnostisk information som tillhandahålls av hello Azure Behållarinstanser resursprovidern. tooview hello händelser för din behållaren, kör hello följande kommando:

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

hello utdata innehåller hello huvudegenskaper för din behållare, tillsammans med distribution av händelser:

```bash
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...

      "events": [
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a>Vanliga distributionsproblem med

Det finns några vanliga problem för de flesta fel i distributionen.

### <a name="unable-toopull-image"></a>Det går inte toopull bild

Om Azure-Behållarinstanser är toopull avbildningen först ett nytt försök under en period innan förr eller senare. Om du inte kan hämtas hello avbildningen, visas händelser, t.ex. hello följande:

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed toopull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

tooresolve, ta bort hello-behållaren och försök distributionen betalande uppmärksam på att du har angett rätt hello avbildningsnamn.

### <a name="container-continually-exits-and-restarts"></a>Behållaren kontinuerligt avslutas och startas om

För närvarande stöder endast Behållarinstanser som Azure tidskrävande tjänster. Om din behållare körs toocompletion och avslutar den automatiskt startar om och körs igen. Om det händer visas händelser som de som följer. Observera hello behållaren startar och sedan startar om snabbt. hello behållaren instanser API innehåller en `retryCount` egenskap som visar hur många gånger en viss behållare har startats om.

```bash
"events": [
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:55+00:00",
    "lastTimestamp": "2017-08-03T22:23:22+00:00",
    "message": "Pulling: pulling image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:23:23+00:00",
    "message": "Pulled: Successfully pulled image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Created: Created container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Started: Started container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Created: Created container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Started: Started container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 13,
    "firstTimestamp": "2017-08-03T22:21:59+00:00",
    "lastTimestamp": "2017-08-03T22:24:36+00:00",
    "message": "BackOff: Back-off restarting failed container",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:22:13+00:00",
    "lastTimestamp": "2017-08-03T22:22:13+00:00",
    "message": "Created: Created container with id 72e347e891290e238135e4a6b3078748ca25a1275dbbff30d8d214f026d89220",
    "type": "Normal"
  },
  ...
```

> [!NOTE]
> De flesta behållaren avbildningar för Linux-distributioner kan du ange ett gränssnitt, till exempel bash, som hello Standardkommandot. Eftersom ett gränssnitt på sin egen inte är en tidskrävande tjänst avsluta omedelbart i dessa behållare och faller inom en omstart loop.

### <a name="container-takes-a-long-time-toostart"></a>Behållaren tar en lång tid toostart

Om din behållaren tar en lång tid toostart, men till slut lyckas, starta genom att titta på hello storleken på behållaren avbildningen. Eftersom Azure Behållarinstanser hämtar avbildningen behållare på begäran, hello starttiden uppstår är direkt relaterade tooits storlek.

Du kan visa hello storleken på behållaren avbildningen med hjälp av hello Docker CLI:

```bash
docker images
```

Resultat:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

hello viktiga tookeeping bildstorleken små är att säkerställa att dina slutliga avbildningen inte innehåller allt som inte krävs vid körning. Enkelriktade toodo detta är med [flera steg versioner](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Versioner av flera steg gör det enkelt tooensure att hello slutliga avbildningen innehåller endast hello artefakter som du behöver för ditt program och inte något av hello extra innehåll som krävdes vid byggning.

hello andra sätt tooreduce hello effekten av hello avbildningen pull på din behållaren starttiden är toohost hello behållaren avbildning med hjälp av hello Azure Container registret i hello samma region där du ska toouse Azure Container instanser. Detta förkortar hello nätverkssökväg som hello behållaren avbildningen måste tootravel avsevärt förkortar hello hämtningstiden.
