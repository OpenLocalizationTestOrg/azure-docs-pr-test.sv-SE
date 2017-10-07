---
title: aaaCreate en virtuell dator med en statisk offentlig IP-adress - Azure CLI 2.0 | Microsoft Docs
description: "Lär dig hur toocreate en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure-kommandoradsgränssnittet (CLI) 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 486060463486462dd8336734a7ad23c4a2cba452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-cli-20"></a>Skapa en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md)
> * [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [Mall](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassisk)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>Skapa hello VM

Du kan göra detta med hjälp av hello Azure CLI 2.0 (den här artikeln) eller hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md). Hej värdena i ”” hello variabler i hello steg som följer skapa resurser med inställningar från hello scenario. Ändra hello värden som är lämpliga för din miljö.

1. Installera hello [Azure CLI 2.0](/cli/azure/install-az-cli2) om du inte redan har installerats.
2. Skapa en SSH offentlig och privat nyckel för virtuella Linux-datorer genom att fylla hello stegen i hello [skapa en SSH offentlig och privat nyckel för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Från en kommandotolk, logga in med hello kommandot `az login`.
4. Skapa hello VM genom att köra hello-skript som följer på en Linux- eller Mac-dator. hello Azure offentlig IP-adress, virtuella nätverk, nätverksgränssnitt och Virtuella resurser måste alla finnas i hello samma plats. Även om hello resurser inte alla har tooexist i hello samma resursgrupp i hello följande skript som de gör.

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using hello --allocation-method Static option.
# If you do not specify this option, hello address is allocated dynamically. hello address is assigned toothe
# resource from a pool of IP adresses unique tooeach Azure region. hello DnsName must be unique within the
# Azure location it's created in. Download and view hello file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists hello ranges for each region.

PipName="PIPWEB1"
DnsName="iaasstoryws1"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static \
--dns-name $DnsName

# Create a virtual network with one subnet

VnetName="TestVNet"
VnetPrefix="192.168.0.0/16"
SubnetName="FrontEnd"
SubnetPrefix="192.168.1.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $SubnetName \
--subnet-prefix $SubnetPrefix

# Create a network interface connected toohello VNet with a static private IP address and associate hello public IP address
# resource toohello NIC.

NicName="NICWEB1"
PrivateIpAddress="192.168.1.101"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $SubnetName \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress \
--public-ip-address $PipName

# Create a new VM with hello NIC

VmName="WEB1"

# Replace hello value for hello VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace hello value for hello OsImage variable with a value for *urn* from hello output returned by entering
# hello `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace hello following value with hello path tooyour public key file.
SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
# If creating a Windows VM, remove hello previous line and you'll be prompted for hello password you want tooconfigure for hello VM.
```

Dessutom toocreating en virtuell dator, hello skriptet skapar:
- En enda premium hanterade disken som standard, men du har andra alternativ för hello disktyp som du kan skapa. Läs hello [skapa en Linux VM som använder hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikeln för information.
- Virtuella nätverk, undernät, nätverkskort och offentliga IP-adress-resurser. Du kan också använda *befintliga* virtuellt nätverk, undernät, nätverkskort eller resurserna för offentlig IP-adress. toolearn hur toouse befintliga nätverksresurser i stället för att skapa ytterligare resurser, ange `az vm create -h`.

## <a name = "validate"></a>Validera skapa Virtuella och den offentliga IP-adress

1. Ange hello kommando `az resource list --resouce-group IaaSStory --output table` toosee en lista över hello resurser som skapats av hello skript. Det bör finnas fem resurser i hello returnerade utdata: network interface, disk, offentlig IP-adress, virtuella nätverk och en virtuell dator.
2. Ange hello kommando `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`. Anteckna hello värdet i hello returnerade utdata, **IP-adress** och hello värdet av **PublicIpAllocationMethod** är *statiska*.
3. Ta bort hello <> innan du kör följande kommando hello ersätter *användarnamn* med hello namn som används för hello **användarnamn** variabeln i hello skript och ersätter *ipAddress* med hello **ipAddress** hello föregående steg. Hello kör följande kommando tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>Ta bort hello VM och associerade resurser

Vi rekommenderar att du tar bort hello resurser skapas i den här övningen om du inte använder dem i produktion. VM, offentlig IP-adress och diskresurser avgifter, så länge som de har etablerats. tooremove hello resurser som skapades under den här övningen fullständig hello följande steg:

1. tooview hello resurser i hello resursgrupp, kör hello `az resource list --resource-group IaaSStory` kommando.
2. Bekräfta att det finns inga resurser i hello resursgrupp än hello resurser som skapats av hello skriptet i den här artikeln. 
3. alla resurser skapas i den här övningen kör hello toodelete `az group delete -n IaaSStory` kommando. hello-kommando bort hello resursgruppen och alla hello-resurser som den innehåller.

## <a name="next-steps"></a>Nästa steg

All nätverkstrafik kan flöda tooand från hello skapas den virtuella datorn i den här artikeln. Du kan definiera inkommande och utgående regler inom en NSG som begränsar hello trafik som kan flöda tooand från hello nätverksgränssnitt, hello undernät eller båda. Mer om NSG: er, läsa hello toolearn [NSG översikt](virtual-networks-nsg.md) artikel.
