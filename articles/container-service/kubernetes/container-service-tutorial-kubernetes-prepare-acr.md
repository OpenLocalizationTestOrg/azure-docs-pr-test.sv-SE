---
title: "aaaAzure Container Service tutorial – förbereda ACR | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - förbereda ACR"
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
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3980e5ce4eb9836f83c761a2f76c944bb3f13060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="2d720-104">Distribuera och använda Azure Container registret</span><span class="sxs-lookup"><span data-stu-id="2d720-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="2d720-105">Azure Container registret (ACR) är en Azure-baserade, privat registret för Docker behållare bilder.</span><span class="sxs-lookup"><span data-stu-id="2d720-105">Azure Container Registry (ACR) is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="2d720-106">Den här självstudiekursen, en del två av sju, går igenom distribuera en Azure-behållare registret-instans och trycka på en behållare avbildningen tooit.</span><span class="sxs-lookup"><span data-stu-id="2d720-106">This tutorial, part two of seven, walks through deploying an Azure Container Registry instance, and pushing a container image tooit.</span></span> <span data-ttu-id="2d720-107">Slutfört stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="2d720-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2d720-108">Distribuera en Azure Container registret (ACR)-instans</span><span class="sxs-lookup"><span data-stu-id="2d720-108">Deploying an Azure Container Registry (ACR) instance</span></span>
> * <span data-ttu-id="2d720-109">En avbildning av behållare för ACR-märkning</span><span class="sxs-lookup"><span data-stu-id="2d720-109">Tagging a container image for ACR</span></span>
> * <span data-ttu-id="2d720-110">Överför hello bilden tooACR</span><span class="sxs-lookup"><span data-stu-id="2d720-110">Uploading hello image tooACR</span></span>

<span data-ttu-id="2d720-111">Den här ACR-instansen är integrerad med ett Azure Container Service Kubernetes-kluster för säker körning av behållare bilder i efterföljande självstudiekurser.</span><span class="sxs-lookup"><span data-stu-id="2d720-111">In subsequent tutorials, this ACR instance is integrated with an Azure Container Service Kubernetes cluster, for securely running container images.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="2d720-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2d720-112">Before you begin</span></span>

<span data-ttu-id="2d720-113">I hello [tidigare kursen](./container-service-tutorial-kubernetes-prepare-app.md), en behållare avbildning har skapats för ett enkelt Azure röstning program.</span><span class="sxs-lookup"><span data-stu-id="2d720-113">In hello [previous tutorial](./container-service-tutorial-kubernetes-prepare-app.md), a container image was created for a simple Azure Voting application.</span></span> <span data-ttu-id="2d720-114">I den här kursen är den här avbildningen pushas tooan Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="2d720-114">In this tutorial, this image is pushed tooan Azure Container Registry.</span></span> <span data-ttu-id="2d720-115">Om du inte har skapat hello Azure röstning appavbildning returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="2d720-115">If you have not created hello Azure Voting app image, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> <span data-ttu-id="2d720-116">Du kan också detaljerad hello steg här fungerar med alla behållare avbildningen.</span><span class="sxs-lookup"><span data-stu-id="2d720-116">Alternatively, hello steps detailed here work with any container image.</span></span>

<span data-ttu-id="2d720-117">Den här kursen kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2d720-117">This tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="2d720-118">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="2d720-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="2d720-119">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2d720-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="2d720-120">Distribuera Azure-behållaren registret</span><span class="sxs-lookup"><span data-stu-id="2d720-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="2d720-121">När du distribuerar ett Azure Container registret, måste du först en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="2d720-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="2d720-122">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="2d720-122">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="2d720-123">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="2d720-123">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="2d720-124">I det här exemplet en resursgrupp med namnet *myResourceGroup* skapas i hello *westeurope* region.</span><span class="sxs-lookup"><span data-stu-id="2d720-124">In this example, a resource group named *myResourceGroup* is created in hello *westeurope* region.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="2d720-125">Skapa en Azure-behållare registret med hello [az acr skapa](/cli/azure/acr#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="2d720-125">Create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="2d720-126">hello namn på en behållare registernyckel **måste vara unika**.</span><span class="sxs-lookup"><span data-stu-id="2d720-126">hello name of a Container Registry **must be unique**.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

<span data-ttu-id="2d720-127">I hela hello resten av den här självstudiekursen använder vi ”acrname” som platshållare för hello registret behållarnamn som du har valt.</span><span class="sxs-lookup"><span data-stu-id="2d720-127">Throughout hello rest of this tutorial, we use "acrname" as a placeholder for hello container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="2d720-128">Behållaren registret inloggning</span><span class="sxs-lookup"><span data-stu-id="2d720-128">Container registry login</span></span>

<span data-ttu-id="2d720-129">Du måste logga in tooyour ACR-instansen innan du skickar bilder tooit.</span><span class="sxs-lookup"><span data-stu-id="2d720-129">You must log in tooyour ACR instance before pushing images tooit.</span></span> <span data-ttu-id="2d720-130">Använd hello [az acr inloggning](https://docs.microsoft.com/en-us/cli/azure/acr#login) kommandot toocomplete hello igen.</span><span class="sxs-lookup"><span data-stu-id="2d720-130">Use hello [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command toocomplete hello operation.</span></span> <span data-ttu-id="2d720-131">Du måste tooprovide hello unika namn som toohello behållare registret när den skapades.</span><span class="sxs-lookup"><span data-stu-id="2d720-131">You need tooprovide hello unique name given toohello container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="2d720-132">hello kommando returnerar meddelandet inloggningen lyckades när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2d720-132">hello command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-images"></a><span data-ttu-id="2d720-133">Taggen behållaren bilder</span><span class="sxs-lookup"><span data-stu-id="2d720-133">Tag container images</span></span>

<span data-ttu-id="2d720-134">Varje behållare avbildning måste toobe märkta med hello loginServer namnet på hello registret.</span><span class="sxs-lookup"><span data-stu-id="2d720-134">Each container image needs toobe tagged with hello loginServer name of hello registry.</span></span> <span data-ttu-id="2d720-135">Den här etiketten används för routning vid push-installation behållaren bilder tooan avbildningen registret.</span><span class="sxs-lookup"><span data-stu-id="2d720-135">This tag is used for routing when pushing container images tooan image registry.</span></span>

<span data-ttu-id="2d720-136">toosee en lista över aktuella bilder, Använd hello [docker bilder](https://docs.docker.com/engine/reference/commandline/images/) kommando.</span><span class="sxs-lookup"><span data-stu-id="2d720-136">toosee a list of current images, use hello [docker images](https://docs.docker.com/engine/reference/commandline/images/) command.</span></span>

```bash
docker images
```

<span data-ttu-id="2d720-137">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2d720-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

<span data-ttu-id="2d720-138">tooget hello loginServer namn, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="2d720-138">tooget hello loginServer name, run hello following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="2d720-139">Nu taggen hello *azure rösten sista* avbildningen med hello loginServer av hello behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="2d720-139">Now, tag hello *azure-vote-front* image with hello loginServer of hello container registry.</span></span> <span data-ttu-id="2d720-140">Lägg även till `:redis-v1` toohello slutet av hello avbildningsnamn.</span><span class="sxs-lookup"><span data-stu-id="2d720-140">Also, add `:redis-v1` toohello end of hello image name.</span></span> <span data-ttu-id="2d720-141">Den här taggen anger hello avbildningen versionen.</span><span class="sxs-lookup"><span data-stu-id="2d720-141">This tag indicates hello image version.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

<span data-ttu-id="2d720-142">När taggade, köra [docker bilder] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello igen.</span><span class="sxs-lookup"><span data-stu-id="2d720-142">Once tagged, run [docker images] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello operation.</span></span>

```bash
docker images
```

<span data-ttu-id="2d720-143">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2d720-143">Output:</span></span>

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-tooregistry"></a><span data-ttu-id="2d720-144">Push-avbildningar tooregistry</span><span class="sxs-lookup"><span data-stu-id="2d720-144">Push images tooregistry</span></span>

<span data-ttu-id="2d720-145">Push-hello *azure rösten sista* bild toohello registret.</span><span class="sxs-lookup"><span data-stu-id="2d720-145">Push hello *azure-vote-front* image toohello registry.</span></span> 

<span data-ttu-id="2d720-146">Med följande exempel hello Ersätt hello ACR loginServer namn med hello loginServer från din miljö.</span><span class="sxs-lookup"><span data-stu-id="2d720-146">Using hello following example, replace hello ACR loginServer name with hello loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

<span data-ttu-id="2d720-147">Detta tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2d720-147">This takes a couple of minutes toocomplete.</span></span>

## <a name="list-images-in-registry"></a><span data-ttu-id="2d720-148">Lista över bilder i registret</span><span class="sxs-lookup"><span data-stu-id="2d720-148">List images in registry</span></span>

<span data-ttu-id="2d720-149">tooreturn en lista över bilder som har aviserats tooyour Azure-behållare registret, användaren hello [az acr databaslistan](/cli/azure/acr/repository#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="2d720-149">tooreturn a list of images that have been pushed tooyour Azure Container registry, user hello [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="2d720-150">Uppdatera hello kommando med hello ACR instansnamn.</span><span class="sxs-lookup"><span data-stu-id="2d720-150">Update hello command with hello ACR instance name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="2d720-151">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2d720-151">Output:</span></span>

```azurecli
Result
----------------
azure-vote-front
```

<span data-ttu-id="2d720-152">Och sedan använda toosee hello taggar för en viss bild hello [az acr databasen Visa-taggar](/cli/azure/acr/repository#show-tags) kommando.</span><span class="sxs-lookup"><span data-stu-id="2d720-152">And then toosee hello tags for a specific image, use hello [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

<span data-ttu-id="2d720-153">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2d720-153">Output:</span></span>

```azurecli
Result
--------
redis-v1
```

<span data-ttu-id="2d720-154">Vid självstudiekursen slut har hello behållaren avbildningen lagrats i en privat Azure-behållare registret-instans.</span><span class="sxs-lookup"><span data-stu-id="2d720-154">At tutorial completion, hello container image has been stored in a private Azure Container Registry instance.</span></span> <span data-ttu-id="2d720-155">Den här avbildningen distribueras från ACR tooa Kubernetes kluster i efterföljande självstudiekurser.</span><span class="sxs-lookup"><span data-stu-id="2d720-155">This image is deployed from ACR tooa Kubernetes cluster in subsequent tutorials.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d720-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2d720-156">Next steps</span></span>

<span data-ttu-id="2d720-157">I den här självstudiekursen förbereddes ett Azure Container registret för användning i ett Kubernetes ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="2d720-157">In this tutorial, an Azure Container Registry was prepared for use in an ACS Kubernetes cluster.</span></span> <span data-ttu-id="2d720-158">hello följande steg har slutförts:</span><span class="sxs-lookup"><span data-stu-id="2d720-158">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2d720-159">Distribuera en Azure-behållare registret-instans</span><span class="sxs-lookup"><span data-stu-id="2d720-159">Deployed an Azure Container Registry instance</span></span>
> * <span data-ttu-id="2d720-160">En avbildning av behållaren har taggats för ACR</span><span class="sxs-lookup"><span data-stu-id="2d720-160">Tagged a container image for ACR</span></span>
> * <span data-ttu-id="2d720-161">Överförda hello bilden tooACR</span><span class="sxs-lookup"><span data-stu-id="2d720-161">Uploaded hello image tooACR</span></span>

<span data-ttu-id="2d720-162">Avancera toohello nästa självstudiekurs toolearn om hur du distribuerar ett Kubernetes kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="2d720-162">Advance toohello next tutorial toolearn about deploying a Kubernetes cluster in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2d720-163">Distribuera Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="2d720-163">Deploy Kubernetes cluster</span></span>](./container-service-tutorial-kubernetes-deploy-cluster.md)