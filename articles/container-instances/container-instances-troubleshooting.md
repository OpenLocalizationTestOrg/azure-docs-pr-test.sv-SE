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
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a><span data-ttu-id="cea21-103">Felsöka distributionsproblem med Azure Container instanser</span><span class="sxs-lookup"><span data-stu-id="cea21-103">Troubleshoot deployment issues with Azure Container Instances</span></span>

<span data-ttu-id="cea21-104">Den här artikeln visar hur tootroubleshoot problem när du distribuerar behållare tooAzure Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="cea21-104">This article shows how tootroubleshoot issues when deploying containers tooAzure Container Instances.</span></span> <span data-ttu-id="cea21-105">Här beskrivs också några hello vanliga problem kan du stöta på.</span><span class="sxs-lookup"><span data-stu-id="cea21-105">It also describes some of hello common issues you may run into.</span></span>

## <a name="getting-diagnostic-events"></a><span data-ttu-id="cea21-106">Hämtning av diagnostiska händelser</span><span class="sxs-lookup"><span data-stu-id="cea21-106">Getting diagnostic events</span></span>

<span data-ttu-id="cea21-107">tooview loggar från din programkod i en behållare, kan du använda hello [az behållaren loggar](/cli/azure/container#logs) kommando.</span><span class="sxs-lookup"><span data-stu-id="cea21-107">tooview logs from your application code within a container, you can use hello [az container logs](/cli/azure/container#logs) command.</span></span> <span data-ttu-id="cea21-108">Men om din behållaren inte distribuerar har du tooreview hello diagnostisk information som tillhandahålls av hello Azure Behållarinstanser resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="cea21-108">But if your container does not deploy successfully, you need tooreview hello diagnostic information provided by hello Azure Container Instances resource provider.</span></span> <span data-ttu-id="cea21-109">tooview hello händelser för din behållaren, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cea21-109">tooview hello events for your container, run hello following command:</span></span>

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

<span data-ttu-id="cea21-110">hello utdata innehåller hello huvudegenskaper för din behållare, tillsammans med distribution av händelser:</span><span class="sxs-lookup"><span data-stu-id="cea21-110">hello output includes hello core properties of your container, along with deployment events:</span></span>

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

## <a name="common-deployment-issues"></a><span data-ttu-id="cea21-111">Vanliga distributionsproblem med</span><span class="sxs-lookup"><span data-stu-id="cea21-111">Common deployment issues</span></span>

<span data-ttu-id="cea21-112">Det finns några vanliga problem för de flesta fel i distributionen.</span><span class="sxs-lookup"><span data-stu-id="cea21-112">There are a few common issues that account for most errors in deployment.</span></span>

### <a name="unable-toopull-image"></a><span data-ttu-id="cea21-113">Det går inte toopull bild</span><span class="sxs-lookup"><span data-stu-id="cea21-113">Unable toopull image</span></span>

<span data-ttu-id="cea21-114">Om Azure-Behållarinstanser är toopull avbildningen först ett nytt försök under en period innan förr eller senare.</span><span class="sxs-lookup"><span data-stu-id="cea21-114">If Azure Container Instances is unable toopull your image initially, it retries for some period before eventually failing.</span></span> <span data-ttu-id="cea21-115">Om du inte kan hämtas hello avbildningen, visas händelser, t.ex. hello följande:</span><span class="sxs-lookup"><span data-stu-id="cea21-115">If hello image cannot be pulled, events like hello following are shown:</span></span>

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

<span data-ttu-id="cea21-116">tooresolve, ta bort hello-behållaren och försök distributionen betalande uppmärksam på att du har angett rätt hello avbildningsnamn.</span><span class="sxs-lookup"><span data-stu-id="cea21-116">tooresolve, delete hello container and retry your deployment, paying close attention that you have typed hello image name correctly.</span></span>

### <a name="container-continually-exits-and-restarts"></a><span data-ttu-id="cea21-117">Behållaren kontinuerligt avslutas och startas om</span><span class="sxs-lookup"><span data-stu-id="cea21-117">Container continually exits and restarts</span></span>

<span data-ttu-id="cea21-118">För närvarande stöder endast Behållarinstanser som Azure tidskrävande tjänster.</span><span class="sxs-lookup"><span data-stu-id="cea21-118">Currently, Azure Container Instances only supports long-running services.</span></span> <span data-ttu-id="cea21-119">Om din behållare körs toocompletion och avslutar den automatiskt startar om och körs igen.</span><span class="sxs-lookup"><span data-stu-id="cea21-119">If your container runs toocompletion and exits, it automatically restarts and runs again.</span></span> <span data-ttu-id="cea21-120">Om det händer visas händelser som de som följer.</span><span class="sxs-lookup"><span data-stu-id="cea21-120">If this happens, events like those following are shown.</span></span> <span data-ttu-id="cea21-121">Observera hello behållaren startar och sedan startar om snabbt.</span><span class="sxs-lookup"><span data-stu-id="cea21-121">Note that hello container successfully starts, then quickly restarts.</span></span> <span data-ttu-id="cea21-122">hello behållaren instanser API innehåller en `retryCount` egenskap som visar hur många gånger en viss behållare har startats om.</span><span class="sxs-lookup"><span data-stu-id="cea21-122">hello Container Instances API includes a `retryCount` property that shows how many times a particular container has restarted.</span></span>

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
> <span data-ttu-id="cea21-123">De flesta behållaren avbildningar för Linux-distributioner kan du ange ett gränssnitt, till exempel bash, som hello Standardkommandot.</span><span class="sxs-lookup"><span data-stu-id="cea21-123">Most container images for Linux distributions set a shell, such as bash, as hello default command.</span></span> <span data-ttu-id="cea21-124">Eftersom ett gränssnitt på sin egen inte är en tidskrävande tjänst avsluta omedelbart i dessa behållare och faller inom en omstart loop.</span><span class="sxs-lookup"><span data-stu-id="cea21-124">Since a shell on its own is not a long-running service, these containers immediately exit and fall into a restart loop.</span></span>

### <a name="container-takes-a-long-time-toostart"></a><span data-ttu-id="cea21-125">Behållaren tar en lång tid toostart</span><span class="sxs-lookup"><span data-stu-id="cea21-125">Container takes a long time toostart</span></span>

<span data-ttu-id="cea21-126">Om din behållaren tar en lång tid toostart, men till slut lyckas, starta genom att titta på hello storleken på behållaren avbildningen.</span><span class="sxs-lookup"><span data-stu-id="cea21-126">If your container takes a long time toostart, but eventually succeeds, start by looking at hello size of your container image.</span></span> <span data-ttu-id="cea21-127">Eftersom Azure Behållarinstanser hämtar avbildningen behållare på begäran, hello starttiden uppstår är direkt relaterade tooits storlek.</span><span class="sxs-lookup"><span data-stu-id="cea21-127">Because Azure Container Instances pulls your container image on demand, hello startup time you experience is directly related tooits size.</span></span>

<span data-ttu-id="cea21-128">Du kan visa hello storleken på behållaren avbildningen med hjälp av hello Docker CLI:</span><span class="sxs-lookup"><span data-stu-id="cea21-128">You can view hello size of your container image using hello Docker CLI:</span></span>

```bash
docker images
```

<span data-ttu-id="cea21-129">Resultat:</span><span class="sxs-lookup"><span data-stu-id="cea21-129">Output:</span></span>

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

<span data-ttu-id="cea21-130">hello viktiga tookeeping bildstorleken små är att säkerställa att dina slutliga avbildningen inte innehåller allt som inte krävs vid körning.</span><span class="sxs-lookup"><span data-stu-id="cea21-130">hello key tookeeping image sizes small is ensuring that your final image does not contain anything that is not required at runtime.</span></span> <span data-ttu-id="cea21-131">Enkelriktade toodo detta är med [flera steg versioner](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="cea21-131">One way toodo this is with [multi-stage builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span></span> <span data-ttu-id="cea21-132">Versioner av flera steg gör det enkelt tooensure att hello slutliga avbildningen innehåller endast hello artefakter som du behöver för ditt program och inte något av hello extra innehåll som krävdes vid byggning.</span><span class="sxs-lookup"><span data-stu-id="cea21-132">Multi-stage builds make it easy tooensure that hello final image contains only hello artifacts you need for your application, and not any of hello extra content that was required at build time.</span></span>

<span data-ttu-id="cea21-133">hello andra sätt tooreduce hello effekten av hello avbildningen pull på din behållaren starttiden är toohost hello behållaren avbildning med hjälp av hello Azure Container registret i hello samma region där du ska toouse Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="cea21-133">hello other way tooreduce hello impact of hello image pull on your container's startup time is toohost hello container image using hello Azure Container Registry in hello same region where you intend toouse Azure Container Instances.</span></span> <span data-ttu-id="cea21-134">Detta förkortar hello nätverkssökväg som hello behållaren avbildningen måste tootravel avsevärt förkortar hello hämtningstiden.</span><span class="sxs-lookup"><span data-stu-id="cea21-134">This shortens hello network path that hello container image needs tootravel, significantly shortening hello download time.</span></span>
