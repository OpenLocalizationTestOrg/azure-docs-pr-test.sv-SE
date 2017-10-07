---
title: "aaaDeploy en Docker-behållare kluster - Azure CLI | Microsoft Docs"
description: "Distribuera en Kubernetes-, DC/OS- eller Docker Swarm-lösning i Azure Container Service med hjälp av Azure CLI 2.0"
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: saudas
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: cdfa4ce69de343dcc7070bc2c58b5132c4062084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-hello-azure-cli-20"></a><span data-ttu-id="68c7c-103">Distribuera en dockerbehållare som värd-lösning med hjälp av hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="68c7c-103">Deploy a Docker container hosting solution using hello Azure CLI 2.0</span></span>

<span data-ttu-id="68c7c-104">Använd hello `az acs` kommandon i hello Azure CLI 2.0 toocreate och hantera kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="68c7c-104">Use hello `az acs` commands in hello Azure CLI 2.0 toocreate and manage clusters in Azure Container Service.</span></span> <span data-ttu-id="68c7c-105">Du kan också distribuera ett Azure Container Service-kluster med hjälp av hello [Azure-portalen](container-service-deployment.md) eller hello Azure Container Service API: er.</span><span class="sxs-lookup"><span data-stu-id="68c7c-105">You can also deploy an Azure Container Service cluster by using hello [Azure portal](container-service-deployment.md) or hello Azure Container Service APIs.</span></span>

<span data-ttu-id="68c7c-106">Hjälp om `az acs` kommandon, skicka hello `-h` parametern tooany kommando.</span><span class="sxs-lookup"><span data-stu-id="68c7c-106">For help on `az acs` commands, pass hello `-h` parameter tooany command.</span></span> <span data-ttu-id="68c7c-107">Till exempel: `az acs create -h`.</span><span class="sxs-lookup"><span data-stu-id="68c7c-107">For example: `az acs create -h`.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="68c7c-108">Krav</span><span class="sxs-lookup"><span data-stu-id="68c7c-108">Prerequisites</span></span>
<span data-ttu-id="68c7c-109">ett Azure Container Service-kluster med toocreate hello Azure CLI 2.0 måste du:</span><span class="sxs-lookup"><span data-stu-id="68c7c-109">toocreate an Azure Container Service cluster using hello Azure CLI 2.0, you must:</span></span>
* <span data-ttu-id="68c7c-110">ha ett Azure-konto ([hämta en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/))</span><span class="sxs-lookup"><span data-stu-id="68c7c-110">have an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/))</span></span>
* <span data-ttu-id="68c7c-111">har installerat och konfigurerat hello [Azure CLI 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="68c7c-111">have installed and set up hello [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

## <a name="get-started"></a><span data-ttu-id="68c7c-112">Kom igång</span><span class="sxs-lookup"><span data-stu-id="68c7c-112">Get started</span></span> 
### <a name="log-in-tooyour-account"></a><span data-ttu-id="68c7c-113">Logga in tooyour konto</span><span class="sxs-lookup"><span data-stu-id="68c7c-113">Log in tooyour account</span></span>
```azurecli
az login 
```

<span data-ttu-id="68c7c-114">Följ hello prompter toolog i interaktivt.</span><span class="sxs-lookup"><span data-stu-id="68c7c-114">Follow hello prompts toolog in interactively.</span></span> <span data-ttu-id="68c7c-115">Andra metoder toolog i finns [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="68c7c-115">For other methods toolog in, see [Get started with Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).</span></span>

### <a name="set-your-azure-subscription"></a><span data-ttu-id="68c7c-116">Ange din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="68c7c-116">Set your Azure subscription</span></span>

<span data-ttu-id="68c7c-117">Om du har mer än en Azure-prenumeration kan du ange hello standard prenumeration.</span><span class="sxs-lookup"><span data-stu-id="68c7c-117">If you have more than one Azure subscription, set hello default subscription.</span></span> <span data-ttu-id="68c7c-118">Exempel:</span><span class="sxs-lookup"><span data-stu-id="68c7c-118">For example:</span></span>

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a><span data-ttu-id="68c7c-119">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="68c7c-119">Create a resource group</span></span>
<span data-ttu-id="68c7c-120">Vi rekommenderar att du skapar en resursgrupp för varje kluster.</span><span class="sxs-lookup"><span data-stu-id="68c7c-120">We recommend that you create a resource group for every cluster.</span></span> <span data-ttu-id="68c7c-121">Ange en Azure-region där Azure Container Service är [tillgänglig](https://azure.microsoft.com/en-us/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="68c7c-121">Specify an Azure region in which Azure Container Service is [available](https://azure.microsoft.com/en-us/regions/services/).</span></span> <span data-ttu-id="68c7c-122">Exempel:</span><span class="sxs-lookup"><span data-stu-id="68c7c-122">For example:</span></span>

```azurecli
az group create -n acsrg1 -l "westus"
```
<span data-ttu-id="68c7c-123">Utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="68c7c-123">Output is similar toohello following:</span></span>

![Skapa en resursgrupp](./media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a><span data-ttu-id="68c7c-125">Skapa ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="68c7c-125">Create an Azure Container Service cluster</span></span>

<span data-ttu-id="68c7c-126">toocreate ett kluster, använda `az acs create`.</span><span class="sxs-lookup"><span data-stu-id="68c7c-126">toocreate a cluster, use `az acs create`.</span></span>
<span data-ttu-id="68c7c-127">Ett namn för hello kluster och hello på hello resursgruppen som skapades i föregående steg i hello är obligatoriska parametrar.</span><span class="sxs-lookup"><span data-stu-id="68c7c-127">A name for hello cluster and hello name of hello resource group created in hello previous step are mandatory parameters.</span></span> 

<span data-ttu-id="68c7c-128">Andra indata är toodefault värden (se följande skärm hello) om inte skrivas över med hjälp av sina respektive växlar.</span><span class="sxs-lookup"><span data-stu-id="68c7c-128">Other inputs are set toodefault values (see hello following screen) unless overwritten using their respective switches.</span></span> <span data-ttu-id="68c7c-129">Hej orchestrator är som standard tooDC/OS.</span><span class="sxs-lookup"><span data-stu-id="68c7c-129">For example, hello orchestrator is set by default tooDC/OS.</span></span> <span data-ttu-id="68c7c-130">Och om du inte anger ett DNS-namnprefix skapas baserat på hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="68c7c-130">And if you don't specify one, a DNS name prefix is created based on hello cluster name.</span></span>

![Användning av az acs create](./media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a><span data-ttu-id="68c7c-132">Snabbt använda `acs create` med standardinställningarna</span><span class="sxs-lookup"><span data-stu-id="68c7c-132">Quick `acs create` using defaults</span></span>
<span data-ttu-id="68c7c-133">Om du har en offentlig nyckelfil SSH RSA `id_rsa.pub` hello standardplatsen (eller skapa en för [OS X- och Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) eller [Windows](../../virtual-machines/linux/ssh-from-windows.md)), använder du ett kommando som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="68c7c-133">If you have an SSH RSA public key file `id_rsa.pub` in hello default location (or created one for [OS X and Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../../virtual-machines/linux/ssh-from-windows.md)), use a command like hello following:</span></span>

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
<span data-ttu-id="68c7c-134">Använd det här andra kommandot om du inte har en offentlig SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="68c7c-134">If you don't have an SSH public key, use this second command.</span></span> <span data-ttu-id="68c7c-135">Det här kommandot med hello `--generate-ssh-keys` växel skapas en åt dig.</span><span class="sxs-lookup"><span data-stu-id="68c7c-135">This command with hello `--generate-ssh-keys` switch creates one for you.</span></span>

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

<span data-ttu-id="68c7c-136">När du anger kommandot hello väntar du cirka 10 minuter för hello klustret toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="68c7c-136">After you enter hello command, wait for about 10 minutes for hello cluster toobe created.</span></span> <span data-ttu-id="68c7c-137">hello kommandoutdata inkluderar fullständigt kvalificerat domännamn (FQDN) för hello master och agent noder och en SSH-kommandot tooconnect toohello första huvudservern.</span><span class="sxs-lookup"><span data-stu-id="68c7c-137">hello command output includes fully qualified domain names (FQDNs) of hello master and agent nodes and an SSH command tooconnect toohello first master.</span></span> <span data-ttu-id="68c7c-138">Här är en sammanfattning över utdata:</span><span class="sxs-lookup"><span data-stu-id="68c7c-138">Here is abbreviated output:</span></span>

![Bild av acs create](./media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> <span data-ttu-id="68c7c-140">Hej [Kubernetes genomgången](../kubernetes/container-service-kubernetes-walkthrough.md) visar hur toouse `az acs create` med standard värden toocreate en Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="68c7c-140">hello [Kubernetes walkthrough](../kubernetes/container-service-kubernetes-walkthrough.md) shows how toouse `az acs create` with default values toocreate a Kubernetes cluster.</span></span>
>

## <a name="manage-acs-clusters"></a><span data-ttu-id="68c7c-141">Hantera ACS-kluster</span><span class="sxs-lookup"><span data-stu-id="68c7c-141">Manage ACS clusters</span></span>

<span data-ttu-id="68c7c-142">Använd ytterligare `az acs` kommandon toomanage klustret.</span><span class="sxs-lookup"><span data-stu-id="68c7c-142">Use additional `az acs` commands toomanage your cluster.</span></span> <span data-ttu-id="68c7c-143">Här följer några exempel.</span><span class="sxs-lookup"><span data-stu-id="68c7c-143">Here are some examples.</span></span>

### <a name="list-clusters-under-a-subscription"></a><span data-ttu-id="68c7c-144">Visa en lista över kluster under en prenumeration</span><span class="sxs-lookup"><span data-stu-id="68c7c-144">List clusters under a subscription</span></span>

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a><span data-ttu-id="68c7c-145">Visa en lista över kluster i en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="68c7c-145">List clusters in a resource group</span></span>

```azurecli
az acs list -g acsrg1 --output table
```

![ACS-lista](./media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a><span data-ttu-id="68c7c-147">Visa detaljer för ett behållartjänstkluster</span><span class="sxs-lookup"><span data-stu-id="68c7c-147">Display details of a container service cluster</span></span>

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![Visa ACS](./media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-hello-cluster"></a><span data-ttu-id="68c7c-149">Hello kluster</span><span class="sxs-lookup"><span data-stu-id="68c7c-149">Scale hello cluster</span></span>
<span data-ttu-id="68c7c-150">Du kan både skala in och ut en agentnod.</span><span class="sxs-lookup"><span data-stu-id="68c7c-150">Both scaling in and scaling out of agent nodes are allowed.</span></span> <span data-ttu-id="68c7c-151">Hej parametern `new-agent-count` är hello nya antalet agenter i hello ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="68c7c-151">hello parameter `new-agent-count` is hello new number of agents in hello ACS cluster.</span></span>

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![ACS-skala](./media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a><span data-ttu-id="68c7c-153">Ta bort ett behållartjänstkluster</span><span class="sxs-lookup"><span data-stu-id="68c7c-153">Delete a container service cluster</span></span>
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
<span data-ttu-id="68c7c-154">Det här kommandot tar inte bort alla resurser (nätverk och lagring) som skapas när du skapar hello behållartjänsten.</span><span class="sxs-lookup"><span data-stu-id="68c7c-154">This command does not delete all resources (network and storage) created while creating hello container service.</span></span> <span data-ttu-id="68c7c-155">toodelete alla resurser enkelt, bör du distribuera varje kluster i en distinkta resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="68c7c-155">toodelete all resources easily, it is recommended you deploy each cluster in a distinct resource group.</span></span> <span data-ttu-id="68c7c-156">Ta sedan bort hello resursgrupp när hello klustret inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="68c7c-156">Then, delete hello resource group when hello cluster is no longer required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68c7c-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68c7c-157">Next steps</span></span>
<span data-ttu-id="68c7c-158">Nu när du har ett fungerande kluster kan du visa dessa dokument för anslutnings- och hanteringsinformation:</span><span class="sxs-lookup"><span data-stu-id="68c7c-158">Now that you have a functioning cluster, see these documents for connection and management details:</span></span>

* [<span data-ttu-id="68c7c-159">Ansluta tooan Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="68c7c-159">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)
* [<span data-ttu-id="68c7c-160">Arbeta med Azure Container Service och DC/OS</span><span class="sxs-lookup"><span data-stu-id="68c7c-160">Work with Azure Container Service and DC/OS</span></span>](container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="68c7c-161">Arbeta med Azure Container Service och Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="68c7c-161">Work with Azure Container Service and Docker Swarm</span></span>](container-service-docker-swarm.md)
* [<span data-ttu-id="68c7c-162">Arbeta med Azure Container Service och Kubernetes</span><span class="sxs-lookup"><span data-stu-id="68c7c-162">Work with Azure Container Service and Kubernetes</span></span>](../kubernetes/container-service-kubernetes-walkthrough.md)