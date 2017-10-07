---
title: "aaaAzure Behållarinstanser tutorial – förbereda Azure Container registret | Microsoft Docs"
description: "Azure Behållarinstanser tutorial – förbereda registret för Azure-behållare"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2525626125740c3c861fad36aad207d0b587ff54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="41a66-104">Distribuera och använda Azure Container registret</span><span class="sxs-lookup"><span data-stu-id="41a66-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="41a66-105">Detta är en del två av tre delar självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="41a66-105">This is part two of a three-part tutorial.</span></span> <span data-ttu-id="41a66-106">I hello [föregående steg](./container-instances-tutorial-prepare-app.md), en behållare avbildning har skapats för en enkel webbapp som skrivits i [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="41a66-106">In hello [previous step](./container-instances-tutorial-prepare-app.md), a container image was created for a simple web application written in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="41a66-107">I den här kursen är den här avbildningen pushas tooan Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="41a66-107">In this tutorial, this image is pushed tooan Azure Container Registry.</span></span> <span data-ttu-id="41a66-108">Om du inte har skapat hello behållaren image returnerar för[kursen 1 – skapa behållaren image](./container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="41a66-108">If you have not created hello container image, return too[Tutorial 1 – Create container image](./container-instances-tutorial-prepare-app.md).</span></span> 

<span data-ttu-id="41a66-109">hello Azure Container registret är en Azure-baserade, privat registret för Docker behållare bilder.</span><span class="sxs-lookup"><span data-stu-id="41a66-109">hello Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="41a66-110">Den här kursen går igenom distribuera en Azure-behållare registret-instans och trycka på en behållare avbildningen tooit.</span><span class="sxs-lookup"><span data-stu-id="41a66-110">This tutorial walks through deploying an Azure Container Registry instance, and pushing a container image tooit.</span></span> <span data-ttu-id="41a66-111">Slutfört stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="41a66-111">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41a66-112">Distribuera en Azure-behållare registret-instans</span><span class="sxs-lookup"><span data-stu-id="41a66-112">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="41a66-113">Behållaren image-märkning för Azure-behållare registernyckeln</span><span class="sxs-lookup"><span data-stu-id="41a66-113">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="41a66-114">Överför avbildningen tooAzure behållare registret</span><span class="sxs-lookup"><span data-stu-id="41a66-114">Uploading image tooAzure Container Registry</span></span>

<span data-ttu-id="41a66-115">I efterföljande självstudiekurser distribuera hello behållare från din privata registret tooAzure Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="41a66-115">In subsequent tutorials, you deploy hello container from your private registry tooAzure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="41a66-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="41a66-116">Before you begin</span></span>

<span data-ttu-id="41a66-117">Den här kursen kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="41a66-117">This tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="41a66-118">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="41a66-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="41a66-119">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="41a66-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="41a66-120">Distribuera Azure-behållaren registret</span><span class="sxs-lookup"><span data-stu-id="41a66-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="41a66-121">När du distribuerar ett Azure Container registret, måste du först en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="41a66-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="41a66-122">En Azure-resursgrupp är en logisk samling i vilka Azure resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="41a66-122">An Azure resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="41a66-123">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="41a66-123">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="41a66-124">I det här exemplet en resursgrupp med namnet *myResourceGroup* skapas i hello *eastus* region.</span><span class="sxs-lookup"><span data-stu-id="41a66-124">In this example, a resource group named *myResourceGroup* is created in hello *eastus* region.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="41a66-125">Skapa en Azure-behållare registret med hello [az acr skapa](/cli/azure/acr#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="41a66-125">Create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="41a66-126">hello namn på en behållare registernyckel **måste vara unika**.</span><span class="sxs-lookup"><span data-stu-id="41a66-126">hello name of a Container Registry **must be unique**.</span></span> <span data-ttu-id="41a66-127">I följande exempel hello, vi använda hello namnet *mycontainerregistry082*.</span><span class="sxs-lookup"><span data-stu-id="41a66-127">In hello following example, we use hello name *mycontainerregistry082*.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

<span data-ttu-id="41a66-128">I hela hello resten av den här självstudiekursen, använder vi `<acrname>` som platshållare för hello registret behållarnamn som du har valt.</span><span class="sxs-lookup"><span data-stu-id="41a66-128">Throughout hello rest of this tutorial, we use `<acrname>` as a placeholder for hello container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="41a66-129">Behållaren registret inloggning</span><span class="sxs-lookup"><span data-stu-id="41a66-129">Container registry login</span></span>

<span data-ttu-id="41a66-130">Du måste logga in tooyour ACR-instansen innan du skickar bilder tooit.</span><span class="sxs-lookup"><span data-stu-id="41a66-130">You must log in tooyour ACR instance before pushing images tooit.</span></span> <span data-ttu-id="41a66-131">Använd hello [az acr inloggning](https://docs.microsoft.com/en-us/cli/azure/acr#login) kommandot toocomplete hello igen.</span><span class="sxs-lookup"><span data-stu-id="41a66-131">Use hello [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command toocomplete hello operation.</span></span> <span data-ttu-id="41a66-132">Du måste tooprovide hello unika namn som toohello behållare registret när den skapades.</span><span class="sxs-lookup"><span data-stu-id="41a66-132">You need tooprovide hello unique name given toohello container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="41a66-133">hello kommando returnerar meddelandet inloggningen lyckades när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="41a66-133">hello command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-image"></a><span data-ttu-id="41a66-134">Taggen behållaren bild</span><span class="sxs-lookup"><span data-stu-id="41a66-134">Tag container image</span></span>

<span data-ttu-id="41a66-135">toodeploy en behållare avbildning från ett privat register hello avbildningen måste toobe märkta med hello `loginServer` namn på hello registret.</span><span class="sxs-lookup"><span data-stu-id="41a66-135">toodeploy a container image from a private registry, hello image needs toobe tagged with hello `loginServer` name of hello registry.</span></span>

<span data-ttu-id="41a66-136">toosee en lista över aktuella bilder, Använd hello `docker images` kommando.</span><span class="sxs-lookup"><span data-stu-id="41a66-136">toosee a list of current images, use hello `docker images` command.</span></span>

```bash
docker images
```

<span data-ttu-id="41a66-137">Resultat:</span><span class="sxs-lookup"><span data-stu-id="41a66-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

<span data-ttu-id="41a66-138">tooget hello loginServer namn, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="41a66-138">tooget hello loginServer name, run hello following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="41a66-139">Taggen hello *aci kursen app* avbildningen med hello loginServer av hello behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="41a66-139">Tag hello *aci-tutorial-app* image with hello loginServer of hello container registry.</span></span> <span data-ttu-id="41a66-140">Lägg även till `:v1` toohello slutet av hello avbildningsnamn.</span><span class="sxs-lookup"><span data-stu-id="41a66-140">Also, add `:v1` toohello end of hello image name.</span></span> <span data-ttu-id="41a66-141">Den här taggen anger versionsnumret för hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="41a66-141">This tag indicates hello image version number.</span></span>

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

<span data-ttu-id="41a66-142">När taggade, köra `docker images` tooverify hello igen.</span><span class="sxs-lookup"><span data-stu-id="41a66-142">Once tagged, run `docker images` tooverify hello operation.</span></span>

```bash
docker images
```

<span data-ttu-id="41a66-143">Resultat:</span><span class="sxs-lookup"><span data-stu-id="41a66-143">Output:</span></span>

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-tooazure-container-registry"></a><span data-ttu-id="41a66-144">Push-avbildningen tooAzure behållare registret</span><span class="sxs-lookup"><span data-stu-id="41a66-144">Push image tooAzure Container Registry</span></span>

<span data-ttu-id="41a66-145">Push-hello *aci kursen app* bild toohello registret.</span><span class="sxs-lookup"><span data-stu-id="41a66-145">Push hello *aci-tutorial-app* image toohello registry.</span></span>

<span data-ttu-id="41a66-146">Med följande exempel hello Ersätt hello behållarnamn registret loginServer med hello loginServer från din miljö.</span><span class="sxs-lookup"><span data-stu-id="41a66-146">Using hello following example, replace hello container registry loginServer name with hello loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a><span data-ttu-id="41a66-147">Listan avbildningar i Azure Container registret</span><span class="sxs-lookup"><span data-stu-id="41a66-147">List images in Azure Container Registry</span></span>

<span data-ttu-id="41a66-148">tooreturn en lista över bilder som har aviserats tooyour Azure-behållare registret, användaren hello [az acr databaslistan](/cli/azure/acr/repository#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="41a66-148">tooreturn a list of images that have been pushed tooyour Azure Container registry, user hello [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="41a66-149">Uppdatera hello kommando med hello behållarnamn registret.</span><span class="sxs-lookup"><span data-stu-id="41a66-149">Update hello command with hello container registry name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="41a66-150">Resultat:</span><span class="sxs-lookup"><span data-stu-id="41a66-150">Output:</span></span>

```azurecli
Result
----------------
aci-tutorial-app
```

<span data-ttu-id="41a66-151">Och sedan använda toosee hello taggar för en viss bild hello [az acr databasen Visa-taggar](/cli/azure/acr/repository#show-tags) kommando.</span><span class="sxs-lookup"><span data-stu-id="41a66-151">And then toosee hello tags for a specific image, use hello [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

<span data-ttu-id="41a66-152">Resultat:</span><span class="sxs-lookup"><span data-stu-id="41a66-152">Output:</span></span>

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a><span data-ttu-id="41a66-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="41a66-153">Next steps</span></span>

<span data-ttu-id="41a66-154">Ett Azure Container registret förbereddes för användning med Azure Container instanser i den här självstudiekursen och hello behållaren avbildningen har flyttas.</span><span class="sxs-lookup"><span data-stu-id="41a66-154">In this tutorial, an Azure Container Registry was prepared for use with Azure Container Instances, and hello container image was pushed.</span></span> <span data-ttu-id="41a66-155">hello följande steg har slutförts:</span><span class="sxs-lookup"><span data-stu-id="41a66-155">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41a66-156">Distribuera en Azure-behållare registret-instans</span><span class="sxs-lookup"><span data-stu-id="41a66-156">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="41a66-157">Behållaren image-märkning för Azure-behållare registernyckeln</span><span class="sxs-lookup"><span data-stu-id="41a66-157">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="41a66-158">Överför avbildningen tooAzure behållare registret</span><span class="sxs-lookup"><span data-stu-id="41a66-158">Uploading image tooAzure Container Registry</span></span>

<span data-ttu-id="41a66-159">Avancera toohello nästa självstudiekurs toolearn om hur du distribuerar hello behållaren tooAzure med Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="41a66-159">Advance toohello next tutorial toolearn about deploying hello container tooAzure using Azure Container Instances.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="41a66-160">Distribuera behållare tooAzure Behållarinstanser</span><span class="sxs-lookup"><span data-stu-id="41a66-160">Deploy containers tooAzure Container Instances</span></span>](./container-instances-tutorial-deploy-app.md)
