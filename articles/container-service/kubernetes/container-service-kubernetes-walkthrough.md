---
title: "aaaQuickstart - Azure Kubernetes kluster för Linux | Microsoft Docs"
description: "Lär dig snabbt toocreate ett Kubernetes kluster på Linux-behållare i Azure Container Service med hello Azure CLI."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 8b0d7a803148c1cbf329f4b76f2e99b4b7e14983
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-linux-containers"></a><span data-ttu-id="2a86b-103">Distribuera Kubernets-kluster för Linux-behållare</span><span class="sxs-lookup"><span data-stu-id="2a86b-103">Deploy Kubernetes cluster for Linux containers</span></span>

<span data-ttu-id="2a86b-104">I den här snabbstartsguide distribueras ett Kubernetes kluster med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="2a86b-104">In this quick start, a Kubernetes cluster is deployed using hello Azure CLI.</span></span> <span data-ttu-id="2a86b-105">Ett program för flera behållare som består av frontwebb och en Redis-instans distribueras sedan och köra på hello kluster.</span><span class="sxs-lookup"><span data-stu-id="2a86b-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on hello cluster.</span></span> <span data-ttu-id="2a86b-106">När klar hello programmet är tillgänglig över hello internet.</span><span class="sxs-lookup"><span data-stu-id="2a86b-106">Once completed, hello application is accessible over hello internet.</span></span> 

<span data-ttu-id="2a86b-107">hello exempelprogram som används i det här dokumentet har skrivits i Python.</span><span class="sxs-lookup"><span data-stu-id="2a86b-107">hello example application used in this document is written in Python.</span></span> <span data-ttu-id="2a86b-108">hello begreppen och stegen som beskrivs här kan vara används toodeploy alla behållare bild i ett Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="2a86b-108">hello concepts and steps detailed here can be used toodeploy any container image into a Kubernetes cluster.</span></span> <span data-ttu-id="2a86b-109">hello kod, Dockerfile och förskapade Kubernetes manifestfiler relaterade toothis projektet är tillgängliga på [GitHub](https://github.com/Azure-Samples/azure-voting-app-redis.git).</span><span class="sxs-lookup"><span data-stu-id="2a86b-109">hello code, Dockerfile, and pre-created Kubernetes manifest files related toothis project are available on [GitHub](https://github.com/Azure-Samples/azure-voting-app-redis.git).</span></span>

![Bild av tooAzure röst-surfning](media/container-service-kubernetes-walkthrough/azure-vote.png)

<span data-ttu-id="2a86b-111">Den här snabbstartsguide förutsätter grundläggande kunskaper om Kubernetes begrepp, detaljerad information om Kubernetes finns hello [Kubernetes dokumentationen]( https://kubernetes.io/docs/home/).</span><span class="sxs-lookup"><span data-stu-id="2a86b-111">This quick start assumes a basic understanding of Kubernetes concepts, for detailed information on Kubernetes see hello [Kubernetes documentation]( https://kubernetes.io/docs/home/).</span></span>

<span data-ttu-id="2a86b-112">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="2a86b-112">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="2a86b-113">Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2a86b-113">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="2a86b-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="2a86b-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="2a86b-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2a86b-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="2a86b-116">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="2a86b-116">Create a resource group</span></span>

<span data-ttu-id="2a86b-117">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="2a86b-117">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="2a86b-118">En Azure-resursgrupp är en logisk grupp där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="2a86b-118">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="2a86b-119">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westeurope* plats.</span><span class="sxs-lookup"><span data-stu-id="2a86b-119">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="2a86b-120">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2a86b-120">Output:</span></span>

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westeurope",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="2a86b-121">Skapa Kubernetes-kluster</span><span class="sxs-lookup"><span data-stu-id="2a86b-121">Create Kubernetes cluster</span></span>

<span data-ttu-id="2a86b-122">Skapa ett Kubernetes-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="2a86b-122">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> <span data-ttu-id="2a86b-123">hello följande exempel skapar ett kluster med namnet *myK8sCluster* med en Linux master-nod och tre noder för Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="2a86b-123">hello following example creates a cluster named *myK8sCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8sCluster --generate-ssh-keys 
```

<span data-ttu-id="2a86b-124">Om några minuter hello-kommandot har slutförts och returnerar json-formaterade information om hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="2a86b-124">After several minutes, hello command completes and returns json formatted information about hello cluster.</span></span> 

## <a name="connect-toohello-cluster"></a><span data-ttu-id="2a86b-125">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="2a86b-125">Connect toohello cluster</span></span>

<span data-ttu-id="2a86b-126">toomanage ett Kubernetes kluster, använda [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes kommandorads-klient.</span><span class="sxs-lookup"><span data-stu-id="2a86b-126">toomanage a Kubernetes cluster, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="2a86b-127">Om du använder Azure CloudShell är kubectl redan installerat.</span><span class="sxs-lookup"><span data-stu-id="2a86b-127">If you're using Azure CloudShell, kubectl is already installed.</span></span> <span data-ttu-id="2a86b-128">Om du vill tooinstall lokalt, du kan använda hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) kommando.</span><span class="sxs-lookup"><span data-stu-id="2a86b-128">If you want tooinstall it locally, you can use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="2a86b-129">tooconfigure kubectl tooconnect tooyour Kubernetes klustret, kör hello [az acs kubernetes get-autentiseringsuppgifter](/cli/azure/acs/kubernetes#get-credentials) kommando.</span><span class="sxs-lookup"><span data-stu-id="2a86b-129">tooconfigure kubectl tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="2a86b-130">Det här steget hämtar autentiseringsuppgifter och konfigurerar hello Kubernetes CLI toouse dem.</span><span class="sxs-lookup"><span data-stu-id="2a86b-130">This step downloads credentials and configures hello Kubernetes CLI toouse them.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

<span data-ttu-id="2a86b-131">tooverify hello anslutning tooyour kluster, Använd hello [kubectl hämta](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommandot tooreturn en lista över hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="2a86b-131">tooverify hello connection tooyour cluster, use hello [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command tooreturn a list of hello cluster nodes.</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="2a86b-132">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2a86b-132">Output:</span></span>

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-14ad53a1-0    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-1    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-2    Ready                      10m       v1.6.6
k8s-master-14ad53a1-0   Ready,SchedulingDisabled   10m       v1.6.6
```

## <a name="run-hello-application"></a><span data-ttu-id="2a86b-133">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="2a86b-133">Run hello application</span></span>

<span data-ttu-id="2a86b-134">En manifestfil Kubernetes definierar en alterntiv hello klustret, inklusive vad behållaren bilder ska köras.</span><span class="sxs-lookup"><span data-stu-id="2a86b-134">A Kubernetes manifest file defines a desired state for hello cluster, including what container images should be running.</span></span> <span data-ttu-id="2a86b-135">I det här exemplet är ett manifest används toocreate alla objekt behövs toorun hello Azure rösten program.</span><span class="sxs-lookup"><span data-stu-id="2a86b-135">For this example, a manifest is used toocreate all objects needed toorun hello Azure Vote application.</span></span> 

<span data-ttu-id="2a86b-136">Skapa en fil med namnet `azure-vote.yml` och kopiera in hello följande YAML.</span><span class="sxs-lookup"><span data-stu-id="2a86b-136">Create a file named `azure-vote.yml` and copy into it hello following YAML.</span></span> <span data-ttu-id="2a86b-137">Om du arbetar i Azure Cloud Shell, kan du skapa filen med vi eller Nano som i ett virtuellt eller fysiskt system.</span><span class="sxs-lookup"><span data-stu-id="2a86b-137">If you are working in Azure Cloud Shell, this file can be created using vi or Nano as if working on a virtual or physical system.</span></span>

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:redis-v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

<span data-ttu-id="2a86b-138">Använd hello [kubectl skapa](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) kommandot toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="2a86b-138">Use hello [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) command toorun hello application.</span></span>

```azurecli-interactive
kubectl create -f azure-vote.yml
```

<span data-ttu-id="2a86b-139">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2a86b-139">Output:</span></span>

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-hello-application"></a><span data-ttu-id="2a86b-140">Testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="2a86b-140">Test hello application</span></span>

<span data-ttu-id="2a86b-141">Eftersom hello programmet körs är en [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) skapas som visar hello programmet klientdelen toohello internet.</span><span class="sxs-lookup"><span data-stu-id="2a86b-141">As hello application is run, a [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) is created that exposes hello application front end toohello internet.</span></span> <span data-ttu-id="2a86b-142">Den här processen kan ta några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2a86b-142">This process can take a few minutes toocomplete.</span></span> 

<span data-ttu-id="2a86b-143">toomonitor pågår, Använd hello [kubectl hämta service](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) med hello `--watch` argumentet.</span><span class="sxs-lookup"><span data-stu-id="2a86b-143">toomonitor progress, use hello [kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command with hello `--watch` argument.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

<span data-ttu-id="2a86b-144">Ursprungligen hello **externa IP-** för hello *azure rösten sista* tjänsten visas som *väntande*.</span><span class="sxs-lookup"><span data-stu-id="2a86b-144">Initially hello **EXTERNAL-IP** for hello *azure-vote-front* service appears as *pending*.</span></span> <span data-ttu-id="2a86b-145">När hello extern IP-adress har ändrats från *väntande* tooan *IP-adress*, använda `CTRL-C` toostop hello kubectl titta på processen.</span><span class="sxs-lookup"><span data-stu-id="2a86b-145">Once hello EXTERNAL-IP address has changed from *pending* tooan *IP address*, use `CTRL-C` toostop hello kubectl watch process.</span></span> 
  
```bash
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

<span data-ttu-id="2a86b-146">Du kan nu bläddra toohello externa IP-adress toosee hello Azure rösten App.</span><span class="sxs-lookup"><span data-stu-id="2a86b-146">You can now browse toohello external IP address toosee hello Azure Vote App.</span></span>

![Bild av tooAzure röst-surfning](media/container-service-kubernetes-walkthrough/azure-vote.png)  

## <a name="delete-cluster"></a><span data-ttu-id="2a86b-148">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="2a86b-148">Delete cluster</span></span>
<span data-ttu-id="2a86b-149">När hello klustret inte längre behövs, kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, behållartjänsten och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="2a86b-149">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a><span data-ttu-id="2a86b-150">Hämta hello kod</span><span class="sxs-lookup"><span data-stu-id="2a86b-150">Get hello code</span></span>

<span data-ttu-id="2a86b-151">I den här snabbstartsguide har skapats i förväg behållaren bilder används toocreate en Kubernetes-distribution.</span><span class="sxs-lookup"><span data-stu-id="2a86b-151">In this quick start, pre-created container images have been used toocreate a Kubernetes deployment.</span></span> <span data-ttu-id="2a86b-152">hello relaterade programkod, Dockerfile, och Kubernetes manifestfilen är tillgängliga på GitHub.</span><span class="sxs-lookup"><span data-stu-id="2a86b-152">hello related application code, Dockerfile, and Kubernetes manifest file are available on GitHub.</span></span>

[<span data-ttu-id="2a86b-153">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="2a86b-153">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="2a86b-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a86b-154">Next steps</span></span>

<span data-ttu-id="2a86b-155">I den här snabbstartsguide distribueras ett Kubernetes kluster och distribuerat en tooit för flera program.</span><span class="sxs-lookup"><span data-stu-id="2a86b-155">In this quick start, you deployed a Kubernetes cluster and deployed a multi-container application tooit.</span></span> 

<span data-ttu-id="2a86b-156">toolearn mer om Azure Container Service och gå igenom ett fullständigt toodeployment kodexempel, fortsätta toohello Kubernetes klustret kursen.</span><span class="sxs-lookup"><span data-stu-id="2a86b-156">toolearn more about Azure Container Service, and walk through a complete code toodeployment example, continue toohello Kubernetes cluster tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2a86b-157">Hantera ett ACS Kubernets-kluster</span><span class="sxs-lookup"><span data-stu-id="2a86b-157">Manage an ACS Kubernetes cluster</span></span>](./container-service-tutorial-kubernetes-prepare-app.md)
