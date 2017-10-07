---
title: "aaaQuickstart - Azure Docker CE kluster för Linux | Microsoft Docs"
description: "Lär dig snabbt toocreate ett Docker CE kluster på Linux-behållare i Azure Container Service med hello Azure CLI."
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
ms.date: 08/25/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 6c26c12ed085ec379c3486095a5fa51379afc5a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-docker-ce-cluster"></a><span data-ttu-id="1621f-103">Distribuera Docker CE-kluster</span><span class="sxs-lookup"><span data-stu-id="1621f-103">Deploy Docker CE cluster</span></span>

<span data-ttu-id="1621f-104">I den här snabbstartsguide distribueras ett Docker CE-kluster med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="1621f-104">In this quick start, a Docker CE cluster is deployed using hello Azure CLI.</span></span> <span data-ttu-id="1621f-105">Ett program för flera behållare som består av frontwebb och en Redis-instans distribueras sedan och köra på hello kluster.</span><span class="sxs-lookup"><span data-stu-id="1621f-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on hello cluster.</span></span> <span data-ttu-id="1621f-106">När klar hello programmet är tillgänglig över hello internet.</span><span class="sxs-lookup"><span data-stu-id="1621f-106">Once completed, hello application is accessible over hello internet.</span></span>

<span data-ttu-id="1621f-107">Docker CE på Azure Container Service är en förhandsversion och **bör inte användas för produktionsarbete**.</span><span class="sxs-lookup"><span data-stu-id="1621f-107">Docker CE on Azure Container Service is in preview and **should not be used for production workloads**.</span></span>

<span data-ttu-id="1621f-108">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="1621f-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="1621f-109">Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1621f-109">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="1621f-110">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="1621f-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1621f-111">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1621f-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="1621f-112">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="1621f-112">Create a resource group</span></span>

<span data-ttu-id="1621f-113">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="1621f-113">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="1621f-114">En Azure-resursgrupp är en logisk grupp där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="1621f-114">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="1621f-115">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *ukwest* plats.</span><span class="sxs-lookup"><span data-stu-id="1621f-115">hello following example creates a resource group named *myResourceGroup* in hello *ukwest* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location ukwest
```

<span data-ttu-id="1621f-116">Resultat:</span><span class="sxs-lookup"><span data-stu-id="1621f-116">Output:</span></span>

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

## <a name="create-docker-swarm-cluster"></a><span data-ttu-id="1621f-117">Skapa Docker Swarm-kluster</span><span class="sxs-lookup"><span data-stu-id="1621f-117">Create Docker Swarm cluster</span></span>

<span data-ttu-id="1621f-118">Skapa ett Docker CE-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="1621f-118">Create a Docker CE cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="1621f-119">hello följande exempel skapar ett kluster med namnet *mySwarmCluster* med en Linux master-nod och tre noder för Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="1621f-119">hello following example creates a cluster named *mySwarmCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type dockerce --resource-group myResourceGroup --generate-ssh-keys
```

<span data-ttu-id="1621f-120">Om några minuter hello-kommandot har slutförts och returnerar json-formaterade information om hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="1621f-120">After several minutes, hello command completes and returns json formatted information about hello cluster.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="1621f-121">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="1621f-121">Connect toohello cluster</span></span>

<span data-ttu-id="1621f-122">I den här snabbstartsguide måste hello FQDN för både hello Docker Swarm och hello Docker agent poolen.</span><span class="sxs-lookup"><span data-stu-id="1621f-122">Throughout this quick start, you need hello FQDN of both hello Docker Swarm master and hello Docker agent pool.</span></span> <span data-ttu-id="1621f-123">Kör följande kommando tooreturn både hello-hanterare och agent FQDN hello.</span><span class="sxs-lookup"><span data-stu-id="1621f-123">Run hello following command tooreturn both hello master and agent FQDNs.</span></span>


```bash
az acs list --resource-group myResourceGroup --query '[*].{Master:masterProfile.fqdn,Agent:agentPoolProfiles[0].fqdn}' -o table
```

<span data-ttu-id="1621f-124">Resultat:</span><span class="sxs-lookup"><span data-stu-id="1621f-124">Output:</span></span>

```bash
Master                                                               Agent
-------------------------------------------------------------------  --------------------------------------------------------------------
myswarmcluster-myresourcegroup-d5b9d4mgmt.ukwest.cloudapp.azure.com  myswarmcluster-myresourcegroup-d5b9d4agent.ukwest.cloudapp.azure.com
```

<span data-ttu-id="1621f-125">Skapa en SSH-tunnel toohello Swarm master.</span><span class="sxs-lookup"><span data-stu-id="1621f-125">Create an SSH tunnel toohello Swarm master.</span></span> <span data-ttu-id="1621f-126">Ersätt `MasterFQDN` med hello FQDN-adressen för hello Swarm master.</span><span class="sxs-lookup"><span data-stu-id="1621f-126">Replace `MasterFQDN` with hello FQDN address of hello Swarm master.</span></span>

```bash
ssh -p 2200 -fNL localhost:2374:/var/run/docker.sock azureuser@MasterFQDN
```

<span data-ttu-id="1621f-127">Ange hello `DOCKER_HOST` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="1621f-127">Set hello `DOCKER_HOST` environment variable.</span></span> <span data-ttu-id="1621f-128">Detta ger dig toorun docker kommandon mot hello Docker Swarm utan toospecify hello namnet på hello-värden.</span><span class="sxs-lookup"><span data-stu-id="1621f-128">This allows you toorun docker commands against hello Docker Swarm without having toospecify hello name of hello host.</span></span>

```bash
export DOCKER_HOST=localhost:2374
```

<span data-ttu-id="1621f-129">Du är nu redo toorun Docker-tjänster på hello Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="1621f-129">You are now ready toorun Docker services on hello Docker Swarm.</span></span>


## <a name="run-hello-application"></a><span data-ttu-id="1621f-130">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="1621f-130">Run hello application</span></span>

<span data-ttu-id="1621f-131">Skapa en fil med namnet `azure-vote.yaml` och kopiera hello efter innehåll till den.</span><span class="sxs-lookup"><span data-stu-id="1621f-131">Create a file named `azure-vote.yaml` and copy hello following content into it.</span></span>


```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

<span data-ttu-id="1621f-132">Kör hello [docker-stacken distribuera](https://docs.docker.com/engine/reference/commandline/stack_deploy/) kommandot toocreate hello Azure röst-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1621f-132">Run hello [docker stack deploy](https://docs.docker.com/engine/reference/commandline/stack_deploy/) command toocreate hello Azure Vote service.</span></span>

```bash
docker stack deploy azure-vote --compose-file azure-vote.yaml
```

<span data-ttu-id="1621f-133">Resultat:</span><span class="sxs-lookup"><span data-stu-id="1621f-133">Output:</span></span>

```bash
Creating network azure-vote_default
Creating service azure-vote_azure-vote-back
Creating service azure-vote_azure-vote-front
```

<span data-ttu-id="1621f-134">Använd hello [docker-stacken ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) kommandot tooreturn hello Distributionsstatus för hello program.</span><span class="sxs-lookup"><span data-stu-id="1621f-134">Use hello [docker stack ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) command tooreturn hello deployment status of hello application.</span></span>

```bash
docker stack ps azure-vote
```

<span data-ttu-id="1621f-135">En gång hello `CURRENT STATE` för varje tjänst är `Running`, hello programmet är redo.</span><span class="sxs-lookup"><span data-stu-id="1621f-135">Once hello `CURRENT STATE` of each service is `Running`, hello application is ready.</span></span>

```bash
ID                  NAME                            IMAGE                                 NODE                               DESIRED STATE       CURRENT STATE                ERROR               PORTS
tnklkv3ogu3i        azure-vote_azure-vote-front.1   microsoft/azure-vote-front:redis-v1   swarmm-agentpool0-66066781000004   Running             Running 5 seconds ago                            
lg99i4hy68r9        azure-vote_azure-vote-back.1    redis:latest                          swarmm-agentpool0-66066781000002   Running             Running about a minute ago
```

## <a name="test-hello-application"></a><span data-ttu-id="1621f-136">Testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="1621f-136">Test hello application</span></span>

<span data-ttu-id="1621f-137">Bläddra toohello FQDN för hello Swarm-agenten poolen tootest ut hello Azure rösten program.</span><span class="sxs-lookup"><span data-stu-id="1621f-137">Browse toohello FQDN of hello Swarm agent pool tootest out hello Azure Vote application.</span></span>

![Bild av tooAzure röst-surfning](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a><span data-ttu-id="1621f-139">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="1621f-139">Delete cluster</span></span>
<span data-ttu-id="1621f-140">När hello klustret inte längre behövs, kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, behållartjänsten och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="1621f-140">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a><span data-ttu-id="1621f-141">Hämta hello kod</span><span class="sxs-lookup"><span data-stu-id="1621f-141">Get hello code</span></span>

<span data-ttu-id="1621f-142">I den här snabbstartsguide har skapats i förväg behållaren bilder används toocreate en Docker-tjänst.</span><span class="sxs-lookup"><span data-stu-id="1621f-142">In this quick start, pre-created container images have been used toocreate a Docker service.</span></span> <span data-ttu-id="1621f-143">hello relaterade programkod, Dockerfile, och Skriv fil finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="1621f-143">hello related application code, Dockerfile, and Compose file are available on GitHub.</span></span>

[<span data-ttu-id="1621f-144">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="1621f-144">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="1621f-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1621f-145">Next steps</span></span>

<span data-ttu-id="1621f-146">I den här snabbstartsguide distribuerats Docker Swarm-kluster och distribuerat en tooit för flera program.</span><span class="sxs-lookup"><span data-stu-id="1621f-146">In this quick start, you deployed a Docker Swarm cluster and deployed a multi-container application tooit.</span></span>

<span data-ttu-id="1621f-147">toolearn om integreringen med Docker varmt med Visual Studio Team Services fortsätta toohello CI/CD med Docker Swarm och VSTS.</span><span class="sxs-lookup"><span data-stu-id="1621f-147">toolearn about integrating Docker warm with Visual Studio Team Services, continue toohello CI/CD with Docker Swarm and VSTS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1621f-148">CI/CD med Docker Swarm och VSTS</span><span class="sxs-lookup"><span data-stu-id="1621f-148">CI/CD with Docker Swarm and VSTS</span></span>](./container-service-docker-swarm-setup-ci-cd.md)