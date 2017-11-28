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
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension-with-hello-azure-cli-10"></a><span data-ttu-id="4c57a-103">Skapa en Docker-miljö i Azure med hello Azure CLI 1.0 hello Docker VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="4c57a-103">Create a Docker environment in Azure using hello Docker VM extension with hello Azure CLI 1.0</span></span>
<span data-ttu-id="4c57a-104">Docker är en populär behållarhantering och avbildningsverktyg plattform som gör att du tooquickly fungerar med behållare på Linux (och även Windows).</span><span class="sxs-lookup"><span data-stu-id="4c57a-104">Docker is a popular container management and imaging platform that allows you tooquickly work with containers on Linux (and Windows as well).</span></span> <span data-ttu-id="4c57a-105">Det finns olika sätt som du kan distribuera Docker enligt tooyour behov i Azure.</span><span class="sxs-lookup"><span data-stu-id="4c57a-105">In Azure, there are various ways you can deploy Docker according tooyour needs.</span></span> <span data-ttu-id="4c57a-106">Den här artikeln fokuserar på att använda hello Docker VM-tillägget och Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="4c57a-106">This article focuses on using hello Docker VM extension and Azure Resource Manager templates.</span></span> 

<span data-ttu-id="4c57a-107">Mer information om hello finns olika distributionsmetoder, inklusive användning av Docker-dator och Azure Behållartjänster hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="4c57a-107">For more information about hello different deployment methods, including using Docker Machine and Azure Container Services, see hello following articles:</span></span>

* <span data-ttu-id="4c57a-108">tooquickly prototyp en app, som du kan skapa en enda Docker-värd med [Docker datorn](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="4c57a-108">tooquickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="4c57a-109">För större och mer stabilt miljöer, kan du använda hello Azure Docker VM-tillägget, som också stöder [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate konsekvent behållardistributionerna.</span><span class="sxs-lookup"><span data-stu-id="4c57a-109">For larger, more stable environments, you can use hello Azure Docker VM extension, which also supports [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate consistent container deployments.</span></span> <span data-ttu-id="4c57a-110">Den här artikeln information med hjälp av hello Azure Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="4c57a-110">This article details using hello Azure Docker VM extension.</span></span>
* <span data-ttu-id="4c57a-111">toobuild produktionsklara, skalbara miljöer som ger ytterligare schemaläggning och hanteringsverktygen, kan du distribuera en [Docker Swarm-kluster på Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="4c57a-111">toobuild production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="4c57a-112">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="4c57a-112">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="4c57a-113">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="4c57a-113">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="4c57a-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="4c57a-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="4c57a-115">[Azure CLI 2.0](dockerextension.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="4c57a-115">[Azure CLI 2.0](dockerextension.md) - our next generation CLI for hello resource management deployment model</span></span> 

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="4c57a-116">Översikt av Azure Docker VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="4c57a-116">Azure Docker VM extension overview</span></span>
<span data-ttu-id="4c57a-117">hello Azure Docker VM-tillägget installerar och konfigurerar hello Docker daemon Docker-klienten och Docker Compose i Linux-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="4c57a-117">hello Azure Docker VM extension installs and configures hello Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="4c57a-118">Genom att använda hello Azure Docker VM-tillägget kan ha du mer kontroll och funktioner än att bara använda Docker-dator eller skapa hello Docker värd själv.</span><span class="sxs-lookup"><span data-stu-id="4c57a-118">By using hello Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating hello Docker host yourself.</span></span> <span data-ttu-id="4c57a-119">Dessa ytterligare funktioner som [Docker Compose](https://docs.docker.com/compose/overview/), se hello Azure Docker VM-tillägget som passar för stabilare utvecklare eller produktion miljöer.</span><span class="sxs-lookup"><span data-stu-id="4c57a-119">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make hello Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="4c57a-120">Azure Resource Manager-mallar definiera hello hela strukturen för din miljö.</span><span class="sxs-lookup"><span data-stu-id="4c57a-120">Azure Resource Manager templates define hello entire structure of your environment.</span></span> <span data-ttu-id="4c57a-121">Mallar kan du toocreate och konfigurera resurser, till exempel hello Docker värden virtuella datorer, lagring, rollbaserad åtkomstkontroll (RBAC) och diagnostik.</span><span class="sxs-lookup"><span data-stu-id="4c57a-121">Templates allow you toocreate and configure resources such as hello Docker host VMs, storage, Role-Based Access Controls (RBAC), and diagnostics.</span></span> <span data-ttu-id="4c57a-122">Du kan återanvända dessa mallar toocreate ytterligare distributioner på ett konsekvent sätt.</span><span class="sxs-lookup"><span data-stu-id="4c57a-122">You can reuse these templates toocreate additional deployments in a consistent manner.</span></span> <span data-ttu-id="4c57a-123">Mer information om Azure Resource Manager och mallar finns [översikt över Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4c57a-123">For more information about Azure Resource Manager and templates, see [Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> 

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a><span data-ttu-id="4c57a-124">Distribuera en mall med hello Azure Docker VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="4c57a-124">Deploy a template with hello Azure Docker VM extension</span></span>
<span data-ttu-id="4c57a-125">Vi använder en befintlig quickstart mallen toocreate en Ubuntu VM som använder hello Azure Docker VM-tillägget tooinstall och konfigurerar hello Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="4c57a-125">Let's use an existing quickstart template toocreate an Ubuntu VM that uses hello Azure Docker VM extension tooinstall and configure hello Docker host.</span></span> <span data-ttu-id="4c57a-126">Du kan visa hello mallen här: [enkel distribution av en Ubuntu VM med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="4c57a-126">You can view hello template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="4c57a-127">Du behöver hello [senaste Azure CLI](../../cli-install-nodejs.md) installerad och logga in med hello Resource Manager-läge på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c57a-127">You need hello [latest Azure CLI](../../cli-install-nodejs.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="4c57a-128">Distribuera hello mallen med hjälp av hello Azure CLI, att ange hello mallen URI.</span><span class="sxs-lookup"><span data-stu-id="4c57a-128">Deploy hello template using hello Azure CLI, specifying hello template URI.</span></span> <span data-ttu-id="4c57a-129">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westus* plats.</span><span class="sxs-lookup"><span data-stu-id="4c57a-129">hello following example creates a resource group named *myResourceGroup* in hello *westus* location.</span></span> <span data-ttu-id="4c57a-130">Använda egna resursgruppens namn och plats på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c57a-130">Use your own resource group name and location as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="4c57a-131">Besvara hello prompter tooname ditt lagringskonto, ange ett användarnamn och lösenord och ange ett DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="4c57a-131">Answer hello prompts tooname your storage account, provide a username and password, and provide a DNS name.</span></span> <span data-ttu-id="4c57a-132">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="4c57a-132">hello output is similar toohello following example:</span></span>

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

<span data-ttu-id="4c57a-133">hello Azure CLI returnerar du toohello fråga efter endast några sekunder, men Docker-värden skapas och konfigureras av hello Azure Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="4c57a-133">hello Azure CLI returns you toohello prompt after only a few seconds, but your Docker host is still being created and configured by hello Azure Docker VM extension.</span></span> <span data-ttu-id="4c57a-134">Det tar några minuter för hello distribution toofinish.</span><span class="sxs-lookup"><span data-stu-id="4c57a-134">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="4c57a-135">Du kan visa information om värdstatusen hello Docker med hello `azure vm show` kommando.</span><span class="sxs-lookup"><span data-stu-id="4c57a-135">You can view details about hello Docker host status using hello `azure vm show` command.</span></span>

<span data-ttu-id="4c57a-136">hello följande exempel kontrollerar hello status för hello virtuella datorn med namnet *myDockerVM* (hello standardnamnet från hello mall - inte ändrar namnet) i hello resursgrupp med namnet *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="4c57a-136">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*.</span></span> <span data-ttu-id="4c57a-137">Ange hello hello resursgruppen som du skapade i föregående steg hello:</span><span class="sxs-lookup"><span data-stu-id="4c57a-137">Enter hello name of hello resource group you created in hello preceding step:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

<span data-ttu-id="4c57a-138">Hej utdata från hello `azure vm show` kommandot är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="4c57a-138">hello output of hello `azure vm show` command is similar toohello following example:</span></span>

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

<span data-ttu-id="4c57a-139">Hello övre delen av hello utdata visas hello **ProvisioningState** av hello VM.</span><span class="sxs-lookup"><span data-stu-id="4c57a-139">Near hello top of hello output, you see hello **ProvisioningState** of hello VM.</span></span> <span data-ttu-id="4c57a-140">När detta visar *lyckades*, hello distributionen är klar och du kan SSH toohello VM.</span><span class="sxs-lookup"><span data-stu-id="4c57a-140">When this displays *Succeeded*, hello deployment has finished and you can SSH toohello VM.</span></span>

<span data-ttu-id="4c57a-141">Hello utgången av hello utdata *FQDN* visar hello fullständigt kvalificerade domännamnet för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="4c57a-141">Towards hello end of hello output, *FQDN* displays hello fully qualified domain name of your Docker host.</span></span> <span data-ttu-id="4c57a-142">Den här FQDN är det du använder tooSSH tooyour Docker-värd i hello återstående steg.</span><span class="sxs-lookup"><span data-stu-id="4c57a-142">This FQDN is what you use tooSSH tooyour Docker host in hello remaining steps.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="4c57a-143">Distribuera din första nginx-behållare</span><span class="sxs-lookup"><span data-stu-id="4c57a-143">Deploy your first nginx container</span></span>
<span data-ttu-id="4c57a-144">En gång hello distributionen har slutförts SSH tooyour nya Docker värden från den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="4c57a-144">Once hello deployment has finished, SSH tooyour new Docker host from your local computer.</span></span> <span data-ttu-id="4c57a-145">Ange användarnamn och FQDN på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c57a-145">Enter your own username and FQDN as follows:</span></span>

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="4c57a-146">När inloggad toohello Docker-värden kan vi köra en nginx-behållare:</span><span class="sxs-lookup"><span data-stu-id="4c57a-146">Once logged in toohello Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="4c57a-147">hello-utdata är liknande toohello följande exempel som hello nginx-avbildningen hämtas och en behållare som är igång:</span><span class="sxs-lookup"><span data-stu-id="4c57a-147">hello output is similar toohello following example as hello nginx image is downloaded and a container started:</span></span>

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

<span data-ttu-id="4c57a-148">Kontrollera status för hello hello-behållare som körs på värden Docker på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c57a-148">Check hello status of hello containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="4c57a-149">hello-utdata är liknande toohello som följande exempel, visar hello nginx behållaren körs och TCP-portarna 80 och 443 och vidarebefordras:</span><span class="sxs-lookup"><span data-stu-id="4c57a-149">hello output is similar toohello following example, showing that hello nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="4c57a-150">toosee din behållare i åtgärden, öppna upp en webbläsare och ange hello FQDN-namn för Docker-värd:</span><span class="sxs-lookup"><span data-stu-id="4c57a-150">toosee your container in action, open up a web browser and enter hello FQDN name of your Docker host:</span></span>

![Körs ngnix behållare](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="4c57a-152">Azure Docker VM-mall tilläggsreferens</span><span class="sxs-lookup"><span data-stu-id="4c57a-152">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="4c57a-153">hello föregående exempel använder en befintlig mall för Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="4c57a-153">hello previous example uses an existing quickstart template.</span></span> <span data-ttu-id="4c57a-154">Du kan också distribuera hello Azure Docker VM-tillägget med egna Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="4c57a-154">You can also deploy hello Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="4c57a-155">toodo Lägg därför till hello följande tooyour Resource Manager-mallar, definiera hello *vmName* på den virtuella datorn på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="4c57a-155">toodo so, add hello following tooyour Resource Manager templates, defining hello *vmName* of your VM appropriately:</span></span>

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

<span data-ttu-id="4c57a-156">Du kan hitta mer detaljerad genomgång om hur du använder Resource Manager-mallar genom att läsa [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4c57a-156">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c57a-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c57a-157">Next steps</span></span>
<span data-ttu-id="4c57a-158">Vill du kanske för[konfigurera hello Docker daemon TCP-port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), Förstå [Docker säkerhet](https://docs.docker.com/engine/security/security/), eller distribuera behållare med [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="4c57a-158">You may wish too[configure hello Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="4c57a-159">Mer information om hello Azure Docker VM-tillägget själva finns hello [GitHub projekt](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="4c57a-159">For more information on hello Azure Docker VM Extension itself, see hello [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="4c57a-160">Läs mer om hello ytterligare Docker distributionsalternativ i Azure:</span><span class="sxs-lookup"><span data-stu-id="4c57a-160">Read more information about hello additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="4c57a-161">Använda Docker-dator med hello Azure drivrutin</span><span class="sxs-lookup"><span data-stu-id="4c57a-161">Use Docker Machine with hello Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="4c57a-162">[Kom igång med Docker Compose toodefine och köra ett program för flera behållare på en virtuell dator i Azure](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="4c57a-162">[Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="4c57a-163">Distribuera ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="4c57a-163">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

