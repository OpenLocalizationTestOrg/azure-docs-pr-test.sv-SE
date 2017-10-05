---
title: "Skapa Docker-värdar i Azure med Docker-dator | Microsoft Docs"
description: "Beskriver användningen av Docker-datorn för att skapa docker-värdar i Azure."
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
ms.openlocfilehash: 766d327a87ed13e04166d71c3d9ae0a1e7a66d19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a><span data-ttu-id="d478d-103">Skapa Docker-värdar i Azure med Docker-dator</span><span class="sxs-lookup"><span data-stu-id="d478d-103">Create Docker Hosts in Azure with Docker-Machine</span></span>
<span data-ttu-id="d478d-104">Kör [Docker](https://www.docker.com/) behållare kräver en virtuell dator som kör daemonen docker-värd.</span><span class="sxs-lookup"><span data-stu-id="d478d-104">Running [Docker](https://www.docker.com/) containers requires a host VM running the docker daemon.</span></span>
<span data-ttu-id="d478d-105">Det här avsnittet beskriver hur du använder den [docker-datorn](https://docs.docker.com/machine/) kommando för att skapa nya Linux virtuella datorer kan konfigureras med Docker-daemon körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="d478d-105">This topic describes how to use the [docker-machine](https://docs.docker.com/machine/) command to create new Linux VMs, configured with the Docker daemon, running in Azure.</span></span> 

<span data-ttu-id="d478d-106">**Obs!**</span><span class="sxs-lookup"><span data-stu-id="d478d-106">**Note:**</span></span> 

* <span data-ttu-id="d478d-107">*Den här artikeln beror på docker-datorn version 0.9.0-rc2 eller större*</span><span class="sxs-lookup"><span data-stu-id="d478d-107">*This article depends on docker-machine version 0.9.0-rc2 or greater*</span></span>
* <span data-ttu-id="d478d-108">*Windows-behållare stöds via docker-datorn i den nära framtiden*</span><span class="sxs-lookup"><span data-stu-id="d478d-108">*Windows Containers will be supported through docker-machine in the near future*</span></span>

## <a name="create-vms-with-docker-machine"></a><span data-ttu-id="d478d-109">Skapa virtuella datorer med Docker-dator</span><span class="sxs-lookup"><span data-stu-id="d478d-109">Create VMs with Docker Machine</span></span>
<span data-ttu-id="d478d-110">Skapa docker värden virtuella datorer i Azure med den `docker-machine create` med den `azure` drivrutin.</span><span class="sxs-lookup"><span data-stu-id="d478d-110">Create docker host VMs in Azure with the `docker-machine create` command using the `azure` driver.</span></span> 

<span data-ttu-id="d478d-111">Azure drivrutinen kräver ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="d478d-111">The Azure driver requires your subscription ID.</span></span> <span data-ttu-id="d478d-112">Du kan använda den [Azure CLI](cli-install-nodejs.md) eller [Azure Portal](https://portal.azure.com) att hämta din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d478d-112">You can use the [Azure CLI](cli-install-nodejs.md) or the [Azure Portal](https://portal.azure.com) to retrieve your Azure Subscription.</span></span> 

<span data-ttu-id="d478d-113">**Med Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="d478d-113">**Using the Azure Portal**</span></span>

* <span data-ttu-id="d478d-114">Välj **prenumerationer** från navigeringen till vänster sida och kopiera prenumerations-id.</span><span class="sxs-lookup"><span data-stu-id="d478d-114">Select **Subscriptions** from the left navigation page and copy the subscription id.</span></span>

<span data-ttu-id="d478d-115">**Använda Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="d478d-115">**Using the Azure CLI**</span></span>

* <span data-ttu-id="d478d-116">Typen ```azure account list``` och kopiera prenumerations-id.</span><span class="sxs-lookup"><span data-stu-id="d478d-116">Type ```azure account list``` and copy the subscription id.</span></span>

<span data-ttu-id="d478d-117">Typen `docker-machine create --driver azure` alternativen och standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="d478d-117">Type `docker-machine create --driver azure` to see the options and their default values.</span></span>
<span data-ttu-id="d478d-118">Du kan också se den [Docker Azure Driver-dokumentationen](https://docs.docker.com/machine/drivers/azure/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d478d-118">You can also see the [Docker Azure Driver documentation](https://docs.docker.com/machine/drivers/azure/) for more info.</span></span> 

<span data-ttu-id="d478d-119">I följande exempel använder det [standardvärdena](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), men den om du vill ange dessa värden:</span><span class="sxs-lookup"><span data-stu-id="d478d-119">The following example relies upon the [default values](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), but it does optionally set these values:</span></span> 

* <span data-ttu-id="d478d-120">Azure-DNS-namn som är associerade med den offentliga IP-Adressen och certifikat som genereras.</span><span class="sxs-lookup"><span data-stu-id="d478d-120">azure-dns for the name associated with the public IP and certificates generated.</span></span> <span data-ttu-id="d478d-121">Detta är DNS-namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d478d-121">This is the DNS name of your virtual machine.</span></span> <span data-ttu-id="d478d-122">Den virtuella datorn kan sedan på ett säkert sätt stoppa, släpper den dynamiska IP-Adressen och ger möjlighet att återansluta efter den virtuella datorn börjar igen med en ny IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d478d-122">The VM can then safely stop, release the dynamic IP, and provide the ability to reconnect after the vm starts again with a new IP.</span></span> <span data-ttu-id="d478d-123">Namnprefixet måste vara unikt för regionen UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="d478d-123">The name prefix must be unique for that region  UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span></span>
* <span data-ttu-id="d478d-124">Öppna port 80 på den virtuella datorn för utgående Internetåtkomst</span><span class="sxs-lookup"><span data-stu-id="d478d-124">open port 80 on the VM for outbound internet access</span></span>
* <span data-ttu-id="d478d-125">storleken på den virtuella datorn att använda snabbare premium-lagring</span><span class="sxs-lookup"><span data-stu-id="d478d-125">size of the VM to utilize faster premium storage</span></span>
* <span data-ttu-id="d478d-126">Premium-lagring som används för den virtuella disken</span><span class="sxs-lookup"><span data-stu-id="d478d-126">premium storage used for the vm disk</span></span>

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a><span data-ttu-id="d478d-127">Välj en docker-värd med docker-dator</span><span class="sxs-lookup"><span data-stu-id="d478d-127">Choose a docker host with docker-machine</span></span>
<span data-ttu-id="d478d-128">När du har en post i docker-datorn för värden, kan du ange standardvärden när du kör docker-kommandon.</span><span class="sxs-lookup"><span data-stu-id="d478d-128">Once you have an entry in docker-machine for your host, you can set the default host when running docker commands.</span></span>

## <a name="using-powershell"></a><span data-ttu-id="d478d-129">Använda PowerShell</span><span class="sxs-lookup"><span data-stu-id="d478d-129">Using PowerShell</span></span>
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a><span data-ttu-id="d478d-130">Med hjälp av Bash</span><span class="sxs-lookup"><span data-stu-id="d478d-130">Using Bash</span></span>
```bash
eval $(docker-machine env MyDockerHost)
```

<span data-ttu-id="d478d-131">Du kan nu köra docker kommandon mot den angivna värden</span><span class="sxs-lookup"><span data-stu-id="d478d-131">You can now run docker commands against the specified host</span></span>

```
docker ps
docker info
```

## <a name="run-a-container"></a><span data-ttu-id="d478d-132">Kör en behållare</span><span class="sxs-lookup"><span data-stu-id="d478d-132">Run a container</span></span>
<span data-ttu-id="d478d-133">Med en värd som har konfigurerats, kan du nu köra en enkel webbserver för att testa om värden har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="d478d-133">With a host configured, you can now run a simple web server to test whether your host was configured correctly.</span></span>
<span data-ttu-id="d478d-134">Här vi använder en standard nginx-bild och ange att den ska lyssna på port 80. Om den Virtuella värddatorn har startats om kommer behållaren starta om samt (`--restart=always`).</span><span class="sxs-lookup"><span data-stu-id="d478d-134">Here we use a standard nginx image, specify that it should listen on port 80, and that if the host VM restarts, the container will restart as well (`--restart=always`).</span></span> 

```bash
docker run -d -p 80:80 --restart=always nginx
```

<span data-ttu-id="d478d-135">Resultatet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="d478d-135">The output should look something like the following:</span></span>

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a><span data-ttu-id="d478d-136">Testa behållaren</span><span class="sxs-lookup"><span data-stu-id="d478d-136">Test the container</span></span>
<span data-ttu-id="d478d-137">Granska körs behållare med `docker ps`:</span><span class="sxs-lookup"><span data-stu-id="d478d-137">Examine running containers using `docker ps`:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

<span data-ttu-id="d478d-138">Och om du vill visa behållaren körs, Skriv `docker-machine ip <VM name>` att hitta IP-adressen anger i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="d478d-138">And, to see the running container, type `docker-machine ip <VM name>` to find the IP address to enter in the browser:</span></span>

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Körs ngnix behållare](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a><span data-ttu-id="d478d-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d478d-140">Summary</span></span>
<span data-ttu-id="d478d-141">Du kan enkelt etablera docker-värdar i Azure för din enskilda docker värden verifieringar med docker-datorn.</span><span class="sxs-lookup"><span data-stu-id="d478d-141">With docker-machine, you can easily provision docker hosts in Azure for your individual docker host validations.</span></span>
<span data-ttu-id="d478d-142">Produktion av behållare, finns det [Azure Container Service](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="d478d-142">For production hosting of containers, see the [Azure Container Service](http://aka.ms/AzureContainerService)</span></span>

<span data-ttu-id="d478d-143">För att utveckla .NET Core-program med Visual Studio finns [Docker Tools för Visual Studio](http://aka.ms/DockerToolsForVS)</span><span class="sxs-lookup"><span data-stu-id="d478d-143">To develop .NET Core Applications with Visual Studio, see [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)</span></span>

