---
title: "aaaAzure Container Service tutorial – förbereda appen | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - Förbered appen"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b537ecc9ff50358fb65b128bfe6eb894dd088cc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-images-toobe-used-with-azure-container-service"></a><span data-ttu-id="459fb-104">Skapa behållaren bilder toobe används med Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="459fb-104">Create container images toobe used with Azure Container Service</span></span>

<span data-ttu-id="459fb-105">I den här självstudiekursen, del 1 av sju, har ett program för flera behållare förberetts för användning i Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="459fb-105">In this tutorial, part one of seven, a multi-container application is prepared for use in Kubernetes.</span></span> <span data-ttu-id="459fb-106">Slutfört stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="459fb-106">Steps completed include:</span></span>  

> [!div class="checklist"]
> * <span data-ttu-id="459fb-107">Klona programkällan från GitHub</span><span class="sxs-lookup"><span data-stu-id="459fb-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="459fb-108">Skapar en behållare avbildning från hello programmet källa</span><span class="sxs-lookup"><span data-stu-id="459fb-108">Creating a container image from hello application source</span></span>
> * <span data-ttu-id="459fb-109">Testa hello program i en lokal Docker-miljö</span><span class="sxs-lookup"><span data-stu-id="459fb-109">Testing hello application in a local Docker environment</span></span>

<span data-ttu-id="459fb-110">När slutförd är hello följande program tillgängligt i din lokala utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="459fb-110">Once completed, hello following application is accessible in your local development environment.</span></span>

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

<span data-ttu-id="459fb-112">I efterföljande självstudiekurser hello behållaren bilden är överförda tooan Azure Container registret, och sedan köras i en Azure värd Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="459fb-112">In subsequent tutorials, hello container image is uploaded tooan Azure Container Registry, and then run in an Azure hosted Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="459fb-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="459fb-113">Before you begin</span></span>

<span data-ttu-id="459fb-114">Den här självstudien förutsätter grundläggande kunskaper om grundläggande Docker-begrepp som behållare, behållaravbildningar och grundläggande docker-kommandon.</span><span class="sxs-lookup"><span data-stu-id="459fb-114">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="459fb-115">Om det behövs kan du läsa [Get started with Docker]( https://docs.docker.com/get-started/) (Komma igång med Docker) för att få en genomgång av grunden för behållare.</span><span class="sxs-lookup"><span data-stu-id="459fb-115">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="459fb-116">toocomplete den här kursen behöver du en Docker-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="459fb-116">toocomplete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="459fb-117">Docker innehåller paket som enkelt kan konfigurera Docker på en [Mac-](https://docs.docker.com/docker-for-mac/), [Windows-](https://docs.docker.com/docker-for-windows/) eller [Linux-](https://docs.docker.com/engine/installation/#supported-platforms)dator.</span><span class="sxs-lookup"><span data-stu-id="459fb-117">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="459fb-118">Hämta programkod</span><span class="sxs-lookup"><span data-stu-id="459fb-118">Get application code</span></span>

<span data-ttu-id="459fb-119">hello exempelprogrammet används i den här kursen är en grundläggande röstning app.</span><span class="sxs-lookup"><span data-stu-id="459fb-119">hello sample application used in this tutorial is a basic voting app.</span></span> <span data-ttu-id="459fb-120">hello program består av en frontend-webbservrar komponent och en serverdel Redis-instans.</span><span class="sxs-lookup"><span data-stu-id="459fb-120">hello application consists of a front-end web component and a back-end Redis instance.</span></span> <span data-ttu-id="459fb-121">hello webbkomponenten paketeras till en bild med anpassade container.</span><span class="sxs-lookup"><span data-stu-id="459fb-121">hello web component is packaged into a custom container image.</span></span> <span data-ttu-id="459fb-122">Hej Redis instans använder en oförändrad bild från Docker-hubben.</span><span class="sxs-lookup"><span data-stu-id="459fb-122">hello Redis instance uses an unmodified image from Docker Hub.</span></span>  

<span data-ttu-id="459fb-123">Använda git toodownload en kopia av hello programmet tooyour utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="459fb-123">Use git toodownload a copy of hello application tooyour development environment.</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="459fb-124">I hello klonade directory hello programmets källkod, en förskapad Docker compose fil- och en Kubernetes manifestfil.</span><span class="sxs-lookup"><span data-stu-id="459fb-124">Inside hello cloned directory is hello application source code, a pre-created Docker compose file, and a Kubernetes manifest file.</span></span> <span data-ttu-id="459fb-125">Dessa filer är används toocreate tillgångar i hela hello självstudiekursen uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="459fb-125">These files are used toocreate assets throughout hello tutorial set.</span></span> 

## <a name="create-container-images"></a><span data-ttu-id="459fb-126">Skapa avbildningar av behållare</span><span class="sxs-lookup"><span data-stu-id="459fb-126">Create container images</span></span>

<span data-ttu-id="459fb-127">[Docker Compose](https://docs.docker.com/compose/) kan vara används tooautomate hello build från behållaren avbildningar och hello distribution av flera behållare program.</span><span class="sxs-lookup"><span data-stu-id="459fb-127">[Docker Compose](https://docs.docker.com/compose/) can be used tooautomate hello build out of container images and hello deployment of multi-container applications.</span></span>

<span data-ttu-id="459fb-128">Kör hello docker-compose.yml fil toocreate hello behållaren bild, hämta hello Redis avbildningen och starta programmet hello.</span><span class="sxs-lookup"><span data-stu-id="459fb-128">Run hello docker-compose.yml file toocreate hello container image, download hello Redis image, and start hello application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up -d
```

<span data-ttu-id="459fb-129">När du är klar, Använd hello [docker bilder](https://docs.docker.com/engine/reference/commandline/images/) kommandot toosee hello skapa avbildningar.</span><span class="sxs-lookup"><span data-stu-id="459fb-129">When completed, use hello [docker images](https://docs.docker.com/engine/reference/commandline/images/) command toosee hello created images.</span></span>

```bash
docker images
```

<span data-ttu-id="459fb-130">Observera att tre bilder har hämtat eller skapat.</span><span class="sxs-lookup"><span data-stu-id="459fb-130">Notice that three images have been downloaded or created.</span></span> <span data-ttu-id="459fb-131">Hej *azure rösten sista* bilden innehåller hello program.</span><span class="sxs-lookup"><span data-stu-id="459fb-131">hello *azure-vote-front* image contains hello application.</span></span> <span data-ttu-id="459fb-132">Det har härletts från hello *nginx flask* bild.</span><span class="sxs-lookup"><span data-stu-id="459fb-132">It was derived from hello *nginx-flask* image.</span></span> <span data-ttu-id="459fb-133">Hej Redis avbildningen hämtades från Docker-hubben.</span><span class="sxs-lookup"><span data-stu-id="459fb-133">hello Redis image was downloaded from Docker Hub.</span></span>

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

<span data-ttu-id="459fb-134">Kör hello [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) kommandot toosee hello behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="459fb-134">Run hello [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) command toosee hello running containers.</span></span>

```bash
docker ps
```

<span data-ttu-id="459fb-135">Resultat:</span><span class="sxs-lookup"><span data-stu-id="459fb-135">Output:</span></span>

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a><span data-ttu-id="459fb-136">Testa programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="459fb-136">Test application locally</span></span>

<span data-ttu-id="459fb-137">Bläddra toohttp://localhost:8080 toosee hello program körs.</span><span class="sxs-lookup"><span data-stu-id="459fb-137">Browse toohttp://localhost:8080 toosee hello running application.</span></span>

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a><span data-ttu-id="459fb-139">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="459fb-139">Clean up resources</span></span>

<span data-ttu-id="459fb-140">Nu när programfunktionen har verifierats, hello behållare som körs stoppas och tas bort.</span><span class="sxs-lookup"><span data-stu-id="459fb-140">Now that application functionality has been validated, hello running containers can be stopped and removed.</span></span> <span data-ttu-id="459fb-141">Ta inte bort hello behållaren bilder.</span><span class="sxs-lookup"><span data-stu-id="459fb-141">Do not delete hello container images.</span></span> <span data-ttu-id="459fb-142">Hej *azure rösten sista* bilden är överförda tooan Azure Container registret instans i nästa kurs i hello.</span><span class="sxs-lookup"><span data-stu-id="459fb-142">hello *azure-vote-front* image is uploaded tooan Azure Container Registry instance in hello next tutorial.</span></span>

<span data-ttu-id="459fb-143">Kör hello följande toostop hello behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="459fb-143">Run hello following toostop hello running containers.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml stop
```

<span data-ttu-id="459fb-144">Ta bort hello stoppats behållare med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="459fb-144">Delete hello stopped containers with hello following command.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml rm
```

<span data-ttu-id="459fb-145">Vid färdigställande har du en avbildning av behållare som innehåller hello Azure rösten program.</span><span class="sxs-lookup"><span data-stu-id="459fb-145">At completion, you have a container image that contains hello Azure Vote application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="459fb-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="459fb-146">Next steps</span></span>

<span data-ttu-id="459fb-147">Ett program har testats och behållare avbildningar som skapats för programmet hello i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="459fb-147">In this tutorial, an application was tested and container images created for hello application.</span></span> <span data-ttu-id="459fb-148">hello följande steg har slutförts:</span><span class="sxs-lookup"><span data-stu-id="459fb-148">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="459fb-149">Kloningen hello programmet källa från GitHub</span><span class="sxs-lookup"><span data-stu-id="459fb-149">Cloning hello application source from GitHub</span></span>  
> * <span data-ttu-id="459fb-150">Skapa en avbildning av behållare från programmet källa</span><span class="sxs-lookup"><span data-stu-id="459fb-150">Created a container image from application source</span></span>
> * <span data-ttu-id="459fb-151">Testad hello program i en lokal Docker-miljö</span><span class="sxs-lookup"><span data-stu-id="459fb-151">Tested hello application in a local Docker environment</span></span>

<span data-ttu-id="459fb-152">Avancera toohello nästa självstudiekurs toolearn om att lagra behållaren bilder i ett Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="459fb-152">Advance toohello next tutorial toolearn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="459fb-153">Push-avbildningar tooAzure behållare registret</span><span class="sxs-lookup"><span data-stu-id="459fb-153">Push images tooAzure Container Registry</span></span>](./container-service-tutorial-kubernetes-prepare-acr.md)
