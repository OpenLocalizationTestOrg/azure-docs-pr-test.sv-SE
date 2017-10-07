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
# <a name="get-started-with-docker-and-compose-toodefine-and-run-a-multi-container-application-in-azure"></a><span data-ttu-id="f47a9-103">Kom igång med Docker och Skriv toodefine och kör ett program för flera behållare i Azure</span><span class="sxs-lookup"><span data-stu-id="f47a9-103">Get started with Docker and Compose toodefine and run a multi-container application in Azure</span></span>
<span data-ttu-id="f47a9-104">Med [Compose](http://github.com/docker/compose), du kan använda en enkel text filen toodefine ett program som består av flera behållare i Docker.</span><span class="sxs-lookup"><span data-stu-id="f47a9-104">With [Compose](http://github.com/docker/compose), you use a simple text file toodefine an application consisting of multiple Docker containers.</span></span> <span data-ttu-id="f47a9-105">Du kan sedan få igång ditt program i ett enda kommando som gör allt toodeploy definierade miljön.</span><span class="sxs-lookup"><span data-stu-id="f47a9-105">You then spin up your application in a single command that does everything toodeploy your defined environment.</span></span> <span data-ttu-id="f47a9-106">Exempelvis visar den här artikeln hur tooquickly du konfigurerar en WordPress-blogg med en serverdel MariaDB SQL-databas på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="f47a9-106">As an example, this article shows you how tooquickly set up a WordPress blog with a backend MariaDB SQL database on an Ubuntu VM.</span></span> <span data-ttu-id="f47a9-107">Du kan också använda Skriv tooset in mer komplexa program.</span><span class="sxs-lookup"><span data-stu-id="f47a9-107">You can also use Compose tooset up more complex applications.</span></span>


## <a name="set-up-a-linux-vm-as-a-docker-host"></a><span data-ttu-id="f47a9-108">Konfigurera en Linux-VM som en Docker-värd</span><span class="sxs-lookup"><span data-stu-id="f47a9-108">Set up a Linux VM as a Docker host</span></span>
<span data-ttu-id="f47a9-109">Du kan använda olika Azure procedurer och tillgängliga avbildningar eller Resource Manager-mallar i hello Azure Marketplace toocreate en Linux VM och konfigurera den som en Docker-värd.</span><span class="sxs-lookup"><span data-stu-id="f47a9-109">You can use various Azure procedures and available images or Resource Manager templates in hello Azure Marketplace toocreate a Linux VM and set it up as a Docker host.</span></span> <span data-ttu-id="f47a9-110">Se exempelvis [med hello Docker VM-tillägget toodeploy miljön](dockerextension.md) tooquickly skapa en Ubuntu VM med hello Azure Docker VM-tillägget med hjälp av en [quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="f47a9-110">For example, see [Using hello Docker VM Extension toodeploy your environment](dockerextension.md) tooquickly create an Ubuntu VM with hello Azure Docker VM extension by using a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="f47a9-111">När du använder hello Docker VM-tillägget, den virtuella datorn konfigureras automatiskt som en Docker-värd och Skriv redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="f47a9-111">When you use hello Docker VM extension, your VM is automatically set up as a Docker host and Compose is already installed.</span></span>


### <a name="create-docker-host-with-azure-cli-20"></a><span data-ttu-id="f47a9-112">Skapa Docker-värd med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f47a9-112">Create Docker host with Azure CLI 2.0</span></span>
<span data-ttu-id="f47a9-113">Installera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="f47a9-113">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="f47a9-114">Börja med att skapa en resursgrupp för din Docker-miljö med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="f47a9-114">First, create a resource group for your Docker environment with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="f47a9-115">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westus* plats:</span><span class="sxs-lookup"><span data-stu-id="f47a9-115">hello following example creates a resource group named *myResourceGroup* in hello *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="f47a9-116">Distribuera en virtuell dator med [az distribution skapa](/cli/azure/group/deployment#create) som innehåller hello Azure Docker VM-tillägget från [Azure Resource Manager-mallen på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="f47a9-116">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes hello Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="f47a9-117">Ange egna värden för *newStorageAccountName*, *adminUsername*, *adminPassword*, och *dnsNameForPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="f47a9-117">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP*:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="f47a9-118">Det tar några minuter för hello distribution toofinish.</span><span class="sxs-lookup"><span data-stu-id="f47a9-118">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="f47a9-119">När hello distributionen är klar [flytta toonext steg](#verify-that-compose-is-installed) tooSSH tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="f47a9-119">Once hello deployment is finished, [move toonext step](#verify-that-compose-is-installed) tooSSH tooyour VM.</span></span> 

<span data-ttu-id="f47a9-120">Tooinstead returnerade kontrollen toohello prompt och låt hello distribution fortsätter i bakgrunden hello, Lägg till hello `--no-wait` flaggan toohello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="f47a9-120">Optionally, tooinstead return control toohello prompt and let hello deployment continue in hello background, add hello `--no-wait` flag toohello preceding command.</span></span> <span data-ttu-id="f47a9-121">Den här processen kan tooperform annat arbete i hello CLI medan hello distributionen fortsätter om en stund.</span><span class="sxs-lookup"><span data-stu-id="f47a9-121">This process allows you tooperform other work in hello CLI while hello deployment continues for a few minutes.</span></span> <span data-ttu-id="f47a9-122">Du kan sedan visa information om hello Docker värdstatusen med [az vm visa](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="f47a9-122">You can then view details about hello Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="f47a9-123">hello följande exempel kontrollerar hello status för hello virtuella datorn med namnet *myDockerVM* (hello standardnamnet från hello mall - inte ändrar namnet) i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="f47a9-123">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="f47a9-124">När det här kommandot returnerar *lyckades*, hello distributionen är klar och du kan SSH toohello VM i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="f47a9-124">When this command returns *Succeeded*, hello deployment has finished and you can SSH toohello VM in hello following step.</span></span>


## <a name="verify-that-compose-is-installed"></a><span data-ttu-id="f47a9-125">Kontrollera att Skriv är installerad</span><span class="sxs-lookup"><span data-stu-id="f47a9-125">Verify that Compose is installed</span></span>
<span data-ttu-id="f47a9-126">När hello distributionen är klar så SSH tooyour nya Docker värden använder hello DNS-namn som du har angett för under distributionen.</span><span class="sxs-lookup"><span data-stu-id="f47a9-126">Once hello deployment is finished, SSH tooyour new Docker host using hello DNS name you provided during deployment.</span></span> <span data-ttu-id="f47a9-127">Du kan använda `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview information på den virtuella datorn, inklusive hello DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="f47a9-127">You can use  `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview details of your VM, including hello DNS name.</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="f47a9-128">toocheck som ska utgöra är installerat på hello VM, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f47a9-128">toocheck that Compose is installed on hello VM, run hello following command:</span></span>

```bash
docker-compose --version
```

<span data-ttu-id="f47a9-129">Du kan se utdata för*docker compose 1.6.2, skapa 4d 72027*.</span><span class="sxs-lookup"><span data-stu-id="f47a9-129">You see output similar too*docker-compose 1.6.2, build 4d72027*.</span></span>

> [!TIP]
> <span data-ttu-id="f47a9-130">Om du använder en annan metod toocreate en Docker-värd och behöver tooinstall skapar själv, se hello [utgöra dokumentationen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="f47a9-130">If you used another method toocreate a Docker host and need tooinstall Compose yourself, see hello [Compose documentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span></span>


## <a name="create-a-docker-composeyml-configuration-file"></a><span data-ttu-id="f47a9-131">Skapa en konfigurationsfil för docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="f47a9-131">Create a docker-compose.yml configuration file</span></span>
<span data-ttu-id="f47a9-132">Sedan skapar du en `docker-compose.yml` fil som är bara en konfigurationsfil för text, toodefine hello Docker behållare toorun på hello VM.</span><span class="sxs-lookup"><span data-stu-id="f47a9-132">Next you create a `docker-compose.yml` file, which is just a text configuration file, toodefine hello Docker containers toorun on hello VM.</span></span> <span data-ttu-id="f47a9-133">hello-filen anger hello avbildningen toorun varje behållare (eller det kan vara en version från en Dockerfile) nödvändiga miljövariabler och beroenden, portar och hello länkar mellan behållare.</span><span class="sxs-lookup"><span data-stu-id="f47a9-133">hello file specifies hello image toorun on each container (or it could be a build from a Dockerfile), necessary environment variables and dependencies, ports, and hello links between containers.</span></span> <span data-ttu-id="f47a9-134">Mer information om yml filen syntax finns [utgöra filreferens](https://docs.docker.com/compose/compose-file/).</span><span class="sxs-lookup"><span data-stu-id="f47a9-134">For details on yml file syntax, see [Compose file reference](https://docs.docker.com/compose/compose-file/).</span></span>

<span data-ttu-id="f47a9-135">Skapa hello *docker-compose.yml* enligt följande:</span><span class="sxs-lookup"><span data-stu-id="f47a9-135">Create hello *docker-compose.yml* file as follows:</span></span>

```bash
touch docker-compose.yml
```

<span data-ttu-id="f47a9-136">Använda din textredigeringsprogram editor tooadd vissa toohello datafil.</span><span class="sxs-lookup"><span data-stu-id="f47a9-136">Use your favorite text editor tooadd some data toohello file.</span></span> <span data-ttu-id="f47a9-137">hello följande exempel används hello *vi* editor:</span><span class="sxs-lookup"><span data-stu-id="f47a9-137">hello following example uses hello *vi* editor:</span></span>

```bash
vi docker-compose.yml
```

<span data-ttu-id="f47a9-138">Klistra in hello som följande exempel i en textfil.</span><span class="sxs-lookup"><span data-stu-id="f47a9-138">Paste hello following example into your text file.</span></span> <span data-ttu-id="f47a9-139">Den här konfigurationen använder avbildningar från hello [DockerHub registret](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello öppen källkod bloggar och innehåll management system) och en länkad serverdel MariaDB SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="f47a9-139">This configuration uses images from hello [DockerHub Registry](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello open source blogging and content management system) and a linked backend MariaDB SQL database.</span></span> <span data-ttu-id="f47a9-140">Ange en egen *MYSQL_ROOT_PASSWORD* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f47a9-140">Enter your own *MYSQL_ROOT_PASSWORD* as follows:</span></span>

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

## <a name="start-hello-containers-with-compose"></a><span data-ttu-id="f47a9-141">Starta hello behållare med Compose</span><span class="sxs-lookup"><span data-stu-id="f47a9-141">Start hello containers with Compose</span></span>
<span data-ttu-id="f47a9-142">Hej i samma katalog som din *docker-compose.yml* filen, kör följande kommando hello (beroende på din miljö kan du behöva toorun `docker-compose` med `sudo`):</span><span class="sxs-lookup"><span data-stu-id="f47a9-142">In hello same directory as your *docker-compose.yml* file, run hello following command (depending on your environment, you might need toorun `docker-compose` using `sudo`):</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="f47a9-143">Detta kommando startar hello Docker-behållare som anges i *docker-compose.yml*.</span><span class="sxs-lookup"><span data-stu-id="f47a9-143">This command starts hello Docker containers specified in *docker-compose.yml*.</span></span> <span data-ttu-id="f47a9-144">Det tar en minut eller två för det här steget toocomplete.</span><span class="sxs-lookup"><span data-stu-id="f47a9-144">It takes a minute or two for this step toocomplete.</span></span> <span data-ttu-id="f47a9-145">Du kan se utdata liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="f47a9-145">You see output similar toohello following example:</span></span>

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> <span data-ttu-id="f47a9-146">Vara säker på att toouse hello **-d** alternativ på uppstart så att hello behållare köras i bakgrunden hello kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="f47a9-146">Be sure toouse hello **-d** option on start-up so that hello containers run in hello background continuously.</span></span>


<span data-ttu-id="f47a9-147">tooverify hello behållare är igång, typen `docker-compose ps`.</span><span class="sxs-lookup"><span data-stu-id="f47a9-147">tooverify that hello containers are up, type `docker-compose ps`.</span></span> <span data-ttu-id="f47a9-148">Du bör se något som liknar:</span><span class="sxs-lookup"><span data-stu-id="f47a9-148">You should see something like:</span></span>

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

<span data-ttu-id="f47a9-149">Nu kan du ansluta tooWordPress direkt på hello VM på port 80.</span><span class="sxs-lookup"><span data-stu-id="f47a9-149">You can now connect tooWordPress directly on hello VM on port 80.</span></span> <span data-ttu-id="f47a9-150">Öppna en webbläsare och ange hello DNS-namnet på den virtuella datorn (t.ex `http://mypublicdns.westus.cloudapp.azure.com`).</span><span class="sxs-lookup"><span data-stu-id="f47a9-150">Open a web browser and enter hello DNS name of your VM (such as `http://mypublicdns.westus.cloudapp.azure.com`).</span></span> <span data-ttu-id="f47a9-151">Du bör nu se hello WordPress startskärmen, där du kan slutföra hello installationen och kom igång med hello program.</span><span class="sxs-lookup"><span data-stu-id="f47a9-151">You should now see hello WordPress start screen, where you can complete hello installation and get started with hello application.</span></span>

![Startskärmen för WordPress][wordpress_start]

## <a name="next-steps"></a><span data-ttu-id="f47a9-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f47a9-153">Next steps</span></span>
* <span data-ttu-id="f47a9-154">Gå toohello [Docker VM-tillägget användarhandboken](https://github.com/Azure/azure-docker-extension/blob/master/README.md) mer alternativ tooconfigure Docker och Skriv i Docker-VM.</span><span class="sxs-lookup"><span data-stu-id="f47a9-154">Go toohello [Docker VM extension user guide](https://github.com/Azure/azure-docker-extension/blob/master/README.md) for more options tooconfigure Docker and Compose in your Docker VM.</span></span> <span data-ttu-id="f47a9-155">Ett alternativ är till exempel tooput hello Skriv yml filen (konverterade tooJSON) direkt i hello konfigurationen av hello Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="f47a9-155">For example, one option is tooput hello Compose yml file (converted tooJSON) directly in hello configuration of hello Docker VM extension.</span></span>
* <span data-ttu-id="f47a9-156">Kolla in hello [utgöra Kommandoradsreferens](http://docs.docker.com/compose/reference/) och [användarhandboken](http://docs.docker.com/compose/) fler exempel på Skapa och distribuera flera behållare appar.</span><span class="sxs-lookup"><span data-stu-id="f47a9-156">Check out hello [Compose command-line reference](http://docs.docker.com/compose/reference/) and [user guide](http://docs.docker.com/compose/) for more examples of building and deploying multi-container apps.</span></span>
* <span data-ttu-id="f47a9-157">Använda en Azure Resource Manager-mall, antingen din eller en bidragit från hello [community](https://azure.microsoft.com/documentation/templates/), toodeploy en Azure-dator med Docker och ett program som konfigurerats med Compose.</span><span class="sxs-lookup"><span data-stu-id="f47a9-157">Use an Azure Resource Manager template, either your own or one contributed from hello [community](https://azure.microsoft.com/documentation/templates/), toodeploy an Azure VM with Docker and an application set up with Compose.</span></span> <span data-ttu-id="f47a9-158">Till exempel hello [distribuerar en WordPress-blogg med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) mallen använder Docker och Skriv tooquickly distribuera WordPress med en MySQL-serverdel på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="f47a9-158">For example, hello [Deploy a WordPress blog with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) template uses Docker and Compose tooquickly deploy WordPress with a MySQL backend on an Ubuntu VM.</span></span>
* <span data-ttu-id="f47a9-159">Försök integrera Docker Compose med Docker Swarm-kluster.</span><span class="sxs-lookup"><span data-stu-id="f47a9-159">Try integrating Docker Compose with a Docker Swarm cluster.</span></span> <span data-ttu-id="f47a9-160">Se [med utgöra med Swarm](https://docs.docker.com/compose/swarm/) för scenarier.</span><span class="sxs-lookup"><span data-stu-id="f47a9-160">See [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) for scenarios.</span></span>

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
