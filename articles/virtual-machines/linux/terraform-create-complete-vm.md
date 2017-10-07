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

# create a resource group if it doesn't exist
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
 tooinitialize hello Terraform provider för Azure. Ändra hello directory också**terraformscripts**, och problemet hello följande kommando:
```
terraform plan
```
Vi använde hello `plan` Terraform kommando som ser ut på hello resurser som definierats i hello skript. Den jämför toohello statusinformation sparas av Terraform och sedan utdata hello planerad körning _utan_ faktiskt att skapa resurser i Azure. 

När du kör hello föregående kommando, bör du se något liknande hello följande skärm:

![Terraform plan](./media/terraform/tf_plan2.png)

Om allt ser rätt, att etablera den här ny resursgrupp i Azure genom att köra följande hello: 
```
terraform apply
```
I hello Azure-portalen, bör du se hello ny tom resursgrupp kallas `terraformtest`. I följande avsnitt hello, du lägger till en virtuell dator och alla hello stödjande infrastruktur för den virtuella dator toohello resursgruppen.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>Etablera en virtuell Ubuntu-dator med Terraform
Utöka hello Terraform skript som vi har skapat med hello information som är nödvändiga tooprovision vi en virtuell dator som kör Ubuntu. hello-resurser som du etablerar i följande avsnitt hello är:

* Ett nätverk med ett enda undernät
* Ett nätverkskort 
* Ett lagringskonto för hello diagnostik för virtuell dator
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
hello föregående skript skapar ett virtuellt nätverk och ett undernät i det virtuella nätverket. Observera hello referens toohello resursgruppen du redan har skapat via ”${azurerm_resource_group.helloterraform.name}” i både hello virtuella nätverk och hello undernätsdefinition.

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
hello föregående skript kodavsnitt skapa en offentlig IP-adress och ett nätverksgränssnitt som använder hello offentliga IP-Adressen skapas. Observera hello referenser toosubnet_id och public_ip_address_id. Terraform har inbyggd intelligens toounderstand som hello nätverksgränssnittet har ett beroende på hello resurser som behöver toobe som skapats före hello skapandet av hello nätverksgränssnitt.

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
Här kan skapat du ett lagringskonto. Det här lagringskontot är där hello virtuella datorn kommer att lagra information om diagnostik.

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
Slutligen skapar hello utdraget en virtuell dator som använder alla hello-resurser som redan etablerats. De är ett lagringskonto för hello diagnostik för virtuell dator, ett nätverksgränssnitt med offentliga IP- och undernät som har angetts och hello resursgruppen du skapat redan. Observera hello vm_size egenskap, där hello skript anger ett Azure-Standard DS1v2-SKU.

Du är obligatoriska toosupply en offentlig SSH-nyckel. Placera hello offentliga nyckel/värde i hello avsnittet **... INFOGA OFFENTLIG OPENSSH-NYCKEL HÄR...**  ovan. Du kan använda en befintlig ssh nyckelpar eller följa hello [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) eller [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) dokumentationen toogenerate hello-nyckelpar.

### <a name="execute-hello-script"></a>Kör hello-skript
Avsluta toohello konsolen/kommandoraden med hello fullständig skript sparas, och Skriv hello följande:
```
terraform apply
```
Efter en stund hello resurser, inklusive en virtuell dator visas i hello `terraformtest` resursgrupp i hello Azure-portalen.

## <a name="complete-terraform-script"></a>Slutföra Terraform skript

För din bekvämlighet visas nedan hello fullständiga Terraform skriptet som tillhandahåller alla hello infrastruktur beskrivs i den här artikeln.

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

## <a name="next-steps"></a>Nästa steg
Du har skapat grundläggande infrastruktur i Azure med hjälp av Terraform. Mer komplicerade scenarier inklusive exempel Använd belastningsutjämning och virtuella skala uppsättningar, finns ett stort antal [Terraform exempel Azure](https://github.com/hashicorp/terraform/tree/master/examples). En aktuell lista över Azure providers som stöds finns i hello [Terraform dokumentationen](https://www.terraform.io/docs/providers/azurerm/index.html).
