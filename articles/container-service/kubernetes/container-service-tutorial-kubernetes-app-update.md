---
title: "Självstudier för aaaAzure Container Service - uppdateringsprogrammet | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - uppdateringsprogrammet"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c467498bab7952926a18e45ffbb21051a98739d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-an-application-in-kubernetes"></a><span data-ttu-id="8d951-104">Uppdatera ett program i Kubernetes</span><span class="sxs-lookup"><span data-stu-id="8d951-104">Update an application in Kubernetes</span></span>

<span data-ttu-id="8d951-105">När du distribuerar ett program i Kubernetes, kan den uppdateras genom att ange en ny behållare bild eller Bildversion.</span><span class="sxs-lookup"><span data-stu-id="8d951-105">After you deploy an application in Kubernetes, it can be updated by specifying a new container image or image version.</span></span> <span data-ttu-id="8d951-106">När du uppdaterar ett program, mellanlagras hello programuppdatering så att endast en del av hello distributionen uppdateras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="8d951-106">When you update an application, hello update rollout is staged so that only a portion of hello deployment is concurrently updated.</span></span> <span data-ttu-id="8d951-107">Uppdateringen mellanlagrade kan hello programmet tookeep körs under hello uppdatering och tillhandahåller en mekanism för återställning om ett distributionsfel inträffar.</span><span class="sxs-lookup"><span data-stu-id="8d951-107">This staged update enables hello application tookeep running during hello update, and provides a rollback mechanism if a deployment failure occurs.</span></span> 

<span data-ttu-id="8d951-108">I den här självstudiekursen uppdateras sex tillhör sju hello Azure röst exempelapp.</span><span class="sxs-lookup"><span data-stu-id="8d951-108">In this tutorial, part six of seven, hello sample Azure Vote app is updated.</span></span> <span data-ttu-id="8d951-109">Uppgifterna som du har slutfört är:</span><span class="sxs-lookup"><span data-stu-id="8d951-109">Tasks that you complete include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8d951-110">Uppdatera hello frontend programkod</span><span class="sxs-lookup"><span data-stu-id="8d951-110">Updating hello front-end application code</span></span>
> * <span data-ttu-id="8d951-111">Skapa en bild av uppdaterade behållare</span><span class="sxs-lookup"><span data-stu-id="8d951-111">Creating an updated container image</span></span>
> * <span data-ttu-id="8d951-112">Skicka hello behållaren image tooAzure behållare registret</span><span class="sxs-lookup"><span data-stu-id="8d951-112">Pushing hello container image tooAzure Container Registry</span></span>
> * <span data-ttu-id="8d951-113">Distribuera hello uppdaterade behållaren avbildningen</span><span class="sxs-lookup"><span data-stu-id="8d951-113">Deploying hello updated container image</span></span>

<span data-ttu-id="8d951-114">I efterföljande självstudiekurser är Operations Management Suite konfigurerade toomonitor hello Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="8d951-114">In subsequent tutorials, Operations Management Suite is configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8d951-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="8d951-115">Before you begin</span></span>

<span data-ttu-id="8d951-116">I tidigare självstudier, ett program som har paketerats till en behållare bild, hello avbildningen överfört tooAzure behållare registret och ett Kubernetes kluster skapas.</span><span class="sxs-lookup"><span data-stu-id="8d951-116">In previous tutorials, an application was packaged into a container image, hello image uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="8d951-117">hello programmet körs sedan hello Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="8d951-117">hello application was then run on hello Kubernetes cluster.</span></span> 

<span data-ttu-id="8d951-118">Om du inte har utfört stegen och vill toofollow längs, returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="8d951-118">If you haven't completed these steps, and want toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

## <a name="update-application"></a><span data-ttu-id="8d951-119">Uppdatera program</span><span class="sxs-lookup"><span data-stu-id="8d951-119">Update application</span></span>

<span data-ttu-id="8d951-120">toocomplete hello steg i den här självstudiekursen, du måste ha klona en kopia av hello Azure rösten program.</span><span class="sxs-lookup"><span data-stu-id="8d951-120">toocomplete hello steps in this tutorial, you must have cloned a copy of hello Azure Vote application.</span></span> <span data-ttu-id="8d951-121">Om det behövs kan du skapa klonade kopian med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8d951-121">If necessary, create this cloned copy with hello following command:</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="8d951-122">Öppna hello `config_file.cfg` fil med någon kod eller textredigerare.</span><span class="sxs-lookup"><span data-stu-id="8d951-122">Open hello `config_file.cfg` file with any code or text editor.</span></span> <span data-ttu-id="8d951-123">Du hittar den här filen under hello följande katalogen för hello klona lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="8d951-123">You can find this file under hello following directory of hello cloned repo.</span></span>

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

<span data-ttu-id="8d951-124">Ändra hello värden för `VOTE1VALUE` och `VOTE2VALUE`, och sedan spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8d951-124">Change hello values for `VOTE1VALUE` and `VOTE2VALUE`, and then save hello file.</span></span>

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

<span data-ttu-id="8d951-125">Använd [docker compose](https://docs.docker.com/compose/) toore-skapa hello frontend image- och kör hello uppdateras.</span><span class="sxs-lookup"><span data-stu-id="8d951-125">Use [docker-compose](https://docs.docker.com/compose/) toore-create hello front-end image and run hello updated application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a><span data-ttu-id="8d951-126">Testa programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="8d951-126">Test application locally</span></span>

<span data-ttu-id="8d951-127">Bläddra för`http://localhost:8080` toosee hello uppdaterade tillämpningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8d951-127">Browse too`http://localhost:8080` toosee hello updated application.</span></span>

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a><span data-ttu-id="8d951-129">Taggen och push-avbildningar</span><span class="sxs-lookup"><span data-stu-id="8d951-129">Tag and push images</span></span>

<span data-ttu-id="8d951-130">Taggen hello *azure rösten sista* avbildningen med hello loginServer av hello behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="8d951-130">Tag hello *azure-vote-front* image with hello loginServer of hello container registry.</span></span>

<span data-ttu-id="8d951-131">Om du använder Azure-behållare registret hämta hello inloggningen servernamn med hello [az acr lista](/cli/azure/acr#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="8d951-131">If you're using Azure Container Registry, get hello login server name with hello [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="8d951-132">Använd [docker-taggen](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="8d951-132">Use [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello image.</span></span> <span data-ttu-id="8d951-133">Ersätt `<acrLoginServer>` med Azure Container registret inloggningsnamnet server eller offentliga registret värdnamn.</span><span class="sxs-lookup"><span data-stu-id="8d951-133">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="8d951-134">Använd [docker push](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello avbildningen tooyour registret.</span><span class="sxs-lookup"><span data-stu-id="8d951-134">Use [docker push](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello image tooyour registry.</span></span> <span data-ttu-id="8d951-135">Ersätt `<acrLoginServer>` med Azure Container registret inloggningsnamnet server eller offentliga registret värdnamn.</span><span class="sxs-lookup"><span data-stu-id="8d951-135">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a><span data-ttu-id="8d951-136">Distribuera program för uppdatering</span><span class="sxs-lookup"><span data-stu-id="8d951-136">Deploy update application</span></span>

<span data-ttu-id="8d951-137">tooensure maximala drifttid, flera instanser av hello programmet baljor måste köras.</span><span class="sxs-lookup"><span data-stu-id="8d951-137">tooensure maximum uptime, multiple instances of hello application pod must be running.</span></span> <span data-ttu-id="8d951-138">Kontrollera konfigurationen med hello [kubectl hämta baljor](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando.</span><span class="sxs-lookup"><span data-stu-id="8d951-138">Verify this configuration with hello [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```bash
kubectl get pod
```

<span data-ttu-id="8d951-139">Resultat:</span><span class="sxs-lookup"><span data-stu-id="8d951-139">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

<span data-ttu-id="8d951-140">Om du inte har flera skida kör hello azure rösten sista bilden skala hello *azure rösten sista* distribution.</span><span class="sxs-lookup"><span data-stu-id="8d951-140">If you don't have multiple pods running hello azure-vote-front image, scale hello *azure-vote-front* deployment.</span></span>


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

<span data-ttu-id="8d951-141">tooupdate hello program, Använd hello [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="8d951-141">tooupdate hello application, use hello [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) command.</span></span> <span data-ttu-id="8d951-142">Uppdatera `<acrLoginServer>` med hello server eller en värd inloggningsnamnet behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="8d951-142">Update `<acrLoginServer>` with hello login server or host name of your container registry.</span></span>

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="8d951-143">toomonitor hello distribution, Använd hello [kubectl hämta baljor](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando.</span><span class="sxs-lookup"><span data-stu-id="8d951-143">toomonitor hello deployment, use hello [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span> <span data-ttu-id="8d951-144">Eftersom hello uppdatera programmet distribueras avslutas din skida och återskapas med hello ny behållare avbildning.</span><span class="sxs-lookup"><span data-stu-id="8d951-144">As hello updated application is deployed, your pods are terminated and re-created with hello new container image.</span></span>

```azurecli-interactive
kubectl get pod
```

<span data-ttu-id="8d951-145">Resultat:</span><span class="sxs-lookup"><span data-stu-id="8d951-145">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a><span data-ttu-id="8d951-146">Testa uppdaterade program</span><span class="sxs-lookup"><span data-stu-id="8d951-146">Test updated application</span></span>

<span data-ttu-id="8d951-147">Hämta hello externa IP-adress hello *azure rösten sista* service.</span><span class="sxs-lookup"><span data-stu-id="8d951-147">Get hello external IP address of hello *azure-vote-front* service.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front
```

<span data-ttu-id="8d951-148">Bläddra toohello IP-adress toosee hello uppdaterade tillämpningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8d951-148">Browse toohello IP address toosee hello updated application.</span></span>

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a><span data-ttu-id="8d951-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d951-150">Next steps</span></span>

<span data-ttu-id="8d951-151">I den här självstudiekursen, uppdatera ett program och distribuerat den här uppdateringen tooa Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="8d951-151">In this tutorial, you updated an application and rolled out this update tooa Kubernetes cluster.</span></span> <span data-ttu-id="8d951-152">hello följande uppgifter har slutförts:</span><span class="sxs-lookup"><span data-stu-id="8d951-152">hello following tasks were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8d951-153">Uppdaterade hello frontend programkod</span><span class="sxs-lookup"><span data-stu-id="8d951-153">Updated hello front-end application code</span></span>
> * <span data-ttu-id="8d951-154">Skapa en bild av uppdaterade behållare</span><span class="sxs-lookup"><span data-stu-id="8d951-154">Created an updated container image</span></span>
> * <span data-ttu-id="8d951-155">Pushas hello behållaren image tooAzure behållare registret</span><span class="sxs-lookup"><span data-stu-id="8d951-155">Pushed hello container image tooAzure Container Registry</span></span>
> * <span data-ttu-id="8d951-156">Distribuerade hello uppdateras program</span><span class="sxs-lookup"><span data-stu-id="8d951-156">Deployed hello updated application</span></span>

<span data-ttu-id="8d951-157">I förväg toohello nästa självstudiekurs toolearn om hur toomonitor Kubernetes med Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="8d951-157">Advance toohello next tutorial toolearn about how toomonitor Kubernetes with Operations Management Suite.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8d951-158">Övervaka Kubernetes med OMS (Monitor Kubernetes with OMS)</span><span class="sxs-lookup"><span data-stu-id="8d951-158">Monitor Kubernetes with OMS</span></span>](./container-service-tutorial-kubernetes-monitor.md)