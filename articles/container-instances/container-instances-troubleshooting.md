---
title: "Felsöka Azure-Behållarinstanser"
description: "Lär dig hur du felsöker problem med Azure Container instanser"
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
ms.openlocfilehash: 86fa4b7dca7c362f95c0243a33f03d1f2dd3ab42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a><span data-ttu-id="d141e-103">Felsöka distributionsproblem med Azure Container instanser</span><span class="sxs-lookup"><span data-stu-id="d141e-103">Troubleshoot deployment issues with Azure Container Instances</span></span>

<span data-ttu-id="d141e-104">Den här artikeln visar hur du felsöker problem när du distribuerar behållare till Azure-Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="d141e-104">This article shows how to troubleshoot issues when deploying containers to Azure Container Instances.</span></span> <span data-ttu-id="d141e-105">Här beskrivs också några vanliga problem kan du stöta på.</span><span class="sxs-lookup"><span data-stu-id="d141e-105">It also describes some of the common issues you may run into.</span></span>

## <a name="getting-diagnostic-events"></a><span data-ttu-id="d141e-106">Hämtning av diagnostiska händelser</span><span class="sxs-lookup"><span data-stu-id="d141e-106">Getting diagnostic events</span></span>

<span data-ttu-id="d141e-107">Om du vill visa loggar från din programkod i en behållare som du kan använda den [az behållaren loggar](/cli/azure/container#logs) kommando.</span><span class="sxs-lookup"><span data-stu-id="d141e-107">To view logs from your application code within a container, you can use the [az container logs](/cli/azure/container#logs) command.</span></span> <span data-ttu-id="d141e-108">Men om din behållaren inte distribuerar har du behöver diagnostisk information som tillhandahålls av Azure Behållarinstanser resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="d141e-108">But if your container does not deploy successfully, you need to review the diagnostic information provided by the Azure Container Instances resource provider.</span></span> <span data-ttu-id="d141e-109">Om du vill visa händelser för din behållare, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d141e-109">To view the events for your container, run the following command:</span></span>

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

<span data-ttu-id="d141e-110">Utdata innehåller huvudegenskaper för din behållare, tillsammans med distribution av händelser:</span><span class="sxs-lookup"><span data-stu-id="d141e-110">The output includes the core properties of your container, along with deployment events:</span></span>

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

## <a name="common-deployment-issues"></a><span data-ttu-id="d141e-111">Vanliga distributionsproblem med</span><span class="sxs-lookup"><span data-stu-id="d141e-111">Common deployment issues</span></span>

<span data-ttu-id="d141e-112">Det finns några vanliga problem för de flesta fel i distributionen.</span><span class="sxs-lookup"><span data-stu-id="d141e-112">There are a few common issues that account for most errors in deployment.</span></span>

### <a name="unable-to-pull-image"></a><span data-ttu-id="d141e-113">Det gick inte att pull-bild</span><span class="sxs-lookup"><span data-stu-id="d141e-113">Unable to pull image</span></span>

<span data-ttu-id="d141e-114">Om Azure Behållarinstanser inte att hämta bilden från början, ett nytt försök under en period innan förr eller senare.</span><span class="sxs-lookup"><span data-stu-id="d141e-114">If Azure Container Instances is unable to pull your image initially, it retries for some period before eventually failing.</span></span> <span data-ttu-id="d141e-115">Om bilden inte kan hämtas, visas händelser som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="d141e-115">If the image cannot be pulled, events like the following are shown:</span></span>

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
    "message": "Failed: Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
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

<span data-ttu-id="d141e-116">Lös, ta bort behållaren och försök distributionen betalande uppmärksam på att du har angett rätt avbildning.</span><span class="sxs-lookup"><span data-stu-id="d141e-116">To resolve, delete the container and retry your deployment, paying close attention that you have typed the image name correctly.</span></span>

### <a name="container-continually-exits-and-restarts"></a><span data-ttu-id="d141e-117">Behållaren kontinuerligt avslutas och startas om</span><span class="sxs-lookup"><span data-stu-id="d141e-117">Container continually exits and restarts</span></span>

<span data-ttu-id="d141e-118">För närvarande stöder endast Behållarinstanser som Azure tidskrävande tjänster.</span><span class="sxs-lookup"><span data-stu-id="d141e-118">Currently, Azure Container Instances only supports long-running services.</span></span> <span data-ttu-id="d141e-119">Om din behållare körs slutförande och avslutar den automatiskt startar och körs igen.</span><span class="sxs-lookup"><span data-stu-id="d141e-119">If your container runs to completion and exits, it automatically restarts and runs again.</span></span> <span data-ttu-id="d141e-120">Om det händer visas händelser som de som följer.</span><span class="sxs-lookup"><span data-stu-id="d141e-120">If this happens, events like those following are shown.</span></span> <span data-ttu-id="d141e-121">Observera att behållaren startar och sedan startar om snabbt.</span><span class="sxs-lookup"><span data-stu-id="d141e-121">Note that the container successfully starts, then quickly restarts.</span></span> <span data-ttu-id="d141e-122">Behållaren instanser API innehåller en `retryCount` egenskap som visar hur många gånger en viss behållare har startats om.</span><span class="sxs-lookup"><span data-stu-id="d141e-122">The Container Instances API includes a `retryCount` property that shows how many times a particular container has restarted.</span></span>

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
> <span data-ttu-id="d141e-123">De flesta behållaren avbildningar för Linux-distributioner kan du ange ett gränssnitt, till exempel bash, som kommandot.</span><span class="sxs-lookup"><span data-stu-id="d141e-123">Most container images for Linux distributions set a shell, such as bash, as the default command.</span></span> <span data-ttu-id="d141e-124">Eftersom ett gränssnitt på sin egen inte är en tidskrävande tjänst avsluta omedelbart i dessa behållare och faller inom en omstart loop.</span><span class="sxs-lookup"><span data-stu-id="d141e-124">Since a shell on its own is not a long-running service, these containers immediately exit and fall into a restart loop.</span></span>

### <a name="container-takes-a-long-time-to-start"></a><span data-ttu-id="d141e-125">Behållaren tar lång tid att starta</span><span class="sxs-lookup"><span data-stu-id="d141e-125">Container takes a long time to start</span></span>

<span data-ttu-id="d141e-126">Om din behållaren tar lång tid att starta, men till slut lyckas, starta genom att titta på storleken på behållaren avbildningen.</span><span class="sxs-lookup"><span data-stu-id="d141e-126">If your container takes a long time to start, but eventually succeeds, start by looking at the size of your container image.</span></span> <span data-ttu-id="d141e-127">Eftersom Azure Behållarinstanser hämtar avbildningen behållare på begäran, relaterat starttiden uppstår direkt till dess storlek.</span><span class="sxs-lookup"><span data-stu-id="d141e-127">Because Azure Container Instances pulls your container image on demand, the startup time you experience is directly related to its size.</span></span>

<span data-ttu-id="d141e-128">Du kan visa storleken på avbildningen behållare med Docker CLI:</span><span class="sxs-lookup"><span data-stu-id="d141e-128">You can view the size of your container image using the Docker CLI:</span></span>

```bash
docker images
```

<span data-ttu-id="d141e-129">Resultat:</span><span class="sxs-lookup"><span data-stu-id="d141e-129">Output:</span></span>

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

<span data-ttu-id="d141e-130">Nyckeln till att hålla bildstorleken små är att säkerställa att dina slutliga avbildningen inte innehåller allt som inte krävs vid körning.</span><span class="sxs-lookup"><span data-stu-id="d141e-130">The key to keeping image sizes small is ensuring that your final image does not contain anything that is not required at runtime.</span></span> <span data-ttu-id="d141e-131">Det här är ett sätt att göra med [flera steg versioner](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="d141e-131">One way to do this is with [multi-stage builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span></span> <span data-ttu-id="d141e-132">Flera steg skapar gör det enkelt att se till att den slutliga avbildningen innehåller de artefakter som du behöver för ditt program och inte något av extra innehåll som krävdes vid byggning.</span><span class="sxs-lookup"><span data-stu-id="d141e-132">Multi-stage builds make it easy to ensure that the final image contains only the artifacts you need for your application, and not any of the extra content that was required at build time.</span></span>

<span data-ttu-id="d141e-133">Ett annat sätt att minska effekten av avbildningen pull på din behållaren starttiden är värd för behållaren avbildningen med Azure Container registret i samma region som du tänker använda Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="d141e-133">The other way to reduce the impact of the image pull on your container's startup time is to host the container image using the Azure Container Registry in the same region where you intend to use Azure Container Instances.</span></span> <span data-ttu-id="d141e-134">Detta förkortar nätverkssökvägen behållaren avbildningen måste reser kan avsevärt minska hämtningstiden.</span><span class="sxs-lookup"><span data-stu-id="d141e-134">This shortens the network path that the container image needs to travel, significantly shortening the download time.</span></span>