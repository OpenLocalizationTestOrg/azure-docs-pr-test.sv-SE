---
title: "Använd Docker Compose på en virtuell Linux-dator i Azure | Microsoft Docs"
description: "Använda Docker och Skriv på Linux-datorer med Azure CLI"
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
ms.openlocfilehash: 541722cb02dd991228726e62a2304b49cdd806f2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Kom igång med Docker och skriv för att definiera och köra ett program för flera behållare i Azure
Med [Compose](http://github.com/docker/compose), du använder en enkel textfil för att definiera ett program som består av flera Docker-behållare. Sedan få igång ditt program i ett enda kommando som gör allt för att distribuera din definierade miljö. Exempelvis visar den här artikeln hur du snabbt ställa in en WordPress-blogg med en serverdel MariaDB SQL-databas på en Ubuntu VM. Du kan också använda Compose för att ställa in mer komplexa program.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Konfigurera en Linux-VM som en Docker-värd
Du kan använda olika Azure procedurer och tillgängliga avbildningar eller Resource Manager-mallar i Azure Marketplace för att skapa en Linux VM och konfigurera den som en Docker-värd. Se exempelvis [använder Docker VM-tillägget för att distribuera din miljö](dockerextension.md) att snabbt skapa en Ubuntu VM med Azure Docker VM-tillägget med hjälp av en [quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

När du använder Docker VM-tillägget, den virtuella datorn konfigureras automatiskt som en Docker-värd och Skriv redan är installerad.


### <a name="create-docker-host-with-azure-cli-20"></a>Skapa Docker-värd med Azure CLI 2.0
Installera senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in till en Azure med hjälp av [az inloggningen](/cli/azure/#login).

Börja med att skapa en resursgrupp för din Docker-miljö med [az gruppen skapa](/cli/azure/group#create). I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *westus* plats:

```azurecli
az group create --name myResourceGroup --location westus
```

Distribuera en virtuell dator med [az distribution skapa](/cli/azure/group/deployment#create) som innehåller Azure Docker VM-tillägget från [Azure Resource Manager-mallen på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Ange egna värden för *newStorageAccountName*, *adminUsername*, *adminPassword*, och *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Det tar några minuter för att distributionen ska slutföras. När distributionen är klar [vidare till nästa steg](#verify-that-compose-is-installed) till SSH till den virtuella datorn. 

För att i stället komma tillbaka till meddelandet och gör distributionen fortsätter i bakgrunden, Lägg till den `--no-wait` flagga för att kommandot ovan. Den här processen kan du utföra annat arbete i CLI medan distributionen fortsätter om en stund. Du kan sedan visa information om värdstatusen Docker med [az vm visa](/cli/azure/vm#show). I följande exempel kontrollerar status för den virtuella datorn med namnet *myDockerVM* (standardnamnet från mall - inte ändrar namnet) i resursgrupp med namnet *myResourceGroup*:

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

När det här kommandot returnerar *lyckades*distributionen är klar och du kan SSH till den virtuella datorn i följande steg.


## <a name="verify-that-compose-is-installed"></a>Kontrollera att Skriv är installerad
När distributionen är klar namn du angav under distributionen av SSH till din nya Docker-värden med hjälp av DNS. Du kan använda `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` att visa detaljer för den virtuella datorn, inklusive DNS-namn.

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

Kontrollera att Skriv är installerad på den virtuella datorn genom att köra följande kommando:

```bash
docker-compose --version
```

Du kan se utdata som liknar *docker compose 1.6.2, skapa 4d 72027*.

> [!TIP]
> Om du använder en annan metod för att skapa en Docker-värd och installera skapar själv, finns det [utgöra dokumentationen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Skapa en konfigurationsfil för docker-compose.yml
Sedan skapar du en `docker-compose.yml` fil som är bara en konfiguration textfil, definiera Docker-behållare för att köra på den virtuella datorn. Filen anger bilden som ska köras på varje behållare (eller det kan vara en version från en Dockerfile) nödvändiga miljövariabler och beroenden, portar och länkarna mellan behållare. Mer information om yml filen syntax finns [utgöra filreferens](https://docs.docker.com/compose/compose-file/).

Skapa den *docker-compose.yml* enligt följande:

```bash
touch docker-compose.yml
```

Du kan använda valfri textredigerare för att lägga till vissa data i filen. I följande exempel används den *vi* editor:

```bash
vi docker-compose.yml
```

Klistra in följande exempel i en textfil. Den här konfigurationen använder avbildningar från den [DockerHub registret](https://registry.hub.docker.com/_/wordpress/) installera WordPress (öppen källkod bloggar och innehåll hanteringssystemet) och en länkad serverdel MariaDB SQL-databas. Ange en egen *MYSQL_ROOT_PASSWORD* på följande sätt:

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

## <a name="start-the-containers-with-compose"></a>Starta behållarna med Compose
I samma katalog som din *docker-compose.yml* filen, kör följande kommando (beroende på din miljö kan du behöva köra `docker-compose` med `sudo`):

```bash
docker-compose up -d
```

Detta kommando startar Docker-behållare som anges i *docker-compose.yml*. Det tar en minut eller två för det här steget att slutföra. Du kan se utdata som liknar följande exempel:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Se till att använda den **-d** alternativ på uppstart så att behållarna som körs kontinuerligt i bakgrunden.


Så här kontrollerar du att behållarna som är igång `docker-compose ps`. Du bör se något som liknar:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Nu kan du ansluta till WordPress direkt på den virtuella datorn på port 80. Öppna en webbläsare och ange DNS-namnet på den virtuella datorn (t.ex `http://mypublicdns.westus.cloudapp.azure.com`). Du bör nu se WordPress startskärmen, där du kan slutföra installationen och kom igång med programmet.

![Startskärmen för WordPress][wordpress_start]

## <a name="next-steps"></a>Nästa steg
* Gå till den [Docker VM-tillägget användarhandboken](https://github.com/Azure/azure-docker-extension/blob/master/README.md) för flera alternativ för att konfigurera Docker och Skriv i Docker-VM. Till exempel är ett alternativ att placera filen Skriv yml (konverteras till JSON) direkt i konfigurationen av Docker VM-tillägget.
* Kolla in den [utgöra Kommandoradsreferens](http://docs.docker.com/compose/reference/) och [användarhandboken](http://docs.docker.com/compose/) fler exempel på Skapa och distribuera flera behållare appar.
* Använda en Azure Resource Manager-mall, antingen din egen eller en bidragit från den [community](https://azure.microsoft.com/documentation/templates/), för att distribuera en virtuell Azure-dator med Docker och ett program med Compose. Till exempel den [distribuerar en WordPress-blogg med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) mallen använder Docker och skriv för att snabbt distribuera WordPress med en MySQL-serverdel på en Ubuntu VM.
* Försök integrera Docker Compose med Docker Swarm-kluster. Se [med utgöra med Swarm](https://docs.docker.com/compose/swarm/) för scenarier.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
