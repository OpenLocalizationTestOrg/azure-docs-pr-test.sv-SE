---
title: "Installera MongoDB på en Linux-VM som använder Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du installerar och konfigurerar MongoDB på en Linux-dator i Azure med hjälp av Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: c97ade0a3d95824f723aad55776de861fe49441f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm-using-the-azure-cli-10"></a><span data-ttu-id="68a3e-103">Installera och konfigurera MongoDB på en Linux VM som använder Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="68a3e-103">How to install and configure MongoDB on a Linux VM using the Azure CLI 1.0</span></span>
<span data-ttu-id="68a3e-104">[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="68a3e-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="68a3e-105">Den här artikeln visar hur du installerar och konfigurerar MongoDB på en Linux-VM i Azure med hjälp av Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="68a3e-105">This article shows you how to install and configure MongoDB on a Linux VM in Azure using the Resource Manager deployment model.</span></span> <span data-ttu-id="68a3e-106">Exempel visas hur detaljer till:</span><span class="sxs-lookup"><span data-stu-id="68a3e-106">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="68a3e-107">Installera och konfigurera en grundläggande MongoDB-instansen manuellt</span><span class="sxs-lookup"><span data-stu-id="68a3e-107">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="68a3e-108">Skapa en grundläggande MongoDB-instans med en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="68a3e-108">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="68a3e-109">Skapa en komplex MongoDB delat kluster med replik anger med hjälp av en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="68a3e-109">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="68a3e-110">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="68a3e-110">CLI versions to complete the task</span></span>
<span data-ttu-id="68a3e-111">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="68a3e-111">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="68a3e-112">Azure CLI 1.0 – vårt CLI för den klassiska distributionsmodellen och Resource Manager-distributionsmodellen (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="68a3e-112">Azure CLI 1.0 – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="68a3e-113">[Azure CLI 2.0](create-cli-complete-nodejs.md) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="68a3e-113">[Azure CLI 2.0](create-cli-complete-nodejs.md) - our next generation CLI for the resource management deployment model</span></span>


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="68a3e-114">Installera och konfigurera MongoDB på en virtuell dator manuellt</span><span class="sxs-lookup"><span data-stu-id="68a3e-114">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="68a3e-115">MongoDB [ange installationsinstruktioner](https://docs.mongodb.com/manual/administration/install-on-linux/) för Linux-distributioner inklusive Red Hat / CentOS, SUSE, Ubuntu och Debian.</span><span class="sxs-lookup"><span data-stu-id="68a3e-115">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="68a3e-116">I följande exempel skapas en *CentOS* VM som använder en SSH-nyckel som lagras på *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="68a3e-116">The following example creates a *CentOS* VM using an SSH key stored at *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="68a3e-117">Besvara anvisningarna för lagringskontonamn, DNS-namn och autentiseringsuppgifter som administratör:</span><span class="sxs-lookup"><span data-stu-id="68a3e-117">Answer the prompts for storage account name, DNS name, and admin credentials:</span></span>

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

<span data-ttu-id="68a3e-118">Logga in på den virtuella datorn med hjälp av den offentliga IP-adressen som visas i slutet av det föregående steget för virtuell dator skapas:</span><span class="sxs-lookup"><span data-stu-id="68a3e-118">Log on to the VM using the public IP address displayed at the end of the preceding VM creation step:</span></span>

```bash
ssh azureuser@40.78.23.145
```

<span data-ttu-id="68a3e-119">För att lägga till installationskällor för MongoDB, skapa en **yum** fil i databasen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="68a3e-119">To add the installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="68a3e-120">Öppna filen MongoDB lagringsplatsen för redigering.</span><span class="sxs-lookup"><span data-stu-id="68a3e-120">Open the MongoDB repo file for editing.</span></span> <span data-ttu-id="68a3e-121">Lägg till följande rader:</span><span class="sxs-lookup"><span data-stu-id="68a3e-121">Add the following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="68a3e-122">Installera MongoDB med **yum** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="68a3e-122">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="68a3e-123">Som standard aktiveras SELinux på CentOS bilder som hindrar dig från att komma åt MongoDB.</span><span class="sxs-lookup"><span data-stu-id="68a3e-123">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="68a3e-124">Installera hanteringsverktyg och konfigurera SELinux för att tillåta MongoDB att använda dess TCP-standardporten 27017 på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="68a3e-124">Install policy management tools and configure SELinux to allow MongoDB to operate on its default TCP port 27017 as follows.</span></span> 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="68a3e-125">Starta tjänsten MongoDB på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="68a3e-125">Start the MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="68a3e-126">Kontrollera installationen av MongoDB genom att ansluta med hjälp av lokalt `mongo` klient:</span><span class="sxs-lookup"><span data-stu-id="68a3e-126">Verify the MongoDB installation by connecting using the local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="68a3e-127">Nu testa MongoDB-instansen genom att lägga till vissa data och sedan söka:</span><span class="sxs-lookup"><span data-stu-id="68a3e-127">Now test the MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="68a3e-128">Om du vill konfigurera MongoDB för att starta automatiskt under en omstart av systemet:</span><span class="sxs-lookup"><span data-stu-id="68a3e-128">If desired, configure MongoDB to start automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="68a3e-129">Skapa en grundläggande MongoDB-instans på CentOS med en mall</span><span class="sxs-lookup"><span data-stu-id="68a3e-129">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="68a3e-130">Du kan skapa en grundläggande MongoDB-instans på en enda CentOS VM som använder följande Azure quickstart-mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="68a3e-130">You can create a basic MongoDB instance on a single CentOS VM using the following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="68a3e-131">Den här mallen använder tillägget för anpassat skript för Linux för att lägga till en `yum` databasen till den nyligen skapade CentOS VM och sedan installera MongoDB.</span><span class="sxs-lookup"><span data-stu-id="68a3e-131">This template uses the Custom Script extension for Linux to add a `yum` repository to your newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="68a3e-132">[Grundläggande MongoDB-instansen på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="68a3e-132">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="68a3e-133">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `eastus` region.</span><span class="sxs-lookup"><span data-stu-id="68a3e-133">The following example creates a resource group with the name `myResourceGroup` in the `eastus` region.</span></span> <span data-ttu-id="68a3e-134">Ange egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="68a3e-134">Enter your own values as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> <span data-ttu-id="68a3e-135">Azure CLI tillbaka till en fråga inom några sekunder för att skapa distributionen men installationen och konfigurationen tar några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="68a3e-135">The Azure CLI returns you to a prompt within a few seconds of creating the deployment, but the installation and configuration takes a few minutes to complete.</span></span> <span data-ttu-id="68a3e-136">Kontrollera statusen för distributionen med `azure group deployment show myResourceGroup`, ange namnet på resursgruppen därefter.</span><span class="sxs-lookup"><span data-stu-id="68a3e-136">Check the status of the deployment with `azure group deployment show myResourceGroup`, entering the name of your resource group accordingly.</span></span> <span data-ttu-id="68a3e-137">Vänta tills den **ProvisioningState** visar *lyckades* innan du försöker SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68a3e-137">Wait until the **ProvisioningState** shows *Succeeded* before trying to SSH to the VM.</span></span>

<span data-ttu-id="68a3e-138">När installationen är klar, SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68a3e-138">Once the deployment is complete, SSH to the VM.</span></span> <span data-ttu-id="68a3e-139">Hämta IP-adressen för din virtuella datorn med hjälp av den `azure vm show` kommandot som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="68a3e-139">Obtain the IP address of your VM using the `azure vm show` command as in the following example:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

<span data-ttu-id="68a3e-140">I slutet av utdata visas den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="68a3e-140">Near the end of the output, the public IP address is displayed.</span></span> <span data-ttu-id="68a3e-141">SSH till den virtuella datorn med IP-adressen för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="68a3e-141">SSH to your VM with the IP address of your VM:</span></span>

```bash
ssh azureuser@138.91.149.74
```

<span data-ttu-id="68a3e-142">Kontrollera installationen av MongoDB genom att ansluta med hjälp av lokalt `mongo` klienten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="68a3e-142">Verify the MongoDB installation by connecting using the local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="68a3e-143">Nu testa instansen genom att lägga till vissa data och söka på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="68a3e-143">Now test the instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="68a3e-144">Skapa ett komplext delat MongoDB-kluster på CentOS med en mall</span><span class="sxs-lookup"><span data-stu-id="68a3e-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="68a3e-145">Du kan skapa komplexa delat MongoDB-kluster med hjälp av följande Azure quickstart-mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="68a3e-145">You can create a complex MongoDB sharded cluster using the following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="68a3e-146">Den här mallen följer den [MongoDB delat kluster metodtips](https://docs.mongodb.com/manual/core/sharded-cluster-components/) att ge redundans och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="68a3e-146">This template follows the [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) to provide redundancy and high availability.</span></span> <span data-ttu-id="68a3e-147">Mallen skapar två delar med tre noder i varje replik.</span><span class="sxs-lookup"><span data-stu-id="68a3e-147">The template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="68a3e-148">En replik för config server med tre noder skapas också, plus två **mongos** router-servrar som tillhandahåller konsekvenskontroll till program från över delar.</span><span class="sxs-lookup"><span data-stu-id="68a3e-148">One config server replica set with three nodes is also created, plus two **mongos** router servers to provide consistency to applications from across the shards.</span></span>

* <span data-ttu-id="68a3e-149">[MongoDB horisontell partitionering kluster på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="68a3e-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="68a3e-150">Distribuera det här komplexa delat MongoDB-kluster kräver mer än 20 kärnor, som vanligtvis är standardvärdet för antal kärnor per region för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="68a3e-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically the default core count per region for a subscription.</span></span> <span data-ttu-id="68a3e-151">Öppna ett Azure supportbegäran för att öka din antal kärnor.</span><span class="sxs-lookup"><span data-stu-id="68a3e-151">Open an Azure support request to increase your core count.</span></span>

<span data-ttu-id="68a3e-152">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* region.</span><span class="sxs-lookup"><span data-stu-id="68a3e-152">The following example creates a resource group with the name *myResourceGroup* in the *eastus* region.</span></span> <span data-ttu-id="68a3e-153">Ange egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="68a3e-153">Enter your own values as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> <span data-ttu-id="68a3e-154">Azure CLI tillbaka till en fråga inom några sekunder för att skapa distributionen men installationen och konfigurationen kan ta över en timme att slutföra.</span><span class="sxs-lookup"><span data-stu-id="68a3e-154">The Azure CLI returns you to a prompt within a few seconds of creating the deployment, but the installation and configuration can take over an hour to complete.</span></span> <span data-ttu-id="68a3e-155">Kontrollera statusen för distributionen med `azure group deployment show myResourceGroup`, justeras efter namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="68a3e-155">Check the status of the deployment with `azure group deployment show myResourceGroup`, adjusting the name of your resource group accordingly.</span></span> <span data-ttu-id="68a3e-156">Vänta tills den **ProvisioningState** visar *lyckades* innan du ansluter till de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="68a3e-156">Wait until the **ProvisioningState** shows *Succeeded* before connecting to the VMs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="68a3e-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68a3e-157">Next steps</span></span>
<span data-ttu-id="68a3e-158">I dessa fall måste ansluta du till MongoDB-instansen lokalt från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="68a3e-158">In these examples, you connect to the MongoDB instance locally from the VM.</span></span> <span data-ttu-id="68a3e-159">Om du vill ansluta till MongoDB-instansen från en annan virtuell dator eller nätverk, se till att rätt [Nätverkssäkerhetsgruppen regler skapas](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="68a3e-159">If you want to connect to the MongoDB instance from another VM or network, ensure the appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="68a3e-160">Mer information om hur du skapar med hjälp av mallar finns i [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68a3e-160">For more information about creating using templates, see the [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="68a3e-161">Azure Resource Manager-mallar använda tillägget för anpassat skript för att hämta och köra skript på din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="68a3e-161">The Azure Resource Manager templates use the Custom Script Extension to download and execute scripts on your VMs.</span></span> <span data-ttu-id="68a3e-162">Mer information finns i [med hjälp av tillägget för anpassat skript på Azure med Linux-datorer](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="68a3e-162">For more information, see [Using the Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

