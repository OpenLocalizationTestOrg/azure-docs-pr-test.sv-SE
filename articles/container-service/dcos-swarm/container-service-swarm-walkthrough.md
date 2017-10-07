---
title: "aaaQuickstart - Azure Docker Swarm-kluster för Linux | Microsoft Docs"
description: "Lär dig snabbt toocreate Docker Swarm-kluster på Linux-behållare i Azure Container Service med hello Azure CLI."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, Docker, Swarm
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/14/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 3028d2d00585360ec163518bf98f69bb0dd44dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-docker-swarm-cluster"></a><span data-ttu-id="b2c37-103">Distribuera Docker Swarm-kluster</span><span class="sxs-lookup"><span data-stu-id="b2c37-103">Deploy Docker Swarm cluster</span></span>

<span data-ttu-id="b2c37-104">I den här snabbstartsguide distribueras Docker Swarm-kluster med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b2c37-104">In this quick start, a Docker Swarm cluster is deployed using hello Azure CLI.</span></span> <span data-ttu-id="b2c37-105">Ett program för flera behållare som består av frontwebb och en Redis-instans distribueras sedan och köra på hello kluster.</span><span class="sxs-lookup"><span data-stu-id="b2c37-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on hello cluster.</span></span> <span data-ttu-id="b2c37-106">När klar hello programmet är tillgänglig över hello internet.</span><span class="sxs-lookup"><span data-stu-id="b2c37-106">Once completed, hello application is accessible over hello internet.</span></span>

<span data-ttu-id="b2c37-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="b2c37-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="b2c37-108">Denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b2c37-108">This quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b2c37-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="b2c37-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="b2c37-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b2c37-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="b2c37-111">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b2c37-111">Create a resource group</span></span>

<span data-ttu-id="b2c37-112">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="b2c37-112">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="b2c37-113">En Azure-resursgrupp är en logisk grupp där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="b2c37-113">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="b2c37-114">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westus* plats.</span><span class="sxs-lookup"><span data-stu-id="b2c37-114">hello following example creates a resource group named *myResourceGroup* in hello *westus* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="b2c37-115">Resultat:</span><span class="sxs-lookup"><span data-stu-id="b2c37-115">Output:</span></span>

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westcentralus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-docker-swarm-cluster"></a><span data-ttu-id="b2c37-116">Skapa Docker Swarm-kluster</span><span class="sxs-lookup"><span data-stu-id="b2c37-116">Create Docker Swarm cluster</span></span>

<span data-ttu-id="b2c37-117">Skapa ett Docker Swarm-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="b2c37-117">Create a Docker Swarm cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="b2c37-118">hello följande exempel skapar ett kluster med namnet *mySwarmCluster* med en Linux master-nod och tre noder för Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="b2c37-118">hello following example creates a cluster named *mySwarmCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

<span data-ttu-id="b2c37-119">Om några minuter hello-kommandot har slutförts och returnerar json-formaterade information om hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="b2c37-119">After several minutes, hello command completes and returns json formatted information about hello cluster.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="b2c37-120">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="b2c37-120">Connect toohello cluster</span></span>

<span data-ttu-id="b2c37-121">I den här snabbstartsguide måste hello IP-adressen för både hello Docker Swarm och hello Docker agent poolen.</span><span class="sxs-lookup"><span data-stu-id="b2c37-121">Throughout this quick start, you need hello IP address of both hello Docker Swarm master and hello Docker agent pool.</span></span> <span data-ttu-id="b2c37-122">Kör följande kommando tooreturn hello båda IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="b2c37-122">Run hello following command tooreturn both IP addresses.</span></span>


```bash
az network public-ip list --resource-group myResourceGroup --query '[*].{Name:name,IPAddress:ipAddress}' -o table
```

<span data-ttu-id="b2c37-123">Resultat:</span><span class="sxs-lookup"><span data-stu-id="b2c37-123">Output:</span></span>

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

<span data-ttu-id="b2c37-124">Skapa en SSH-tunnel toohello Swarm master.</span><span class="sxs-lookup"><span data-stu-id="b2c37-124">Create an SSH tunnel toohello Swarm master.</span></span> <span data-ttu-id="b2c37-125">Ersätt `IPAddress` med hello hello Swarm master IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b2c37-125">Replace `IPAddress` with hello IP address of hello Swarm master.</span></span>

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

<span data-ttu-id="b2c37-126">Ange hello `DOCKER_HOST` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="b2c37-126">Set hello `DOCKER_HOST` environment variable.</span></span> <span data-ttu-id="b2c37-127">Detta ger dig toorun docker kommandon mot hello Docker Swarm utan toospecify hello namnet på hello-värden.</span><span class="sxs-lookup"><span data-stu-id="b2c37-127">This allows you toorun docker commands against hello Docker Swarm without having toospecify hello name of hello host.</span></span>

```bash
export DOCKER_HOST=:2375
```

<span data-ttu-id="b2c37-128">Du är nu redo toorun Docker-tjänster på hello Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="b2c37-128">You are now ready toorun Docker services on hello Docker Swarm.</span></span>


## <a name="run-hello-application"></a><span data-ttu-id="b2c37-129">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="b2c37-129">Run hello application</span></span>

<span data-ttu-id="b2c37-130">Skapa en fil med namnet `docker-compose.yaml` och kopiera hello efter innehåll till den.</span><span class="sxs-lookup"><span data-stu-id="b2c37-130">Create a file named `docker-compose.yaml` and copy hello following content into it.</span></span>

```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    container_name: azure-vote-back
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    container_name: azure-vote-front
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

<span data-ttu-id="b2c37-131">Kör hello efter kommandot toocreate hello Azure rösten service.</span><span class="sxs-lookup"><span data-stu-id="b2c37-131">Run hello following command toocreate hello Azure Vote service.</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="b2c37-132">Resultat:</span><span class="sxs-lookup"><span data-stu-id="b2c37-132">Output:</span></span>

```bash
Creating network "user_default" with hello default driver
Pulling azure-vote-front (microsoft/azure-vote-front:redis-v1)...
swarm-agent-EE873B23000005: Pulling microsoft/azure-vote-front:redis-v1...
swarm-agent-EE873B23000004: Pulling microsoft/azure-vote-front:redis-v1... : downloaded
Pulling azure-vote-back (redis:latest)...
swarm-agent-EE873B23000004: Pulling redis:latest... : downloaded
Creating azure-vote-front ... 
Creating azure-vote-back ... 
Creating azure-vote-front
Creating azure-vote-back ...
```

## <a name="test-hello-application"></a><span data-ttu-id="b2c37-133">Testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="b2c37-133">Test hello application</span></span>

<span data-ttu-id="b2c37-134">Bläddra toohello IP-adressen för hello Swarm-agenten poolen tootest ut hello Azure rösten program.</span><span class="sxs-lookup"><span data-stu-id="b2c37-134">Browse toohello IP address of hello Swarm agent pool tootest out hello Azure Vote application.</span></span>

![Bild av tooAzure röst-surfning](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a><span data-ttu-id="b2c37-136">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="b2c37-136">Delete cluster</span></span>
<span data-ttu-id="b2c37-137">När hello klustret inte längre behövs, kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, behållartjänsten och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="b2c37-137">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a><span data-ttu-id="b2c37-138">Hämta hello kod</span><span class="sxs-lookup"><span data-stu-id="b2c37-138">Get hello code</span></span>

<span data-ttu-id="b2c37-139">I den här snabbstartsguide har skapats i förväg behållaren bilder används toocreate en Docker-tjänst.</span><span class="sxs-lookup"><span data-stu-id="b2c37-139">In this quick start, pre-created container images have been used toocreate a Docker service.</span></span> <span data-ttu-id="b2c37-140">hello relaterade programkod, Dockerfile, och Skriv fil finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="b2c37-140">hello related application code, Dockerfile, and Compose file are available on GitHub.</span></span>

[<span data-ttu-id="b2c37-141">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="b2c37-141">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="b2c37-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2c37-142">Next steps</span></span>

<span data-ttu-id="b2c37-143">I den här snabbstartsguide distribuerats Docker Swarm-kluster och distribuerat en tooit för flera program.</span><span class="sxs-lookup"><span data-stu-id="b2c37-143">In this quick start, you deployed a Docker Swarm cluster and deployed a multi-container application tooit.</span></span>

<span data-ttu-id="b2c37-144">toolearn om integreringen med Docker varmt med Visual Studio Team Services fortsätta toohello CI/CD med Docker Swarm och VSTS.</span><span class="sxs-lookup"><span data-stu-id="b2c37-144">toolearn about integrating Docker warm with Visual Studio Team Services, continue toohello CI/CD with Docker Swarm and VSTS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b2c37-145">CI/CD med Docker Swarm och VSTS</span><span class="sxs-lookup"><span data-stu-id="b2c37-145">CI/CD with Docker Swarm and VSTS</span></span>](./container-service-docker-swarm-setup-ci-cd.md)