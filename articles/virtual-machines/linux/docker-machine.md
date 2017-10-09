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
# <a name="how-toouse-docker-machine-toocreate-hosts-in-azure"></a><span data-ttu-id="028bf-103">Hur toouse Docker datorn toocreate värd i Azure</span><span class="sxs-lookup"><span data-stu-id="028bf-103">How toouse Docker Machine toocreate hosts in Azure</span></span>
<span data-ttu-id="028bf-104">Den här artikeln information om hur toouse [Docker datorn](https://docs.docker.com/machine/) toocreate värdar i Azure.</span><span class="sxs-lookup"><span data-stu-id="028bf-104">This article details how toouse [Docker Machine](https://docs.docker.com/machine/) toocreate hosts in Azure.</span></span> <span data-ttu-id="028bf-105">Hej `docker-machine` kommando skapar en Linux-dator (VM) i Azure och sedan installerar Docker.</span><span class="sxs-lookup"><span data-stu-id="028bf-105">hello `docker-machine` command creates a Linux virtual machine (VM) in Azure then installs Docker.</span></span> <span data-ttu-id="028bf-106">Du kan sedan hantera Docker-värdar i Azure med hjälp av hello samma lokala verktyg och arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="028bf-106">You can then manage your Docker hosts in Azure using hello same local tools and workflows.</span></span>

## <a name="create-vms-with-docker-machine"></a><span data-ttu-id="028bf-107">Skapa virtuella datorer med Docker-dator</span><span class="sxs-lookup"><span data-stu-id="028bf-107">Create VMs with Docker Machine</span></span>
<span data-ttu-id="028bf-108">Först hämta ditt Azure prenumerations-ID med [az konto visa](/cli/azure/account#show) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="028bf-108">First, obtain your Azure subscription ID with [az account show](/cli/azure/account#show) as follows:</span></span>

```azurecli
sub=$(az account show --query "id" -o tsv)
```

<span data-ttu-id="028bf-109">Du skapar virtuella datorer Docker-värden i Azure med `docker-machine create` genom att ange *azure* som hello drivrutin.</span><span class="sxs-lookup"><span data-stu-id="028bf-109">You create Docker host VMs in Azure with `docker-machine create` by specifying *azure* as hello driver.</span></span> <span data-ttu-id="028bf-110">Mer information finns i hello [Docker Azure Driver-dokumentationen](https://docs.docker.com/machine/drivers/azure/)</span><span class="sxs-lookup"><span data-stu-id="028bf-110">For more information, see hello [Docker Azure Driver documentation](https://docs.docker.com/machine/drivers/azure/)</span></span>

<span data-ttu-id="028bf-111">hello följande exempel skapas en virtuell dator med namnet *myVM*, skapar ett användarkonto med namnet *azureuser*, och öppnar port *80* på hello VM-värd.</span><span class="sxs-lookup"><span data-stu-id="028bf-111">hello following example creates a VM named *myVM*, creates a user account named *azureuser*, and opens port *80* on hello host VM.</span></span> <span data-ttu-id="028bf-112">Följ alla prompter toolog i tooyour Azure-konto och bevilja Docker datorn behörigheter toocreate och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="028bf-112">Follow any prompts toolog in tooyour Azure account and grant Docker Machine permissions toocreate and manage resources.</span></span>

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    myvm
```

<span data-ttu-id="028bf-113">hello utdata ser ut ungefär toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="028bf-113">hello output looks similar toohello following example:</span></span>

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

## <a name="configure-your-docker-shell"></a><span data-ttu-id="028bf-114">Konfigurera Docker-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="028bf-114">Configure your Docker shell</span></span>
<span data-ttu-id="028bf-115">tooconnect tooyour Docker-värden i Azure, definiera hello rätt anslutningsinställningar.</span><span class="sxs-lookup"><span data-stu-id="028bf-115">tooconnect tooyour Docker host in Azure, define hello appropriate connection settings.</span></span> <span data-ttu-id="028bf-116">Som nämnts hello slutet av hello utdata visa hello anslutningsinformationen för Docker-värden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="028bf-116">As noted at hello end of hello output, view hello connection information for your Docker host as follows:</span></span> 

```bash
docker-machine env myvmdocker
```

<span data-ttu-id="028bf-117">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="028bf-117">hello output is similar toohello following example:</span></span>

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command tooconfigure your shell:
# eval $(docker-machine env myvmdocker)
```

<span data-ttu-id="028bf-118">toodefine hello anslutningsinställningar kan du antingen kör hello förslag configuration kommando (`eval $(docker-machine env myvmdocker)`), eller att du anger miljövariabler för hello manuellt.</span><span class="sxs-lookup"><span data-stu-id="028bf-118">toodefine hello connection settings you can either run hello suggested configuration command (`eval $(docker-machine env myvmdocker)`), or you can set hello environment variables manually.</span></span> 

## <a name="run-a-container"></a><span data-ttu-id="028bf-119">Kör en behållare</span><span class="sxs-lookup"><span data-stu-id="028bf-119">Run a container</span></span>
<span data-ttu-id="028bf-120">toosee en behållare i åtgärden, kan du köra en grundläggande NGINX-webbserver.</span><span class="sxs-lookup"><span data-stu-id="028bf-120">toosee a container in action, lets run a basic NGINX webserver.</span></span> <span data-ttu-id="028bf-121">Skapa en behållare med `docker run` och exponera port 80 för Internet-trafik på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="028bf-121">Create a container with `docker run` and expose port 80 for web traffic as follows:</span></span>

```bash
docker run -d -p 80:80 --restart=always nginx
```

<span data-ttu-id="028bf-122">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="028bf-122">hello output is similar toohello following example:</span></span>

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

<span data-ttu-id="028bf-123">Visa kör behållare med `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="028bf-123">View running containers with `docker ps`.</span></span> <span data-ttu-id="028bf-124">hello visas följande exempel hello NGINX-behållaren med port 80 som visas:</span><span class="sxs-lookup"><span data-stu-id="028bf-124">hello following example output shows hello NGINX container running with port 80 exposed:</span></span>

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-hello-container"></a><span data-ttu-id="028bf-125">Testa hello behållare</span><span class="sxs-lookup"><span data-stu-id="028bf-125">Test hello container</span></span>
<span data-ttu-id="028bf-126">Hämta hello offentliga IP-adressen för Docker-värden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="028bf-126">Obtain hello public IP address of Docker host as follows:</span></span>


```bash
docker-machine ip myvmdocker
```

<span data-ttu-id="028bf-127">toosee hello behållare i åtgärden, öppna en webbläsare och ange hello offentlig IP-adress som anges i hello utdata från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="028bf-127">toosee hello container in action, open a web browser and enter hello public IP address noted in hello output of hello preceding command:</span></span>

![Körs ngnix behållare](./media/docker-machine/nginx.png)

## <a name="next-steps"></a><span data-ttu-id="028bf-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="028bf-129">Next steps</span></span>
<span data-ttu-id="028bf-130">Du kan också skapa värdar med hello [Docker VM-tillägget](dockerextension.md).</span><span class="sxs-lookup"><span data-stu-id="028bf-130">You can also create hosts with hello [Docker VM Extension](dockerextension.md).</span></span> <span data-ttu-id="028bf-131">Exempel på med Docker Compose finns [Kom igång med Docker och Skriv i Azure](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="028bf-131">For examples on using Docker Compose, see [Get started with Docker and Compose in Azure](docker-compose-quickstart.md).</span></span>
