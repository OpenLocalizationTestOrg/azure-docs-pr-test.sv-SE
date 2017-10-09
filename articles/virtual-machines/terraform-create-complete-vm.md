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
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a>Skapa en grundläggande infrastruktur i Azure med hjälp av Terraform
Den här artikeln beskriver hello stegen tootake tooprovision en virtuell dator, tillsammans med underliggande infrastruktur till Azure. Du får lära dig hur toowrite Terraform skript och hur toovisualize hello ändras innan du ser dem i din molninfrastruktur. Du får också lära dig hur toocreate infrastrukturen i Azure med hjälp av Terraform.

tooget igång, skapa en fil med namnet \terraform_azure101.tf i textredigeraren väljer (Visual Studio Code/Sublime/Vim/etc.). hello exakt namnet på hello-filen inte är viktig eftersom Terraform accepterar hello mappnamn som en parameter: körs alla skript i hello mapp. Klistra in hello följande kod i hello ny fil:

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
I hello `provider` avsnitt i hello skript anger du Terraform toouse en Azure-providern tooprovision resurser i hello skript. tooget värden för PRENUMERATIONSID, appId, lösenord och tenant_id, se hello [installera och konfigurera Terraform](terraform-install-configure.md) guide. Om du har skapat miljövariabler för hello värden i det här blocket, behöver du inte tooinclude den. 

Hej `azurerm_resource_group` resurs instruerar Terraform toocreate en ny resursgrupp. Du kan se flera resurstyper som är tillgängliga i Terraform senare i den här artikeln.

## <a name="execute-hello-script"></a>Kör hello-skript
När du har sparat hello skript avsluta toohello konsolen/kommandoraden och skriver hello följande:
```
terraform init
```
tooinitialize Terraform provider för Azure. Skriv hello följande:
```
terraform plan terraformscripts
```
Vi anta att `terraformscripts` är hello mapp där hello skript har sparats. Vi använde hello `plan` Terraform kommando som ser ut på hello resurser som definierats i hello skript. Den jämför toohello statusinformation sparas av Terraform och sedan utdata hello planerad körning _utan_ faktiskt att skapa resurser i Azure. 

När du kör hello föregående kommando, bör du se något liknande hello följande skärm:

![Terraform plan](linux/media/terraform/tf_plan2.png)

Om allt ser rätt, att etablera den här ny resursgrupp i Azure genom att köra följande hello: 
```
terraform apply terraformscripts
```
I hello Azure-portalen, bör du se hello ny tom resursgrupp kallas `terraformtest`. I följande avsnitt hello, du lägger till en virtuell dator och alla hello stödjande infrastruktur för den virtuella dator toohello resursgruppen.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>Etablera en virtuell Ubuntu-dator med Terraform
Utöka hello Terraform skript som vi har skapat med hello information som är nödvändiga tooprovision vi en virtuell dator som kör Ubuntu. hello-resurser som du etablerar i följande avsnitt hello är:

* Ett nätverk med ett enda undernät
* Ett nätverkskort 
* Ett lagringskonto med en lagringsbehållare
* En offentlig IP-adress
* En virtuell dator som använder alla hello tidigare resurser 

Omfattande dokumentation för varje hello Azure Terraform resurser finns hello [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).

hello fullständig version av hello [skript](#complete-terraform-script) ges också i informationssyfte.

### <a name="extend-hello-terraform-script"></a>Utöka hello Terraform skript
Utöka hello-skript som har skapats med hello följande resurser: 
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
hello föregående skript skapar ett virtuellt nätverk och ett undernät i det virtuella nätverket. Observera hello referens toohello resursgruppen du redan har skapat via ”${azurerm_resource_group.helloterraform.name}” i både hello virtuella nätverk och hello undernätsdefinition.

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
hello föregående skript kodavsnitt skapa en offentlig IP-adress och ett nätverksgränssnitt som använder hello offentliga IP-Adressen skapas. Observera hello referenser toosubnet_id och public_ip_address_id. Terraform har inbyggd intelligens toounderstand som hello nätverksgränssnittet har ett beroende på hello resurser som behöver toobe som skapats före hello skapandet av hello nätverksgränssnitt.

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
Här kan du skapat ett lagringskonto och en lagringsbehållare i detta lagringskonto. Det här lagringskontot är där du lagrar virtuella hårddiskar (VHD) för hello virtuell dator om toobe skapas.

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
Slutligen skapar hello utdraget en virtuell dator som använder alla hello-resurser som redan etablerats. De är ett lagringskonto och en behållare för en virtuell Hårddisk, ett nätverksgränssnitt med offentliga IP- och undernät har angetts, och hello resursgruppen du skapat redan. Observera hello vm_size egenskap, där hello skript anger en Azure A0 SKU.

### <a name="execute-hello-script"></a>Kör hello-skript
Avsluta toohello konsolen/kommandoraden med hello fullständig skript sparas, och Skriv hello följande:
```
terraform apply terraformscripts
```
Efter en stund hello resurser, inklusive en virtuell dator visas i hello `terraformtest` resursgrupp i hello Azure-portalen.

## <a name="complete-terraform-script"></a>Slutföra Terraform skript

För din bekvämlighet visas nedan hello fullständiga Terraform skriptet som tillhandahåller alla hello infrastruktur beskrivs i den här artikeln.

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

## <a name="next-steps"></a>Nästa steg
Du har skapat grundläggande infrastruktur i Azure med hjälp av Terraform. Mer komplicerade scenarier inklusive exempel Använd belastningsutjämning och virtuella skala uppsättningar, finns ett stort antal [Terraform exempel Azure](https://github.com/hashicorp/terraform/tree/master/examples). En aktuell lista över Azure providers som stöds finns i hello [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).
