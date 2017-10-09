---
title: "aaaCreate Docker-värdar i Azure med Docker-dator | Microsoft Docs"
description: "Beskriver användningen av Docker datorn toocreate docker-värdar i Azure."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: fbf67e8189bbf33f874c4a9b619a931f28ccee12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a><span data-ttu-id="0eebc-103">Skapa Docker-värdar i Azure med Docker-dator</span><span class="sxs-lookup"><span data-stu-id="0eebc-103">Create Docker Hosts in Azure with Docker-Machine</span></span>
<span data-ttu-id="0eebc-104">Kör [Docker](https://www.docker.com/) behållare kräver en värd VM körs hello docker daemon.</span><span class="sxs-lookup"><span data-stu-id="0eebc-104">Running [Docker](https://www.docker.com/) containers requires a host VM running hello docker daemon.</span></span>
<span data-ttu-id="0eebc-105">Det här avsnittet beskrivs hur toouse hello [docker-datorn](https://docs.docker.com/machine/) kommandot toocreate nya Linux virtuella datorer som konfigurerats med hello Docker-daemon körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="0eebc-105">This topic describes how toouse hello [docker-machine](https://docs.docker.com/machine/) command toocreate new Linux VMs, configured with hello Docker daemon, running in Azure.</span></span> 

<span data-ttu-id="0eebc-106">**Obs!**</span><span class="sxs-lookup"><span data-stu-id="0eebc-106">**Note:**</span></span> 

* <span data-ttu-id="0eebc-107">*Den här artikeln beror på docker-datorn version 0.9.0-rc2 eller större*</span><span class="sxs-lookup"><span data-stu-id="0eebc-107">*This article depends on docker-machine version 0.9.0-rc2 or greater*</span></span>
* <span data-ttu-id="0eebc-108">*Windows-behållare kommer att stödjas via docker-dator i hello nära framtiden*</span><span class="sxs-lookup"><span data-stu-id="0eebc-108">*Windows Containers will be supported through docker-machine in hello near future*</span></span>

## <a name="create-vms-with-docker-machine"></a><span data-ttu-id="0eebc-109">Skapa virtuella datorer med Docker-dator</span><span class="sxs-lookup"><span data-stu-id="0eebc-109">Create VMs with Docker Machine</span></span>
<span data-ttu-id="0eebc-110">Skapa docker värden virtuella datorer i Azure med hello `docker-machine create` kommandot med hello `azure` drivrutin.</span><span class="sxs-lookup"><span data-stu-id="0eebc-110">Create docker host VMs in Azure with hello `docker-machine create` command using hello `azure` driver.</span></span> 

<span data-ttu-id="0eebc-111">hello Azure drivrutinen kräver ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="0eebc-111">hello Azure driver requires your subscription ID.</span></span> <span data-ttu-id="0eebc-112">Du kan använda hello [Azure CLI](cli-install-nodejs.md) eller hello [Azure Portal](https://portal.azure.com) tooretrieve din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0eebc-112">You can use hello [Azure CLI](cli-install-nodejs.md) or hello [Azure Portal](https://portal.azure.com) tooretrieve your Azure Subscription.</span></span> 

<span data-ttu-id="0eebc-113">**Med hjälp av hello Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="0eebc-113">**Using hello Azure Portal**</span></span>

* <span data-ttu-id="0eebc-114">Välj **prenumerationer** från hello navigeringen till vänster sida och kopiera hello prenumerations-id.</span><span class="sxs-lookup"><span data-stu-id="0eebc-114">Select **Subscriptions** from hello left navigation page and copy hello subscription id.</span></span>

<span data-ttu-id="0eebc-115">**Med hjälp av hello Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="0eebc-115">**Using hello Azure CLI**</span></span>

* <span data-ttu-id="0eebc-116">Typen ```azure account list``` och kopiera hello prenumerations-id.</span><span class="sxs-lookup"><span data-stu-id="0eebc-116">Type ```azure account list``` and copy hello subscription id.</span></span>

<span data-ttu-id="0eebc-117">Typen `docker-machine create --driver azure` toosee hello alternativ och standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="0eebc-117">Type `docker-machine create --driver azure` toosee hello options and their default values.</span></span>
<span data-ttu-id="0eebc-118">Du kan också se hello [Docker Azure Driver-dokumentationen](https://docs.docker.com/machine/drivers/azure/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="0eebc-118">You can also see hello [Docker Azure Driver documentation](https://docs.docker.com/machine/drivers/azure/) for more info.</span></span> 

<span data-ttu-id="0eebc-119">hello följande exempel använder hello [standardvärdena](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), men den om du vill ange dessa värden:</span><span class="sxs-lookup"><span data-stu-id="0eebc-119">hello following example relies upon hello [default values](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), but it does optionally set these values:</span></span> 

* <span data-ttu-id="0eebc-120">Azure-dns för hello namnet som associeras med hello offentlig IP-adress och certifikat som genereras.</span><span class="sxs-lookup"><span data-stu-id="0eebc-120">azure-dns for hello name associated with hello public IP and certificates generated.</span></span> <span data-ttu-id="0eebc-121">Detta är hello DNS-namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0eebc-121">This is hello DNS name of your virtual machine.</span></span> <span data-ttu-id="0eebc-122">hello VM kan sedan på ett säkert sätt stoppa, släpper hello dynamisk IP och ange hello möjlighet tooreconnect när hello vm börjar igen med en ny IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0eebc-122">hello VM can then safely stop, release hello dynamic IP, and provide hello ability tooreconnect after hello vm starts again with a new IP.</span></span> <span data-ttu-id="0eebc-123">hello prefixet måste vara unikt för regionen UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="0eebc-123">hello name prefix must be unique for that region  UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span></span>
* <span data-ttu-id="0eebc-124">Öppna port 80 på hello VM för utgående Internetåtkomst</span><span class="sxs-lookup"><span data-stu-id="0eebc-124">open port 80 on hello VM for outbound internet access</span></span>
* <span data-ttu-id="0eebc-125">storleken på hello VM tooutilize snabbare premium-lagring</span><span class="sxs-lookup"><span data-stu-id="0eebc-125">size of hello VM tooutilize faster premium storage</span></span>
* <span data-ttu-id="0eebc-126">Premium-lagring som används för hello virtuell disk</span><span class="sxs-lookup"><span data-stu-id="0eebc-126">premium storage used for hello vm disk</span></span>

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a><span data-ttu-id="0eebc-127">Välj en docker-värd med docker-dator</span><span class="sxs-lookup"><span data-stu-id="0eebc-127">Choose a docker host with docker-machine</span></span>
<span data-ttu-id="0eebc-128">När du har en post i docker-datorn för värden kan du ange standardvärden för hello när du kör docker-kommandon.</span><span class="sxs-lookup"><span data-stu-id="0eebc-128">Once you have an entry in docker-machine for your host, you can set hello default host when running docker commands.</span></span>

## <a name="using-powershell"></a><span data-ttu-id="0eebc-129">Använda PowerShell</span><span class="sxs-lookup"><span data-stu-id="0eebc-129">Using PowerShell</span></span>
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a><span data-ttu-id="0eebc-130">Med hjälp av Bash</span><span class="sxs-lookup"><span data-stu-id="0eebc-130">Using Bash</span></span>
```bash
eval $(docker-machine env MyDockerHost)
```

<span data-ttu-id="0eebc-131">Du kan nu köra docker kommandon mot hello angivna värden</span><span class="sxs-lookup"><span data-stu-id="0eebc-131">You can now run docker commands against hello specified host</span></span>

```
docker ps
docker info
```

## <a name="run-a-container"></a><span data-ttu-id="0eebc-132">Kör en behållare</span><span class="sxs-lookup"><span data-stu-id="0eebc-132">Run a container</span></span>
<span data-ttu-id="0eebc-133">Med en värd som har konfigurerats och kan du nu köra en enkel web server tootest om värden har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="0eebc-133">With a host configured, you can now run a simple web server tootest whether your host was configured correctly.</span></span>
<span data-ttu-id="0eebc-134">Här vi använder en standard nginx-avbildning, ange att den ska lyssna på port 80 och om hello värden VM har startats om kommer hello behållare startar om samt (`--restart=always`).</span><span class="sxs-lookup"><span data-stu-id="0eebc-134">Here we use a standard nginx image, specify that it should listen on port 80, and that if hello host VM restarts, hello container will restart as well (`--restart=always`).</span></span> 

```bash
docker run -d -p 80:80 --restart=always nginx
```

<span data-ttu-id="0eebc-135">hello utdata ska se ut ungefär hello följande:</span><span class="sxs-lookup"><span data-stu-id="0eebc-135">hello output should look something like hello following:</span></span>

```
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-hello-container"></a><span data-ttu-id="0eebc-136">Testa hello behållare</span><span class="sxs-lookup"><span data-stu-id="0eebc-136">Test hello container</span></span>
<span data-ttu-id="0eebc-137">Granska körs behållare med `docker ps`:</span><span class="sxs-lookup"><span data-stu-id="0eebc-137">Examine running containers using `docker ps`:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

<span data-ttu-id="0eebc-138">Och toosee hello kör behållare, typen `docker-machine ip <VM name>` toofind hello IP-adress tooenter i hello webbläsare:</span><span class="sxs-lookup"><span data-stu-id="0eebc-138">And, toosee hello running container, type `docker-machine ip <VM name>` toofind hello IP address tooenter in hello browser:</span></span>

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Körs ngnix behållare](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a><span data-ttu-id="0eebc-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0eebc-140">Summary</span></span>
<span data-ttu-id="0eebc-141">Du kan enkelt etablera docker-värdar i Azure för din enskilda docker värden verifieringar med docker-datorn.</span><span class="sxs-lookup"><span data-stu-id="0eebc-141">With docker-machine, you can easily provision docker hosts in Azure for your individual docker host validations.</span></span>
<span data-ttu-id="0eebc-142">Produktion av behållare finns hello [Azure Container Service](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="0eebc-142">For production hosting of containers, see hello [Azure Container Service](http://aka.ms/AzureContainerService)</span></span>

<span data-ttu-id="0eebc-143">toodevelop .NET Core-program med Visual Studio finns [Docker Tools för Visual Studio](http://aka.ms/DockerToolsForVS)</span><span class="sxs-lookup"><span data-stu-id="0eebc-143">toodevelop .NET Core Applications with Visual Studio, see [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)</span></span>

