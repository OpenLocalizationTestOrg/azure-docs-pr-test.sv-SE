---
title: "aaaUse Docker Compose på en Linux-VM i Azure | Microsoft Docs"
description: "Hur toouse Docker och Skriv på Linux virtuella datorer med hello Azure CLI"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: cf7254ad4813ccdc641fcacbb06ed1514a93cee5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-docker-and-compose-toodefine-and-run-a-multi-container-application-in-azure"></a>Kom igång med Docker och Skriv toodefine och kör ett program för flera behållare i Azure
Med [Compose](http://github.com/docker/compose), du kan använda en enkel text filen toodefine ett program som består av flera behållare i Docker. Du kan sedan få igång ditt program i ett enda kommando som gör allt toodeploy definierade miljön. Exempelvis visar den här artikeln hur tooquickly du konfigurerar en WordPress-blogg med en serverdel MariaDB SQL-databas på en Ubuntu VM. Du kan också använda Skriv tooset in mer komplexa program.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Konfigurera en Linux-VM som en Docker-värd
Du kan använda olika Azure procedurer och tillgängliga avbildningar eller Resource Manager-mallar i hello Azure Marketplace toocreate en Linux VM och konfigurera den som en Docker-värd. Se exempelvis [med hello Docker VM-tillägget toodeploy miljön](dockerextension.md) tooquickly skapa en Ubuntu VM med hello Azure Docker VM-tillägget med hjälp av en [quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

När du använder hello Docker VM-tillägget, den virtuella datorn konfigureras automatiskt som en Docker-värd och Skriv redan är installerad.


### <a name="create-docker-host-with-azure-cli-20"></a>Skapa Docker-värd med Azure CLI 2.0
Installera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Börja med att skapa en resursgrupp för din Docker-miljö med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westus* plats:

```azurecli
az group create --name myResourceGroup --location westus
```

Distribuera en virtuell dator med [az distribution skapa](/cli/azure/group/deployment#create) som innehåller hello Azure Docker VM-tillägget från [Azure Resource Manager-mallen på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Ange egna värden för *newStorageAccountName*, *adminUsername*, *adminPassword*, och *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Det tar några minuter för hello distribution toofinish. När hello distributionen är klar [flytta toonext steg](#verify-that-compose-is-installed) tooSSH tooyour VM. 

Tooinstead returnerade kontrollen toohello prompt och låt hello distribution fortsätter i bakgrunden hello, Lägg till hello `--no-wait` flaggan toohello föregående kommando. Den här processen kan tooperform annat arbete i hello CLI medan hello distributionen fortsätter om en stund. Du kan sedan visa information om hello Docker värdstatusen med [az vm visa](/cli/azure/vm#show). hello följande exempel kontrollerar hello status för hello virtuella datorn med namnet *myDockerVM* (hello standardnamnet från hello mall - inte ändrar namnet) i hello resursgrupp med namnet *myResourceGroup*:

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

När det här kommandot returnerar *lyckades*, hello distributionen är klar och du kan SSH toohello VM i hello följande steg.


## <a name="verify-that-compose-is-installed"></a>Kontrollera att Skriv är installerad
När hello distributionen är klar så SSH tooyour nya Docker värden använder hello DNS-namn som du har angett för under distributionen. Du kan använda `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview information på den virtuella datorn, inklusive hello DNS-namn.

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

toocheck som ska utgöra är installerat på hello VM, kör hello följande kommando:

```bash
docker-compose --version
```

Du kan se utdata för*docker compose 1.6.2, skapa 4d 72027*.

> [!TIP]
> Om du använder en annan metod toocreate en Docker-värd och behöver tooinstall skapar själv, se hello [utgöra dokumentationen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Skapa en konfigurationsfil för docker-compose.yml
Sedan skapar du en `docker-compose.yml` fil som är bara en konfigurationsfil för text, toodefine hello Docker behållare toorun på hello VM. hello-filen anger hello avbildningen toorun varje behållare (eller det kan vara en version från en Dockerfile) nödvändiga miljövariabler och beroenden, portar och hello länkar mellan behållare. Mer information om yml filen syntax finns [utgöra filreferens](https://docs.docker.com/compose/compose-file/).

Skapa hello *docker-compose.yml* enligt följande:

```bash
touch docker-compose.yml
```

Använda din textredigeringsprogram editor tooadd vissa toohello datafil. hello följande exempel används hello *vi* editor:

```bash
vi docker-compose.yml
```

Klistra in hello som följande exempel i en textfil. Den här konfigurationen använder avbildningar från hello [DockerHub registret](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello öppen källkod bloggar och innehåll management system) och en länkad serverdel MariaDB SQL-databasen. Ange en egen *MYSQL_ROOT_PASSWORD* på följande sätt:

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-hello-containers-with-compose"></a>Starta hello behållare med Compose
Hej i samma katalog som din *docker-compose.yml* filen, kör följande kommando hello (beroende på din miljö kan du behöva toorun `docker-compose` med `sudo`):

```bash
docker-compose up -d
```

Detta kommando startar hello Docker-behållare som anges i *docker-compose.yml*. Det tar en minut eller två för det här steget toocomplete. Du kan se utdata liknande toohello följande exempel:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Vara säker på att toouse hello **-d** alternativ på uppstart så att hello behållare köras i bakgrunden hello kontinuerligt.


tooverify hello behållare är igång, typen `docker-compose ps`. Du bör se något som liknar:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Nu kan du ansluta tooWordPress direkt på hello VM på port 80. Öppna en webbläsare och ange hello DNS-namnet på den virtuella datorn (t.ex `http://mypublicdns.westus.cloudapp.azure.com`). Du bör nu se hello WordPress startskärmen, där du kan slutföra hello installationen och kom igång med hello program.

![Startskärmen för WordPress][wordpress_start]

## <a name="next-steps"></a>Nästa steg
* Gå toohello [Docker VM-tillägget användarhandboken](https://github.com/Azure/azure-docker-extension/blob/master/README.md) mer alternativ tooconfigure Docker och Skriv i Docker-VM. Ett alternativ är till exempel tooput hello Skriv yml filen (konverterade tooJSON) direkt i hello konfigurationen av hello Docker VM-tillägget.
* Kolla in hello [utgöra Kommandoradsreferens](http://docs.docker.com/compose/reference/) och [användarhandboken](http://docs.docker.com/compose/) fler exempel på Skapa och distribuera flera behållare appar.
* Använda en Azure Resource Manager-mall, antingen din eller en bidragit från hello [community](https://azure.microsoft.com/documentation/templates/), toodeploy en Azure-dator med Docker och ett program som konfigurerats med Compose. Till exempel hello [distribuerar en WordPress-blogg med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) mallen använder Docker och Skriv tooquickly distribuera WordPress med en MySQL-serverdel på en Ubuntu VM.
* Försök integrera Docker Compose med Docker Swarm-kluster. Se [med utgöra med Swarm](https://docs.docker.com/compose/swarm/) för scenarier.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
