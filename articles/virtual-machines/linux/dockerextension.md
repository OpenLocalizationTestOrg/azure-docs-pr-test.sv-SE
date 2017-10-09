---
title: "aaaUse hello Azure Docker VM-tillägget | Microsoft Docs"
description: "Lär dig hur toouse hello Docker VM-tillägget tooquickly och säkert distribuera en Docker-miljö i Azure med hjälp av Resource Manager-mallar och hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 8e43adc594192773466ccd2d3e455105f14c1a61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension"></a>Skapa en Docker-miljö i Azure med hjälp av hello Docker VM-tillägget
Docker är en populär behållarhantering och avbildningsverktyg plattform som gör att du tooquickly fungerar med behållare på Linux. Det finns olika sätt som du kan distribuera Docker enligt tooyour behov i Azure. Den här artikeln fokuserar på med hello Docker VM-tillägget och Azure Resource Manager-mallar med hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](dockerextension-nodejs.md).

## <a name="azure-docker-vm-extension-overview"></a>Översikt av Azure Docker VM-tillägg
hello Azure Docker VM-tillägget installerar och konfigurerar hello Docker daemon Docker-klienten och Docker Compose i Linux-dator (VM). Genom att använda hello Azure Docker VM-tillägget kan ha du mer kontroll och funktioner än att bara använda Docker-dator eller skapa hello Docker värd själv. Dessa ytterligare funktioner som [Docker Compose](https://docs.docker.com/compose/overview/), se hello Azure Docker VM-tillägget som passar för stabilare utvecklare eller produktion miljöer.

Mer information om hello finns olika distributionsmetoder, inklusive användning av Docker-dator och Azure Behållartjänster hello följande artiklar:

* tooquickly prototyp en app, som du kan skapa en enda Docker-värd med [Docker datorn](docker-machine.md).
* toobuild produktionsklara, skalbara miljöer som ger ytterligare schemaläggning och hanteringsverktygen, kan du distribuera en [Docker Swarm-kluster på Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md).

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a>Distribuera en mall med hello Azure Docker VM-tillägget
Vi använder en befintlig quickstart mallen toocreate en Ubuntu VM som använder hello Azure Docker VM-tillägget tooinstall och konfigurerar hello Docker-värden. Du kan visa hello mallen här: [enkel distribution av en Ubuntu VM med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westus* plats:

```azurecli
az group create --name myResourceGroup --location westus
```

Distribuera en virtuell dator med [az distribution skapa](/cli/azure/group/deployment#create) som innehåller hello Azure Docker VM-tillägget från [Azure Resource Manager-mallen på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Ange egna värden för *newStorageAccountName*, *adminUsername*, *adminPassword*, och *dnsNameForPublicIP* på följande sätt:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Det tar några minuter för hello distribution toofinish. När hello distributionen är klar [flytta toonext steg](#deploy-your-first-nginx-container) tooSSH tooyour VM. 

Tooinstead returnerade kontrollen toohello prompt och låt hello distribution fortsätter i bakgrunden hello, Lägg till hello `--no-wait` flaggan toohello föregående kommando. Den här processen kan tooperform annat arbete i hello CLI medan hello distributionen fortsätter om en stund. 

Du kan sedan visa information om hello Docker värdstatusen med [az vm visa](/cli/azure/vm#show). hello följande exempel kontrollerar hello status för hello virtuella datorn med namnet *myDockerVM* (hello standardnamnet från hello mall - inte ändrar namnet) i hello resursgrupp med namnet *myResourceGroup*:

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

När det här kommandot returnerar *lyckades*, hello distributionen är klar och du kan SSH toohello VM i hello följande steg.

## <a name="deploy-your-first-nginx-container"></a>Distribuera din första nginx-behållare
tooview information på den virtuella datorn, inklusive hello DNS-namn, Använd `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`. SSH tooyour nya Docker värdar från din lokala dator enligt följande:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

När inloggad toohello Docker-värden kan vi köra en nginx-behållare:

```bash
sudo docker run -d -p 80:80 nginx
```

hello-utdata är liknande toohello följande exempel som hello nginx-avbildningen hämtas och en behållare som är igång:

```bash
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Kontrollera status för hello hello-behållare som körs på värden Docker på följande sätt:

```bash
sudo docker ps
```

hello-utdata är liknande toohello som följande exempel, visar hello nginx behållaren körs och TCP-portarna 80 och 443 och vidarebefordras:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

toosee din behållare i åtgärden, öppna upp en webbläsare och ange hello DNS-namn för Docker-värd:

![Körs ngnix behållare](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM-mall tilläggsreferens
hello föregående exempel använder en befintlig mall för Snabbstart. Du kan också distribuera hello Azure Docker VM-tillägget med egna Resource Manager-mallar. toodo Lägg därför till hello följande tooyour Resource Manager-mallar, definiera hello `vmName` på den virtuella datorn på rätt sätt:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.*",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Du kan hitta mer detaljerad genomgång om hur du använder Resource Manager-mallar genom att läsa [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Nästa steg
Vill du kanske för[konfigurera hello Docker daemon TCP-port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), Förstå [Docker säkerhet](https://docs.docker.com/engine/security/security/), eller distribuera behållare med [Docker Compose](https://docs.docker.com/compose/overview/). Mer information om hello Azure Docker VM-tillägget själva finns hello [GitHub projekt](https://github.com/Azure/azure-docker-extension/).

Läs mer om hello ytterligare Docker distributionsalternativ i Azure:

* [Använda Docker-dator med hello Azure drivrutin](docker-machine.md)  
* [Kom igång med Docker Compose toodefine och köra ett program för flera behållare på en virtuell dator i Azure](docker-compose-quickstart.md).
* [Distribuera ett Azure Container Service-kluster](../../container-service/dcos-swarm/container-service-deployment.md)

