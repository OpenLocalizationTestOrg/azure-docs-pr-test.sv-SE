---
title: "Självstudiekurs om Azure Container Instances – förbereda din app | Azure Docs"
description: "Förbereda en app för distribution till Azure Container Instances"
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
ms.openlocfilehash: 167297e10eed11833623ff797e676ad43c65f9ad
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a><span data-ttu-id="21a40-103">Skapa behållare för distribution till Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="21a40-103">Create container for deployment to Azure Container Instances</span></span>

<span data-ttu-id="21a40-104">Azure Container Instances möjliggör distribution av Docker-behållare till en Azure-infrastruktur utan att tillhandahålla några virtuella datorer eller anpassa eventuella tjänster på högre nivå.</span><span class="sxs-lookup"><span data-stu-id="21a40-104">Azure Container Instances enables deployment of Docker containers onto Azure infrastructure without provisioning any virtual machines or adopting any higher-level service.</span></span> <span data-ttu-id="21a40-105">I den här självstudien får du bygga en enkel webbapp i Node.js och paketera den i en behållare som kan köras med Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="21a40-105">In this tutorial, you will build a simple web application in Node.js and package it in a container that can be run using Azure Container Instances.</span></span> <span data-ttu-id="21a40-106">Vi kommer att ta upp:</span><span class="sxs-lookup"><span data-stu-id="21a40-106">We will cover:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21a40-107">Klona programkällan från GitHub</span><span class="sxs-lookup"><span data-stu-id="21a40-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="21a40-108">Skapa behållaravbildningar från en programkälla</span><span class="sxs-lookup"><span data-stu-id="21a40-108">Creating container images from application source</span></span>
> * <span data-ttu-id="21a40-109">Testa avbildningarna i en lokal Docker-miljö</span><span class="sxs-lookup"><span data-stu-id="21a40-109">Testing the images in a local Docker environment</span></span>

<span data-ttu-id="21a40-110">I efterföljande självstudier får du ladda upp din avbildning till ett Azure Container Registry och sedan distribuera den till Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="21a40-110">In subsequent tutorials, you will upload your image to an Azure Container Registry, and then deploy them to Azure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="21a40-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="21a40-111">Before you begin</span></span>

<span data-ttu-id="21a40-112">Den här självstudien förutsätter grundläggande kunskaper om grundläggande Docker-begrepp som behållare, behållaravbildningar och grundläggande docker-kommandon.</span><span class="sxs-lookup"><span data-stu-id="21a40-112">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="21a40-113">Om det behövs kan du läsa [Get started with Docker]( https://docs.docker.com/get-started/) (Komma igång med Docker) för att få en genomgång av grunden för behållare.</span><span class="sxs-lookup"><span data-stu-id="21a40-113">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="21a40-114">För att slutföra den här självstudien behöver du en Docker-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="21a40-114">To complete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="21a40-115">Docker innehåller paket som enkelt kan konfigurera Docker på en [Mac-](https://docs.docker.com/docker-for-mac/), [Windows-](https://docs.docker.com/docker-for-windows/) eller [Linux-](https://docs.docker.com/engine/installation/#supported-platforms)dator.</span><span class="sxs-lookup"><span data-stu-id="21a40-115">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="21a40-116">Hämta programkod</span><span class="sxs-lookup"><span data-stu-id="21a40-116">Get application code</span></span>

<span data-ttu-id="21a40-117">Exemplet i den här självstudien innehåller en enkel webbapp som skapats i [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="21a40-117">The sample in this tutorial includes a simple web application built in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="21a40-118">Appen har en statisk HTML-sida och ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="21a40-118">The app serves a static HTML page and looks like this:</span></span>

![Självstudieappen visas i webbläsare][aci-tutorial-app]

<span data-ttu-id="21a40-120">Ladda ned exempelfilerna med Git:</span><span class="sxs-lookup"><span data-stu-id="21a40-120">Use git to download the sample:</span></span>

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a><span data-ttu-id="21a40-121">Bygga behållaravbildningen</span><span class="sxs-lookup"><span data-stu-id="21a40-121">Build the container image</span></span>

<span data-ttu-id="21a40-122">Den Dockerfile som finns i exempelrepon visar hur behållaren är byggd.</span><span class="sxs-lookup"><span data-stu-id="21a40-122">The Dockerfile provided in the sample repo shows how the container is built.</span></span> <span data-ttu-id="21a40-123">Den börjar från en [officiell Node.js-avbildning][dockerhub-nodeimage] baserat på [Alpine Linux](https://alpinelinux.org/), en liten distribution som är lämplig för användning med behållare.</span><span class="sxs-lookup"><span data-stu-id="21a40-123">It starts from an [official Node.js image][dockerhub-nodeimage] based on [Alpine Linux](https://alpinelinux.org/), a small distribution that is well suited to use with containers.</span></span> <span data-ttu-id="21a40-124">Den kopierar sedan programfilerna till behållaren, installerar beroenden med Node Package Manager (nodpaketshanteraren) och startar slutligen programmet.</span><span class="sxs-lookup"><span data-stu-id="21a40-124">It then copies the application files into the container, installs dependencies using the Node Package Manager, and finally starts the application.</span></span>

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

<span data-ttu-id="21a40-125">Använd kommandot `docker build` för att skapa behållaravbildningen, och märk den med *aci-tutorial-app*:</span><span class="sxs-lookup"><span data-stu-id="21a40-125">Use the `docker build` command to create the container image, tagging it as *aci-tutorial-app*:</span></span>

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

<span data-ttu-id="21a40-126">Använd `docker images` för att se den skapade avbildningen:</span><span class="sxs-lookup"><span data-stu-id="21a40-126">Use the `docker images` to see the built image:</span></span>

```bash
docker images
```

<span data-ttu-id="21a40-127">Resultat:</span><span class="sxs-lookup"><span data-stu-id="21a40-127">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a><span data-ttu-id="21a40-128">Kör behållaren lokalt</span><span class="sxs-lookup"><span data-stu-id="21a40-128">Run the container locally</span></span>

<span data-ttu-id="21a40-129">Innan du försöker distribuera behållaren till Azure Container Instances ska du köra den lokalt för att bekräfta att den fungerar.</span><span class="sxs-lookup"><span data-stu-id="21a40-129">Before you try deploying the container to Azure Container Instances, run it locally to confirm that it works.</span></span> <span data-ttu-id="21a40-130">Med växeln `-d` kan behållaren köras i bakgrunden, medan `-p` gör att du kan mappa en godtycklig port för dina beräkningar till port 80 i behållaren.</span><span class="sxs-lookup"><span data-stu-id="21a40-130">The `-d` switch lets the container run in the background, while `-p` allows you to map an arbitrary port on your compute to port 80 in the container.</span></span>

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

<span data-ttu-id="21a40-131">Öppna webbläsaren till http://localhost:8080 för att kontrollera att behållaren körs.</span><span class="sxs-lookup"><span data-stu-id="21a40-131">Open the browser to http://localhost:8080 to confirm that the container is running.</span></span>

![Köra appen lokalt i webbläsaren][aci-tutorial-app-local]

## <a name="next-steps"></a><span data-ttu-id="21a40-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="21a40-133">Next steps</span></span>

<span data-ttu-id="21a40-134">I den här självstudien har du skapat en behållaravbildning som kan distribueras till Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="21a40-134">In this tutorial, you created a container image that can be deployed to Azure Container Instances.</span></span> <span data-ttu-id="21a40-135">Följande steg har slutförts:</span><span class="sxs-lookup"><span data-stu-id="21a40-135">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21a40-136">Klona programkällan från GitHub</span><span class="sxs-lookup"><span data-stu-id="21a40-136">Cloning the application source from GitHub</span></span>  
> * <span data-ttu-id="21a40-137">Skapa behållaravbildningar från en programkälla</span><span class="sxs-lookup"><span data-stu-id="21a40-137">Creating container images from application source</span></span>
> * <span data-ttu-id="21a40-138">Testa behållaren lokalt</span><span class="sxs-lookup"><span data-stu-id="21a40-138">Testing the container locally</span></span>

<span data-ttu-id="21a40-139">Fortsätt till nästa självstudie och lär dig om att lagra behållaravbildningar i ett Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="21a40-139">Advance to the next tutorial to learn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="21a40-140">Push-avbildningar med Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="21a40-140">Push images to Azure Container Registry</span></span>](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png