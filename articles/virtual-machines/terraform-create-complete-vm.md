---
title: "aaaCreate grundläggande infrastruktur i Azure med hjälp av Terraform | Microsoft Docs"
description: "Lär dig hur toocreate Azure resurser med hjälp av Terraform"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: 916a838c118f28b3fbd373188e0acb2afc655081
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="633dc-103">Skapa en grundläggande infrastruktur i Azure med hjälp av Terraform</span><span class="sxs-lookup"><span data-stu-id="633dc-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="633dc-104">Den här artikeln beskriver hello stegen tootake tooprovision en virtuell dator, tillsammans med underliggande infrastruktur till Azure.</span><span class="sxs-lookup"><span data-stu-id="633dc-104">This article describes hello steps you need tootake tooprovision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="633dc-105">Du får lära dig hur toowrite Terraform skript och hur toovisualize hello ändras innan du ser dem i din molninfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="633dc-105">You will learn how toowrite Terraform scripts and how toovisualize hello changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="633dc-106">Du får också lära dig hur toocreate infrastrukturen i Azure med hjälp av Terraform.</span><span class="sxs-lookup"><span data-stu-id="633dc-106">You also will learn how toocreate infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="633dc-107">tooget igång, skapa en fil med namnet \terraform_azure101.tf i textredigeraren väljer (Visual Studio Code/Sublime/Vim/etc.).</span><span class="sxs-lookup"><span data-stu-id="633dc-107">tooget started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="633dc-108">hello exakt namnet på hello-filen inte är viktig eftersom Terraform accepterar hello mappnamn som en parameter: körs alla skript i hello mapp.</span><span class="sxs-lookup"><span data-stu-id="633dc-108">hello exact name of hello file isn't important because Terraform accepts hello folder name as a parameter: all scripts in hello folder get executed.</span></span> <span data-ttu-id="633dc-109">Klistra in hello följande kod i hello ny fil:</span><span class="sxs-lookup"><span data-stu-id="633dc-109">Paste hello following code in hello new file:</span></span>

~~~~
# Configure hello Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have tooinclude this block
provider "azurerm" {
  subscription_id = "your_subscription_id_from_script_execution"
  client_id       = "your_appId_from_script_execution"
  client_secret   = "your_password_from_script_execution"
  tenant_id       = "your_tenant_id_from_script_execution"
}

# create a resource group 
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}
~~~~
<span data-ttu-id="633dc-110">I hello `provider` avsnitt i hello skript anger du Terraform toouse en Azure-providern tooprovision resurser i hello skript.</span><span class="sxs-lookup"><span data-stu-id="633dc-110">In hello `provider` section of hello script, you tell Terraform toouse an Azure provider tooprovision resources in hello script.</span></span> <span data-ttu-id="633dc-111">tooget värden för PRENUMERATIONSID, appId, lösenord och tenant_id, se hello [installera och konfigurera Terraform](terraform-install-configure.md) guide.</span><span class="sxs-lookup"><span data-stu-id="633dc-111">tooget values for subscription_id, appId, password, and tenant_id, see hello [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="633dc-112">Om du har skapat miljövariabler för hello värden i det här blocket, behöver du inte tooinclude den.</span><span class="sxs-lookup"><span data-stu-id="633dc-112">If you have created environment variables for hello values in this block, you don't need tooinclude it.</span></span> 

<span data-ttu-id="633dc-113">Hej `azurerm_resource_group` resurs instruerar Terraform toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="633dc-113">hello `azurerm_resource_group` resource instructs Terraform toocreate a new resource group.</span></span> <span data-ttu-id="633dc-114">Du kan se flera resurstyper som är tillgängliga i Terraform senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="633dc-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-hello-script"></a><span data-ttu-id="633dc-115">Kör hello-skript</span><span class="sxs-lookup"><span data-stu-id="633dc-115">Execute hello script</span></span>
<span data-ttu-id="633dc-116">När du har sparat hello skript avsluta toohello konsolen/kommandoraden och skriver hello följande:</span><span class="sxs-lookup"><span data-stu-id="633dc-116">After you save hello script, exit toohello console/command line, and type hello following:</span></span>
```
terraform init
```
<span data-ttu-id="633dc-117">tooinitialize Terraform provider för Azure.</span><span class="sxs-lookup"><span data-stu-id="633dc-117">tooinitialize Terraform provider for Azure.</span></span> <span data-ttu-id="633dc-118">Skriv hello följande:</span><span class="sxs-lookup"><span data-stu-id="633dc-118">Then type hello following:</span></span>
```
terraform plan terraformscripts
```
<span data-ttu-id="633dc-119">Vi anta att `terraformscripts` är hello mapp där hello skript har sparats.</span><span class="sxs-lookup"><span data-stu-id="633dc-119">We assume that `terraformscripts` is hello folder where hello script was saved.</span></span> <span data-ttu-id="633dc-120">Vi använde hello `plan` Terraform kommando som ser ut på hello resurser som definierats i hello skript.</span><span class="sxs-lookup"><span data-stu-id="633dc-120">We used hello `plan` Terraform command, which looks at hello resources defined in hello scripts.</span></span> <span data-ttu-id="633dc-121">Den jämför toohello statusinformation sparas av Terraform och sedan utdata hello planerad körning _utan_ faktiskt att skapa resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="633dc-121">It compares them toohello state information saved by Terraform and then outputs hello planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="633dc-122">När du kör hello föregående kommando, bör du se något liknande hello följande skärm:</span><span class="sxs-lookup"><span data-stu-id="633dc-122">After you execute hello previous command, you should see something like hello following screen:</span></span>

![Terraform plan](linux/media/terraform/tf_plan2.png)

<span data-ttu-id="633dc-124">Om allt ser rätt, att etablera den här ny resursgrupp i Azure genom att köra följande hello:</span><span class="sxs-lookup"><span data-stu-id="633dc-124">If everything looks correct, provision this new resource group in Azure by executing hello following:</span></span> 
```
terraform apply terraformscripts
```
<span data-ttu-id="633dc-125">I hello Azure-portalen, bör du se hello ny tom resursgrupp kallas `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="633dc-125">In hello Azure portal, you should see hello new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="633dc-126">I följande avsnitt hello, du lägger till en virtuell dator och alla hello stödjande infrastruktur för den virtuella dator toohello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="633dc-126">In hello following section, you add a virtual machine and all hello supporting infrastructure for that virtual machine toohello resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="633dc-127">Etablera en virtuell Ubuntu-dator med Terraform</span><span class="sxs-lookup"><span data-stu-id="633dc-127">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="633dc-128">Utöka hello Terraform skript som vi har skapat med hello information som är nödvändiga tooprovision vi en virtuell dator som kör Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="633dc-128">Let's extend hello Terraform script we've created with hello details that are necessary tooprovision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="633dc-129">hello-resurser som du etablerar i följande avsnitt hello är:</span><span class="sxs-lookup"><span data-stu-id="633dc-129">hello resources that you provision in hello following sections are:</span></span>

* <span data-ttu-id="633dc-130">Ett nätverk med ett enda undernät</span><span class="sxs-lookup"><span data-stu-id="633dc-130">A network with a single subnet</span></span>
* <span data-ttu-id="633dc-131">Ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="633dc-131">A network interface card</span></span> 
* <span data-ttu-id="633dc-132">Ett lagringskonto med en lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="633dc-132">A storage account with a storage container</span></span>
* <span data-ttu-id="633dc-133">En offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="633dc-133">A public IP</span></span>
* <span data-ttu-id="633dc-134">En virtuell dator som använder alla hello tidigare resurser</span><span class="sxs-lookup"><span data-stu-id="633dc-134">A virtual machine that utilizes all hello previous resources</span></span> 

<span data-ttu-id="633dc-135">Omfattande dokumentation för varje hello Azure Terraform resurser finns hello [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="633dc-135">For thorough documentation for each of hello Azure Terraform resources, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="633dc-136">hello fullständig version av hello [skript](#complete-terraform-script) ges också i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="633dc-136">hello full version of hello [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-hello-terraform-script"></a><span data-ttu-id="633dc-137">Utöka hello Terraform skript</span><span class="sxs-lookup"><span data-stu-id="633dc-137">Extend hello Terraform script</span></span>
<span data-ttu-id="633dc-138">Utöka hello-skript som har skapats med hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="633dc-138">Extend hello script that was created with hello following resources:</span></span> 
~~~~
# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}
~~~~
<span data-ttu-id="633dc-139">hello föregående skript skapar ett virtuellt nätverk och ett undernät i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="633dc-139">hello previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="633dc-140">Observera hello referens toohello resursgruppen du redan har skapat via ”${azurerm_resource_group.helloterraform.name}” i både hello virtuella nätverk och hello undernätsdefinition.</span><span class="sxs-lookup"><span data-stu-id="633dc-140">Note hello reference toohello resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both hello virtual network and hello subnet definition.</span></span>

~~~~
# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "TerraformDemo"
    }
}

# create network interface
resource "azurerm_network_interface" "helloterraformnic" {
    name = "tfni"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    ip_configuration {
        name = "testconfiguration1"
        subnet_id = "${azurerm_subnet.helloterraformsubnet.id}"
        private_ip_address_allocation = "static"
        private_ip_address = "10.0.2.5"
        public_ip_address_id = "${azurerm_public_ip.helloterraformips.id}"
    }
}
~~~~
<span data-ttu-id="633dc-141">hello föregående skript kodavsnitt skapa en offentlig IP-adress och ett nätverksgränssnitt som använder hello offentliga IP-Adressen skapas.</span><span class="sxs-lookup"><span data-stu-id="633dc-141">hello previous script snippets create a public IP and a network interface that makes use of hello public IP created.</span></span> <span data-ttu-id="633dc-142">Observera hello referenser toosubnet_id och public_ip_address_id.</span><span class="sxs-lookup"><span data-stu-id="633dc-142">Note hello references toosubnet_id and public_ip_address_id.</span></span> <span data-ttu-id="633dc-143">Terraform har inbyggd intelligens toounderstand som hello nätverksgränssnittet har ett beroende på hello resurser som behöver toobe som skapats före hello skapandet av hello nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="633dc-143">Terraform has built-in intelligence toounderstand that hello network interface has a dependency on hello resources that need toobe created before hello creation of hello network interface.</span></span>

~~~~
# create a random id
resource "random_id" "randomId" {
  byte_length = 4
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "tfstorage${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "westus"
    account_type = "Standard_LRS"

    tags {
        environment = "staging"
    }
}

# create storage container
resource "azurerm_storage_container" "helloterraformstoragestoragecontainer" {
    name = "vhd"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    storage_account_name = "${azurerm_storage_account.helloterraformstorage.name}"
    container_access_type = "private"
    depends_on = ["azurerm_storage_account.helloterraformstorage"]
}
~~~~
<span data-ttu-id="633dc-144">Här kan du skapat ett lagringskonto och en lagringsbehållare i detta lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="633dc-144">Here, you created a storage account and defined a storage container within that storage account.</span></span> <span data-ttu-id="633dc-145">Det här lagringskontot är där du lagrar virtuella hårddiskar (VHD) för hello virtuell dator om toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="633dc-145">This storage account is where you store virtual hard disks (VHDs) for hello virtual machine about toobe created.</span></span>

~~~~
# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_A0"

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "14.04.2-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        vhd_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}${azurerm_storage_container.helloterraformstoragestoragecontainer.name}/myosdisk.vhd"
        caching = "ReadWrite"
        create_option = "FromImage"
    }

    os_profile {
        computer_name = "hostname"
        admin_username = "testadmin"
        admin_password = "Password1234!"
    }

    os_profile_linux_config {
        disable_password_authentication = false
    }

    tags {
        environment = "staging"
    }
}
~~~~
<span data-ttu-id="633dc-146">Slutligen skapar hello utdraget en virtuell dator som använder alla hello-resurser som redan etablerats.</span><span class="sxs-lookup"><span data-stu-id="633dc-146">Finally, hello previous snippet creates a virtual machine that utilizes all hello resources provisioned already.</span></span> <span data-ttu-id="633dc-147">De är ett lagringskonto och en behållare för en virtuell Hårddisk, ett nätverksgränssnitt med offentliga IP- och undernät har angetts, och hello resursgruppen du skapat redan.</span><span class="sxs-lookup"><span data-stu-id="633dc-147">They are a storage account and container for a VHD, a network interface with public IP and subnet specified, and hello resource group you already created.</span></span> <span data-ttu-id="633dc-148">Observera hello vm_size egenskap, där hello skript anger en Azure A0 SKU.</span><span class="sxs-lookup"><span data-stu-id="633dc-148">Note hello vm_size property, where hello script specifies an Azure A0 SKU.</span></span>

### <a name="execute-hello-script"></a><span data-ttu-id="633dc-149">Kör hello-skript</span><span class="sxs-lookup"><span data-stu-id="633dc-149">Execute hello script</span></span>
<span data-ttu-id="633dc-150">Avsluta toohello konsolen/kommandoraden med hello fullständig skript sparas, och Skriv hello följande:</span><span class="sxs-lookup"><span data-stu-id="633dc-150">With hello full script saved, exit toohello console/command line and type hello following:</span></span>
```
terraform apply terraformscripts
```
<span data-ttu-id="633dc-151">Efter en stund hello resurser, inklusive en virtuell dator visas i hello `terraformtest` resursgrupp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="633dc-151">After some time, hello resources, including a virtual machine, appear in hello `terraformtest` resource group in hello Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="633dc-152">Slutföra Terraform skript</span><span class="sxs-lookup"><span data-stu-id="633dc-152">Complete Terraform script</span></span>

<span data-ttu-id="633dc-153">För din bekvämlighet visas nedan hello fullständiga Terraform skriptet som tillhandahåller alla hello infrastruktur beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="633dc-153">For your convenience, below is hello complete Terraform script that provisions all of hello infrastructure discussed in this article.</span></span>

```
variable "resourcesname" {
  default = "helloterraform"
}

# Configure hello Microsoft Azure Provider
provider "azurerm" {
  subscription_id = "XXX"
  client_id       = "XXX"
  client_secret   = "XXX"
  tenant_id       = "XXX"
}

# create a resource group if it doesn't exist
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}

# create virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "tfvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "tfsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}


# create public IPs
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "TerraformDemo"
    }
}

# create network interface
resource "azurerm_network_interface" "helloterraformnic" {
    name = "tfni"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    ip_configuration {
        name = "testconfiguration1"
        subnet_id = "${azurerm_subnet.helloterraformsubnet.id}"
        private_ip_address_allocation = "static"
        private_ip_address = "10.0.2.5"
        public_ip_address_id = "${azurerm_public_ip.helloterraformips.id}"
    }
}

# create a random id
resource "random_id" "randomId" {
  byte_length = 4
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "tfstorage${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "westus"
    account_type = "Standard_LRS"

    tags {
        environment = "staging"
    }
}

# create storage container
resource "azurerm_storage_container" "helloterraformstoragestoragecontainer" {
    name = "vhd"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    storage_account_name = "${azurerm_storage_account.helloterraformstorage.name}"
    container_access_type = "private"
    depends_on = ["azurerm_storage_account.helloterraformstorage"]
}

# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_A0"

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "14.04.2-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        vhd_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}${azurerm_storage_container.helloterraformstoragestoragecontainer.name}/myosdisk.vhd"
        caching = "ReadWrite"
        create_option = "FromImage"
    }

    os_profile {
        computer_name = "hostname"
        admin_username = "testadmin"
        admin_password = "Password1234!"
    }

    os_profile_linux_config {
        disable_password_authentication = false
    }

    tags {
        environment = "staging"
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="633dc-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="633dc-154">Next steps</span></span>
<span data-ttu-id="633dc-155">Du har skapat grundläggande infrastruktur i Azure med hjälp av Terraform.</span><span class="sxs-lookup"><span data-stu-id="633dc-155">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="633dc-156">Mer komplicerade scenarier inklusive exempel Använd belastningsutjämning och virtuella skala uppsättningar, finns ett stort antal [Terraform exempel Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="633dc-156">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="633dc-157">En aktuell lista över Azure providers som stöds finns i hello [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="633dc-157">For an up-to-date list of supported Azure providers, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
