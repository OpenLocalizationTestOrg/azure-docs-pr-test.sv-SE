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
ms.openlocfilehash: 52591009ee7cb906402b8bca2ce63794ac7afcc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="1e7b1-103">Skapa en grundläggande infrastruktur i Azure med hjälp av Terraform</span><span class="sxs-lookup"><span data-stu-id="1e7b1-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="1e7b1-104">Den här artikeln beskriver hello stegen tootake tooprovision en virtuell dator, tillsammans med underliggande infrastruktur till Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-104">This article describes hello steps you need tootake tooprovision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="1e7b1-105">Du får lära dig hur toowrite Terraform skript och hur toovisualize hello ändras innan du ser dem i din molninfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-105">You will learn how toowrite Terraform scripts and how toovisualize hello changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="1e7b1-106">Du får också lära dig hur toocreate infrastrukturen i Azure med hjälp av Terraform.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-106">You also will learn how toocreate infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="1e7b1-107">tooget igång, skapa en fil med namnet \terraform_azure101.tf i textredigeraren väljer (Visual Studio Code/Sublime/Vim/etc.).</span><span class="sxs-lookup"><span data-stu-id="1e7b1-107">tooget started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="1e7b1-108">hello exakt namnet på hello-filen inte är viktig eftersom Terraform accepterar hello mappnamn som en parameter: körs alla skript i hello mapp.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-108">hello exact name of hello file isn't important because Terraform accepts hello folder name as a parameter: all scripts in hello folder get executed.</span></span> <span data-ttu-id="1e7b1-109">Klistra in hello följande kod i hello ny fil:</span><span class="sxs-lookup"><span data-stu-id="1e7b1-109">Paste hello following code in hello new file:</span></span>

~~~~
# Configure hello Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have tooinclude this block
provider "azurerm" {
  subscription_id = "your_subscription_id_from_script_execution"
  client_id       = "your_appId_from_script_execution"
  client_secret   = "your_password_from_script_execution"
  tenant_id       = "your_tenant_id_from_script_execution"
}

# create a resource group if it doesn't exist
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}
~~~~
<span data-ttu-id="1e7b1-110">I hello `provider` avsnitt i hello skript anger du Terraform toouse en Azure-providern tooprovision resurser i hello skript.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-110">In hello `provider` section of hello script, you tell Terraform toouse an Azure provider tooprovision resources in hello script.</span></span> <span data-ttu-id="1e7b1-111">tooget värden för PRENUMERATIONSID, appId, lösenord och tenant_id, se hello [installera och konfigurera Terraform](terraform-install-configure.md) guide.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-111">tooget values for subscription_id, appId, password, and tenant_id, see hello [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="1e7b1-112">Om du har skapat miljövariabler för hello värden i det här blocket, behöver du inte tooinclude den.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-112">If you have created environment variables for hello values in this block, you don't need tooinclude it.</span></span> 

<span data-ttu-id="1e7b1-113">Hej `azurerm_resource_group` resurs instruerar Terraform toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-113">hello `azurerm_resource_group` resource instructs Terraform toocreate a new resource group.</span></span> <span data-ttu-id="1e7b1-114">Du kan se flera resurstyper som är tillgängliga i Terraform senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-hello-script"></a><span data-ttu-id="1e7b1-115">Kör hello-skript</span><span class="sxs-lookup"><span data-stu-id="1e7b1-115">Execute hello script</span></span>
<span data-ttu-id="1e7b1-116">När du har sparat hello skript avsluta toohello konsolen/kommandoraden och skriver hello följande:</span><span class="sxs-lookup"><span data-stu-id="1e7b1-116">After you save hello script, exit toohello console/command line, and type hello following:</span></span>
```
terraform init
```
 <span data-ttu-id="1e7b1-117">tooinitialize hello Terraform provider för Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-117">tooinitialize hello Terraform provider for Azure.</span></span> <span data-ttu-id="1e7b1-118">Ändra hello directory också**terraformscripts**, och problemet hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1e7b1-118">Then change hello directory too**terraformscripts**, and issue hello following command:</span></span>
```
terraform plan
```
<span data-ttu-id="1e7b1-119">Vi använde hello `plan` Terraform kommando som ser ut på hello resurser som definierats i hello skript.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-119">We used hello `plan` Terraform command, which looks at hello resources defined in hello scripts.</span></span> <span data-ttu-id="1e7b1-120">Den jämför toohello statusinformation sparas av Terraform och sedan utdata hello planerad körning _utan_ faktiskt att skapa resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-120">It compares them toohello state information saved by Terraform and then outputs hello planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="1e7b1-121">När du kör hello föregående kommando, bör du se något liknande hello följande skärm:</span><span class="sxs-lookup"><span data-stu-id="1e7b1-121">After you execute hello previous command, you should see something like hello following screen:</span></span>

![Terraform plan](./media/terraform/tf_plan2.png)

<span data-ttu-id="1e7b1-123">Om allt ser rätt, att etablera den här ny resursgrupp i Azure genom att köra följande hello:</span><span class="sxs-lookup"><span data-stu-id="1e7b1-123">If everything looks correct, provision this new resource group in Azure by executing hello following:</span></span> 
```
terraform apply
```
<span data-ttu-id="1e7b1-124">I hello Azure-portalen, bör du se hello ny tom resursgrupp kallas `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-124">In hello Azure portal, you should see hello new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="1e7b1-125">I följande avsnitt hello, du lägger till en virtuell dator och alla hello stödjande infrastruktur för den virtuella dator toohello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-125">In hello following section, you add a virtual machine and all hello supporting infrastructure for that virtual machine toohello resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="1e7b1-126">Etablera en virtuell Ubuntu-dator med Terraform</span><span class="sxs-lookup"><span data-stu-id="1e7b1-126">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="1e7b1-127">Utöka hello Terraform skript som vi har skapat med hello information som är nödvändiga tooprovision vi en virtuell dator som kör Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-127">Let's extend hello Terraform script we've created with hello details that are necessary tooprovision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="1e7b1-128">hello-resurser som du etablerar i följande avsnitt hello är:</span><span class="sxs-lookup"><span data-stu-id="1e7b1-128">hello resources that you provision in hello following sections are:</span></span>

* <span data-ttu-id="1e7b1-129">Ett nätverk med ett enda undernät</span><span class="sxs-lookup"><span data-stu-id="1e7b1-129">A network with a single subnet</span></span>
* <span data-ttu-id="1e7b1-130">Ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="1e7b1-130">A network interface card</span></span> 
* <span data-ttu-id="1e7b1-131">Ett lagringskonto för hello diagnostik för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1e7b1-131">A storage account for hello virtual machine diagnostics</span></span>
* <span data-ttu-id="1e7b1-132">En offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="1e7b1-132">A public IP</span></span>
* <span data-ttu-id="1e7b1-133">En virtuell dator som använder alla hello tidigare resurser</span><span class="sxs-lookup"><span data-stu-id="1e7b1-133">A virtual machine that utilizes all hello previous resources</span></span> 

<span data-ttu-id="1e7b1-134">Omfattande dokumentation för varje hello Azure Terraform resurser finns hello [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="1e7b1-134">For thorough documentation for each of hello Azure Terraform resources, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="1e7b1-135">hello fullständig version av hello [skript](#complete-terraform-script) ges också i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-135">hello full version of hello [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-hello-terraform-script"></a><span data-ttu-id="1e7b1-136">Utöka hello Terraform skript</span><span class="sxs-lookup"><span data-stu-id="1e7b1-136">Extend hello Terraform script</span></span>
<span data-ttu-id="1e7b1-137">Utöka hello-skript som har skapats med hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="1e7b1-137">Extend hello script that was created with hello following resources:</span></span> 
~~~~
# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}
~~~~
<span data-ttu-id="1e7b1-138">hello föregående skript skapar ett virtuellt nätverk och ett undernät i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-138">hello previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="1e7b1-139">Observera hello referens toohello resursgruppen du redan har skapat via ”${azurerm_resource_group.helloterraform.name}” i både hello virtuella nätverk och hello undernätsdefinition.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-139">Note hello reference toohello resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both hello virtual network and hello subnet definition.</span></span>

~~~~
# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
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

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
<span data-ttu-id="1e7b1-140">hello föregående skript kodavsnitt skapa en offentlig IP-adress och ett nätverksgränssnitt som använder hello offentliga IP-Adressen skapas.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-140">hello previous script snippets create a public IP and a network interface that makes use of hello public IP created.</span></span> <span data-ttu-id="1e7b1-141">Observera hello referenser toosubnet_id och public_ip_address_id.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-141">Note hello references toosubnet_id and public_ip_address_id.</span></span> <span data-ttu-id="1e7b1-142">Terraform har inbyggd intelligens toounderstand som hello nätverksgränssnittet har ett beroende på hello resurser som behöver toobe som skapats före hello skapandet av hello nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-142">Terraform has built-in intelligence toounderstand that hello network interface has a dependency on hello resources that need toobe created before hello creation of hello network interface.</span></span>

~~~~
# create a random id
resource "random_id" "randomId" {
  keepers = {
    # Generate a new id only when a new resource group is defined
    resource_group = "${azurerm_resource_group.helloterraform.name}"
  }

  byte_length = 8
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "West US"
    account_type = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
<span data-ttu-id="1e7b1-143">Här kan skapat du ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-143">Here, you created a storage account.</span></span> <span data-ttu-id="1e7b1-144">Det här lagringskontot är där hello virtuella datorn kommer att lagra information om diagnostik.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-144">This storage account is where hello virtual machine will store its diagnostics details.</span></span>

~~~~
# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_DS1_v2"

    os_profile {
        computer_name = "hostname"
        admin_username = "azureuser"
        admin_password = ""
    }

    os_profile_linux_config {
        disable_password_authentication = true

        ssh_keys {
            path = "/home/azureuser/.ssh/authorized_keys"
            key_data = "... INSERT OPENSSH PUBLIC KEY HERE ..."
        }
    }

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "16.04.0-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        managed_disk_type = "Premium_LRS"
        create_option = "FromImage"
    } 

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
<span data-ttu-id="1e7b1-145">Slutligen skapar hello utdraget en virtuell dator som använder alla hello-resurser som redan etablerats.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-145">Finally, hello previous snippet creates a virtual machine that utilizes all hello resources provisioned already.</span></span> <span data-ttu-id="1e7b1-146">De är ett lagringskonto för hello diagnostik för virtuell dator, ett nätverksgränssnitt med offentliga IP- och undernät som har angetts och hello resursgruppen du skapat redan.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-146">They are a storage account for hello virtual machine diagnostics, a network interface with public IP and subnet specified, and hello resource group you already created.</span></span> <span data-ttu-id="1e7b1-147">Observera hello vm_size egenskap, där hello skript anger ett Azure-Standard DS1v2-SKU.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-147">Note hello vm_size property, where hello script specifies an Azure Standard DS1v2 SKU.</span></span>

<span data-ttu-id="1e7b1-148">Du är obligatoriska toosupply en offentlig SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-148">You are required toosupply an SSH public key.</span></span> <span data-ttu-id="1e7b1-149">Placera hello offentliga nyckel/värde i hello avsnittet **... INFOGA OFFENTLIG OPENSSH-NYCKEL HÄR...**  ovan.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-149">Place hello public key value into hello section **... INSERT OPENSSH PUBLIC KEY HERE ...** above.</span></span> <span data-ttu-id="1e7b1-150">Du kan använda en befintlig ssh nyckelpar eller följa hello [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) eller [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) dokumentationen toogenerate hello-nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-150">You can use an existing ssh key pair or follow hello [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) or [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) documentation toogenerate hello key pair.</span></span>

### <a name="execute-hello-script"></a><span data-ttu-id="1e7b1-151">Kör hello-skript</span><span class="sxs-lookup"><span data-stu-id="1e7b1-151">Execute hello script</span></span>
<span data-ttu-id="1e7b1-152">Avsluta toohello konsolen/kommandoraden med hello fullständig skript sparas, och Skriv hello följande:</span><span class="sxs-lookup"><span data-stu-id="1e7b1-152">With hello full script saved, exit toohello console/command line and type hello following:</span></span>
```
terraform apply
```
<span data-ttu-id="1e7b1-153">Efter en stund hello resurser, inklusive en virtuell dator visas i hello `terraformtest` resursgrupp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-153">After some time, hello resources, including a virtual machine, appear in hello `terraformtest` resource group in hello Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="1e7b1-154">Slutföra Terraform skript</span><span class="sxs-lookup"><span data-stu-id="1e7b1-154">Complete Terraform script</span></span>

<span data-ttu-id="1e7b1-155">För din bekvämlighet visas nedan hello fullständiga Terraform skriptet som tillhandahåller alla hello infrastruktur beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-155">For your convenience, below is hello complete Terraform script that provisions all of hello infrastructure discussed in this article.</span></span>

```
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

# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}

# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
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

    tags {
        environment = "Terraform Demo"
    }
}

# create a random id
resource "random_id" "randomId" {
  keepers = {
    # Generate a new id only when a new resource group is defined
    resource_group = "${azurerm_resource_group.helloterraform.name}"
  }

  byte_length = 8
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "West US"
    account_type = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}

# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_DS1_v2"

    os_profile {
        computer_name = "hostname"
        admin_username = "azureuser"
        admin_password = ""
    }

    os_profile_linux_config {
        disable_password_authentication = true

        ssh_keys {
            path = "/home/azureuser/.ssh/authorized_keys"
            key_data = "... INSERT OPENSSH PUBLIC KEY HERE ..."
        }
    }

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "16.04.0-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        managed_disk_type = "Premium_LRS"
        create_option = "FromImage"
    } 

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="1e7b1-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e7b1-156">Next steps</span></span>
<span data-ttu-id="1e7b1-157">Du har skapat grundläggande infrastruktur i Azure med hjälp av Terraform.</span><span class="sxs-lookup"><span data-stu-id="1e7b1-157">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="1e7b1-158">Mer komplicerade scenarier inklusive exempel Använd belastningsutjämning och virtuella skala uppsättningar, finns ett stort antal [Terraform exempel Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="1e7b1-158">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="1e7b1-159">En aktuell lista över Azure providers som stöds finns i hello [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="1e7b1-159">For an up-to-date list of supported Azure providers, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
