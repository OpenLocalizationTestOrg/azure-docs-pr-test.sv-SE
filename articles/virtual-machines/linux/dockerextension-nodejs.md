---
title: "Använda Azure Docker VM-tillägget med Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du använder Docker VM-tillägget för att snabbt och säkert distribuera en Docker-miljö i Azure med hjälp av Resource Manager-mallar."
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
ms.openlocfilehash: a3cbcf63533f4042dcd695e141655c5814bd7068
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension-with-the-azure-cli-10"></a><span data-ttu-id="47b5f-103">Skapa en Docker-miljö i Azure med Azure CLI 1.0 Docker VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="47b5f-103">Create a Docker environment in Azure using the Docker VM extension with the Azure CLI 1.0</span></span>
<span data-ttu-id="47b5f-104">Docker är en populär behållarhantering och avbildningsverktyg plattform som gör att du snabbt arbeta med behållare för Linux (och även Windows).</span><span class="sxs-lookup"><span data-stu-id="47b5f-104">Docker is a popular container management and imaging platform that allows you to quickly work with containers on Linux (and Windows as well).</span></span> <span data-ttu-id="47b5f-105">I Azure det, finns olika sätt som du kan distribuera Docker efter dina behov.</span><span class="sxs-lookup"><span data-stu-id="47b5f-105">In Azure, there are various ways you can deploy Docker according to your needs.</span></span> <span data-ttu-id="47b5f-106">Den här artikeln fokuserar på att använda Docker VM-tillägget och Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="47b5f-106">This article focuses on using the Docker VM extension and Azure Resource Manager templates.</span></span> 

<span data-ttu-id="47b5f-107">Mer information om olika distributionsmetoder, inklusive användning av Docker-dator och Azure Container Services finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="47b5f-107">For more information about the different deployment methods, including using Docker Machine and Azure Container Services, see the following articles:</span></span>

* <span data-ttu-id="47b5f-108">Att snabbt prototyp en app du kan skapa en enda Docker-värd med [Docker datorn](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="47b5f-108">To quickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="47b5f-109">För större och mer stabilt miljöer, kan du använda Azure Docker VM-tillägget, som också stöder [Docker Compose](https://docs.docker.com/compose/overview/) att generera konsekvent behållardistributionerna.</span><span class="sxs-lookup"><span data-stu-id="47b5f-109">For larger, more stable environments, you can use the Azure Docker VM extension, which also supports [Docker Compose](https://docs.docker.com/compose/overview/) to generate consistent container deployments.</span></span> <span data-ttu-id="47b5f-110">Den här artikeln information med hjälp av Azure Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="47b5f-110">This article details using the Azure Docker VM extension.</span></span>
* <span data-ttu-id="47b5f-111">När du skapar produktionsklara, skalbara miljöer som ger ytterligare schemaläggning och hanteringsverktygen ska du distribuera en [Docker Swarm-kluster på Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="47b5f-111">To build production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="47b5f-112">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="47b5f-112">CLI versions to complete the task</span></span>
<span data-ttu-id="47b5f-113">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="47b5f-113">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="47b5f-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="47b5f-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="47b5f-115">[Azure CLI 2.0](dockerextension.md) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="47b5f-115">[Azure CLI 2.0](dockerextension.md) - our next generation CLI for the resource management deployment model</span></span> 

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="47b5f-116">Översikt av Azure Docker VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="47b5f-116">Azure Docker VM extension overview</span></span>
<span data-ttu-id="47b5f-117">Azure Docker VM-tillägget installeras och konfigureras Docker-daemon, Docker-klienten och Docker Compose i Linux-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="47b5f-117">The Azure Docker VM extension installs and configures the Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="47b5f-118">Med hjälp av Azure Docker VM-tillägget kan ha mer kontroll och funktioner än att bara använda Docker-dator eller skapa Docker-värd själv.</span><span class="sxs-lookup"><span data-stu-id="47b5f-118">By using the Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating the Docker host yourself.</span></span> <span data-ttu-id="47b5f-119">Dessa ytterligare funktioner som [Docker Compose](https://docs.docker.com/compose/overview/), se Azure Docker VM-tillägget som passar för stabilare utvecklare eller produktion miljöer.</span><span class="sxs-lookup"><span data-stu-id="47b5f-119">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make the Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="47b5f-120">Azure Resource Manager-mallar definiera hela strukturen för din miljö.</span><span class="sxs-lookup"><span data-stu-id="47b5f-120">Azure Resource Manager templates define the entire structure of your environment.</span></span> <span data-ttu-id="47b5f-121">Mallar kan du skapa och konfigurera resurser, till exempel Docker värden virtuella datorer, lagring, rollbaserad åtkomstkontroll (RBAC) och diagnostik.</span><span class="sxs-lookup"><span data-stu-id="47b5f-121">Templates allow you to create and configure resources such as the Docker host VMs, storage, Role-Based Access Controls (RBAC), and diagnostics.</span></span> <span data-ttu-id="47b5f-122">Du kan återanvända dessa mallar för att skapa ytterligare distributioner på ett konsekvent sätt.</span><span class="sxs-lookup"><span data-stu-id="47b5f-122">You can reuse these templates to create additional deployments in a consistent manner.</span></span> <span data-ttu-id="47b5f-123">Mer information om Azure Resource Manager och mallar finns [översikt över Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="47b5f-123">For more information about Azure Resource Manager and templates, see [Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> 

## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a><span data-ttu-id="47b5f-124">Distribuera en mall med Azure Docker VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="47b5f-124">Deploy a template with the Azure Docker VM extension</span></span>
<span data-ttu-id="47b5f-125">Vi ska använda en befintlig mall för Snabbstart för att skapa en Ubuntu VM som använder Azure Docker VM-tillägget för att installera och konfigurera Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="47b5f-125">Let's use an existing quickstart template to create an Ubuntu VM that uses the Azure Docker VM extension to install and configure the Docker host.</span></span> <span data-ttu-id="47b5f-126">Du kan visa den här mallen: [enkel distribution av en Ubuntu VM med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="47b5f-126">You can view the template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="47b5f-127">Du behöver den [senaste Azure CLI](../../cli-install-nodejs.md) installerad och logga in med Resource Manager-läget på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="47b5f-127">You need the [latest Azure CLI](../../cli-install-nodejs.md) installed and logged in using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="47b5f-128">Distribuera mallen med hjälp av Azure CLI, ange URI för mallen.</span><span class="sxs-lookup"><span data-stu-id="47b5f-128">Deploy the template using the Azure CLI, specifying the template URI.</span></span> <span data-ttu-id="47b5f-129">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *westus*.</span><span class="sxs-lookup"><span data-stu-id="47b5f-129">The following example creates a resource group named *myResourceGroup* in the *westus* location.</span></span> <span data-ttu-id="47b5f-130">Använda egna resursgruppens namn och plats på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="47b5f-130">Use your own resource group name and location as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="47b5f-131">Besvara anvisningarna för att namnge ditt lagringskonto, ange ett användarnamn och lösenord och ange ett DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="47b5f-131">Answer the prompts to name your storage account, provide a username and password, and provide a DNS name.</span></span> <span data-ttu-id="47b5f-132">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="47b5f-132">The output is similar to the following example:</span></span>

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
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

<span data-ttu-id="47b5f-133">Azure CLI-returnerar du fråga efter endast några sekunder, men Docker-värden är fortfarande som skapat och konfigurerat Azure Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="47b5f-133">The Azure CLI returns you to the prompt after only a few seconds, but your Docker host is still being created and configured by the Azure Docker VM extension.</span></span> <span data-ttu-id="47b5f-134">Det tar några minuter för att distributionen ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="47b5f-134">It takes a few minutes for the deployment to finish.</span></span> <span data-ttu-id="47b5f-135">Du kan visa information om hur du använder för Docker värden status i `azure vm show` kommando.</span><span class="sxs-lookup"><span data-stu-id="47b5f-135">You can view details about the Docker host status using the `azure vm show` command.</span></span>

<span data-ttu-id="47b5f-136">I följande exempel kontrollerar status för den virtuella datorn med namnet *myDockerVM* (standardnamnet från mall - inte ändrar namnet) i resursgrupp med namnet *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="47b5f-136">The following example checks the status of the VM named *myDockerVM* (the default name from the template - don't change this name) in the resource group named *myResourceGroup*.</span></span> <span data-ttu-id="47b5f-137">Ange namnet på resursgruppen som du skapade i föregående steg:</span><span class="sxs-lookup"><span data-stu-id="47b5f-137">Enter the name of the resource group you created in the preceding step:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

<span data-ttu-id="47b5f-138">Utdata från den `azure vm show` kommando som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="47b5f-138">The output of the `azure vm show` command is similar to the following example:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
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

<span data-ttu-id="47b5f-139">Längst upp i utdata, finns det **ProvisioningState** av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="47b5f-139">Near the top of the output, you see the **ProvisioningState** of the VM.</span></span> <span data-ttu-id="47b5f-140">När detta visar *lyckades*distributionen är klar och du kan SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="47b5f-140">When this displays *Succeeded*, the deployment has finished and you can SSH to the VM.</span></span>

<span data-ttu-id="47b5f-141">Mot slutet av utdata, *FQDN* visar fullständigt kvalificerade domännamnet för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="47b5f-141">Towards the end of the output, *FQDN* displays the fully qualified domain name of your Docker host.</span></span> <span data-ttu-id="47b5f-142">Den här FQDN är det du använder till SSH till Docker-värden i stegen.</span><span class="sxs-lookup"><span data-stu-id="47b5f-142">This FQDN is what you use to SSH to your Docker host in the remaining steps.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="47b5f-143">Distribuera din första nginx-behållare</span><span class="sxs-lookup"><span data-stu-id="47b5f-143">Deploy your first nginx container</span></span>
<span data-ttu-id="47b5f-144">När distributionen är klar, SSH till din nya Docker-värden från den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="47b5f-144">Once the deployment has finished, SSH to your new Docker host from your local computer.</span></span> <span data-ttu-id="47b5f-145">Ange användarnamn och FQDN på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="47b5f-145">Enter your own username and FQDN as follows:</span></span>

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="47b5f-146">När inloggad till Docker-värden kan vi köra en nginx-behållare:</span><span class="sxs-lookup"><span data-stu-id="47b5f-146">Once logged in to the Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="47b5f-147">Utdata liknar följande exempel som nginx-avbildningen hämtas och en behållare igång:</span><span class="sxs-lookup"><span data-stu-id="47b5f-147">The output is similar to the following example as the nginx image is downloaded and a container started:</span></span>

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

<span data-ttu-id="47b5f-148">Kontrollera status för de behållare som körs på värden Docker på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="47b5f-148">Check the status of the containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="47b5f-149">Utdata liknar följande exempel, visar att nginx-behållaren körs och TCP-portarna 80 och 443 och vidarebefordras:</span><span class="sxs-lookup"><span data-stu-id="47b5f-149">The output is similar to the following example, showing that the nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="47b5f-150">För att se din behållare i åtgärden, öppna en webbläsare och ange FQDN-namnet på din Docker-värd:</span><span class="sxs-lookup"><span data-stu-id="47b5f-150">To see your container in action, open up a web browser and enter the FQDN name of your Docker host:</span></span>

![Körs ngnix behållare](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="47b5f-152">Azure Docker VM-mall tilläggsreferens</span><span class="sxs-lookup"><span data-stu-id="47b5f-152">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="47b5f-153">Det föregående exemplet används en befintlig mall för Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="47b5f-153">The previous example uses an existing quickstart template.</span></span> <span data-ttu-id="47b5f-154">Du kan också distribuera Azure Docker VM-tillägget med egna Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="47b5f-154">You can also deploy the Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="47b5f-155">Om du vill göra det lägger du till följande Resource Manager-mallar, definiera den *vmName* på den virtuella datorn på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="47b5f-155">To do so, add the following to your Resource Manager templates, defining the *vmName* of your VM appropriately:</span></span>

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

<span data-ttu-id="47b5f-156">Du kan hitta mer detaljerad genomgång om hur du använder Resource Manager-mallar genom att läsa [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="47b5f-156">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="47b5f-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47b5f-157">Next steps</span></span>
<span data-ttu-id="47b5f-158">Du kanske vill [konfigurera Docker-daemon TCP-port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), Förstå [Docker säkerhet](https://docs.docker.com/engine/security/security/), eller distribuera behållare med [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="47b5f-158">You may wish to [configure the Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="47b5f-159">Mer information om Azure Docker VM tillägget själva finns i [GitHub projekt](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="47b5f-159">For more information on the Azure Docker VM Extension itself, see the [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="47b5f-160">Läs mer om ytterligare Docker distributionsalternativen i Azure:</span><span class="sxs-lookup"><span data-stu-id="47b5f-160">Read more information about the additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="47b5f-161">Använda Docker-datorn med Azure-drivrutin</span><span class="sxs-lookup"><span data-stu-id="47b5f-161">Use Docker Machine with the Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="47b5f-162">[Kom igång med Docker och skriv för att definiera och köra ett program för flera behållare på en virtuell dator i Azure](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="47b5f-162">[Get Started with Docker and Compose to define and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="47b5f-163">Distribuera ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="47b5f-163">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

