---
title: "aaaInstall MongoDB på en Linux-VM med hello Azure CLI | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera MongoDB på en Linux virtuella iusing hello Azure CLI 2.0"
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
ms.openlocfilehash: 97a4d9913f0eeaf7b8bf15d7fc81befe538cdc8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm"></a><span data-ttu-id="4a07c-103">Hur tooinstall och konfigurera MongoDB på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="4a07c-103">How tooinstall and configure MongoDB on a Linux VM</span></span>
<span data-ttu-id="4a07c-104">[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="4a07c-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="4a07c-105">Den här artikeln beskrivs hur du tooinstall och konfigurera MongoDB på en Linux-VM med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="4a07c-105">This article shows you how tooinstall and configure MongoDB on a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="4a07c-106">Du kan också utföra dessa steg med hello [Azure CLI 1.0](install-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4a07c-106">You can also perform these steps with hello [Azure CLI 1.0](install-mongodb-nodejs.md).</span></span> <span data-ttu-id="4a07c-107">Exempel visas hur detaljer till:</span><span class="sxs-lookup"><span data-stu-id="4a07c-107">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="4a07c-108">Installera och konfigurera en grundläggande MongoDB-instansen manuellt</span><span class="sxs-lookup"><span data-stu-id="4a07c-108">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="4a07c-109">Skapa en grundläggande MongoDB-instans med en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="4a07c-109">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="4a07c-110">Skapa en komplex MongoDB delat kluster med replik anger med hjälp av en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="4a07c-110">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="4a07c-111">Installera och konfigurera MongoDB på en virtuell dator manuellt</span><span class="sxs-lookup"><span data-stu-id="4a07c-111">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="4a07c-112">MongoDB [ange installationsinstruktioner](https://docs.mongodb.com/manual/administration/install-on-linux/) för Linux-distributioner inklusive Red Hat / CentOS, SUSE, Ubuntu och Debian.</span><span class="sxs-lookup"><span data-stu-id="4a07c-112">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="4a07c-113">hello följande exempel skapas en *CentOS* VM.</span><span class="sxs-lookup"><span data-stu-id="4a07c-113">hello following example creates a *CentOS* VM.</span></span> <span data-ttu-id="4a07c-114">toocreate miljö, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4a07c-114">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="4a07c-115">Skapa en resursgrupp med [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4a07c-115">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4a07c-116">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="4a07c-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4a07c-117">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="4a07c-117">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="4a07c-118">hello följande exempel skapas en virtuell dator med namnet *myVM* med en användare med namnet *azureuser* med SSH-autentisering för offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="4a07c-118">hello following example creates a VM named *myVM* with a user named *azureuser* using SSH public key authentication</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="4a07c-119">SSH toohello VM med hjälp av användarnamn och hello `publicIpAddress` som anges i hello utdata från hello föregående steg:</span><span class="sxs-lookup"><span data-stu-id="4a07c-119">SSH toohello VM using your own username and hello `publicIpAddress` listed in hello output from hello previous step:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="4a07c-120">tooadd hello installationskällor för MongoDB, skapa en **yum** fil i databasen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4a07c-120">tooadd hello installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="4a07c-121">Öppna hello MongoDB-repo-filen för redigering.</span><span class="sxs-lookup"><span data-stu-id="4a07c-121">Open hello MongoDB repo file for editing.</span></span> <span data-ttu-id="4a07c-122">Lägg till hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="4a07c-122">Add hello following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="4a07c-123">Installera MongoDB med **yum** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4a07c-123">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="4a07c-124">Som standard aktiveras SELinux på CentOS bilder som hindrar dig från att komma åt MongoDB.</span><span class="sxs-lookup"><span data-stu-id="4a07c-124">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="4a07c-125">Installera hanteringsverktyg och konfigurera SELinux tooallow MongoDB toooperate på dess TCP-standardporten 27017 på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4a07c-125">Install policy management tools and configure SELinux tooallow MongoDB toooperate on its default TCP port 27017 as follows:</span></span>

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="4a07c-126">Starta hello MongoDB-tjänsten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4a07c-126">Start hello MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="4a07c-127">Kontrollera hello MongoDB installationen genom att ansluta med hello lokala `mongo` klient:</span><span class="sxs-lookup"><span data-stu-id="4a07c-127">Verify hello MongoDB installation by connecting using hello local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="4a07c-128">Nu testa hello MongoDB-instansen genom att lägga till vissa data och sedan söka:</span><span class="sxs-lookup"><span data-stu-id="4a07c-128">Now test hello MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="4a07c-129">Om du vill konfigurera MongoDB toostart automatiskt under en omstart av systemet:</span><span class="sxs-lookup"><span data-stu-id="4a07c-129">If desired, configure MongoDB toostart automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="4a07c-130">Skapa en grundläggande MongoDB-instans på CentOS med en mall</span><span class="sxs-lookup"><span data-stu-id="4a07c-130">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="4a07c-131">Du kan skapa en grundläggande MongoDB-instans på en enda CentOS VM som använder hello följande Azure quickstart-mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="4a07c-131">You can create a basic MongoDB instance on a single CentOS VM using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="4a07c-132">Den här mallen använder hello tillägget för anpassat skript för Linux tooadd en **yum** databasen tooyour nyskapad CentOS VM och sedan installera MongoDB.</span><span class="sxs-lookup"><span data-stu-id="4a07c-132">This template uses hello Custom Script extension for Linux tooadd a **yum** repository tooyour newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="4a07c-133">[Grundläggande MongoDB-instansen på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="4a07c-133">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="4a07c-134">toocreate miljö, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4a07c-134">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="4a07c-135">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4a07c-135">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4a07c-136">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="4a07c-136">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4a07c-137">Distribuera hello MongoDB mall med [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="4a07c-137">Next, deploy hello MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="4a07c-138">Definiera en egen resurs namn och storlekar vid behov, till exempel som för *newStorageAccountName*, *virtualNetworkName*, och *vmSize*:</span><span class="sxs-lookup"><span data-stu-id="4a07c-138">Define your own resource names and sizes where needed such as for *newStorageAccountName*, *virtualNetworkName*, and *vmSize*:</span></span>

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

<span data-ttu-id="4a07c-139">Logga in toohello VM med hello offentlig DNS-adress på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4a07c-139">Log on toohello VM using hello public DNS address of your VM.</span></span> <span data-ttu-id="4a07c-140">Du kan visa hello offentlig DNS-adress med [az vm visa](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="4a07c-140">You can view hello public DNS address with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

<span data-ttu-id="4a07c-141">SSH tooyour VM med hjälp av användarnamn och offentliga DNS-adress:</span><span class="sxs-lookup"><span data-stu-id="4a07c-141">SSH tooyour VM using your own username and public DNS address:</span></span>

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

<span data-ttu-id="4a07c-142">Kontrollera hello MongoDB installationen genom att ansluta med hello lokala `mongo` klienten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4a07c-142">Verify hello MongoDB installation by connecting using hello local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="4a07c-143">Nu testa hello-instans genom att lägga till vissa data och söka på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4a07c-143">Now test hello instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="4a07c-144">Skapa ett komplext delat MongoDB-kluster på CentOS med en mall</span><span class="sxs-lookup"><span data-stu-id="4a07c-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="4a07c-145">Du kan skapa komplexa delat MongoDB-kluster med hjälp av hello följande Azure quickstart-mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="4a07c-145">You can create a complex MongoDB sharded cluster using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="4a07c-146">Den här mallen följer hello [MongoDB delat kluster metodtips](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundans och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="4a07c-146">This template follows hello [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancy and high availability.</span></span> <span data-ttu-id="4a07c-147">hello mallen skapar två delar med tre noder i varje replik.</span><span class="sxs-lookup"><span data-stu-id="4a07c-147">hello template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="4a07c-148">En replik för config server med tre noder skapas också, plus två **mongos** router servrar tooprovide konsekvenskontroll tooapplications från över hello delar.</span><span class="sxs-lookup"><span data-stu-id="4a07c-148">One config server replica set with three nodes is also created, plus two **mongos** router servers tooprovide consistency tooapplications from across hello shards.</span></span>

* <span data-ttu-id="4a07c-149">[MongoDB horisontell partitionering kluster på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="4a07c-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="4a07c-150">Distribuera det här komplexa delat MongoDB-kluster kräver mer än 20 kärnor, som vanligtvis är antal hello standard kärnor per region för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4a07c-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically hello default core count per region for a subscription.</span></span> <span data-ttu-id="4a07c-151">Öppna ett Azure-supporten begäran tooincrease din antal kärnor.</span><span class="sxs-lookup"><span data-stu-id="4a07c-151">Open an Azure support request tooincrease your core count.</span></span>

<span data-ttu-id="4a07c-152">toocreate miljö, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4a07c-152">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="4a07c-153">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4a07c-153">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4a07c-154">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="4a07c-154">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4a07c-155">Distribuera hello MongoDB mall med [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="4a07c-155">Next, deploy hello MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="4a07c-156">Definiera en egen resurs namn och storlekar vid behov, till exempel som för *mongoAdminUsername*, *sizeOfDataDiskInGB*, och *configNodeVmSize*:</span><span class="sxs-lookup"><span data-stu-id="4a07c-156">Define your own resource names and sizes where needed such as for *mongoAdminUsername*, *sizeOfDataDiskInGB*, and *configNodeVmSize*:</span></span>

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

<span data-ttu-id="4a07c-157">Den här distributionen kan ta över en timme toodeploy och konfigurera alla hello VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="4a07c-157">This deployment can take over an hour toodeploy and configure all hello VM instances.</span></span> <span data-ttu-id="4a07c-158">Hej `--no-wait` flaggan används hello slutet av hello före kommandot tooreturn kontrollen toohello Kommandotolken när hello malldistribution har godkänts av hello Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="4a07c-158">hello `--no-wait` flag is used at hello end of hello preceding command tooreturn control toohello command prompt once hello template deployment has been accepted by hello Azure platform.</span></span> <span data-ttu-id="4a07c-159">Du kan sedan visa hello Distributionsstatus med [az grupp distribution visa](/cli/azure/group/deployment#show).</span><span class="sxs-lookup"><span data-stu-id="4a07c-159">You can then view hello deployment status with [az group deployment show](/cli/azure/group/deployment#show).</span></span> <span data-ttu-id="4a07c-160">hello följande exempel vyer hello status för hello *myMongoDBCluster* distribution i hello *myResourceGroup* resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="4a07c-160">hello following example views hello status for hello *myMongoDBCluster* deployment in hello *myResourceGroup* resource group:</span></span>

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a><span data-ttu-id="4a07c-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a07c-161">Next steps</span></span>
<span data-ttu-id="4a07c-162">I dessa fall måste ansluta du toohello MongoDB-instansen lokalt från hello VM.</span><span class="sxs-lookup"><span data-stu-id="4a07c-162">In these examples, you connect toohello MongoDB instance locally from hello VM.</span></span> <span data-ttu-id="4a07c-163">Om du vill tooconnect toohello MongoDB-instansen från en annan virtuell dator eller nätverk, se till att lämpliga hello [Nätverkssäkerhetsgruppen regler skapas](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="4a07c-163">If you want tooconnect toohello MongoDB instance from another VM or network, ensure hello appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="4a07c-164">De här exemplen distribuera hello core MongoDB-miljö för utveckling.</span><span class="sxs-lookup"><span data-stu-id="4a07c-164">These examples deploy hello core MongoDB environment for development purposes.</span></span> <span data-ttu-id="4a07c-165">Tillämpa hello krävs säkerhetskonfigurationsalternativ för din miljö.</span><span class="sxs-lookup"><span data-stu-id="4a07c-165">Apply hello required security configuration options for your environment.</span></span> <span data-ttu-id="4a07c-166">Mer information finns i hello [MongoDB säkerhet docs](https://docs.mongodb.com/manual/security/).</span><span class="sxs-lookup"><span data-stu-id="4a07c-166">For more information, see hello [MongoDB security docs](https://docs.mongodb.com/manual/security/).</span></span>

<span data-ttu-id="4a07c-167">Mer information om hur du skapar med hjälp av mallar finns hello [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4a07c-167">For more information about creating using templates, see hello [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="4a07c-168">hello Azure Resource Manager-Mallar Använd hello toodownload för tillägget för anpassat skript och köra skript på din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="4a07c-168">hello Azure Resource Manager templates use hello Custom Script Extension toodownload and execute scripts on your VMs.</span></span> <span data-ttu-id="4a07c-169">Mer information finns i [Using hello tillägget för Azure anpassat skript med Linux-datorer](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="4a07c-169">For more information, see [Using hello Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

