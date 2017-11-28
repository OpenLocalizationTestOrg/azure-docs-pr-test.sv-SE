---
title: "Installera MongoDB på en virtuell Linux-dator med Azure CLI | Microsoft Docs"
description: "Lär dig hur du installerar och konfigurerar MongoDB på en Linux virtuella iusing Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: e19c09558285497f29eb78b4f4ae5b15d7f1a191
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm"></a><span data-ttu-id="204aa-103">Installera och konfigurera MongoDB på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="204aa-103">How to install and configure MongoDB on a Linux VM</span></span>
<span data-ttu-id="204aa-104">[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="204aa-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="204aa-105">Den här artikeln visar hur du installerar och konfigurerar MongoDB på en Linux-VM med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="204aa-105">This article shows you how to install and configure MongoDB on a Linux VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="204aa-106">Du kan också utföra dessa steg med [Azure CLI 1.0](install-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="204aa-106">You can also perform these steps with the [Azure CLI 1.0](install-mongodb-nodejs.md).</span></span> <span data-ttu-id="204aa-107">Exempel visas hur detaljer till:</span><span class="sxs-lookup"><span data-stu-id="204aa-107">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="204aa-108">Installera och konfigurera en grundläggande MongoDB-instansen manuellt</span><span class="sxs-lookup"><span data-stu-id="204aa-108">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="204aa-109">Skapa en grundläggande MongoDB-instans med en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="204aa-109">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="204aa-110">Skapa en komplex MongoDB delat kluster med replik anger med hjälp av en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="204aa-110">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="204aa-111">Installera och konfigurera MongoDB på en virtuell dator manuellt</span><span class="sxs-lookup"><span data-stu-id="204aa-111">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="204aa-112">MongoDB [ange installationsinstruktioner](https://docs.mongodb.com/manual/administration/install-on-linux/) för Linux-distributioner inklusive Red Hat / CentOS, SUSE, Ubuntu och Debian.</span><span class="sxs-lookup"><span data-stu-id="204aa-112">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="204aa-113">I följande exempel skapas en *CentOS* VM.</span><span class="sxs-lookup"><span data-stu-id="204aa-113">The following example creates a *CentOS* VM.</span></span> <span data-ttu-id="204aa-114">Om du vill skapa den här miljön måste senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="204aa-114">To create this environment, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="204aa-115">Skapa en resursgrupp med [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="204aa-115">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="204aa-116">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="204aa-116">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="204aa-117">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="204aa-117">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="204aa-118">I följande exempel skapas en virtuell dator med namnet *myVM* med en användare med namnet *azureuser* med SSH-autentisering för offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="204aa-118">The following example creates a VM named *myVM* with a user named *azureuser* using SSH public key authentication</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="204aa-119">SSH till den virtuella datorn med hjälp av egna användarnamn och `publicIpAddress` anges i resultatet från föregående steg:</span><span class="sxs-lookup"><span data-stu-id="204aa-119">SSH to the VM using your own username and the `publicIpAddress` listed in the output from the previous step:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="204aa-120">För att lägga till installationskällor för MongoDB, skapa en **yum** fil i databasen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="204aa-120">To add the installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="204aa-121">Öppna filen MongoDB lagringsplatsen för redigering.</span><span class="sxs-lookup"><span data-stu-id="204aa-121">Open the MongoDB repo file for editing.</span></span> <span data-ttu-id="204aa-122">Lägg till följande rader:</span><span class="sxs-lookup"><span data-stu-id="204aa-122">Add the following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="204aa-123">Installera MongoDB med **yum** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="204aa-123">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="204aa-124">Som standard aktiveras SELinux på CentOS bilder som hindrar dig från att komma åt MongoDB.</span><span class="sxs-lookup"><span data-stu-id="204aa-124">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="204aa-125">Installera hanteringsverktyg och konfigurera SELinux för att tillåta MongoDB att använda dess TCP-standardporten 27017 på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="204aa-125">Install policy management tools and configure SELinux to allow MongoDB to operate on its default TCP port 27017 as follows:</span></span>

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="204aa-126">Starta tjänsten MongoDB på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="204aa-126">Start the MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="204aa-127">Kontrollera installationen av MongoDB genom att ansluta med hjälp av lokalt `mongo` klient:</span><span class="sxs-lookup"><span data-stu-id="204aa-127">Verify the MongoDB installation by connecting using the local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="204aa-128">Nu testa MongoDB-instansen genom att lägga till vissa data och sedan söka:</span><span class="sxs-lookup"><span data-stu-id="204aa-128">Now test the MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="204aa-129">Om du vill konfigurera MongoDB för att starta automatiskt under en omstart av systemet:</span><span class="sxs-lookup"><span data-stu-id="204aa-129">If desired, configure MongoDB to start automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="204aa-130">Skapa en grundläggande MongoDB-instans på CentOS med en mall</span><span class="sxs-lookup"><span data-stu-id="204aa-130">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="204aa-131">Du kan skapa en grundläggande MongoDB-instans på en enda CentOS VM som använder följande Azure quickstart-mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="204aa-131">You can create a basic MongoDB instance on a single CentOS VM using the following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="204aa-132">Den här mallen använder tillägget för anpassat skript för Linux för att lägga till en **yum** databasen till den nyligen skapade CentOS VM och sedan installera MongoDB.</span><span class="sxs-lookup"><span data-stu-id="204aa-132">This template uses the Custom Script extension for Linux to add a **yum** repository to your newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="204aa-133">[Grundläggande MongoDB-instansen på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="204aa-133">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="204aa-134">Om du vill skapa den här miljön måste senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="204aa-134">To create this environment, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="204aa-135">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="204aa-135">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="204aa-136">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="204aa-136">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="204aa-137">Distribuera mallen med MongoDB [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="204aa-137">Next, deploy the MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="204aa-138">Definiera en egen resurs namn och storlekar vid behov, till exempel som för *newStorageAccountName*, *virtualNetworkName*, och *vmSize*:</span><span class="sxs-lookup"><span data-stu-id="204aa-138">Define your own resource names and sizes where needed such as for *newStorageAccountName*, *virtualNetworkName*, and *vmSize*:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"},
    "virtualNetworkName": {"value": "myVnet"},
    "vmSize": {"value": "Standard_DS2_v2"},
    "vmName": {"value": "myVM"},
    "publicIPAddressName": {"value": "myPublicIP"},
    "nicName": {"value": "myNic"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

<span data-ttu-id="204aa-139">Logga in på den virtuella datorn med hjälp av den offentliga DNS-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="204aa-139">Log on to the VM using the public DNS address of your VM.</span></span> <span data-ttu-id="204aa-140">Du kan visa den offentliga DNS-adressen med [az vm visa](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="204aa-140">You can view the public DNS address with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

<span data-ttu-id="204aa-141">SSH till den virtuella datorn med hjälp av användarnamn och offentliga DNS-adress:</span><span class="sxs-lookup"><span data-stu-id="204aa-141">SSH to your VM using your own username and public DNS address:</span></span>

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

<span data-ttu-id="204aa-142">Kontrollera installationen av MongoDB genom att ansluta med hjälp av lokalt `mongo` klienten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="204aa-142">Verify the MongoDB installation by connecting using the local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="204aa-143">Nu testa instansen genom att lägga till vissa data och söka på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="204aa-143">Now test the instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="204aa-144">Skapa ett komplext delat MongoDB-kluster på CentOS med en mall</span><span class="sxs-lookup"><span data-stu-id="204aa-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="204aa-145">Du kan skapa komplexa delat MongoDB-kluster med hjälp av följande Azure quickstart-mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="204aa-145">You can create a complex MongoDB sharded cluster using the following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="204aa-146">Den här mallen följer den [MongoDB delat kluster metodtips](https://docs.mongodb.com/manual/core/sharded-cluster-components/) att ge redundans och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="204aa-146">This template follows the [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) to provide redundancy and high availability.</span></span> <span data-ttu-id="204aa-147">Mallen skapar två delar med tre noder i varje replik.</span><span class="sxs-lookup"><span data-stu-id="204aa-147">The template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="204aa-148">En replik för config server med tre noder skapas också, plus två **mongos** router-servrar som tillhandahåller konsekvenskontroll till program från över delar.</span><span class="sxs-lookup"><span data-stu-id="204aa-148">One config server replica set with three nodes is also created, plus two **mongos** router servers to provide consistency to applications from across the shards.</span></span>

* <span data-ttu-id="204aa-149">[MongoDB horisontell partitionering kluster på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="204aa-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="204aa-150">Distribuera det här komplexa delat MongoDB-kluster kräver mer än 20 kärnor, som vanligtvis är standardvärdet för antal kärnor per region för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="204aa-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically the default core count per region for a subscription.</span></span> <span data-ttu-id="204aa-151">Öppna ett Azure supportbegäran för att öka din antal kärnor.</span><span class="sxs-lookup"><span data-stu-id="204aa-151">Open an Azure support request to increase your core count.</span></span>

<span data-ttu-id="204aa-152">Om du vill skapa den här miljön måste senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="204aa-152">To create this environment, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="204aa-153">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="204aa-153">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="204aa-154">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="204aa-154">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="204aa-155">Distribuera mallen med MongoDB [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="204aa-155">Next, deploy the MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="204aa-156">Definiera en egen resurs namn och storlekar vid behov, till exempel som för *mongoAdminUsername*, *sizeOfDataDiskInGB*, och *configNodeVmSize*:</span><span class="sxs-lookup"><span data-stu-id="204aa-156">Define your own resource names and sizes where needed such as for *mongoAdminUsername*, *sizeOfDataDiskInGB*, and *configNodeVmSize*:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "mongoAdminUsername": {"value": "mongoadmin"},
    "mongoAdminPassword": {"value": "P@ssw0rd!"},
    "dnsNamePrefix": {"value": "mypublicdns"},
    "environment": {"value": "AzureCloud"},
    "numDataDisks": {"value": "4"},
    "sizeOfDataDiskInGB": {"value": 20},
    "centOsVersion": {"value": "7.0"},
    "routerNodeVmSize": {"value": "Standard_DS3_v2"},
    "configNodeVmSize": {"value": "Standard_DS3_v2"},
    "replicaNodeVmSize": {"value": "Standard_DS3_v2"},
    "zabbixServerIPAddress": {"value": "Null"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json \
  --name myMongoDBCluster \
  --no-wait
```

<span data-ttu-id="204aa-157">Den här distributionen kan ta över en timme att distribuera och konfigurera alla VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="204aa-157">This deployment can take over an hour to deploy and configure all the VM instances.</span></span> <span data-ttu-id="204aa-158">Den `--no-wait` används i slutet av kommandot ovan för att komma tillbaka till Kommandotolken när mallen distribueras har godkänts av Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="204aa-158">The `--no-wait` flag is used at the end of the preceding command to return control to the command prompt once the template deployment has been accepted by the Azure platform.</span></span> <span data-ttu-id="204aa-159">Du kan sedan visa Distributionsstatus med [az grupp distribution visa](/cli/azure/group/deployment#show).</span><span class="sxs-lookup"><span data-stu-id="204aa-159">You can then view the deployment status with [az group deployment show](/cli/azure/group/deployment#show).</span></span> <span data-ttu-id="204aa-160">I följande exempel visar statusen för den *myMongoDBCluster* distribution i den *myResourceGroup* resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="204aa-160">The following example views the status for the *myMongoDBCluster* deployment in the *myResourceGroup* resource group:</span></span>

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a><span data-ttu-id="204aa-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="204aa-161">Next steps</span></span>
<span data-ttu-id="204aa-162">I dessa fall måste ansluta du till MongoDB-instansen lokalt från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="204aa-162">In these examples, you connect to the MongoDB instance locally from the VM.</span></span> <span data-ttu-id="204aa-163">Om du vill ansluta till MongoDB-instansen från en annan virtuell dator eller nätverk, se till att rätt [Nätverkssäkerhetsgruppen regler skapas](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="204aa-163">If you want to connect to the MongoDB instance from another VM or network, ensure the appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="204aa-164">De här exemplen distribuera core MongoDB-miljö för utveckling.</span><span class="sxs-lookup"><span data-stu-id="204aa-164">These examples deploy the core MongoDB environment for development purposes.</span></span> <span data-ttu-id="204aa-165">Tillämpa obligatoriska säkerhet konfigurationsalternativ för din miljö.</span><span class="sxs-lookup"><span data-stu-id="204aa-165">Apply the required security configuration options for your environment.</span></span> <span data-ttu-id="204aa-166">Mer information finns i [MongoDB säkerhet docs](https://docs.mongodb.com/manual/security/).</span><span class="sxs-lookup"><span data-stu-id="204aa-166">For more information, see the [MongoDB security docs](https://docs.mongodb.com/manual/security/).</span></span>

<span data-ttu-id="204aa-167">Mer information om hur du skapar med hjälp av mallar finns i [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="204aa-167">For more information about creating using templates, see the [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="204aa-168">Azure Resource Manager-mallar använda tillägget för anpassat skript för att hämta och köra skript på din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="204aa-168">The Azure Resource Manager templates use the Custom Script Extension to download and execute scripts on your VMs.</span></span> <span data-ttu-id="204aa-169">Mer information finns i [med hjälp av tillägget för anpassat skript på Azure med Linux-datorer](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="204aa-169">For more information, see [Using the Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

