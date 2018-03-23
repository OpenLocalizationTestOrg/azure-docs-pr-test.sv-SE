---
title: Självstudie för Azure Container Service – Uppdatera ett program
description: Självstudie för Azure Container Service – Uppdatera ett program
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5ecaa3a79270e29ac002e91065f7df4f7e8914e7
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2018
---
# <a name="update-an-application-in-kubernetes"></a><span data-ttu-id="c1c60-103">Uppdatera ett program i Kubernetes</span><span class="sxs-lookup"><span data-stu-id="c1c60-103">Update an application in Kubernetes</span></span>

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

<span data-ttu-id="c1c60-104">När ett program har distribuerats i Kubernetes kan du uppdatera det genom att ange en ny behållaravbildning eller avbildningsversion.</span><span class="sxs-lookup"><span data-stu-id="c1c60-104">After an application has been deployed in Kubernetes, it can be updated by specifying a new container image or image version.</span></span> <span data-ttu-id="c1c60-105">När du gör det mellanlagras uppdateringen så att endast en del av distributionen uppdateras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c1c60-105">When doing so, the update is staged so that only a portion of the deployment is concurrently updated.</span></span> <span data-ttu-id="c1c60-106">Den här mellanlagrade uppdateringen gör att programmet kan fortsätta att köras under uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="c1c60-106">This staged update enables the application to keep running during the update.</span></span> <span data-ttu-id="c1c60-107">Det ger också en mekanism för återställning om ett distributionsfel inträffar.</span><span class="sxs-lookup"><span data-stu-id="c1c60-107">It also provides a rollback mechanism if a deployment failure occurs.</span></span> 

<span data-ttu-id="c1c60-108">I den här självstudien, som är del sex av sju, uppdateras Azure Vote-exempelappen.</span><span class="sxs-lookup"><span data-stu-id="c1c60-108">In this tutorial, part six of seven, the sample Azure Vote app is updated.</span></span> <span data-ttu-id="c1c60-109">Här är några av uppgifterna:</span><span class="sxs-lookup"><span data-stu-id="c1c60-109">Tasks that you complete include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1c60-110">Uppdatera klientdelens programkod</span><span class="sxs-lookup"><span data-stu-id="c1c60-110">Updating the front-end application code</span></span>
> * <span data-ttu-id="c1c60-111">Skapa en uppdaterad behållaravbildning</span><span class="sxs-lookup"><span data-stu-id="c1c60-111">Creating an updated container image</span></span>
> * <span data-ttu-id="c1c60-112">Push-överföra behållaravbildningen till Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="c1c60-112">Pushing the container image to Azure Container Registry</span></span>
> * <span data-ttu-id="c1c60-113">Distribuera den uppdaterade behållaravbildningen</span><span class="sxs-lookup"><span data-stu-id="c1c60-113">Deploying the updated container image</span></span>

<span data-ttu-id="c1c60-114">I senare självstudier konfigureras Operations Management Suite för att övervaka Kubernetes-klustret.</span><span class="sxs-lookup"><span data-stu-id="c1c60-114">In subsequent tutorials, Operations Management Suite is configured to monitor the Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c1c60-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c1c60-115">Before you begin</span></span>

<span data-ttu-id="c1c60-116">I tidigare självstudier paketerades ett program i en behållaravbildning, avbildningen laddades upp till Azure Container Registry och ett Kubernetes-kluster skapades.</span><span class="sxs-lookup"><span data-stu-id="c1c60-116">In previous tutorials, an application was packaged into a container image, the image uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="c1c60-117">Programmet kördes därefter i Kubernetes-klustret.</span><span class="sxs-lookup"><span data-stu-id="c1c60-117">The application was then run on the Kubernetes cluster.</span></span> 

<span data-ttu-id="c1c60-118">En programlagringsplats klonades också, med programmets källkod och en färdig Docker Compose-fil som används i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="c1c60-118">An application repository was also cloned which includes the application source code, and a pre-created Docker Compose file used in this tutorial.</span></span> <span data-ttu-id="c1c60-119">Verifiera att du har skapat en klon av lagringsplatsen och att du har ändrat kataloger i den klonade katalogen.</span><span class="sxs-lookup"><span data-stu-id="c1c60-119">Verify that you have created a clone of the repo, and that you have changed directories into the cloned directory.</span></span> <span data-ttu-id="c1c60-120">Inuti finns en katalog med namnet `azure-vote` och en fil med namnet `docker-compose.yml`.</span><span class="sxs-lookup"><span data-stu-id="c1c60-120">Inside is a directory named `azure-vote` and a file named `docker-compose.yml`.</span></span>

<span data-ttu-id="c1c60-121">Om du inte har slutfört dessa steg och vill följa med återgår du till [Självstudie 1 – Skapa behållaravbildningar](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="c1c60-121">If you haven't completed these steps, and want to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

## <a name="update-application"></a><span data-ttu-id="c1c60-122">Uppdatera programmet</span><span class="sxs-lookup"><span data-stu-id="c1c60-122">Update application</span></span>

<span data-ttu-id="c1c60-123">För den här självstudien görs en ändring av programmet, och det uppdaterade programmet distribueras till Kubernetes-klustret.</span><span class="sxs-lookup"><span data-stu-id="c1c60-123">For this tutorial, a change is made to the application, and the updated application deployed to the Kubernetes cluster.</span></span> 

<span data-ttu-id="c1c60-124">Programmets källkod finns i katalogen `azure-vote`.</span><span class="sxs-lookup"><span data-stu-id="c1c60-124">The application source code can be found inside of the `azure-vote` directory.</span></span> <span data-ttu-id="c1c60-125">Öppna filen `config_file.cfg` i valfritt redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="c1c60-125">Open the `config_file.cfg` file with any code or text editor.</span></span> <span data-ttu-id="c1c60-126">I det här exemplet används `vi`.</span><span class="sxs-lookup"><span data-stu-id="c1c60-126">In this example `vi` is used.</span></span>

```bash
vi azure-vote/azure-vote/config_file.cfg
```

<span data-ttu-id="c1c60-127">Ändra värdena för `VOTE1VALUE` och `VOTE2VALUE` och spara filen.</span><span class="sxs-lookup"><span data-stu-id="c1c60-127">Change the values for `VOTE1VALUE` and `VOTE2VALUE`, and then save the file.</span></span>

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

<span data-ttu-id="c1c60-128">Spara och stäng filen.</span><span class="sxs-lookup"><span data-stu-id="c1c60-128">Save and close the file.</span></span>

## <a name="update-container-image"></a><span data-ttu-id="c1c60-129">Uppdatera behållaravbildningen</span><span class="sxs-lookup"><span data-stu-id="c1c60-129">Update container image</span></span>

<span data-ttu-id="c1c60-130">Använd [docker-compose](https://docs.docker.com/compose/) till att skapa kilentdelsavbildningen igen och kör det uppdaterade programmet.</span><span class="sxs-lookup"><span data-stu-id="c1c60-130">Use [docker-compose](https://docs.docker.com/compose/) to re-create the front-end image and run the updated application.</span></span> <span data-ttu-id="c1c60-131">Argumentet `--build` används till att instruera Docker Compose att skapa programavbildningen på nytt.</span><span class="sxs-lookup"><span data-stu-id="c1c60-131">The `--build` argument is used to instruct Docker Compose to re-create the application image.</span></span>

```bash
docker-compose up --build -d
```

## <a name="test-application-locally"></a><span data-ttu-id="c1c60-132">Testa programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="c1c60-132">Test application locally</span></span>

<span data-ttu-id="c1c60-133">Gå till http://localhost:8080 om du vill se det uppdaterade programmet.</span><span class="sxs-lookup"><span data-stu-id="c1c60-133">Browse to http://localhost:8080 to see the updated application.</span></span>

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a><span data-ttu-id="c1c60-135">Tagga och push-överföra avbildningar</span><span class="sxs-lookup"><span data-stu-id="c1c60-135">Tag and push images</span></span>

<span data-ttu-id="c1c60-136">Tagga avbildningen `azure-vote-front` med namnet på inloggningsservern för behållarregistret.</span><span class="sxs-lookup"><span data-stu-id="c1c60-136">Tag the `azure-vote-front` image with the loginServer of the container registry.</span></span> 

<span data-ttu-id="c1c60-137">Hämta inloggningsserverns namn med kommandot [az acr list](/cli/azure/acr#list).</span><span class="sxs-lookup"><span data-stu-id="c1c60-137">Get the login server name with the [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="c1c60-138">Använd [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) till att tagga avbildningen.</span><span class="sxs-lookup"><span data-stu-id="c1c60-138">Use [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) to tag the image.</span></span> <span data-ttu-id="c1c60-139">Ersätt `<acrLoginServer>` med namnet på inloggningsservern för Azure Container Registry eller värdnamnet för ett offentligt register.</span><span class="sxs-lookup"><span data-stu-id="c1c60-139">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span> <span data-ttu-id="c1c60-140">Lägg även märke till att avbildningsversionen har uppdaterats till `redis-v2`.</span><span class="sxs-lookup"><span data-stu-id="c1c60-140">Also notice that the image version is updated to `redis-v2`.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="c1c60-141">Använd [docker push](https://docs.docker.com/engine/reference/commandline/push/) och ladda upp avbildningen till registret.</span><span class="sxs-lookup"><span data-stu-id="c1c60-141">Use [docker push](https://docs.docker.com/engine/reference/commandline/push/) to upload the image to your registry.</span></span> <span data-ttu-id="c1c60-142">Ersätt `<acrLoginServer>` med namnet på inloggningsservern för Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="c1c60-142">Replace `<acrLoginServer>` with your Azure Container Registry login server name.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a><span data-ttu-id="c1c60-143">Distribuera det uppdaterade programmet</span><span class="sxs-lookup"><span data-stu-id="c1c60-143">Deploy update application</span></span>

<span data-ttu-id="c1c60-144">Du får minimala störningar om flera instanser av programpodden körs.</span><span class="sxs-lookup"><span data-stu-id="c1c60-144">To ensure maximum uptime, multiple instances of the application pod must be running.</span></span> <span data-ttu-id="c1c60-145">Verifiera konfigurationen med kommandot [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get).</span><span class="sxs-lookup"><span data-stu-id="c1c60-145">Verify this configuration with the [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```bash
kubectl get pod
```

<span data-ttu-id="c1c60-146">Resultat:</span><span class="sxs-lookup"><span data-stu-id="c1c60-146">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

<span data-ttu-id="c1c60-147">Om du inte har flera poddar som kör avbildningen azure-vote-front skalar du ut `azure-vote-front`-distributionen.</span><span class="sxs-lookup"><span data-stu-id="c1c60-147">If you do not have multiple pods running the azure-vote-front image, scale the `azure-vote-front` deployment.</span></span>


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

<span data-ttu-id="c1c60-148">När du ska uppdatera programmet använder du kommandot [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set).</span><span class="sxs-lookup"><span data-stu-id="c1c60-148">To update the application, use the [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) command.</span></span> <span data-ttu-id="c1c60-149">Uppdatera `<acrLoginServer>` med inloggningsservern eller värdnamnet för ditt behållarregister.</span><span class="sxs-lookup"><span data-stu-id="c1c60-149">Update `<acrLoginServer>` with the login server or host name of your container registry.</span></span>

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="c1c60-150">Du övervakar distributionen med kommandot [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get).</span><span class="sxs-lookup"><span data-stu-id="c1c60-150">To monitor the deployment, use the [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span> <span data-ttu-id="c1c60-151">Eftersom det uppdaterade programmet är distribuerat avslutas dina poddar och återskapas med den nya behållaravbildningen.</span><span class="sxs-lookup"><span data-stu-id="c1c60-151">As the updated application is deployed, your pods are terminated and re-created with the new container image.</span></span>

```azurecli-interactive
kubectl get pod
```

<span data-ttu-id="c1c60-152">Resultat:</span><span class="sxs-lookup"><span data-stu-id="c1c60-152">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a><span data-ttu-id="c1c60-153">Testa det uppdaterade programmet</span><span class="sxs-lookup"><span data-stu-id="c1c60-153">Test updated application</span></span>

<span data-ttu-id="c1c60-154">Hämta den externa IP-adressen för tjänsten `azure-vote-front`.</span><span class="sxs-lookup"><span data-stu-id="c1c60-154">Get the external IP address of the `azure-vote-front` service.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front
```

<span data-ttu-id="c1c60-155">Gå till IP-adressen om du vill se det uppdaterade programmet.</span><span class="sxs-lookup"><span data-stu-id="c1c60-155">Browse to the IP address to see the updated application.</span></span>

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a><span data-ttu-id="c1c60-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1c60-157">Next steps</span></span>

<span data-ttu-id="c1c60-158">I den här självstudien har du uppdaterat ett program och distribuerat uppdateringen till ett Kubernetes-kluster.</span><span class="sxs-lookup"><span data-stu-id="c1c60-158">In this tutorial, you updated an application and rolled out this update to a Kubernetes cluster.</span></span> <span data-ttu-id="c1c60-159">Följande uppgifter har slutförts:</span><span class="sxs-lookup"><span data-stu-id="c1c60-159">The following tasks were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1c60-160">Uppdaterade klientdelens programkod</span><span class="sxs-lookup"><span data-stu-id="c1c60-160">Updated the front-end application code</span></span>
> * <span data-ttu-id="c1c60-161">Skapade en uppdaterad behållaravbildning</span><span class="sxs-lookup"><span data-stu-id="c1c60-161">Created an updated container image</span></span>
> * <span data-ttu-id="c1c60-162">Push-överförde behållaravbildningen till Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="c1c60-162">Pushed the container image to Azure Container Registry</span></span>
> * <span data-ttu-id="c1c60-163">Distribuerade det uppdaterade programmet</span><span class="sxs-lookup"><span data-stu-id="c1c60-163">Deployed the updated application</span></span>

<span data-ttu-id="c1c60-164">Gå vidare till nästa självstudie om du vill lära dig hur du övervakar Kubernetes med Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="c1c60-164">Advance to the next tutorial to learn about how to monitor Kubernetes with Operations Management Suite.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c1c60-165">Övervaka Kubernetes med OMS (Monitor Kubernetes with OMS)</span><span class="sxs-lookup"><span data-stu-id="c1c60-165">Monitor Kubernetes with OMS</span></span>](./container-service-tutorial-kubernetes-monitor.md)