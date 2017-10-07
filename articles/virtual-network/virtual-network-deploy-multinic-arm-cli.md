---
title: "aaaCreate en virtuell dator med flera nätverkskort – Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator med flera nätverkskort med hello Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac0291a978e2c8682c69104915196cc6c4fcf8dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-20"></a>Skapa en virtuell dator med flera nätverkskort med hello Azure CLI 2.0

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).  Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](virtual-network-deploy-multinic-classic-cli.md).
>

## <a name="create"></a>Skapa hello VM

Du kan göra detta med hjälp av hello Azure CLI 2.0 (den här artikeln) eller hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md). Hej värdena i ”” hello variabler i hello steg som följer skapa resurser med inställningar från hello scenario. Ändra hello värden som är lämpliga för din miljö.

1. Installera hello [Azure CLI 2.0](/cli/azure/install-az-cli2) om du inte redan har installerats.
2. Skapa en SSH offentlig och privat nyckel för virtuella Linux-datorer genom att fylla hello stegen i hello [skapa en SSH offentlig och privat nyckel för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Från en kommandotolk, logga in med hello kommandot `az login`.
4. Skapa hello VM genom att köra hello-skript som följer på en Linux- eller Mac-dator. hello skriptet skapar en resursgrupp, ett virtuellt nätverk (VNet) med två undernät, två nätverkskort och en virtuell dator med hello två nätverkskort anslutna tooit. En av hello nätverkskort är anslutet tooone undernät och tilldelas en statisk IP-adress för offentliga och privata. hello andra nätverkskortet är anslutet toohello andra undernät och tilldelas en statisk privat IP-adress och ingen offentlig IP-adress. Hej NIC, offentlig IP-adress, virtuella nätverk och nätverksresurser på Virtuella måste alla finnas i hello samma plats och prenumeration. Även om hello resurser inte alla har tooexist i hello samma resursgrupp i hello följande skript som de gör.

```bash
#!/bin/sh

RgName="Multi-NIC-VM"
Location="westus"

# Create a resource group.
az group create \
--name $RgName \
--location $Location

# hello address is assigned toohello resource from a pool of IP adresses unique tooeach Azure region. 
# Download and view hello file from https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists
# hello ranges for each region.

PipName="PIP-WEB"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static

# Create a virtual network with one subnet

VnetName="VNet1"
VnetPrefix="10.0.0.0/16"
VnetSubnet1Name="Front-End"
VnetSubnet1Prefix="10.0.0.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnet1Name \
--subnet-prefix $VnetSubnet1Prefix

# Create a second subnet within hello VNet

VnetSubnet2Name="Back-end"
VnetSubnet2Prefix="10.0.1.0/24"
az network vnet subnet create \
--vnet-name $VnetName \
--resource-group $RgName \
--name $VnetSubnet2Name \
--address-prefix $VnetSubnet2Prefix

# Create a network interface connected tooone of hello subnets. hello NIC is assigned a single dynamic private and
# public IP address by default, but you can instead, assign static addresses, or no public IP address at all.
# You can also assign multiple private or public IP addresses tooeach NIC. toolearn more about IP addressing
# options for NICs, enter hello az network nic create -h command.

Nic1Name="NIC-FE"
PrivateIpAddress1="10.0.0.5"

az network nic create \
--name $Nic1Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress1 \
--public-ip-address $PipName

# Create a second network interface and connect it toohello other subnet. Though multiple NICs attached toohello same
# VM can be connected toodifferent subnets, hello subnets must all be within hello same VNet. Add additional NICs as necessary.

Nic2Name="NIC-BE"
PrivateIpAddress2="10.0.1.5"

az network nic create \
--name $Nic2Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet2Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress2

# Create a VM and attach hello two NICs.

VmName="WEB"

# Replace hello value for hello following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article. Not all VM sizes support
# more than one NIC, so be sure tooselect a VM size that supports hello number of NICs you want tooattach toohello VM.
# You must create hello VM with at least two NICs if you want tooadd more after VM creation. If you create a VM with
# only one NIC, you can't add additional NICs toohello VM after VM creation, regardless of how many NICs hello VM supports.
# hello VM size specified in hello following variable supports two NICs.

VmSize="Standard_DS2"

# Replace hello value for hello OsImage variable value with a value for *urn* from hello output returned by entering the
# az vm image list command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace hello following value with hello path tooyour public key file.

SshKeyValue="~/.ssh/id_rsa.pub"

# Before executing hello following command, add variable names of additional NICs you may have added toohello script that
# you want tooattach toohello VM. If creating a Windows VM, remove hello **ssh-key-value** line and you'll be prompted for
# hello password you want tooconfigure for hello VM.

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $Nic1Name $Nic2Name \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

Dessutom toocreating en virtuell dator med två nätverkskort, hello skriptet skapar:
- En enda premium hanterade disken som standard, men du har andra alternativ för hello disktyp som du kan skapa. Läs hello [skapa en Linux VM som använder hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikeln för information.
- Ett virtuellt nätverk med två undernät och en offentlig IP-adress. Du kan också använda *befintliga* virtuellt nätverk, undernät, nätverkskort eller resurserna för offentlig IP-adress. toolearn hur toouse befintliga nätverksresurser i stället för att skapa ytterligare resurser, ange `az vm create -h`.

## <a name = "validate"></a>Verifiera att skapa en virtuell dator och nätverkskort

1. Ange hello kommando `az resource list --resouce-group Multi-NIC-VM --output table` toosee en lista över hello resurser som skapats av hello skript. Det bör finnas sex resurser i hello returnerade utdata: två nätverkskort, en disk, en offentlig IP-adress, ett virtuellt nätverk och en virtuell dator.
2. Ange hello kommando `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`. Anteckna hello värdet i hello returnerade utdata, **IP-adress** och hello värdet av **PublicIpAllocationMethod** är *statiska*.
3. Ta bort hello <> innan du kör följande kommando hello ersätter *användarnamn* med hello namn som används för hello **användarnamn** variabeln i hello skript och ersätter *ipAddress* med hello **ipAddress** hello föregående steg. Hello kör följande kommando tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 
4. När du är ansluten toohello VM kör hello `sudo ifconfig` kommandot toosee *eth0* och *eth1* gränssnitt. Varje nätverkskort har tilldelats hello statiska privata IP-adresser som anges i hello skript hello Azure DHCP-servrar. hello ändras IP- och MAC-adresser som tilldelats toohello nätverkskort inte förrän hello VM tas bort. Vi rekommenderar att du inte ändrar IP-adresser inom ett operativsystem som den kan du inaktivera anslutning toohello dator. Offentliga IP-adresser visas inte i hello operativsystem, som de är network adress översättas tooand från hello privat IP-adress av hello Azure-infrastrukturen.

## <a name= "clean-up"></a>Ta bort hello VM och associerade resurser

Vi rekommenderar att du tar bort hello resurser skapas i den här övningen om du inte använder dem i produktion. VM, offentlig IP-adress och diskresurser avgifter, så länge som de har etablerats. tooremove hello resurser som skapades under den här övningen fullständig hello följande steg:
1. tooview hello resurser i hello resursgrupp, kör hello `az resource list --resource-group Multi-NIC-VM` kommando.
2. Bekräfta att det finns inga resurser i hello resursgrupp än hello resurser som skapats av hello skriptet i den här artikeln. 
3. alla resurser skapas i den här övningen kör hello toodelete `az group delete --name Multi-NIC-VM` kommando. hello-kommando bort hello resursgruppen och alla hello-resurser som den innehåller.

## <a name="next-steps"></a>Nästa steg

All nätverkstrafik kan flöda tooand från hello skapas den virtuella datorn i den här artikeln. Du kan definiera inkommande och utgående regler inom en NSG som begränsar hello trafik som kan flöda tooand från varje nätverksgränssnitt, varje undernät eller båda. Mer om NSG: er, läsa hello toolearn [NSG översikt](virtual-networks-nsg.md) artikel.
