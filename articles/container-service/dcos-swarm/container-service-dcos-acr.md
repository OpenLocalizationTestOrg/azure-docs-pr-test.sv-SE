---
title: aaaUsing ACR med ett Azure DC/OS-kluster | Microsoft Docs
description: "Använda en Azure-behållare registret med ett DC/OS-kluster i Azure Container Service"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service, acr, azure-container-registry
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure, filresursen, cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 9a2802da788b50147d8b4259436bdcdb0c929db8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-acr-with-a-dcos-cluster-toodeploy-your-application"></a>Använda ACR med ett DC/OS-klustret toodeploy ditt program

I den här artikeln förklarar vi hur toouse registret för Azure-behållare med DC/OS-kluster. Med hjälp av ACR kan du tooprivately store och hantera behållare avbildningar. Den här kursen ingår hello följande uppgifter:

> [!div class="checklist"]
> * Distribuera Azure-behållare registret (vid behov)
> * Konfigurera ACR autentisering på en DC/OS-klustret
> * Överföra en bild toohello Azure Container registret
> * Kör en avbildning av behållare från hello Azure Container registret

Du behöver en ACS-DC /-OS klustret toocomplete hello stegen i den här kursen. Om det behövs, [detta skriptexempel](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kan skapa en åt dig.

Den här kursen kräver hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a>Distribuera Azure-behållaren registret

Om det behövs, skapa ett Azure-behållare registret med hello [az acr skapa](/cli/azure/acr#create) kommando. 

hello följande exempel skapas ett register med ett slumpmässigt Generera namn. hello registret konfigureras också med ett administratörskonto med hello `--admin-enabled` argumentet.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic --admin-enabled true
```

När du har skapat hello registret hello Azure CLI matar ut data liknande toohello följande. Anteckna hello `name` och `loginServer`, dessa används i senare steg.

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Hämta hello behållaren registret autentiseringsuppgifter med hjälp av hello [az acr autentiseringsuppgifter visa](/cli/azure/acr/credential) kommando. Ersätt hello `--name` med hello en anges i hello sista steget. Ta del av lösenord, det behövs i ett senare steg.

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

Läs mer om Azure-behållare registret [introduktion tooprivate Docker behållare register](../../container-registry/container-registry-intro.md). 

## <a name="manage-acr-authentication"></a>Hantera ACR-autentisering

hello konventionellt sätt toopush och pull-avbildning från en privat registret är toofirst autentisera med hello registret. toodo så använder du hello `docker login` på alla klienter som behöver tooaccess hello privata registret. Eftersom ett DC/OS-kluster kan innehålla flera noder, alla måste autentiseras med hello ACR toobe är användbara tooautomate processen på varje nod. 

### <a name="create-shared-storage"></a>Skapa delad lagring

Den här processen använder en Azure-filresurs som har monterats på varje nod i klustret hello. Om du inte redan har installerat delad lagring, se [konfigurera en filresurs i ett DC/OS-kluster](container-service-dcos-fileshare.md).

### <a name="configure-acr-authentication"></a>Konfigurera ACR-autentisering

Först hämta hello FQDN för hello DC/OS master och lagrar den i en variabel.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Skapa en SSH-anslutning med hello master (eller hello första master) på ditt DC/OS-baserade kluster. Uppdatera hello användarnamn om ett standardvärde används när du skapar hello kluster.

```azurecli-interactive
ssh azureuser@$FQDN
```

Hello kör följande kommando toologin toohello Azure Container registret. Ersätt hello `--username` med hello namnet hello behållaren registret och hello `--password` med något av hello som lösenord. Ersätt hello sista argumentet *mycontainerregistry.azurecr.io* i hello exempel med hello loginServer namn på hello behållaren register. 

Det här kommandot lagrar hello autentisering värden lokalt under hello `~/.docker` sökväg.

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

Skapa en komprimerad fil som innehåller hello behållaren registervärden för autentisering.

```azurecli-interactive
tar czf docker.tar.gz .docker
```

Kopiera filen toohello klusterdelade lagringen. Det här steget blir hello tillgänglig på alla noder i hello DC/OS-klustret.

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-tooacr"></a>Överför avbildningen tooACR

Från en utvecklingsdator eller andra system med Docker installerat kan nu skapa en avbildning och överför den toohello Azure Container registret.

Skapa en behållare från hello Ubuntu avbildning.

```azurecli-interactive
docker run ubunut --name base-image
```

Samla in hello behållare till en ny avbildning. hello avbildningsnamn måste tooinclude hello `loginServer` namn på hello behållaren registrywith ett format för `loginServer/imageName`.

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

Logga in på hello Azure Container registret. Ersätt hello namn med hello loginServer namn, hello--användarnamn med hello namnet hello behållaren registret och hello--lösenord med en hello som lösenord.

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

Slutligen överför hello avbildningen toohello ACR registret. Det här exemplet överför en bild med namnet DC/OS-demonstrationen.

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a>Kör en bild från ACR

toouse en bild från hello ACR registret, skapa en fil namn *acrDemo.json* och kopiera hello följande text i den. Ersätt hello avbildningsnamn med hello behållarnamn registret loginServer och namn, till exempel `loginServer/imageName`. Anteckna hello `uris` egenskapen. Den här egenskapen innehåller hello plats hello behållaren autentisering registerfilen, som i det här fallet är hello Azure-filresursen som är monterade på varje nod i hello DC/OS-klustret.

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

Distribuera programmet hello med hello DC/OC CLI.

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a>Nästa steg

I den här kursen har du konfigurera DC/OS toouse Azure Container registret inklusive hello följande uppgifter:

> [!div class="checklist"]
> * Distribuera Azure-behållare registret (vid behov)
> * Konfigurera ACR autentisering på en DC/OS-klustret
> * Överföra en bild toohello Azure Container registret
> * Kör en avbildning av behållare från hello Azure Container registret
