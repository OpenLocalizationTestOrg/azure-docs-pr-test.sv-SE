---
title: "aaaAzure behållaren Service Quickstart - distribuera DC/OS-kluster | Microsoft Docs"
description: Azure Container Service Quickstart - distribuera DC/OS-klustret
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
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b961f15bd73deeafda5a3fc25ab53c839195219b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-dcos-cluster"></a>Distribuera ett DC/OS-kluster

DC/OS är en distribuerad plattform för moderna och körning av program. Med Azure Container Service är etablering av en produktion redo DC/OS-kluster snabbt och enkelt. Den här snabbstartsguide information hello grundläggande steg behövs toodeploy ett DC/OS-klustret och köra grundläggande arbetsbelastning.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

Den här kursen kräver hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="log-in-tooazure"></a>Logga in tooAzure 

Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a>Skapa DC/OS-klustret

Skapa ett DC/OS-kluster med hello [az acs skapa](/cli/azure/acs#create) kommando.

hello följande exempel skapas ett DC/OS-kluster med namnet *myDCOSCluster* och skapar SSH-nycklar, om de inte redan finns. toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Om några minuter hello-kommandot har slutförts och returnerar information om hello-distribution.

## <a name="connect-toodcos-cluster"></a>Ansluta tooDC/OS-klustret

När ett DC/OS-klustret har skapats, kan det vara åtkomst via en SSH-tunnel. Hello kör följande kommando tooreturn hello offentliga IP-adressen för hello DC/OS-hanteraren. Den här IP-adressen är lagrad i en variabel och användas i hello nästa steg.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

toocreate hello SSH-tunnel, kör följande kommando hello och följer hello på skärmen instruktionerna. Om port 80 används misslyckas hello kommandot. Uppdatera hello tunneldata port tooone används som `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

hello SSH-tunnel kan testas genom att bläddra för`http://localhost`. Om en port andra att 80 har använts, justera hello plats toomatch. 

Om hello SSH-tunnel har skapats, returneras hello DC/OS-portalen.

![DC/OS-GRÄNSSNITTET](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a>Installera DC/OS CLI

hello DC/OS-kommandoradsgränssnittet är används toomanage ett DC/OS-kluster från hello kommandoraden. Installera hello DC/OS cli med hello [az installera acs-DC/OS-cli](/azure/acs/dcos#install-cli) kommando. Om du använder Azure CloudShell har hello DC/OS CLI redan installerats. 

Om du kör hello Azure CLI på macOS- eller Linux behöva toorun hello kommandot med sudo.

```azurecli
az acs dcos install-cli
```

Innan du hello CLI kan användas med hello kluster, måste det vara konfigurerade toouse hello SSH-tunnel. toodo kör så hello följande kommando, justera hello porten om det behövs.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Köra ett program

hello standard schemaläggning mekanism för en ACS DC/OS-klustret är Marathon. Marathon är används toostart ett program och hantera hello tillståndet för programmet hello på hello DC/OS-klustret. tooschedule ett program via Marathon, skapa en fil med namnet *marathon app.json*, och kopiera hello efter innehållet i den. 

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Kör hello efter kommandot tooschedule hello programmet toorun hello DC/OS-klustret.

```azurecli
dcos marathon app add marathon-app.json
```

toosee hello Distributionsstatus för hello app, kör följande kommando hello.

```azurecli
dcos marathon app list
```

När hello **väntar på** kolumnvärde växlar från *SANT* för*FALSKT*, programdistribution har slutförts.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

Hämta hello offentliga IP-adress hello DC/OS-klustret agenterna.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Bläddra toothis adress returnerar hello NGINX standardwebbplatsen.

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a>Ta bort DC/OS-klustret

När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, DC/OS-kluster och alla relaterade resurser.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Nästa steg

I den här snabbstartsguide du har distribuerat ett DC/OS-kluster och har kört en enkel dockerbehållare på hello klustret. toolearn mer om Azure Container Service fortsätta toohello ACS självstudier.

> [!div class="nextstepaction"]
> [Hantera en ACS DC/OS-klustret](container-service-dcos-manage-tutorial.md)