---
title: "aaaAzure Behållarinstanser tutorial – förbereda din app | Azure-dokument"
description: "Förbereda en app för distribution tooAzure Behållarinstanser"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 406ba796e5fefb1527f2e894cc3f7bbd8f7a5fd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-for-deployment-tooazure-container-instances"></a><span data-ttu-id="c0d66-103">Skapa en behållare för distribution tooAzure Behållarinstanser</span><span class="sxs-lookup"><span data-stu-id="c0d66-103">Create container for deployment tooAzure Container Instances</span></span>

<span data-ttu-id="c0d66-104">Azure Container Instances möjliggör distribution av Docker-behållare till en Azure-infrastruktur utan att tillhandahålla några virtuella datorer eller anpassa eventuella tjänster på högre nivå.</span><span class="sxs-lookup"><span data-stu-id="c0d66-104">Azure Container Instances enables deployment of Docker containers onto Azure infrastructure without provisioning any virtual machines or adopting any higher-level service.</span></span> <span data-ttu-id="c0d66-105">I den här självstudien får du bygga en enkel webbapp i Node.js och paketera den i en behållare som kan köras med Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="c0d66-105">In this tutorial, you will build a simple web application in Node.js and package it in a container that can be run using Azure Container Instances.</span></span> <span data-ttu-id="c0d66-106">Vi kommer att ta upp:</span><span class="sxs-lookup"><span data-stu-id="c0d66-106">We will cover:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c0d66-107">Klona programkällan från GitHub</span><span class="sxs-lookup"><span data-stu-id="c0d66-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="c0d66-108">Skapa behållaravbildningar från en programkälla</span><span class="sxs-lookup"><span data-stu-id="c0d66-108">Creating container images from application source</span></span>
> * <span data-ttu-id="c0d66-109">Testar hello-avbildningar i en lokal Docker-miljö</span><span class="sxs-lookup"><span data-stu-id="c0d66-109">Testing hello images in a local Docker environment</span></span>

<span data-ttu-id="c0d66-110">I efterföljande självstudiekurser du överför din bild tooan Azure Container registret, och sedan distribuera dem tooAzure Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="c0d66-110">In subsequent tutorials, you will upload your image tooan Azure Container Registry, and then deploy them tooAzure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c0d66-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c0d66-111">Before you begin</span></span>

<span data-ttu-id="c0d66-112">Den här självstudien förutsätter grundläggande kunskaper om grundläggande Docker-begrepp som behållare, behållaravbildningar och grundläggande docker-kommandon.</span><span class="sxs-lookup"><span data-stu-id="c0d66-112">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="c0d66-113">Om det behövs kan du läsa [Get started with Docker]( https://docs.docker.com/get-started/) (Komma igång med Docker) för att få en genomgång av grunden för behållare.</span><span class="sxs-lookup"><span data-stu-id="c0d66-113">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="c0d66-114">toocomplete den här kursen behöver du en Docker-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c0d66-114">toocomplete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="c0d66-115">Docker innehåller paket som enkelt kan konfigurera Docker på en [Mac-](https://docs.docker.com/docker-for-mac/), [Windows-](https://docs.docker.com/docker-for-windows/) eller [Linux-](https://docs.docker.com/engine/installation/#supported-platforms)dator.</span><span class="sxs-lookup"><span data-stu-id="c0d66-115">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="c0d66-116">Hämta programkod</span><span class="sxs-lookup"><span data-stu-id="c0d66-116">Get application code</span></span>

<span data-ttu-id="c0d66-117">hello-exempel i den här självstudiekursen innehåller en enkel webbapp som skapats [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="c0d66-117">hello sample in this tutorial includes a simple web application built in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="c0d66-118">hello appen fungerar statiska HTML-sidan och ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="c0d66-118">hello app serves a static HTML page and looks like this:</span></span>

![Självstudieappen visas i webbläsare][aci-tutorial-app]

<span data-ttu-id="c0d66-120">Använda git toodownload hello exemplet:</span><span class="sxs-lookup"><span data-stu-id="c0d66-120">Use git toodownload hello sample:</span></span>

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-hello-container-image"></a><span data-ttu-id="c0d66-121">Skapa hello behållaren image</span><span class="sxs-lookup"><span data-stu-id="c0d66-121">Build hello container image</span></span>

<span data-ttu-id="c0d66-122">hello Dockerfile i lagringsplatsen för hello exempel visar hur hello behållare skapas.</span><span class="sxs-lookup"><span data-stu-id="c0d66-122">hello Dockerfile provided in hello sample repo shows how hello container is built.</span></span> <span data-ttu-id="c0d66-123">Den startar från en [officiella Node.js bild] [ dockerhub-nodeimage] baserat på [Alpine Linux](https://alpinelinux.org/), en liten distribution som är väl lämpade toouse med behållare.</span><span class="sxs-lookup"><span data-stu-id="c0d66-123">It starts from an [official Node.js image][dockerhub-nodeimage] based on [Alpine Linux](https://alpinelinux.org/), a small distribution that is well suited toouse with containers.</span></span> <span data-ttu-id="c0d66-124">Den sedan kopierar hello programfiler till hello container, installerar beroenden med hello noden Package Manager och slutligen startar hello-program.</span><span class="sxs-lookup"><span data-stu-id="c0d66-124">It then copies hello application files into hello container, installs dependencies using hello Node Package Manager, and finally starts hello application.</span></span>

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

<span data-ttu-id="c0d66-125">Använd hello `docker build` kommandot toocreate hello behållaren bilden, märkning som *aci kursen app*:</span><span class="sxs-lookup"><span data-stu-id="c0d66-125">Use hello `docker build` command toocreate hello container image, tagging it as *aci-tutorial-app*:</span></span>

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

<span data-ttu-id="c0d66-126">Använd hello `docker images` toosee hello inbyggda avbildningen:</span><span class="sxs-lookup"><span data-stu-id="c0d66-126">Use hello `docker images` toosee hello built image:</span></span>

```bash
docker images
```

<span data-ttu-id="c0d66-127">Resultat:</span><span class="sxs-lookup"><span data-stu-id="c0d66-127">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-hello-container-locally"></a><span data-ttu-id="c0d66-128">Kör hello behållaren lokalt</span><span class="sxs-lookup"><span data-stu-id="c0d66-128">Run hello container locally</span></span>

<span data-ttu-id="c0d66-129">Innan du försöker distribuera hello behållaren tooAzure Behållarinstanser köra det lokalt tooconfirm att det fungerar.</span><span class="sxs-lookup"><span data-stu-id="c0d66-129">Before you try deploying hello container tooAzure Container Instances, run it locally tooconfirm that it works.</span></span> <span data-ttu-id="c0d66-130">Hej `-d` växel kan hello-behållare som körs i bakgrunden hello, medan `-p` kan du toomap en valfri port på din beräknings-tooport 80 i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="c0d66-130">hello `-d` switch lets hello container run in hello background, while `-p` allows you toomap an arbitrary port on your compute tooport 80 in hello container.</span></span>

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

<span data-ttu-id="c0d66-131">Öppna hello webbläsare toohttp://localhost:8080 tooconfirm som hello behållare körs.</span><span class="sxs-lookup"><span data-stu-id="c0d66-131">Open hello browser toohttp://localhost:8080 tooconfirm that hello container is running.</span></span>

![Hello-app som körs lokalt i hello webbläsare][aci-tutorial-app-local]

## <a name="next-steps"></a><span data-ttu-id="c0d66-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0d66-133">Next steps</span></span>

<span data-ttu-id="c0d66-134">I kursen får skapat du en avbildning av behållare som kan vara distribuerade tooAzure Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="c0d66-134">In this tutorial, you created a container image that can be deployed tooAzure Container Instances.</span></span> <span data-ttu-id="c0d66-135">hello följande steg har slutförts:</span><span class="sxs-lookup"><span data-stu-id="c0d66-135">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c0d66-136">Kloningen hello programmet källa från GitHub</span><span class="sxs-lookup"><span data-stu-id="c0d66-136">Cloning hello application source from GitHub</span></span>  
> * <span data-ttu-id="c0d66-137">Skapa behållaravbildningar från en programkälla</span><span class="sxs-lookup"><span data-stu-id="c0d66-137">Creating container images from application source</span></span>
> * <span data-ttu-id="c0d66-138">Testa hello behållaren lokalt</span><span class="sxs-lookup"><span data-stu-id="c0d66-138">Testing hello container locally</span></span>

<span data-ttu-id="c0d66-139">Avancera toohello nästa självstudiekurs toolearn om att lagra behållaren bilder i ett Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="c0d66-139">Advance toohello next tutorial toolearn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c0d66-140">Push-avbildningar tooAzure behållare registret</span><span class="sxs-lookup"><span data-stu-id="c0d66-140">Push images tooAzure Container Registry</span></span>](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png