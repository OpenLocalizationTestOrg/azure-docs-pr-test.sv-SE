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
# <a name="deploy-docker-ce-cluster"></a>Distribuera Docker CE-kluster

I den här snabbstartsguide distribueras ett Docker CE-kluster med hello Azure CLI. Ett program för flera behållare som består av frontwebb och en Redis-instans distribueras sedan och köra på hello kluster. När klar hello programmet är tillgänglig över hello internet.

Docker CE på Azure Container Service är en förhandsversion och **bör inte användas för produktionsarbete**.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk grupp där Azure-resurser distribueras och hanteras.

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *ukwest* plats.

```azurecli-interactive
az group create --name myResourceGroup --location ukwest
```

Resultat:

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

## <a name="create-docker-swarm-cluster"></a>Skapa Docker Swarm-kluster

Skapa ett Docker CE-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando. 

hello följande exempel skapar ett kluster med namnet *mySwarmCluster* med en Linux master-nod och tre noder för Linux-agenten.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type dockerce --resource-group myResourceGroup --generate-ssh-keys
```

Om några minuter hello-kommandot har slutförts och returnerar json-formaterade information om hello-klustret.

## <a name="connect-toohello-cluster"></a>Ansluta toohello kluster

I den här snabbstartsguide måste hello FQDN för både hello Docker Swarm och hello Docker agent poolen. Kör följande kommando tooreturn både hello-hanterare och agent FQDN hello.


```bash
az acs list --resource-group myResourceGroup --query '[*].{Master:masterProfile.fqdn,Agent:agentPoolProfiles[0].fqdn}' -o table
```

Resultat:

```bash
Master                                                               Agent
-------------------------------------------------------------------  --------------------------------------------------------------------
myswarmcluster-myresourcegroup-d5b9d4mgmt.ukwest.cloudapp.azure.com  myswarmcluster-myresourcegroup-d5b9d4agent.ukwest.cloudapp.azure.com
```

Skapa en SSH-tunnel toohello Swarm master. Ersätt `MasterFQDN` med hello FQDN-adressen för hello Swarm master.

```bash
ssh -p 2200 -fNL localhost:2374:/var/run/docker.sock azureuser@MasterFQDN
```

Ange hello `DOCKER_HOST` miljövariabeln. Detta ger dig toorun docker kommandon mot hello Docker Swarm utan toospecify hello namnet på hello-värden.

```bash
export DOCKER_HOST=localhost:2374
```

Du är nu redo toorun Docker-tjänster på hello Docker Swarm.


## <a name="run-hello-application"></a>Kör programmet hello

Skapa en fil med namnet `azure-vote.yaml` och kopiera hello efter innehåll till den.


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

Kör hello [docker-stacken distribuera](https://docs.docker.com/engine/reference/commandline/stack_deploy/) kommandot toocreate hello Azure röst-tjänsten.

```bash
docker stack deploy azure-vote --compose-file azure-vote.yaml
```

Resultat:

```bash
Creating network azure-vote_default
Creating service azure-vote_azure-vote-back
Creating service azure-vote_azure-vote-front
```

Använd hello [docker-stacken ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) kommandot tooreturn hello Distributionsstatus för hello program.

```bash
docker stack ps azure-vote
```

En gång hello `CURRENT STATE` för varje tjänst är `Running`, hello programmet är redo.

```bash
ID                  NAME                            IMAGE                                 NODE                               DESIRED STATE       CURRENT STATE                ERROR               PORTS
tnklkv3ogu3i        azure-vote_azure-vote-front.1   microsoft/azure-vote-front:redis-v1   swarmm-agentpool0-66066781000004   Running             Running 5 seconds ago                            
lg99i4hy68r9        azure-vote_azure-vote-back.1    redis:latest                          swarmm-agentpool0-66066781000002   Running             Running about a minute ago
```

## <a name="test-hello-application"></a>Testa programmet hello

Bläddra toohello FQDN för hello Swarm-agenten poolen tootest ut hello Azure rösten program.

![Bild av tooAzure röst-surfning](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Ta bort klustret
När hello klustret inte längre behövs, kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, behållartjänsten och alla relaterade resurser.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a>Hämta hello kod

I den här snabbstartsguide har skapats i förväg behållaren bilder används toocreate en Docker-tjänst. hello relaterade programkod, Dockerfile, och Skriv fil finns på GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Nästa steg

I den här snabbstartsguide distribuerats Docker Swarm-kluster och distribuerat en tooit för flera program.

toolearn om integreringen med Docker varmt med Visual Studio Team Services fortsätta toohello CI/CD med Docker Swarm och VSTS.

> [!div class="nextstepaction"]
> [CI/CD med Docker Swarm och VSTS](./container-service-docker-swarm-setup-ci-cd.md)