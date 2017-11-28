---
title: "aaaInstall MongoDB på en Linux VM som använder hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera MongoDB på en Linux-dator i Azure med hjälp av hello Resource Manager-modellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 4ce21a2c63da7d00a4422e0a6766e2103e7f12d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm-using-hello-azure-cli-10"></a><span data-ttu-id="4c2f0-103">Hur tooinstall och konfigurera MongoDB på en Linux VM som använder hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4c2f0-103">How tooinstall and configure MongoDB on a Linux VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="4c2f0-104">[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="4c2f0-105">Den här artikeln beskrivs hur du tooinstall och konfigurera MongoDB på en Linux-VM i Azure med hjälp av hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-105">This article shows you how tooinstall and configure MongoDB on a Linux VM in Azure using hello Resource Manager deployment model.</span></span> <span data-ttu-id="4c2f0-106">Exempel visas hur detaljer till:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-106">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="4c2f0-107">Installera och konfigurera en grundläggande MongoDB-instansen manuellt</span><span class="sxs-lookup"><span data-stu-id="4c2f0-107">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="4c2f0-108">Skapa en grundläggande MongoDB-instans med en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="4c2f0-108">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="4c2f0-109">Skapa en komplex MongoDB delat kluster med replik anger med hjälp av en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="4c2f0-109">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="4c2f0-110">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="4c2f0-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="4c2f0-111">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="4c2f0-112">Azure CLI 1.0 – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="4c2f0-112">Azure CLI 1.0 – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="4c2f0-113">[Azure CLI 2.0](create-cli-complete-nodejs.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="4c2f0-113">[Azure CLI 2.0](create-cli-complete-nodejs.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="4c2f0-114">Installera och konfigurera MongoDB på en virtuell dator manuellt</span><span class="sxs-lookup"><span data-stu-id="4c2f0-114">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="4c2f0-115">MongoDB [ange installationsinstruktioner](https://docs.mongodb.com/manual/administration/install-on-linux/) för Linux-distributioner inklusive Red Hat / CentOS, SUSE, Ubuntu och Debian.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-115">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="4c2f0-116">hello följande exempel skapas en *CentOS* VM som använder en SSH-nyckel som lagras på *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-116">hello following example creates a *CentOS* VM using an SSH key stored at *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="4c2f0-117">Svaret hello uppmanar för lagringskontonamn, DNS-namn och autentiseringsuppgifter som administratör:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-117">Answer hello prompts for storage account name, DNS name, and admin credentials:</span></span>

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

<span data-ttu-id="4c2f0-118">Logga in toohello VM med hello offentliga IP-adress visas hello slutet av hello föregående steg i att skapa VM:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-118">Log on toohello VM using hello public IP address displayed at hello end of hello preceding VM creation step:</span></span>

```bash
ssh azureuser@40.78.23.145
```

<span data-ttu-id="4c2f0-119">tooadd hello installationskällor för MongoDB, skapa en **yum** fil i databasen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-119">tooadd hello installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="4c2f0-120">Öppna hello MongoDB-repo-filen för redigering.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-120">Open hello MongoDB repo file for editing.</span></span> <span data-ttu-id="4c2f0-121">Lägg till hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-121">Add hello following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="4c2f0-122">Installera MongoDB med **yum** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-122">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="4c2f0-123">Som standard aktiveras SELinux på CentOS bilder som hindrar dig från att komma åt MongoDB.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-123">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="4c2f0-124">Installera hanteringsverktyg och konfigurera SELinux tooallow MongoDB toooperate på dess TCP-standardporten 27017 på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-124">Install policy management tools and configure SELinux tooallow MongoDB toooperate on its default TCP port 27017 as follows.</span></span> 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="4c2f0-125">Starta hello MongoDB-tjänsten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-125">Start hello MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="4c2f0-126">Kontrollera hello MongoDB installationen genom att ansluta med hello lokala `mongo` klient:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-126">Verify hello MongoDB installation by connecting using hello local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="4c2f0-127">Nu testa hello MongoDB-instansen genom att lägga till vissa data och sedan söka:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-127">Now test hello MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="4c2f0-128">Om du vill konfigurera MongoDB toostart automatiskt under en omstart av systemet:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-128">If desired, configure MongoDB toostart automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="4c2f0-129">Skapa en grundläggande MongoDB-instans på CentOS med en mall</span><span class="sxs-lookup"><span data-stu-id="4c2f0-129">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="4c2f0-130">Du kan skapa en grundläggande MongoDB-instans på en enda CentOS VM som använder hello följande Azure quickstart-mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-130">You can create a basic MongoDB instance on a single CentOS VM using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="4c2f0-131">Den här mallen använder hello tillägget för anpassat skript för Linux tooadd en `yum` databasen tooyour nyskapad CentOS VM och sedan installera MongoDB.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-131">This template uses hello Custom Script extension for Linux tooadd a `yum` repository tooyour newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="4c2f0-132">[Grundläggande MongoDB-instansen på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="4c2f0-132">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="4c2f0-133">hello följande exempel skapar en resursgrupp med namnet hello `myResourceGroup` i hello `eastus` region.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-133">hello following example creates a resource group with hello name `myResourceGroup` in hello `eastus` region.</span></span> <span data-ttu-id="4c2f0-134">Ange egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-134">Enter your own values as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> <span data-ttu-id="4c2f0-135">hello Azure CLI returnerar du tooa prompt inom några sekunder för att skapa hello distributionen, men hello installation och konfiguration tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-135">hello Azure CLI returns you tooa prompt within a few seconds of creating hello deployment, but hello installation and configuration takes a few minutes toocomplete.</span></span> <span data-ttu-id="4c2f0-136">Kontrollera hello status hello-distribution med `azure group deployment show myResourceGroup`, därför att ange hello namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-136">Check hello status of hello deployment with `azure group deployment show myResourceGroup`, entering hello name of your resource group accordingly.</span></span> <span data-ttu-id="4c2f0-137">Vänta tills hello **ProvisioningState** visar *lyckades* innan du försöker tooSSH toohello VM.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-137">Wait until hello **ProvisioningState** shows *Succeeded* before trying tooSSH toohello VM.</span></span>

<span data-ttu-id="4c2f0-138">När hello distribution är Slutför SSH toohello VM.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-138">Once hello deployment is complete, SSH toohello VM.</span></span> <span data-ttu-id="4c2f0-139">Hämta hello IP-adressen för den virtuella datorn med hjälp av hello `azure vm show` kommandot som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-139">Obtain hello IP address of your VM using hello `azure vm show` command as in hello following example:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

<span data-ttu-id="4c2f0-140">Nära hello ände hello utdata visas hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-140">Near hello end of hello output, hello public IP address is displayed.</span></span> <span data-ttu-id="4c2f0-141">SSH tooyour VM med hello IP-adressen för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-141">SSH tooyour VM with hello IP address of your VM:</span></span>

```bash
ssh azureuser@138.91.149.74
```

<span data-ttu-id="4c2f0-142">Kontrollera hello MongoDB installationen genom att ansluta med hello lokala `mongo` klienten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-142">Verify hello MongoDB installation by connecting using hello local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="4c2f0-143">Nu testa hello-instans genom att lägga till vissa data och söka på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-143">Now test hello instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="4c2f0-144">Skapa ett komplext delat MongoDB-kluster på CentOS med en mall</span><span class="sxs-lookup"><span data-stu-id="4c2f0-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="4c2f0-145">Du kan skapa komplexa delat MongoDB-kluster med hjälp av hello följande Azure quickstart-mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-145">You can create a complex MongoDB sharded cluster using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="4c2f0-146">Den här mallen följer hello [MongoDB delat kluster metodtips](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundans och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-146">This template follows hello [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancy and high availability.</span></span> <span data-ttu-id="4c2f0-147">hello mallen skapar två delar med tre noder i varje replik.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-147">hello template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="4c2f0-148">En replik för config server med tre noder skapas också, plus två **mongos** router servrar tooprovide konsekvenskontroll tooapplications från över hello delar.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-148">One config server replica set with three nodes is also created, plus two **mongos** router servers tooprovide consistency tooapplications from across hello shards.</span></span>

* <span data-ttu-id="4c2f0-149">[MongoDB horisontell partitionering kluster på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="4c2f0-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="4c2f0-150">Distribuera det här komplexa delat MongoDB-kluster kräver mer än 20 kärnor, som vanligtvis är antal hello standard kärnor per region för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically hello default core count per region for a subscription.</span></span> <span data-ttu-id="4c2f0-151">Öppna ett Azure-supporten begäran tooincrease din antal kärnor.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-151">Open an Azure support request tooincrease your core count.</span></span>

<span data-ttu-id="4c2f0-152">hello följande exempel skapar en resursgrupp med namnet hello *myResourceGroup* i hello *eastus* region.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-152">hello following example creates a resource group with hello name *myResourceGroup* in hello *eastus* region.</span></span> <span data-ttu-id="4c2f0-153">Ange egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="4c2f0-153">Enter your own values as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> <span data-ttu-id="4c2f0-154">hello Azure CLI returnerar du tooa prompt inom några sekunder för att skapa hello distributionen, men hello installation och konfiguration kan ta över en timme toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-154">hello Azure CLI returns you tooa prompt within a few seconds of creating hello deployment, but hello installation and configuration can take over an hour toocomplete.</span></span> <span data-ttu-id="4c2f0-155">Kontrollera hello status hello-distribution med `azure group deployment show myResourceGroup`, justeras efter hello namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-155">Check hello status of hello deployment with `azure group deployment show myResourceGroup`, adjusting hello name of your resource group accordingly.</span></span> <span data-ttu-id="4c2f0-156">Vänta tills hello **ProvisioningState** visar *lyckades* innan du ansluter toohello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-156">Wait until hello **ProvisioningState** shows *Succeeded* before connecting toohello VMs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4c2f0-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c2f0-157">Next steps</span></span>
<span data-ttu-id="4c2f0-158">I dessa fall måste ansluta du toohello MongoDB-instansen lokalt från hello VM.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-158">In these examples, you connect toohello MongoDB instance locally from hello VM.</span></span> <span data-ttu-id="4c2f0-159">Om du vill tooconnect toohello MongoDB-instansen från en annan virtuell dator eller nätverk, se till att lämpliga hello [Nätverkssäkerhetsgruppen regler skapas](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="4c2f0-159">If you want tooconnect toohello MongoDB instance from another VM or network, ensure hello appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="4c2f0-160">Mer information om hur du skapar med hjälp av mallar finns hello [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4c2f0-160">For more information about creating using templates, see hello [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="4c2f0-161">hello Azure Resource Manager-Mallar Använd hello toodownload för tillägget för anpassat skript och köra skript på din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="4c2f0-161">hello Azure Resource Manager templates use hello Custom Script Extension toodownload and execute scripts on your VMs.</span></span> <span data-ttu-id="4c2f0-162">Mer information finns i [Using hello tillägget för Azure anpassat skript med Linux-datorer](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="4c2f0-162">For more information, see [Using hello Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

