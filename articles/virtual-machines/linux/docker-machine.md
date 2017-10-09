---
title: "aaaUse Docker datorn toocreate Linux-värdar i Azure | Microsoft Docs"
description: "Beskriver hur toouse Docker datorn toocreate Docker-värdar i Azure."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: iainfou
ms.openlocfilehash: 905c645add51c78305aac85a7013441b3a73972f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-docker-machine-toocreate-hosts-in-azure"></a>Hur toouse Docker datorn toocreate värd i Azure
Den här artikeln information om hur toouse [Docker datorn](https://docs.docker.com/machine/) toocreate värdar i Azure. Hej `docker-machine` kommando skapar en Linux-dator (VM) i Azure och sedan installerar Docker. Du kan sedan hantera Docker-värdar i Azure med hjälp av hello samma lokala verktyg och arbetsflöden.

## <a name="create-vms-with-docker-machine"></a>Skapa virtuella datorer med Docker-dator
Först hämta ditt Azure prenumerations-ID med [az konto visa](/cli/azure/account#show) på följande sätt:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

Du skapar virtuella datorer Docker-värden i Azure med `docker-machine create` genom att ange *azure* som hello drivrutin. Mer information finns i hello [Docker Azure Driver-dokumentationen](https://docs.docker.com/machine/drivers/azure/)

hello följande exempel skapas en virtuell dator med namnet *myVM*, skapar ett användarkonto med namnet *azureuser*, och öppnar port *80* på hello VM-värd. Följ alla prompter toolog i tooyour Azure-konto och bevilja Docker datorn behörigheter toocreate och hantera resurser.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    myvm
```

hello utdata ser ut ungefär toohello följande exempel:

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvmdocker) Completed machine pre-create checks.
Creating machine...
(myvmdocker) Querying existing resource group.  name="docker-machine"
(myvmdocker) Creating resource group.  name="docker-machine" location="westus"
(myvmdocker) Configuring availability set.  name="docker-machine"
(myvmdocker) Configuring network security group.  name="myvmdocker-firewall" location="westus"
(myvmdocker) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvmdocker) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvmdocker) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvmdocker) Creating public IP address.  name="myvmdocker-ip" static=false
(myvmdocker) Creating network interface.  name="myvmdocker-nic"
(myvmdocker) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvmdocker) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvmdocker"
Waiting for machine toobe running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH toobe available...
Detecting hello provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs toohello local machine directory...
Copying certs toohello remote machine...
Setting Docker configuration on hello remote daemon...
Checking connection tooDocker...
Docker is up and running!
toosee how tooconnect your Docker Client toohello Docker Engine running on this virtual machine, run: docker-machine env myvmdocker
```

## <a name="configure-your-docker-shell"></a>Konfigurera Docker-gränssnittet
tooconnect tooyour Docker-värden i Azure, definiera hello rätt anslutningsinställningar. Som nämnts hello slutet av hello utdata visa hello anslutningsinformationen för Docker-värden på följande sätt: 

```bash
docker-machine env myvmdocker
```

hello utdata är liknande toohello följande exempel:

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command tooconfigure your shell:
# eval $(docker-machine env myvmdocker)
```

toodefine hello anslutningsinställningar kan du antingen kör hello förslag configuration kommando (`eval $(docker-machine env myvmdocker)`), eller att du anger miljövariabler för hello manuellt. 

## <a name="run-a-container"></a>Kör en behållare
toosee en behållare i åtgärden, kan du köra en grundläggande NGINX-webbserver. Skapa en behållare med `docker run` och exponera port 80 för Internet-trafik på följande sätt:

```bash
docker run -d -p 80:80 --restart=always nginx
```

hello utdata är liknande toohello följande exempel:

```bash
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
ff3d52d8f55f: Pull complete
226f4ec56ba3: Pull complete
53d7dd52b97d: Pull complete
Digest: sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
Status: Downloaded newer image for nginx:latest
675e6056cb81167fe38ab98bf397164b01b998346d24e567f9eb7a7e94fba14a
```

Visa kör behållare med `docker ps`. hello visas följande exempel hello NGINX-behållaren med port 80 som visas:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-hello-container"></a>Testa hello behållare
Hämta hello offentliga IP-adressen för Docker-värden på följande sätt:


```bash
docker-machine ip myvmdocker
```

toosee hello behållare i åtgärden, öppna en webbläsare och ange hello offentlig IP-adress som anges i hello utdata från hello föregående kommando:

![Körs ngnix behållare](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Nästa steg
Du kan också skapa värdar med hello [Docker VM-tillägget](dockerextension.md). Exempel på med Docker Compose finns [Kom igång med Docker och Skriv i Azure](docker-compose-quickstart.md).
