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
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension"></a><span data-ttu-id="fc3a6-103">Skapa en Docker-miljö i Azure med hjälp av hello Docker VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="fc3a6-103">Create a Docker environment in Azure using hello Docker VM extension</span></span>
<span data-ttu-id="fc3a6-104">Docker är en populär behållarhantering och avbildningsverktyg plattform som gör att du tooquickly fungerar med behållare på Linux.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-104">Docker is a popular container management and imaging platform that allows you tooquickly work with containers on Linux.</span></span> <span data-ttu-id="fc3a6-105">Det finns olika sätt som du kan distribuera Docker enligt tooyour behov i Azure.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-105">In Azure, there are various ways you can deploy Docker according tooyour needs.</span></span> <span data-ttu-id="fc3a6-106">Den här artikeln fokuserar på med hello Docker VM-tillägget och Azure Resource Manager-mallar med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-106">This article focuses on using hello Docker VM extension and Azure Resource Manager templates with hello Azure CLI 2.0.</span></span> <span data-ttu-id="fc3a6-107">Du kan också utföra dessa steg med hello [Azure CLI 1.0](dockerextension-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-107">You can also perform these steps with hello [Azure CLI 1.0](dockerextension-nodejs.md).</span></span>

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="fc3a6-108">Översikt av Azure Docker VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="fc3a6-108">Azure Docker VM extension overview</span></span>
<span data-ttu-id="fc3a6-109">hello Azure Docker VM-tillägget installerar och konfigurerar hello Docker daemon Docker-klienten och Docker Compose i Linux-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-109">hello Azure Docker VM extension installs and configures hello Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="fc3a6-110">Genom att använda hello Azure Docker VM-tillägget kan ha du mer kontroll och funktioner än att bara använda Docker-dator eller skapa hello Docker värd själv.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-110">By using hello Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating hello Docker host yourself.</span></span> <span data-ttu-id="fc3a6-111">Dessa ytterligare funktioner som [Docker Compose](https://docs.docker.com/compose/overview/), se hello Azure Docker VM-tillägget som passar för stabilare utvecklare eller produktion miljöer.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-111">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make hello Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="fc3a6-112">Mer information om hello finns olika distributionsmetoder, inklusive användning av Docker-dator och Azure Behållartjänster hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-112">For more information about hello different deployment methods, including using Docker Machine and Azure Container Services, see hello following articles:</span></span>

* <span data-ttu-id="fc3a6-113">tooquickly prototyp en app, som du kan skapa en enda Docker-värd med [Docker datorn](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-113">tooquickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="fc3a6-114">toobuild produktionsklara, skalbara miljöer som ger ytterligare schemaläggning och hanteringsverktygen, kan du distribuera en [Docker Swarm-kluster på Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-114">toobuild production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a><span data-ttu-id="fc3a6-115">Distribuera en mall med hello Azure Docker VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="fc3a6-115">Deploy a template with hello Azure Docker VM extension</span></span>
<span data-ttu-id="fc3a6-116">Vi använder en befintlig quickstart mallen toocreate en Ubuntu VM som använder hello Azure Docker VM-tillägget tooinstall och konfigurerar hello Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-116">Let's use an existing quickstart template toocreate an Ubuntu VM that uses hello Azure Docker VM extension tooinstall and configure hello Docker host.</span></span> <span data-ttu-id="fc3a6-117">Du kan visa hello mallen här: [enkel distribution av en Ubuntu VM med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-117">You can view hello template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="fc3a6-118">Du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-118">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="fc3a6-119">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-119">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="fc3a6-120">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westus* plats:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-120">hello following example creates a resource group named *myResourceGroup* in hello *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="fc3a6-121">Distribuera en virtuell dator med [az distribution skapa](/cli/azure/group/deployment#create) som innehåller hello Azure Docker VM-tillägget från [Azure Resource Manager-mallen på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-121">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes hello Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="fc3a6-122">Ange egna värden för *newStorageAccountName*, *adminUsername*, *adminPassword*, och *dnsNameForPublicIP* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-122">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP* as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="fc3a6-123">Det tar några minuter för hello distribution toofinish.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-123">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="fc3a6-124">När hello distributionen är klar [flytta toonext steg](#deploy-your-first-nginx-container) tooSSH tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-124">Once hello deployment is finished, [move toonext step](#deploy-your-first-nginx-container) tooSSH tooyour VM.</span></span> 

<span data-ttu-id="fc3a6-125">Tooinstead returnerade kontrollen toohello prompt och låt hello distribution fortsätter i bakgrunden hello, Lägg till hello `--no-wait` flaggan toohello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-125">Optionally, tooinstead return control toohello prompt and let hello deployment continue in hello background, add hello `--no-wait` flag toohello preceding command.</span></span> <span data-ttu-id="fc3a6-126">Den här processen kan tooperform annat arbete i hello CLI medan hello distributionen fortsätter om en stund.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-126">This process allows you tooperform other work in hello CLI while hello deployment continues for a few minutes.</span></span> 

<span data-ttu-id="fc3a6-127">Du kan sedan visa information om hello Docker värdstatusen med [az vm visa](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-127">You can then view details about hello Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="fc3a6-128">hello följande exempel kontrollerar hello status för hello virtuella datorn med namnet *myDockerVM* (hello standardnamnet från hello mall - inte ändrar namnet) i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-128">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="fc3a6-129">När det här kommandot returnerar *lyckades*, hello distributionen är klar och du kan SSH toohello VM i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-129">When this command returns *Succeeded*, hello deployment has finished and you can SSH toohello VM in hello following step.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="fc3a6-130">Distribuera din första nginx-behållare</span><span class="sxs-lookup"><span data-stu-id="fc3a6-130">Deploy your first nginx container</span></span>
<span data-ttu-id="fc3a6-131">tooview information på den virtuella datorn, inklusive hello DNS-namn, Använd `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-131">tooview details of your VM, including hello DNS name, use `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`.</span></span> <span data-ttu-id="fc3a6-132">SSH tooyour nya Docker värdar från din lokala dator enligt följande:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-132">SSH tooyour new Docker host from your local computer as follows:</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="fc3a6-133">När inloggad toohello Docker-värden kan vi köra en nginx-behållare:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-133">Once logged in toohello Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="fc3a6-134">hello-utdata är liknande toohello följande exempel som hello nginx-avbildningen hämtas och en behållare som är igång:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-134">hello output is similar toohello following example as hello nginx image is downloaded and a container started:</span></span>

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

<span data-ttu-id="fc3a6-135">Kontrollera status för hello hello-behållare som körs på värden Docker på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-135">Check hello status of hello containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="fc3a6-136">hello-utdata är liknande toohello som följande exempel, visar hello nginx behållaren körs och TCP-portarna 80 och 443 och vidarebefordras:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-136">hello output is similar toohello following example, showing that hello nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="fc3a6-137">toosee din behållare i åtgärden, öppna upp en webbläsare och ange hello DNS-namn för Docker-värd:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-137">toosee your container in action, open up a web browser and enter hello DNS name of your Docker host:</span></span>

![Körs ngnix behållare](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="fc3a6-139">Azure Docker VM-mall tilläggsreferens</span><span class="sxs-lookup"><span data-stu-id="fc3a6-139">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="fc3a6-140">hello föregående exempel använder en befintlig mall för Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-140">hello previous example uses an existing quickstart template.</span></span> <span data-ttu-id="fc3a6-141">Du kan också distribuera hello Azure Docker VM-tillägget med egna Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="fc3a6-141">You can also deploy hello Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="fc3a6-142">toodo Lägg därför till hello följande tooyour Resource Manager-mallar, definiera hello `vmName` på den virtuella datorn på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-142">toodo so, add hello following tooyour Resource Manager templates, defining hello `vmName` of your VM appropriately:</span></span>

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

<span data-ttu-id="fc3a6-143">Du kan hitta mer detaljerad genomgång om hur du använder Resource Manager-mallar genom att läsa [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-143">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc3a6-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc3a6-144">Next steps</span></span>
<span data-ttu-id="fc3a6-145">Vill du kanske för[konfigurera hello Docker daemon TCP-port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), Förstå [Docker säkerhet](https://docs.docker.com/engine/security/security/), eller distribuera behållare med [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-145">You may wish too[configure hello Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="fc3a6-146">Mer information om hello Azure Docker VM-tillägget själva finns hello [GitHub projekt](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-146">For more information on hello Azure Docker VM Extension itself, see hello [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="fc3a6-147">Läs mer om hello ytterligare Docker distributionsalternativ i Azure:</span><span class="sxs-lookup"><span data-stu-id="fc3a6-147">Read more information about hello additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="fc3a6-148">Använda Docker-dator med hello Azure drivrutin</span><span class="sxs-lookup"><span data-stu-id="fc3a6-148">Use Docker Machine with hello Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="fc3a6-149">[Kom igång med Docker Compose toodefine och köra ett program för flera behållare på en virtuell dator i Azure](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="fc3a6-149">[Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="fc3a6-150">Distribuera ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="fc3a6-150">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

