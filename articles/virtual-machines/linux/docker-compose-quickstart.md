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
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a><span data-ttu-id="82be2-103">Kom igång med Docker och skriv för att definiera och köra ett program för flera behållare i Azure</span><span class="sxs-lookup"><span data-stu-id="82be2-103">Get started with Docker and Compose to define and run a multi-container application in Azure</span></span>
<span data-ttu-id="82be2-104">Med [Compose](http://github.com/docker/compose), du använder en enkel textfil för att definiera ett program som består av flera Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="82be2-104">With [Compose](http://github.com/docker/compose), you use a simple text file to define an application consisting of multiple Docker containers.</span></span> <span data-ttu-id="82be2-105">Sedan få igång ditt program i ett enda kommando som gör allt för att distribuera din definierade miljö.</span><span class="sxs-lookup"><span data-stu-id="82be2-105">You then spin up your application in a single command that does everything to deploy your defined environment.</span></span> <span data-ttu-id="82be2-106">Exempelvis visar den här artikeln hur du snabbt ställa in en WordPress-blogg med en serverdel MariaDB SQL-databas på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="82be2-106">As an example, this article shows you how to quickly set up a WordPress blog with a backend MariaDB SQL database on an Ubuntu VM.</span></span> <span data-ttu-id="82be2-107">Du kan också använda Compose för att ställa in mer komplexa program.</span><span class="sxs-lookup"><span data-stu-id="82be2-107">You can also use Compose to set up more complex applications.</span></span>


## <a name="set-up-a-linux-vm-as-a-docker-host"></a><span data-ttu-id="82be2-108">Konfigurera en Linux-VM som en Docker-värd</span><span class="sxs-lookup"><span data-stu-id="82be2-108">Set up a Linux VM as a Docker host</span></span>
<span data-ttu-id="82be2-109">Du kan använda olika Azure procedurer och tillgängliga avbildningar eller Resource Manager-mallar i Azure Marketplace för att skapa en Linux VM och konfigurera den som en Docker-värd.</span><span class="sxs-lookup"><span data-stu-id="82be2-109">You can use various Azure procedures and available images or Resource Manager templates in the Azure Marketplace to create a Linux VM and set it up as a Docker host.</span></span> <span data-ttu-id="82be2-110">Se exempelvis [använder Docker VM-tillägget för att distribuera din miljö](dockerextension.md) att snabbt skapa en Ubuntu VM med Azure Docker VM-tillägget med hjälp av en [quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="82be2-110">For example, see [Using the Docker VM Extension to deploy your environment](dockerextension.md) to quickly create an Ubuntu VM with the Azure Docker VM extension by using a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="82be2-111">När du använder Docker VM-tillägget, den virtuella datorn konfigureras automatiskt som en Docker-värd och Skriv redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="82be2-111">When you use the Docker VM extension, your VM is automatically set up as a Docker host and Compose is already installed.</span></span>


### <a name="create-docker-host-with-azure-cli-20"></a><span data-ttu-id="82be2-112">Skapa Docker-värd med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="82be2-112">Create Docker host with Azure CLI 2.0</span></span>
<span data-ttu-id="82be2-113">Installera senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in till en Azure med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="82be2-113">Install the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="82be2-114">Börja med att skapa en resursgrupp för din Docker-miljö med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="82be2-114">First, create a resource group for your Docker environment with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="82be2-115">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *westus* plats:</span><span class="sxs-lookup"><span data-stu-id="82be2-115">The following example creates a resource group named *myResourceGroup* in the *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="82be2-116">Distribuera en virtuell dator med [az distribution skapa](/cli/azure/group/deployment#create) som innehåller Azure Docker VM-tillägget från [Azure Resource Manager-mallen på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="82be2-116">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes the Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="82be2-117">Ange egna värden för *newStorageAccountName*, *adminUsername*, *adminPassword*, och *dnsNameForPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="82be2-117">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP*:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="82be2-118">Det tar några minuter för att distributionen ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="82be2-118">It takes a few minutes for the deployment to finish.</span></span> <span data-ttu-id="82be2-119">När distributionen är klar [vidare till nästa steg](#verify-that-compose-is-installed) till SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="82be2-119">Once the deployment is finished, [move to next step](#verify-that-compose-is-installed) to SSH to your VM.</span></span> 

<span data-ttu-id="82be2-120">För att i stället komma tillbaka till meddelandet och gör distributionen fortsätter i bakgrunden, Lägg till den `--no-wait` flagga för att kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="82be2-120">Optionally, to instead return control to the prompt and let the deployment continue in the background, add the `--no-wait` flag to the preceding command.</span></span> <span data-ttu-id="82be2-121">Den här processen kan du utföra annat arbete i CLI medan distributionen fortsätter om en stund.</span><span class="sxs-lookup"><span data-stu-id="82be2-121">This process allows you to perform other work in the CLI while the deployment continues for a few minutes.</span></span> <span data-ttu-id="82be2-122">Du kan sedan visa information om värdstatusen Docker med [az vm visa](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="82be2-122">You can then view details about the Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="82be2-123">I följande exempel kontrollerar status för den virtuella datorn med namnet *myDockerVM* (standardnamnet från mall - inte ändrar namnet) i resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="82be2-123">The following example checks the status of the VM named *myDockerVM* (the default name from the template - don't change this name) in the resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="82be2-124">När det här kommandot returnerar *lyckades*distributionen är klar och du kan SSH till den virtuella datorn i följande steg.</span><span class="sxs-lookup"><span data-stu-id="82be2-124">When this command returns *Succeeded*, the deployment has finished and you can SSH to the VM in the following step.</span></span>


## <a name="verify-that-compose-is-installed"></a><span data-ttu-id="82be2-125">Kontrollera att Skriv är installerad</span><span class="sxs-lookup"><span data-stu-id="82be2-125">Verify that Compose is installed</span></span>
<span data-ttu-id="82be2-126">När distributionen är klar namn du angav under distributionen av SSH till din nya Docker-värden med hjälp av DNS.</span><span class="sxs-lookup"><span data-stu-id="82be2-126">Once the deployment is finished, SSH to your new Docker host using the DNS name you provided during deployment.</span></span> <span data-ttu-id="82be2-127">Du kan använda `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` att visa detaljer för den virtuella datorn, inklusive DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="82be2-127">You can use  `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` to view details of your VM, including the DNS name.</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="82be2-128">Kontrollera att Skriv är installerad på den virtuella datorn genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="82be2-128">To check that Compose is installed on the VM, run the following command:</span></span>

```bash
docker-compose --version
```

<span data-ttu-id="82be2-129">Du kan se utdata som liknar *docker compose 1.6.2, skapa 4d 72027*.</span><span class="sxs-lookup"><span data-stu-id="82be2-129">You see output similar to *docker-compose 1.6.2, build 4d72027*.</span></span>

> [!TIP]
> <span data-ttu-id="82be2-130">Om du använder en annan metod för att skapa en Docker-värd och installera skapar själv, finns det [utgöra dokumentationen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="82be2-130">If you used another method to create a Docker host and need to install Compose yourself, see the [Compose documentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span></span>


## <a name="create-a-docker-composeyml-configuration-file"></a><span data-ttu-id="82be2-131">Skapa en konfigurationsfil för docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="82be2-131">Create a docker-compose.yml configuration file</span></span>
<span data-ttu-id="82be2-132">Sedan skapar du en `docker-compose.yml` fil som är bara en konfiguration textfil, definiera Docker-behållare för att köra på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="82be2-132">Next you create a `docker-compose.yml` file, which is just a text configuration file, to define the Docker containers to run on the VM.</span></span> <span data-ttu-id="82be2-133">Filen anger bilden som ska köras på varje behållare (eller det kan vara en version från en Dockerfile) nödvändiga miljövariabler och beroenden, portar och länkarna mellan behållare.</span><span class="sxs-lookup"><span data-stu-id="82be2-133">The file specifies the image to run on each container (or it could be a build from a Dockerfile), necessary environment variables and dependencies, ports, and the links between containers.</span></span> <span data-ttu-id="82be2-134">Mer information om yml filen syntax finns [utgöra filreferens](https://docs.docker.com/compose/compose-file/).</span><span class="sxs-lookup"><span data-stu-id="82be2-134">For details on yml file syntax, see [Compose file reference](https://docs.docker.com/compose/compose-file/).</span></span>

<span data-ttu-id="82be2-135">Skapa den *docker-compose.yml* enligt följande:</span><span class="sxs-lookup"><span data-stu-id="82be2-135">Create the *docker-compose.yml* file as follows:</span></span>

```bash
touch docker-compose.yml
```

<span data-ttu-id="82be2-136">Du kan använda valfri textredigerare för att lägga till vissa data i filen.</span><span class="sxs-lookup"><span data-stu-id="82be2-136">Use your favorite text editor to add some data to the file.</span></span> <span data-ttu-id="82be2-137">I följande exempel används den *vi* editor:</span><span class="sxs-lookup"><span data-stu-id="82be2-137">The following example uses the *vi* editor:</span></span>

```bash
vi docker-compose.yml
```

<span data-ttu-id="82be2-138">Klistra in följande exempel i en textfil.</span><span class="sxs-lookup"><span data-stu-id="82be2-138">Paste the following example into your text file.</span></span> <span data-ttu-id="82be2-139">Den här konfigurationen använder avbildningar från den [DockerHub registret](https://registry.hub.docker.com/_/wordpress/) installera WordPress (öppen källkod bloggar och innehåll hanteringssystemet) och en länkad serverdel MariaDB SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="82be2-139">This configuration uses images from the [DockerHub Registry](https://registry.hub.docker.com/_/wordpress/) to install WordPress (the open source blogging and content management system) and a linked backend MariaDB SQL database.</span></span> <span data-ttu-id="82be2-140">Ange en egen *MYSQL_ROOT_PASSWORD* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="82be2-140">Enter your own *MYSQL_ROOT_PASSWORD* as follows:</span></span>

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

## <a name="start-the-containers-with-compose"></a><span data-ttu-id="82be2-141">Starta behållarna med Compose</span><span class="sxs-lookup"><span data-stu-id="82be2-141">Start the containers with Compose</span></span>
<span data-ttu-id="82be2-142">I samma katalog som din *docker-compose.yml* filen, kör följande kommando (beroende på din miljö kan du behöva köra `docker-compose` med `sudo`):</span><span class="sxs-lookup"><span data-stu-id="82be2-142">In the same directory as your *docker-compose.yml* file, run the following command (depending on your environment, you might need to run `docker-compose` using `sudo`):</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="82be2-143">Detta kommando startar Docker-behållare som anges i *docker-compose.yml*.</span><span class="sxs-lookup"><span data-stu-id="82be2-143">This command starts the Docker containers specified in *docker-compose.yml*.</span></span> <span data-ttu-id="82be2-144">Det tar en minut eller två för det här steget att slutföra.</span><span class="sxs-lookup"><span data-stu-id="82be2-144">It takes a minute or two for this step to complete.</span></span> <span data-ttu-id="82be2-145">Du kan se utdata som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="82be2-145">You see output similar to the following example:</span></span>

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> <span data-ttu-id="82be2-146">Se till att använda den **-d** alternativ på uppstart så att behållarna som körs kontinuerligt i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="82be2-146">Be sure to use the **-d** option on start-up so that the containers run in the background continuously.</span></span>


<span data-ttu-id="82be2-147">Så här kontrollerar du att behållarna som är igång `docker-compose ps`.</span><span class="sxs-lookup"><span data-stu-id="82be2-147">To verify that the containers are up, type `docker-compose ps`.</span></span> <span data-ttu-id="82be2-148">Du bör se något som liknar:</span><span class="sxs-lookup"><span data-stu-id="82be2-148">You should see something like:</span></span>

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

<span data-ttu-id="82be2-149">Nu kan du ansluta till WordPress direkt på den virtuella datorn på port 80.</span><span class="sxs-lookup"><span data-stu-id="82be2-149">You can now connect to WordPress directly on the VM on port 80.</span></span> <span data-ttu-id="82be2-150">Öppna en webbläsare och ange DNS-namnet på den virtuella datorn (t.ex `http://mypublicdns.westus.cloudapp.azure.com`).</span><span class="sxs-lookup"><span data-stu-id="82be2-150">Open a web browser and enter the DNS name of your VM (such as `http://mypublicdns.westus.cloudapp.azure.com`).</span></span> <span data-ttu-id="82be2-151">Du bör nu se WordPress startskärmen, där du kan slutföra installationen och kom igång med programmet.</span><span class="sxs-lookup"><span data-stu-id="82be2-151">You should now see the WordPress start screen, where you can complete the installation and get started with the application.</span></span>

![Startskärmen för WordPress][wordpress_start]

## <a name="next-steps"></a><span data-ttu-id="82be2-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82be2-153">Next steps</span></span>
* <span data-ttu-id="82be2-154">Gå till den [Docker VM-tillägget användarhandboken](https://github.com/Azure/azure-docker-extension/blob/master/README.md) för flera alternativ för att konfigurera Docker och Skriv i Docker-VM.</span><span class="sxs-lookup"><span data-stu-id="82be2-154">Go to the [Docker VM extension user guide](https://github.com/Azure/azure-docker-extension/blob/master/README.md) for more options to configure Docker and Compose in your Docker VM.</span></span> <span data-ttu-id="82be2-155">Till exempel är ett alternativ att placera filen Skriv yml (konverteras till JSON) direkt i konfigurationen av Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="82be2-155">For example, one option is to put the Compose yml file (converted to JSON) directly in the configuration of the Docker VM extension.</span></span>
* <span data-ttu-id="82be2-156">Kolla in den [utgöra Kommandoradsreferens](http://docs.docker.com/compose/reference/) och [användarhandboken](http://docs.docker.com/compose/) fler exempel på Skapa och distribuera flera behållare appar.</span><span class="sxs-lookup"><span data-stu-id="82be2-156">Check out the [Compose command-line reference](http://docs.docker.com/compose/reference/) and [user guide](http://docs.docker.com/compose/) for more examples of building and deploying multi-container apps.</span></span>
* <span data-ttu-id="82be2-157">Använda en Azure Resource Manager-mall, antingen din egen eller en bidragit från den [community](https://azure.microsoft.com/documentation/templates/), för att distribuera en virtuell Azure-dator med Docker och ett program med Compose.</span><span class="sxs-lookup"><span data-stu-id="82be2-157">Use an Azure Resource Manager template, either your own or one contributed from the [community](https://azure.microsoft.com/documentation/templates/), to deploy an Azure VM with Docker and an application set up with Compose.</span></span> <span data-ttu-id="82be2-158">Till exempel den [distribuerar en WordPress-blogg med Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) mallen använder Docker och skriv för att snabbt distribuera WordPress med en MySQL-serverdel på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="82be2-158">For example, the [Deploy a WordPress blog with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) template uses Docker and Compose to quickly deploy WordPress with a MySQL backend on an Ubuntu VM.</span></span>
* <span data-ttu-id="82be2-159">Försök integrera Docker Compose med Docker Swarm-kluster.</span><span class="sxs-lookup"><span data-stu-id="82be2-159">Try integrating Docker Compose with a Docker Swarm cluster.</span></span> <span data-ttu-id="82be2-160">Se [med utgöra med Swarm](https://docs.docker.com/compose/swarm/) för scenarier.</span><span class="sxs-lookup"><span data-stu-id="82be2-160">See [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) for scenarios.</span></span>

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
