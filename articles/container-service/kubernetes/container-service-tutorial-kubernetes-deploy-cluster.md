---
title: "aaaAzure Container Service tutorial – distribuera klustret | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - distribuera kluster"
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
ms.openlocfilehash: c4c8cc95c88d9c2077d0322f57e5d3159e2dd0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a><span data-ttu-id="8ec6d-104">Distribuera ett Kubernetes kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="8ec6d-104">Deploy a Kubernetes cluster in Azure Container Service</span></span>

<span data-ttu-id="8ec6d-105">Kubernetes tillhandahåller en distribuerad plattform för av program.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-105">Kubernetes provides a distributed platform for containerized applications.</span></span> <span data-ttu-id="8ec6d-106">Med Azure Container Service är etablering av en klar Kubernetes produktionskluster snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-106">With Azure Container Service, provisioning of a production ready Kubernetes cluster is simple and quick.</span></span> <span data-ttu-id="8ec6d-107">I den här självstudiekursen, del 3 i 7, distribueras ett Azure Container Service Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-107">In this tutorial, part 3 of 7, an Azure Container Service Kubernetes cluster is deployed.</span></span> <span data-ttu-id="8ec6d-108">Slutfört stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="8ec6d-108">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8ec6d-109">Distribuera en Kubernetes ACS-kluster</span><span class="sxs-lookup"><span data-stu-id="8ec6d-109">Deploying a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="8ec6d-110">Installation av hello Kubernetes CLI (kubectl)</span><span class="sxs-lookup"><span data-stu-id="8ec6d-110">Installation of hello Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="8ec6d-111">Konfigurationen av kubectl</span><span class="sxs-lookup"><span data-stu-id="8ec6d-111">Configuration of kubectl</span></span>

<span data-ttu-id="8ec6d-112">I efterföljande självstudiekurser hello Azure rösten program är distribuerat toohello kluster, skalas, uppdateras och Operations Management Suite är konfigurerade toomonitor hello Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-112">In subsequent tutorials, hello Azure Vote application is deployed toohello cluster, scaled, updated, and Operations Management Suite is configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8ec6d-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="8ec6d-113">Before you begin</span></span>

<span data-ttu-id="8ec6d-114">I föregående självstudiekurser en behållare avbildning har skapats och överföra tooan Azure Container registret instans.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-114">In previous tutorials, a container image was created and uploaded tooan Azure Container Registry instance.</span></span> <span data-ttu-id="8ec6d-115">Om du inte har gjort dessa steg och vill toofollow längs, returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="8ec6d-115">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span>

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="8ec6d-116">Skapa Kubernetes-kluster</span><span class="sxs-lookup"><span data-stu-id="8ec6d-116">Create Kubernetes cluster</span></span>

<span data-ttu-id="8ec6d-117">I hello [tidigare kursen](./container-service-tutorial-kubernetes-prepare-acr.md), en resursgrupp med namnet *myResourceGroup* skapades.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-117">In hello [previous tutorial](./container-service-tutorial-kubernetes-prepare-acr.md), a resource group named *myResourceGroup* was created.</span></span> <span data-ttu-id="8ec6d-118">Om du inte har gjort det, skapa den här resursgruppen nu.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-118">If you have not done so, create this resource group now.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="8ec6d-119">Skapa ett Kubernetes-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-119">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="8ec6d-120">hello följande exempel skapar ett kluster med namnet *myK8sCluster* med en Linux master-nod och tre noder för Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-120">hello following example creates a cluster named *myK8sCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

<span data-ttu-id="8ec6d-121">Om några minuter hello-kommandot har slutförts och returnerar json-formaterad information om hello ACS-distribution.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-121">After several minutes, hello command completes, and returns json formatted information about hello ACS deployment.</span></span>

## <a name="install-hello-kubectl-cli"></a><span data-ttu-id="8ec6d-122">Installera hello kubectl CLI</span><span class="sxs-lookup"><span data-stu-id="8ec6d-122">Install hello kubectl CLI</span></span>

<span data-ttu-id="8ec6d-123">tooconnect toohello Kubernetes kluster från din klientdator, Använd [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes kommandorads-klient.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-123">tooconnect toohello Kubernetes cluster from your client computer, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="8ec6d-124">Om du använder Azure CloudShell är `kubectl` redan installerad.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-124">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="8ec6d-125">Om du vill tooinstall den lokalt, använda hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) kommando.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-125">If you want tooinstall it locally, use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="8ec6d-126">Om du kör i Linux eller macOS behöva toorun med sudo.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-126">If running in Linux or macOS, you may need toorun with sudo.</span></span> <span data-ttu-id="8ec6d-127">Windows, se till att gränssnittet har körts som administratör.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-127">On Windows, ensure your shell has been run as administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli 
```

<span data-ttu-id="8ec6d-128">På Windows hello standardinstallation är *c:\program files (x86)\kubectl.exe*.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-128">On Windows, hello default installation is *c:\program files (x86)\kubectl.exe*.</span></span> <span data-ttu-id="8ec6d-129">Du kan behöva tooadd den här sökvägen toohello Windows.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-129">You may need tooadd this file toohello Windows path.</span></span> 

## <a name="connect-with-kubectl"></a><span data-ttu-id="8ec6d-130">Ansluta med kubectl</span><span class="sxs-lookup"><span data-stu-id="8ec6d-130">Connect with kubectl</span></span>

<span data-ttu-id="8ec6d-131">tooconfigure `kubectl` tooconnect tooyour Kubernetes klustret, kör hello [az acs kubernetes get-autentiseringsuppgifter](/cli/azure/acs/kubernetes#get-credentials) kommando.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-131">tooconfigure `kubectl` tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

<span data-ttu-id="8ec6d-132">tooverify hello anslutning tooyour klustret, kör hello [kubectl hämta noder](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-132">tooverify hello connection tooyour cluster, run hello [kubectl get nodes](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="8ec6d-133">Resultat:</span><span class="sxs-lookup"><span data-stu-id="8ec6d-133">Output:</span></span>

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

<span data-ttu-id="8ec6d-134">I kursen slutförande har du ett Kubernetes ACS-kluster redo för arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-134">At tutorial completion, you have an ACS Kubernetes cluster ready for workloads.</span></span> <span data-ttu-id="8ec6d-135">I efterföljande självstudiekurser är ett program för flera behållare distribuerade toothis kluster, skala ut, uppdateras och övervakas.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-135">In subsequent tutorials, a multi-container application is deployed toothis cluster, scaled out, updated, and monitored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ec6d-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8ec6d-136">Next steps</span></span>

<span data-ttu-id="8ec6d-137">Ett Azure Container Service Kubernetes kluster har distribuerats i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-137">In this tutorial, an Azure Container Service Kubernetes cluster was deployed.</span></span> <span data-ttu-id="8ec6d-138">hello följande steg har slutförts:</span><span class="sxs-lookup"><span data-stu-id="8ec6d-138">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8ec6d-139">Distribuera ett Kubernetes ACS-kluster</span><span class="sxs-lookup"><span data-stu-id="8ec6d-139">Deployed a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="8ec6d-140">Installerade hello Kubernetes CLI (kubectl)</span><span class="sxs-lookup"><span data-stu-id="8ec6d-140">Installed hello Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="8ec6d-141">Konfigurerade kubectl</span><span class="sxs-lookup"><span data-stu-id="8ec6d-141">Configured kubectl</span></span>

<span data-ttu-id="8ec6d-142">Avancera toohello nästa självstudiekurs toolearn om att köra programmet på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8ec6d-142">Advance toohello next tutorial toolearn about running application on hello cluster.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8ec6d-143">Distribuera program i Kubernetes</span><span class="sxs-lookup"><span data-stu-id="8ec6d-143">Deploy application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-deploy-application.md)
