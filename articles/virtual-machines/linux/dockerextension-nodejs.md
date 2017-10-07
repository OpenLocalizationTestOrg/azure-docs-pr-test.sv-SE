---
title: "aaaUse hello Azure Docker VM-tillägget med hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toouse hello Docker VM-tillägget tooquickly och säkert distribuera en Docker-miljö i Azure med hjälp av Resource Manager-mallar."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 2133cdb1af741fe30093910fae5c3b2c91e8d5fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension-with-hello-azure-cli-10"></a>Skapa en Docker-miljö i Azure med hello Azure CLI 1.0 hello Docker VM-tillägget
Docker är en populär behållarhantering och avbildningsverktyg plattform som gör att du tooquickly fungerar med behållare på Linux (och även Windows). Det finns olika sätt som du kan distribuera Docker enligt tooyour behov i Azure. Den här artikeln fokuserar på att använda hello Docker VM-tillägget och Azure Resource Manager-mallar. 

Mer information om hello finns olika distributionsmetoder, inklusive användning av Docker-dator och Azure Behållartjänster hello följande artiklar:

* tooquickly prototyp en app, som du kan skapa en enda Docker-värd med [Docker datorn](docker-machine.md).
* För större och mer stabilt miljöer, kan du använda hello Azure Docker VM-tillägget, som också stöder [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate konsekvent behållardistributionerna. Den här artikeln information med hjälp av hello Azure Docker VM-tillägget.
* toobuild produktionsklara, skalbara miljöer som ger ytterligare schemaläggning och hanteringsverktygen, kan du distribuera en [Docker Swarm-kluster på Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md).

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#azure-docker-vm-extension-overview) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](dockerextension.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering 

## <a name="azure-docker-vm-extension-overview"></a>Översikt av Azure Docker VM-tillägg
hello Azure Docker VM-tillägget installerar och konfigurerar hello Docker daemon Docker-klienten och Docker Compose i Linux-dator (VM). Genom att använda hello Azure Docker VM-tillägget kan ha du mer kontroll och funktioner än att bara använda Docker-dator eller skapa hello Docker värd själv. Dessa ytterligare funktioner som [Docker Compose](https://docs.docker.com/compose/overview/), se hello Azure Docker VM-tillägget som passar för stabilare utvecklare eller produktion miljöer.

Azure Resource Manager-mallar definiera hello hela strukturen för din miljö. Mallar kan du toocreate och konfigurera resurser, till exempel hello Docker värden virtuella datorer, lagring, rollbaserad åtkomstkontroll (RBAC) och diagnostik. Du kan återanvända dessa mallar toocreate ytterligare distributioner på ett konsekvent sätt. Mer information om Azure Resource Manager och mallar finns [översikt över Resource Manager](../../azure-resource-manager/resource-group-overview.md). 

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a>Distribuera en mall med hello Azure Docker VM-tillägget
Vi använder en befintlig quickstart mallen toocreate en Ubuntu VM som använder hello Azure Docker VM-tillägget tooinstall och konfigurerar hello Docker-värden. Du kan visa hello mallen här: [enkel distribution av en Ubuntu VM med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Du behöver hello [senaste Azure CLI](../../cli-install-nodejs.md) installerad och logga in med hello Resource Manager-läge på följande sätt:

```azurecli
azure config mode arm
```

Distribuera hello mallen med hjälp av hello Azure CLI, att ange hello mallen URI. hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westus* plats. Använda egna resursgruppens namn och plats på följande sätt:

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Besvara hello prompter tooname ditt lagringskonto, ange ett användarnamn och lösenord och ange ett DNS-namn. hello utdata är liknande toohello följande exempel:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for hello following parameters
newStorageAccountName: mystorageaccount
adminUsername: azureuser
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicidns
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

hello Azure CLI returnerar du toohello fråga efter endast några sekunder, men Docker-värden skapas och konfigureras av hello Azure Docker VM-tillägget. Det tar några minuter för hello distribution toofinish. Du kan visa information om värdstatusen hello Docker med hello `azure vm show` kommando.

hello följande exempel kontrollerar hello status för hello virtuella datorn med namnet *myDockerVM* (hello standardnamnet från hello mall - inte ändrar namnet) i hello resursgrupp med namnet *myResourceGroup*. Ange hello hello resursgruppen som du skapade i föregående steg hello:

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

Hej utdata från hello `azure vm show` kommandot är liknande toohello följande exempel:

```azurecli
info:    Executing command vm show
+ Looking up hello VM "myDockerVM"
+ Looking up hello NIC "myVMNicD"
+ Looking up hello public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicdns.westus.cloudapp.azure.com
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Hello övre delen av hello utdata visas hello **ProvisioningState** av hello VM. När detta visar *lyckades*, hello distributionen är klar och du kan SSH toohello VM.

Hello utgången av hello utdata *FQDN* visar hello fullständigt kvalificerade domännamnet för Docker-värden. Den här FQDN är det du använder tooSSH tooyour Docker-värd i hello återstående steg.

## <a name="deploy-your-first-nginx-container"></a>Distribuera din första nginx-behållare
En gång hello distributionen har slutförts SSH tooyour nya Docker värden från den lokala datorn. Ange användarnamn och FQDN på följande sätt:

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
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

toosee din behållare i åtgärden, öppna upp en webbläsare och ange hello FQDN-namn för Docker-värd:

![Körs ngnix behållare](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM-mall tilläggsreferens
hello föregående exempel använder en befintlig mall för Snabbstart. Du kan också distribuera hello Azure Docker VM-tillägget med egna Resource Manager-mallar. toodo Lägg därför till hello följande tooyour Resource Manager-mallar, definiera hello *vmName* på den virtuella datorn på rätt sätt:

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
    "typeHandlerVersion": "1.1",
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

