---
title: "aaaQuickstart - Azure Kubernetes kluster för Windows | Microsoft Docs"
description: "Lär dig snabbt toocreate ett Kubernetes kluster för Windows-behållare i Azure Container Service med hello Azure CLI."
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 85fe65a46ae8c78797e8a8a097c2a37f06329335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-windows-containers"></a><span data-ttu-id="55229-103">Distribuera Kubernets-kluster för Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="55229-103">Deploy Kubernetes cluster for Windows containers</span></span>

<span data-ttu-id="55229-104">hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="55229-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="55229-105">Den här guiden information med hjälp av hello Azure CLI toodeploy en [Kubernetes](https://kubernetes.io/docs/home/) klustret i [Azure Container Service](../container-service-intro.md).</span><span class="sxs-lookup"><span data-stu-id="55229-105">This guide details using hello Azure CLI toodeploy a [Kubernetes](https://kubernetes.io/docs/home/) cluster in [Azure Container Service](../container-service-intro.md).</span></span> <span data-ttu-id="55229-106">När hello klustret distribueras kan du ansluta tooit med hello Kubernetes `kubectl` kommandoradsverktyget och distribuera din första Windows-behållare.</span><span class="sxs-lookup"><span data-stu-id="55229-106">Once hello cluster is deployed, you connect tooit with hello Kubernetes `kubectl` command-line tool, and you deploy your first Windows container.</span></span>

<span data-ttu-id="55229-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="55229-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="55229-108">Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="55229-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="55229-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="55229-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="55229-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="55229-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

> [!NOTE]
> <span data-ttu-id="55229-111">Stöd för Windows-behållare med Kubernetes i Azure Container Service är tillgängligt som en förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="55229-111">Support for Windows containers on Kubernetes in Azure Container Service is in preview.</span></span> 
>

## <a name="create-a-resource-group"></a><span data-ttu-id="55229-112">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="55229-112">Create a resource group</span></span>

<span data-ttu-id="55229-113">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="55229-113">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="55229-114">En Azure-resursgrupp är en logisk grupp där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="55229-114">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="55229-115">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="55229-115">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="55229-116">Skapa Kubernetes-kluster</span><span class="sxs-lookup"><span data-stu-id="55229-116">Create Kubernetes cluster</span></span>
<span data-ttu-id="55229-117">Skapa ett Kubernetes-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="55229-117">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="55229-118">hello följande exempel skapar ett kluster med namnet *myK8sCluster* med en Linux master-nod och två noder för Windows-agenten.</span><span class="sxs-lookup"><span data-stu-id="55229-118">hello following example creates a cluster named *myK8sCluster* with one Linux master node and two Windows agent nodes.</span></span> <span data-ttu-id="55229-119">Det här exemplet skapar SSH nycklar krävs tooconnect toohello Linux master.</span><span class="sxs-lookup"><span data-stu-id="55229-119">This example creates SSH keys needed tooconnect toohello Linux master.</span></span> <span data-ttu-id="55229-120">Det här exemplet används *azureuser* för en administrativ användarnamn och *myPassword12* som hello lösenord på hello Windows noder.</span><span class="sxs-lookup"><span data-stu-id="55229-120">This example uses *azureuser* for an administrative user name and *myPassword12* as hello password on hello Windows nodes.</span></span> <span data-ttu-id="55229-121">Uppdatera dessa värden toosomething lämpliga tooyour miljö.</span><span class="sxs-lookup"><span data-stu-id="55229-121">Update these values toosomething appropriate tooyour environment.</span></span> 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

<span data-ttu-id="55229-122">Om några minuter hello-kommandot har slutförts och visar information om din distribution.</span><span class="sxs-lookup"><span data-stu-id="55229-122">After several minutes, hello command completes, and shows you information about your deployment.</span></span>

## <a name="install-kubectl"></a><span data-ttu-id="55229-123">Installera kubectl</span><span class="sxs-lookup"><span data-stu-id="55229-123">Install kubectl</span></span>

<span data-ttu-id="55229-124">tooconnect toohello Kubernetes kluster från din klientdator, Använd [ `kubectl` ](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes kommandorads-klient.</span><span class="sxs-lookup"><span data-stu-id="55229-124">tooconnect toohello Kubernetes cluster from your client computer, use [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="55229-125">Om du använder Azure CloudShell är `kubectl` redan installerad.</span><span class="sxs-lookup"><span data-stu-id="55229-125">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="55229-126">Om du vill tooinstall lokalt, du kan använda hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) kommando.</span><span class="sxs-lookup"><span data-stu-id="55229-126">If you want tooinstall it locally, you can use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="55229-127">Hej följande Azure CLI exempel installerar `kubectl` tooyour system.</span><span class="sxs-lookup"><span data-stu-id="55229-127">hello following Azure CLI example installs `kubectl` tooyour system.</span></span> <span data-ttu-id="55229-128">I Windows kör du kommandot som administratör.</span><span class="sxs-lookup"><span data-stu-id="55229-128">On Windows, run this command as an administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli
```


## <a name="connect-with-kubectl"></a><span data-ttu-id="55229-129">Ansluta med kubectl</span><span class="sxs-lookup"><span data-stu-id="55229-129">Connect with kubectl</span></span>

<span data-ttu-id="55229-130">tooconfigure `kubectl` tooconnect tooyour Kubernetes klustret, kör hello [az acs kubernetes get-autentiseringsuppgifter](/cli/azure/acs/kubernetes#get-credentials) kommando.</span><span class="sxs-lookup"><span data-stu-id="55229-130">tooconfigure `kubectl` tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="55229-131">hello hämtar följande exempel hello klusterkonfiguration för Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="55229-131">hello following example downloads hello cluster configuration for your Kubernetes cluster.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

<span data-ttu-id="55229-132">tooverify hello anslutning tooyour kluster från din dator, kör:</span><span class="sxs-lookup"><span data-stu-id="55229-132">tooverify hello connection tooyour cluster from your machine, try running:</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="55229-133">`kubectl`Visar hello-hanterare och agent noder.</span><span class="sxs-lookup"><span data-stu-id="55229-133">`kubectl` lists hello master and agent nodes.</span></span>

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```

## <a name="deploy-a-windows-iis-container"></a><span data-ttu-id="55229-134">Distribuera en Windows-IIS-behållare</span><span class="sxs-lookup"><span data-stu-id="55229-134">Deploy a Windows IIS container</span></span>

<span data-ttu-id="55229-135">Du kan köra en dockerbehållare i en Kubernetes-*pod*, som innehåller en eller flera behållare.</span><span class="sxs-lookup"><span data-stu-id="55229-135">You can run a Docker container inside a Kubernetes *pod*, which contains one or more containers.</span></span> 

<span data-ttu-id="55229-136">Det här enkla exemplet används en JSON-fil toospecify en behållare för Microsoft Internet Information Server (IIS) och skapar sedan hello baljor med hello `kubctl apply` kommando.</span><span class="sxs-lookup"><span data-stu-id="55229-136">This basic example uses a JSON file toospecify a Microsoft Internet Information Server (IIS) container, and then creates hello pod using hello `kubctl apply` command.</span></span> 

<span data-ttu-id="55229-137">Skapa en lokal fil med namnet `iis.json` och kopiera hello efter texten.</span><span class="sxs-lookup"><span data-stu-id="55229-137">Create a local file named `iis.json` and copy hello following text.</span></span> <span data-ttu-id="55229-138">Den här filen talar om Kubernetes toorun IIS på Windows Server 2016 Nano Server med en offentlig behållare bild från [Docker-hubb](https://hub.docker.com/r/nanoserver/iis/).</span><span class="sxs-lookup"><span data-stu-id="55229-138">This file tells Kubernetes toorun IIS on Windows Server 2016 Nano Server, using a public container image from [Docker Hub](https://hub.docker.com/r/nanoserver/iis/).</span></span> <span data-ttu-id="55229-139">hello behållare använder port 80, men är först endast tillgänglig hello klusternätverk.</span><span class="sxs-lookup"><span data-stu-id="55229-139">hello container uses port 80, but initially is only accessible within hello cluster network.</span></span>

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "nanoserver/iis",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

<span data-ttu-id="55229-140">toostart hello baljor, typ:</span><span class="sxs-lookup"><span data-stu-id="55229-140">toostart hello pod, type:</span></span>
  
```azurecli-interactive
kubectl apply -f iis.json
```  

<span data-ttu-id="55229-141">tootrack hello distribution, typ:</span><span class="sxs-lookup"><span data-stu-id="55229-141">tootrack hello deployment, type:</span></span>
  
```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="55229-142">När du distribuerar hello baljor hello status är `ContainerCreating`.</span><span class="sxs-lookup"><span data-stu-id="55229-142">While hello pod is deploying, hello status is `ContainerCreating`.</span></span> <span data-ttu-id="55229-143">Det kan ta några minuter för hello behållaren tooenter hello `Running` tillstånd.</span><span class="sxs-lookup"><span data-stu-id="55229-143">It can take a few minutes for hello container tooenter hello `Running` state.</span></span>

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="55229-144">Visa hello välkomstsidan för IIS</span><span class="sxs-lookup"><span data-stu-id="55229-144">View hello IIS welcome page</span></span>

<span data-ttu-id="55229-145">tooexpose baljor toohello hälsningsmeddelande med en offentlig IP-adress, typen hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="55229-145">tooexpose hello pod toohello world with a public IP address, type hello following command:</span></span>

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

<span data-ttu-id="55229-146">Med det här kommandot Kubernetes skapar en tjänst och en [Azure belastningsutjämningsregeln](container-service-kubernetes-load-balancing.md) med en offentlig IP-adress för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="55229-146">With this command, Kubernetes creates a service and an [Azure load balancer rule](container-service-kubernetes-load-balancing.md) with a public IP address for hello service.</span></span> 

<span data-ttu-id="55229-147">Kör följande kommando toosee hello hello tjänstens status hello.</span><span class="sxs-lookup"><span data-stu-id="55229-147">Run hello following command toosee hello status of hello service.</span></span>

```azurecli-interactive
kubectl get svc
```

<span data-ttu-id="55229-148">Hello IP-adress visas först som `pending`.</span><span class="sxs-lookup"><span data-stu-id="55229-148">Initially hello IP address appears as `pending`.</span></span> <span data-ttu-id="55229-149">Efter några minuter hello externa IP-adress hello `iis` baljor anges:</span><span class="sxs-lookup"><span data-stu-id="55229-149">After a few minutes, hello external IP address of hello `iis` pod is set:</span></span>
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

<span data-ttu-id="55229-150">Du kan använda en webbläsare för din choice toosee hello IIS välkomstsidan på hello extern IP-adress:</span><span class="sxs-lookup"><span data-stu-id="55229-150">You can use a web browser of your choice toosee hello default IIS welcome page at hello external IP address:</span></span>

![Bild av surfning tooIIS](./media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## <a name="delete-cluster"></a><span data-ttu-id="55229-152">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="55229-152">Delete cluster</span></span>
<span data-ttu-id="55229-153">När hello klustret inte längre behövs, kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, behållartjänsten och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="55229-153">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a><span data-ttu-id="55229-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55229-154">Next steps</span></span>

<span data-ttu-id="55229-155">I den här snabbstartsguide du har distribuerat ett Kubernetes-kluster med `kubectl` och distribuerat en pod med en IIS-behållare.</span><span class="sxs-lookup"><span data-stu-id="55229-155">In this quick start, you deployed a Kubernetes cluster, connected with `kubectl`, and deployed a pod with an IIS container.</span></span> <span data-ttu-id="55229-156">toolearn mer om Azure Container Service fortsätta toohello Kubernetes kursen.</span><span class="sxs-lookup"><span data-stu-id="55229-156">toolearn more about Azure Container Service, continue toohello Kubernetes tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="55229-157">Hantera ett ACS Kubernets-kluster</span><span class="sxs-lookup"><span data-stu-id="55229-157">Manage an ACS Kubernetes cluster</span></span>](container-service-tutorial-kubernetes-prepare-app.md)
