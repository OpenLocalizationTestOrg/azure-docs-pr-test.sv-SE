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
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a>Skapa en grundläggande infrastruktur i Azure med hjälp av Terraform
Den här artikeln beskriver de steg som du behöver göra för att etablera en virtuell dator, tillsammans med underliggande infrastruktur till Azure. Du kommer lära dig hur du skriver skript för Terraform och visualisera ändringarna innan du gör dem i din molninfrastruktur. Du också lära dig hur du skapar infrastrukturen i Azure med hjälp av Terraform.

Kom igång genom att skapa en fil med namnet \terraform_azure101.tf i textredigeraren väljer (Visual Studio Code/Sublime/Vim/etc.). Det exakta namnet på filen är inte viktigt eftersom Terraform godkänner namnet på mappen som en parameter: körs alla skript i mappen. Klistra in följande kod i den nya filen:

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
I den `provider` avsnitt av skript, anger du Terraform att använda en Azure leverantör för att etablera resurser i skriptet. Värden för PRENUMERATIONSID, appId, lösenord och tenant_id finns i [installera och konfigurera Terraform](terraform-install-configure.md) guide. Om du har skapat miljövariabler för värdena i det här blocket, behöver du inte lägger till den. 

Den `azurerm_resource_group` resursen instruerar Terraform att skapa en ny resursgrupp. Du kan se flera resurstyper som är tillgängliga i Terraform senare i den här artikeln.

## <a name="execute-the-script"></a>Kör skriptet för
När du har sparat skriptet Avsluta till konsolen/kommandoraden och Skriv följande:
```
terraform init
```
initiera Terraform provider för Azure. Skriv följande:
```
terraform plan terraformscripts
```
Vi anta att `terraformscripts` är den mapp där skriptet sparades. Vi använde den `plan` Terraform kommando som tittar på de resurser som definierats i skripten. Den jämför dem med tillståndsinformationen sparas av Terraform och matar ut planerad körningen _utan_ faktiskt att skapa resurser i Azure. 

När du kör det föregående kommandot, bör du se något som liknar följande skärm:

![Terraform plan](linux/media/terraform/tf_plan2.png)

Om allt ser rätt, att etablera den här ny resursgrupp i Azure genom att köra följande: 
```
terraform apply terraformscripts
```
I Azure-portalen ska du se den nya tomma resursgruppen kallas `terraformtest`. I följande avsnitt du lägga till en virtuell dator och infrastrukturen som stöder för den virtuella datorn till resursgruppen.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>Etablera en virtuell Ubuntu-dator med Terraform
Vi utöka Terraform-skriptet som vi har skapat med den information som krävs för att etablera en virtuell dator som kör Ubuntu. De resurser som du etablerar i följande avsnitt finns:

* Ett nätverk med ett enda undernät
* Ett nätverkskort 
* Ett lagringskonto med en lagringsbehållare
* En offentlig IP-adress
* En virtuell dator som använder alla tidigare resurser 

Omfattande dokumentation för varje Terraform Azure-resurser finns i [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).

Den fullständiga versionen av den [skript](#complete-terraform-script) ges också i informationssyfte.

### <a name="extend-the-terraform-script"></a>Utöka skriptet Terraform
Utöka det skript som har skapats med följande resurser: 
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
Föregående skript skapar ett virtuellt nätverk och ett undernät i det virtuella nätverket. Observera referensen till den resursgrupp som du redan har skapat via ”${azurerm_resource_group.helloterraform.name}” i både det virtuella nätverket och definitionen för undernätet.

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
Föregående skript kodavsnitt skapa en offentlig IP-adress och ett nätverksgränssnitt som använder offentliga IP-Adressen skapas. Observera referenser till subnet_id och public_ip_address_id. Terraform har inbyggd intelligens att förstå att nätverksgränssnittet har ett beroende på de resurser som måste skapas innan det har skapandet för nätverksgränssnittet.

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
Här kan du skapat ett lagringskonto och en lagringsbehållare i detta lagringskonto. Det här lagringskontot är där du lagrar virtuella hårddiskar (VHD) för den virtuella datorn ska skapas.

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
Slutligen skapar utdraget en virtuell dator som använder alla resurser som redan etablerats. De är ett lagringskonto och en behållare för en virtuell Hårddisk, ett nätverksgränssnitt med offentliga IP- och undernät har angetts, och resursgruppen du skapat redan. Observera egenskapen vm_size där skriptet anger en Azure A0 SKU.

### <a name="execute-the-script"></a>Kör skriptet för
Avsluta till konsolen/kommandoraden med fullständig skriptet sparas, och Skriv följande:
```
terraform apply terraformscripts
```
Efter en stund resurser, inklusive en virtuell dator visas i den `terraformtest` resursgrupp i Azure-portalen.

## <a name="complete-terraform-script"></a>Slutföra Terraform skript

För din bekvämlighet visas nedan det fullständiga Terraform-skriptet som tillhandahåller alla infrastrukturen som beskrivs i den här artikeln.

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

## <a name="next-steps"></a>Nästa steg
Du har skapat grundläggande infrastruktur i Azure med hjälp av Terraform. Mer komplicerade scenarier inklusive exempel Använd belastningsutjämning och virtuella skala uppsättningar, finns ett stort antal [Terraform exempel Azure](https://github.com/hashicorp/terraform/tree/master/examples). En aktuell lista över Azure providers som stöds finns i [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).
