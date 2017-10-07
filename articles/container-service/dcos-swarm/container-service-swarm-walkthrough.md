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
# <a name="deploy-docker-swarm-cluster"></a>Distribuera Docker Swarm-kluster

I den här snabbstartsguide distribueras Docker Swarm-kluster med hjälp av hello Azure CLI. Ett program för flera behållare som består av frontwebb och en Redis-instans distribueras sedan och köra på hello kluster. När klar hello programmet är tillgänglig över hello internet.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

Denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk grupp där Azure-resurser distribueras och hanteras.

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westus* plats.

```azurecli-interactive
az group create --name myResourceGroup --location westus
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

Skapa ett Docker Swarm-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando. 

hello följande exempel skapar ett kluster med namnet *mySwarmCluster* med en Linux master-nod och tre noder för Linux-agenten.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

Om några minuter hello-kommandot har slutförts och returnerar json-formaterade information om hello-klustret.

## <a name="connect-toohello-cluster"></a>Ansluta toohello kluster

I den här snabbstartsguide måste hello IP-adressen för både hello Docker Swarm och hello Docker agent poolen. Kör följande kommando tooreturn hello båda IP-adresser.


```bash
az network public-ip list --resource-group myResourceGroup --query '[*].{Name:name,IPAddress:ipAddress}' -o table
```

Resultat:

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

Skapa en SSH-tunnel toohello Swarm master. Ersätt `IPAddress` med hello hello Swarm master IP-adress.

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

Ange hello `DOCKER_HOST` miljövariabeln. Detta ger dig toorun docker kommandon mot hello Docker Swarm utan toospecify hello namnet på hello-värden.

```bash
export DOCKER_HOST=:2375
```

Du är nu redo toorun Docker-tjänster på hello Docker Swarm.


## <a name="run-hello-application"></a>Kör programmet hello

Skapa en fil med namnet `docker-compose.yaml` och kopiera hello efter innehåll till den.

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

Kör hello efter kommandot toocreate hello Azure rösten service.

```bash
docker-compose up -d
```

Resultat:

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

## <a name="test-hello-application"></a>Testa programmet hello

Bläddra toohello IP-adressen för hello Swarm-agenten poolen tootest ut hello Azure rösten program.

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