---
title: "Skapa en grundläggande infrastruktur i Azure med hjälp av Terraform | Microsoft Docs"
description: "Lär dig att skapa Azure-resurser med hjälp av Terraform"
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
ms.openlocfilehash: 9660a95b440c2e4311829979e270d9f10099f624
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="a9ec5-103">Skapa en grundläggande infrastruktur i Azure med hjälp av Terraform</span><span class="sxs-lookup"><span data-stu-id="a9ec5-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="a9ec5-104">Den här artikeln beskriver de steg som du behöver göra för att etablera en virtuell dator, tillsammans med underliggande infrastruktur till Azure.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-104">This article describes the steps you need to take to provision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="a9ec5-105">Du kommer lära dig hur du skriver skript för Terraform och visualisera ändringarna innan du gör dem i din molninfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-105">You will learn how to write Terraform scripts and how to visualize the changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="a9ec5-106">Du också lära dig hur du skapar infrastrukturen i Azure med hjälp av Terraform.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-106">You also will learn how to create infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="a9ec5-107">Kom igång genom att skapa en fil med namnet \terraform_azure101.tf i textredigeraren väljer (Visual Studio Code/Sublime/Vim/etc.).</span><span class="sxs-lookup"><span data-stu-id="a9ec5-107">To get started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="a9ec5-108">Det exakta namnet på filen är inte viktigt eftersom Terraform godkänner namnet på mappen som en parameter: körs alla skript i mappen.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-108">The exact name of the file isn't important because Terraform accepts the folder name as a parameter: all scripts in the folder get executed.</span></span> <span data-ttu-id="a9ec5-109">Klistra in följande kod i den nya filen:</span><span class="sxs-lookup"><span data-stu-id="a9ec5-109">Paste the following code in the new file:</span></span>

~~~~
# Configure the Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have to include this block
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
<span data-ttu-id="a9ec5-110">I den `provider` avsnitt av skript, anger du Terraform att använda en Azure leverantör för att etablera resurser i skriptet.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-110">In the `provider` section of the script, you tell Terraform to use an Azure provider to provision resources in the script.</span></span> <span data-ttu-id="a9ec5-111">Värden för PRENUMERATIONSID, appId, lösenord och tenant_id finns i [installera och konfigurera Terraform](terraform-install-configure.md) guide.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-111">To get values for subscription_id, appId, password, and tenant_id, see the [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="a9ec5-112">Om du har skapat miljövariabler för värdena i det här blocket, behöver du inte lägger till den.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-112">If you have created environment variables for the values in this block, you don't need to include it.</span></span> 

<span data-ttu-id="a9ec5-113">Den `azurerm_resource_group` resursen instruerar Terraform att skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-113">The `azurerm_resource_group` resource instructs Terraform to create a new resource group.</span></span> <span data-ttu-id="a9ec5-114">Du kan se flera resurstyper som är tillgängliga i Terraform senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-the-script"></a><span data-ttu-id="a9ec5-115">Kör skriptet för</span><span class="sxs-lookup"><span data-stu-id="a9ec5-115">Execute the script</span></span>
<span data-ttu-id="a9ec5-116">När du har sparat skriptet Avsluta till konsolen/kommandoraden och Skriv följande:</span><span class="sxs-lookup"><span data-stu-id="a9ec5-116">After you save the script, exit to the console/command line, and type the following:</span></span>
```
terraform init
```
<span data-ttu-id="a9ec5-117">initiera Terraform provider för Azure.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-117">to initialize Terraform provider for Azure.</span></span> <span data-ttu-id="a9ec5-118">Skriv följande:</span><span class="sxs-lookup"><span data-stu-id="a9ec5-118">Then type the following:</span></span>
```
terraform plan terraformscripts
```
<span data-ttu-id="a9ec5-119">Vi anta att `terraformscripts` är den mapp där skriptet sparades.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-119">We assume that `terraformscripts` is the folder where the script was saved.</span></span> <span data-ttu-id="a9ec5-120">Vi använde den `plan` Terraform kommando som tittar på de resurser som definierats i skripten.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-120">We used the `plan` Terraform command, which looks at the resources defined in the scripts.</span></span> <span data-ttu-id="a9ec5-121">Den jämför dem med tillståndsinformationen sparas av Terraform och matar ut planerad körningen _utan_ faktiskt att skapa resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-121">It compares them to the state information saved by Terraform and then outputs the planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="a9ec5-122">När du kör det föregående kommandot, bör du se något som liknar följande skärm:</span><span class="sxs-lookup"><span data-stu-id="a9ec5-122">After you execute the previous command, you should see something like the following screen:</span></span>

![Terraform plan](linux/media/terraform/tf_plan2.png)

<span data-ttu-id="a9ec5-124">Om allt ser rätt, att etablera den här ny resursgrupp i Azure genom att köra följande:</span><span class="sxs-lookup"><span data-stu-id="a9ec5-124">If everything looks correct, provision this new resource group in Azure by executing the following:</span></span> 
```
terraform apply terraformscripts
```
<span data-ttu-id="a9ec5-125">I Azure-portalen ska du se den nya tomma resursgruppen kallas `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-125">In the Azure portal, you should see the new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="a9ec5-126">I följande avsnitt du lägga till en virtuell dator och infrastrukturen som stöder för den virtuella datorn till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-126">In the following section, you add a virtual machine and all the supporting infrastructure for that virtual machine to the resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="a9ec5-127">Etablera en virtuell Ubuntu-dator med Terraform</span><span class="sxs-lookup"><span data-stu-id="a9ec5-127">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="a9ec5-128">Vi utöka Terraform-skriptet som vi har skapat med den information som krävs för att etablera en virtuell dator som kör Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-128">Let's extend the Terraform script we've created with the details that are necessary to provision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="a9ec5-129">De resurser som du etablerar i följande avsnitt finns:</span><span class="sxs-lookup"><span data-stu-id="a9ec5-129">The resources that you provision in the following sections are:</span></span>

* <span data-ttu-id="a9ec5-130">Ett nätverk med ett enda undernät</span><span class="sxs-lookup"><span data-stu-id="a9ec5-130">A network with a single subnet</span></span>
* <span data-ttu-id="a9ec5-131">Ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="a9ec5-131">A network interface card</span></span> 
* <span data-ttu-id="a9ec5-132">Ett lagringskonto med en lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="a9ec5-132">A storage account with a storage container</span></span>
* <span data-ttu-id="a9ec5-133">En offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="a9ec5-133">A public IP</span></span>
* <span data-ttu-id="a9ec5-134">En virtuell dator som använder alla tidigare resurser</span><span class="sxs-lookup"><span data-stu-id="a9ec5-134">A virtual machine that utilizes all the previous resources</span></span> 

<span data-ttu-id="a9ec5-135">Omfattande dokumentation för varje Terraform Azure-resurser finns i [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="a9ec5-135">For thorough documentation for each of the Azure Terraform resources, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="a9ec5-136">Den fullständiga versionen av den [skript](#complete-terraform-script) ges också i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-136">The full version of the [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-the-terraform-script"></a><span data-ttu-id="a9ec5-137">Utöka skriptet Terraform</span><span class="sxs-lookup"><span data-stu-id="a9ec5-137">Extend the Terraform script</span></span>
<span data-ttu-id="a9ec5-138">Utöka det skript som har skapats med följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a9ec5-138">Extend the script that was created with the following resources:</span></span> 
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
<span data-ttu-id="a9ec5-139">Föregående skript skapar ett virtuellt nätverk och ett undernät i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-139">The previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="a9ec5-140">Observera referensen till den resursgrupp som du redan har skapat via ”${azurerm_resource_group.helloterraform.name}” i både det virtuella nätverket och definitionen för undernätet.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-140">Note the reference to the resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both the virtual network and the subnet definition.</span></span>

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
<span data-ttu-id="a9ec5-141">Föregående skript kodavsnitt skapa en offentlig IP-adress och ett nätverksgränssnitt som använder offentliga IP-Adressen skapas.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-141">The previous script snippets create a public IP and a network interface that makes use of the public IP created.</span></span> <span data-ttu-id="a9ec5-142">Observera referenser till subnet_id och public_ip_address_id.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-142">Note the references to subnet_id and public_ip_address_id.</span></span> <span data-ttu-id="a9ec5-143">Terraform har inbyggd intelligens att förstå att nätverksgränssnittet har ett beroende på de resurser som måste skapas innan det har skapandet för nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-143">Terraform has built-in intelligence to understand that the network interface has a dependency on the resources that need to be created before the creation of the network interface.</span></span>

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
<span data-ttu-id="a9ec5-144">Här kan du skapat ett lagringskonto och en lagringsbehållare i detta lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-144">Here, you created a storage account and defined a storage container within that storage account.</span></span> <span data-ttu-id="a9ec5-145">Det här lagringskontot är där du lagrar virtuella hårddiskar (VHD) för den virtuella datorn ska skapas.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-145">This storage account is where you store virtual hard disks (VHDs) for the virtual machine about to be created.</span></span>

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
<span data-ttu-id="a9ec5-146">Slutligen skapar utdraget en virtuell dator som använder alla resurser som redan etablerats.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-146">Finally, the previous snippet creates a virtual machine that utilizes all the resources provisioned already.</span></span> <span data-ttu-id="a9ec5-147">De är ett lagringskonto och en behållare för en virtuell Hårddisk, ett nätverksgränssnitt med offentliga IP- och undernät har angetts, och resursgruppen du skapat redan.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-147">They are a storage account and container for a VHD, a network interface with public IP and subnet specified, and the resource group you already created.</span></span> <span data-ttu-id="a9ec5-148">Observera egenskapen vm_size där skriptet anger en Azure A0 SKU.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-148">Note the vm_size property, where the script specifies an Azure A0 SKU.</span></span>

### <a name="execute-the-script"></a><span data-ttu-id="a9ec5-149">Kör skriptet för</span><span class="sxs-lookup"><span data-stu-id="a9ec5-149">Execute the script</span></span>
<span data-ttu-id="a9ec5-150">Avsluta till konsolen/kommandoraden med fullständig skriptet sparas, och Skriv följande:</span><span class="sxs-lookup"><span data-stu-id="a9ec5-150">With the full script saved, exit to the console/command line and type the following:</span></span>
```
terraform apply terraformscripts
```
<span data-ttu-id="a9ec5-151">Efter en stund resurser, inklusive en virtuell dator visas i den `terraformtest` resursgrupp i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-151">After some time, the resources, including a virtual machine, appear in the `terraformtest` resource group in the Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="a9ec5-152">Slutföra Terraform skript</span><span class="sxs-lookup"><span data-stu-id="a9ec5-152">Complete Terraform script</span></span>

<span data-ttu-id="a9ec5-153">För din bekvämlighet visas nedan det fullständiga Terraform-skriptet som tillhandahåller alla infrastrukturen som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-153">For your convenience, below is the complete Terraform script that provisions all of the infrastructure discussed in this article.</span></span>

```
variable "resourcesname" {
  default = "helloterraform"
}

# Configure the Microsoft Azure Provider
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

## <a name="next-steps"></a><span data-ttu-id="a9ec5-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9ec5-154">Next steps</span></span>
<span data-ttu-id="a9ec5-155">Du har skapat grundläggande infrastruktur i Azure med hjälp av Terraform.</span><span class="sxs-lookup"><span data-stu-id="a9ec5-155">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="a9ec5-156">Mer komplicerade scenarier inklusive exempel Använd belastningsutjämning och virtuella skala uppsättningar, finns ett stort antal [Terraform exempel Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="a9ec5-156">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="a9ec5-157">En aktuell lista över Azure providers som stöds finns i [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="a9ec5-157">For an up-to-date list of supported Azure providers, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
