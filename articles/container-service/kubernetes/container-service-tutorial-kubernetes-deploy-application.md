---
title: "aaaAzure Container Service tutorial – distribuera programmet | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - distribuera program"
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
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7e2fa06d359caf83e684df3966624a6e9a8e7efa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-applications-in-kubernetes"></a><span data-ttu-id="e93a8-104">Köra program i Kubernetes</span><span class="sxs-lookup"><span data-stu-id="e93a8-104">Run applications in Kubernetes</span></span>

<span data-ttu-id="e93a8-105">I den här självstudiekursen del fyra av sju, ett exempelprogram har distribuerats i ett Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="e93a8-105">In this tutorial, part four of seven, a sample application is deployed into a Kubernetes cluster.</span></span> <span data-ttu-id="e93a8-106">Slutfört stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="e93a8-106">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e93a8-107">Hämta Kubernetes manifestfiler</span><span class="sxs-lookup"><span data-stu-id="e93a8-107">Download Kubernetes manifest files</span></span>
> * <span data-ttu-id="e93a8-108">Kör program i Kubernetes</span><span class="sxs-lookup"><span data-stu-id="e93a8-108">Run application in Kubernetes</span></span>
> * <span data-ttu-id="e93a8-109">Testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="e93a8-109">Test hello application</span></span>

<span data-ttu-id="e93a8-110">Det här programmet skalas ut, uppdateras i efterföljande självstudiekurser och Operations Management Suite konfigureras toomonitor hello Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="e93a8-110">In subsequent tutorials, this application is scaled out, updated, and Operations Management Suite configured toomonitor hello Kubernetes cluster.</span></span>

<span data-ttu-id="e93a8-111">Den här kursen förutsätter grundläggande kunskaper om Kubernetes begrepp, detaljerad information om Kubernetes finns hello [Kubernetes dokumentationen](https://kubernetes.io/docs/home/).</span><span class="sxs-lookup"><span data-stu-id="e93a8-111">This tutorial assumes a basic understanding of Kubernetes concepts, for detailed information on Kubernetes see hello [Kubernetes documentation](https://kubernetes.io/docs/home/).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="e93a8-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="e93a8-112">Before you begin</span></span>

<span data-ttu-id="e93a8-113">Ett program som har paketerats till en behållare bild i föregående självstudiekurser, avbildningen har överförda tooAzure behållare registret och en Kubernetes klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="e93a8-113">In previous tutorials, an application was packaged into a container image, this image was uploaded tooAzure Container Registry, and a Kubernetes cluster was created.</span></span> <span data-ttu-id="e93a8-114">Om du inte har gjort dessa steg och vill toofollow längs, returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="e93a8-114">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="e93a8-115">Den här kursen kräver åtminstone ett Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="e93a8-115">At minimum, this tutorial requires a Kubernetes cluster.</span></span>

## <a name="get-manifest-file"></a><span data-ttu-id="e93a8-116">Hämta manifestfilen</span><span class="sxs-lookup"><span data-stu-id="e93a8-116">Get manifest file</span></span>

<span data-ttu-id="e93a8-117">Den här självstudien [Kubernetes objekt](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) distribueras med ett Kubernetes manifest.</span><span class="sxs-lookup"><span data-stu-id="e93a8-117">For this tutorial, [Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) are deployed using a Kubernetes manifest.</span></span> <span data-ttu-id="e93a8-118">Ett manifest för Kubernetes är en YAML eller JSON formaterad fil som innehåller Kubernetes objektet anvisningar för distribution och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e93a8-118">A Kubernetes manifest is a YAML or JSON formatted file containing Kubernetes object deployment and configuration instructions.</span></span>

<span data-ttu-id="e93a8-119">Hej programmanifestfilen för den här kursen finns i hello Azure rösten programmet lagringsplatsen som klonades i en tidigare vägledning.</span><span class="sxs-lookup"><span data-stu-id="e93a8-119">hello application manifest file for this tutorial is available in hello Azure Vote application repo, which was cloned in a previous tutorial.</span></span> <span data-ttu-id="e93a8-120">Om du inte redan har gjort det klona lagringsplatsen hello med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e93a8-120">If you have not already done so, clone hello repo with hello following command:</span></span> 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="e93a8-121">hello manifestfilen finns i hello följande katalogen för hello klona lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="e93a8-121">hello manifest file is found in hello following directory of hello cloned repo.</span></span>

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

## <a name="update-manifest-file"></a><span data-ttu-id="e93a8-122">Uppdatera manifestfilen</span><span class="sxs-lookup"><span data-stu-id="e93a8-122">Update manifest file</span></span>

<span data-ttu-id="e93a8-123">Om du använder Azure-behållare registret toostore hello behållaren bilder, uppdateras hello manifestet måste toobe med hello ACR loginServer namn.</span><span class="sxs-lookup"><span data-stu-id="e93a8-123">If using Azure Container Registry toostore hello container images, hello manifest needs toobe updated with hello ACR loginServer name.</span></span>

<span data-ttu-id="e93a8-124">Hämta hello ACR inloggningsnamnet server med hello [az acr lista](/cli/azure/acr#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="e93a8-124">Get hello ACR login server name with hello [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="e93a8-125">hello exempel manifestet har skapats i förväg med databasnamnet *microsoft*.</span><span class="sxs-lookup"><span data-stu-id="e93a8-125">hello sample manifest has been pre-created with a repository name of *microsoft*.</span></span> <span data-ttu-id="e93a8-126">Öppna hello-filen med en textredigerare och Ersätt hello *microsoft* värdet med hello inloggningen servernamnet för din ACR-instans.</span><span class="sxs-lookup"><span data-stu-id="e93a8-126">Open hello file with any text editor, and replace hello *microsoft* value with hello login server name of your ACR instance.</span></span>

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a><span data-ttu-id="e93a8-127">Distribuera program</span><span class="sxs-lookup"><span data-stu-id="e93a8-127">Deploy application</span></span>

<span data-ttu-id="e93a8-128">Använd hello [kubectl skapa](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) kommandot toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="e93a8-128">Use hello [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) command toorun hello application.</span></span> <span data-ttu-id="e93a8-129">Det här kommandot Parsar hello manifestfilen och skapa hello definierats Kubernetes objekt.</span><span class="sxs-lookup"><span data-stu-id="e93a8-129">This command parses hello manifest file and create hello defined Kubernetes objects.</span></span>

```azurecli-interactive
kubectl create -f ./azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

<span data-ttu-id="e93a8-130">Resultat:</span><span class="sxs-lookup"><span data-stu-id="e93a8-130">Output:</span></span>

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a><span data-ttu-id="e93a8-131">Testa program</span><span class="sxs-lookup"><span data-stu-id="e93a8-131">Test application</span></span>

<span data-ttu-id="e93a8-132">En [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) skapas som visar hello programmet toohello internet.</span><span class="sxs-lookup"><span data-stu-id="e93a8-132">A [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) is created which exposes hello application toohello internet.</span></span> <span data-ttu-id="e93a8-133">Den här processen kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="e93a8-133">This process can take a few minutes.</span></span> 

<span data-ttu-id="e93a8-134">toomonitor pågår, Använd hello [kubectl hämta service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) med hello `--watch` argumentet.</span><span class="sxs-lookup"><span data-stu-id="e93a8-134">toomonitor progress, use hello [kubectl get service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) command with hello `--watch` argument.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

<span data-ttu-id="e93a8-135">Inledningsvis hello **externa IP-** för hello *azure rösten sista* tjänsten visas som *väntande*.</span><span class="sxs-lookup"><span data-stu-id="e93a8-135">Initially, hello **EXTERNAL-IP** for hello *azure-vote-front* service appears as *pending*.</span></span> <span data-ttu-id="e93a8-136">När hello extern IP-adress har ändrats från *väntande* tooan *IP-adress*, använda `CTRL-C` toostop hello kubectl titta på processen.</span><span class="sxs-lookup"><span data-stu-id="e93a8-136">Once hello EXTERNAL-IP address has changed from *pending* tooan *IP address*, use `CTRL-C` toostop hello kubectl watch process.</span></span>

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

<span data-ttu-id="e93a8-137">toosee hello program, bläddra toohello extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e93a8-137">toosee hello application, browse toohello external IP address.</span></span>

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a><span data-ttu-id="e93a8-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e93a8-139">Next steps</span></span>

<span data-ttu-id="e93a8-140">I den här självstudiekursen har hello Azure rösten program distribuerade tooan Kubernetes för Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="e93a8-140">In this tutorial, hello Azure vote application was deployed tooan Azure Container Service Kubernetes cluster.</span></span> <span data-ttu-id="e93a8-141">Aktiviteter har slutförts är:</span><span class="sxs-lookup"><span data-stu-id="e93a8-141">Tasks completed include:</span></span>  

> [!div class="checklist"]
> * <span data-ttu-id="e93a8-142">Hämta Kubernetes manifestfiler</span><span class="sxs-lookup"><span data-stu-id="e93a8-142">Download Kubernetes manifest files</span></span>
> * <span data-ttu-id="e93a8-143">Kör hello program i Kubernetes</span><span class="sxs-lookup"><span data-stu-id="e93a8-143">Run hello application in Kubernetes</span></span>
> * <span data-ttu-id="e93a8-144">Testad hello program</span><span class="sxs-lookup"><span data-stu-id="e93a8-144">Tested hello application</span></span>

<span data-ttu-id="e93a8-145">Avancera toohello nästa självstudiekurs toolearn om att skala både ett Kubernetes program och hello underliggande Kubernetes infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="e93a8-145">Advance toohello next tutorial toolearn about scaling both a Kubernetes application and hello underlying Kubernetes infrastructure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="e93a8-146">Skala Kubernetes program och infrastruktur</span><span class="sxs-lookup"><span data-stu-id="e93a8-146">Scale Kubernetes application and infrastructure</span></span>](./container-service-tutorial-kubernetes-scale.md)