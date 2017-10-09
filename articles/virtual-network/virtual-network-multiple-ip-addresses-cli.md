---
title: "aaaVM med flera IP-adresser med hjälp av hello Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur tooassign flera IP-adresser tooa virtuella datorn använder hello Azure CLI 2.0 | Resource Manager."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 15efd853cc7c31bacb64ed052dabedd3fe4d3079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-cli-20"></a>Tilldela flera IP-adresser toovirtual datorer med hjälp av hello Azure CLI 2.0

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Den här artikeln förklarar hur toocreate en virtuell dator (VM) via hello Azure Resource Manager distribution modellen med hello Azure CLI 2.0. Flera IP-adresser kan inte tilldelas tooresources som skapats via hello klassiska distributionsmodellen. Mer om Azure distributionsmodeller läsa hello toolearn [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Skapa en virtuell dator med flera IP-adresser

Du kan göra detta med hjälp av hello Azure CLI 2.0 (den här artikeln) eller hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md). Ändra hello värden som är lämpliga för din miljö. hello steg som följer beskrivs hur toocreate exempel VM med flera IP-adresser, enligt beskrivningen i hello scenario. Ändra variabelvärden i ”” och IP-adresstyper som krävs för din implementering. 

1. Installera hello [Azure CLI 2.0](/cli/azure/install-az-cli2) om du inte redan har installerats.
2. Skapa en SSH offentlig och privat nyckel för virtuella Linux-datorer genom att fylla hello stegen i hello [skapa en SSH offentlig och privat nyckel för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Från en kommandotolk, logga in med kommandot hello `az login` och välja hello-prenumeration som du använder.
4. Skapa hello VM genom att köra hello-skript som följer på en Linux- eller Mac-dator. hello skriptet skapar en resursgrupp, ett virtuellt nätverk (VNet), ett nätverkskort med tre IP-konfigurationer och en virtuell dator med hello två nätverkskort som är anslutna tooit. Hej NIC, offentlig IP-adress, virtuella nätverk och nätverksresurser på Virtuella måste alla finnas i hello samma plats och prenumeration. Även om hello resurser inte alla har tooexist i hello samma resursgrupp i hello följande skript som de gör.

```bash
    
#!/bin/sh
    
RgName="myResourceGroup"
Location="westcentralus"
az group create --name $RgName --location $Location
    
# Create a public IP address resource with a static IP address using hello `--allocation-method Static` option. If you
# do not specify this option, hello address is allocated dynamically. hello address is assigned toohello resource from a pool
# of IP adresses unique tooeach Azure region. Download and view hello file from
# https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists hello ranges for each region.

PipName="myPublicIP"

# This name must be unique within an Azure location.
DnsName="myDNSName"

az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--dns-name $DnsName\
--allocation-method Static

# Create a virtual network with one subnet

VnetName="myVnet"
VnetPrefix="10.0.0.0/16"
VnetSubnetName="mySubnet"
VnetSubnetPrefix="10.0.0.0/24"

az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnetName \
--subnet-prefix $VnetSubnetPrefix

# Create a network interface connected toohello subnet and associate hello public IP address tooit. Azure will create the
# first IP configuration with a static private IP address and will associate hello public IP address resource tooit.

NicName="MyNic1"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--private-ip-address 10.0.0.4
--vnet-name $VnetName \
--public-ip-address $PipName
    
# Create a second public IP address, a second IP configuration, and associate it toohello NIC. This configuration has a
# static public IP address and a static private IP address.

az network public-ip create \
--resource-group $RgName \
--location $Location \
--name myPublicIP2 \
--dns-name mypublicdns2 \
--allocation-method Static

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--name IPConfig-2 \
--private-ip-address 10.0.0.5 \
--public-ip-name myPublicIP2

# Create a third IP configuration, and associate it toohello NIC. This configuration has  static private IP address and # no public IP address.

azure network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--private-ip-address 10.0.0.6 \
--name IPConfig-3

# Note: Though this article assigns all IP configurations tooa single NIC, you can also assign multiple IP configurations
# tooany NIC in a VM. toolearn how toocreate a VM with multiple NICs, read hello Create a VM with multiple NICs 
# article: https://docs.microsoft.com/azure/virtual-network/virtual-network-deploy-multinic-arm-cli.

# Create a VM and attach hello NIC.

VmName="myVm"

# Replace hello value for hello following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes rticle. hello script fails if hello VM size
# is not supported in hello location you select. Run hello `azure vm sizes --location estcentralus` command tooget a full list
# of VMs in US West Central, for example.

VmSize="Standard_DS1"

# Replace hello value for hello OsImage variable value with a value for *urn* from hello utput returned by entering the
# `az vm image list` command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace hello following value with hello path tooyour public key file. If you're creating a Windows VM, remove hello following
# line and you'll be prompted for hello password you want tooconfigure for hello VM.

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
```

Dessutom toocreating en virtuell dator med ett nätverkskort med 3 IP-konfigurationer, hello skriptet skapar:

- En enda premium hanterade disken som standard, men du har andra alternativ för hello disktyp som du kan skapa. Läs hello [skapa en Linux VM som använder hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikeln för information.
- Ett virtuellt nätverk med ett undernät och två offentliga IP-adresser. Du kan också använda *befintliga* virtuellt nätverk, undernät, nätverkskort eller resurserna för offentlig IP-adress. toolearn hur toouse befintliga nätverksresurser i stället för att skapa ytterligare resurser, ange `az vm create -h`.

Offentliga IP-adresser har en låg kostnad. Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration. Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.

När du har skapat hello VM ange hello `az network nic show --name MyNic1 --resource-group myResourceGroup` tooview hello Nätverkskortets konfiguration. Ange hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview en lista över hello IP-konfigurationer som är associerade med toohello NIC.

Lägg till hello privata IP-adresser toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.

## <a name="add"></a>Lägg till IP-adresser tooa VM

Du kan lägga till ytterligare privata och offentliga IP-adresser tooan befintliga nätverkskort genom att slutföra hello steg som följer. hello exempel bygger på hello [scenariot](#Scenario) beskrivs i den här artikeln.

1. Öppna en kommandotolk och fullständig hello återstående stegen i det här avsnittet i en enda session. Om du inte redan har installerat och konfigurerat Azure-CLI, fullständig hello stegen i hello [installationen Azure CLI 2.0](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel och logga in tooyour Azure-konto med hello `az-login` kommando.

2. Slutför hello stegen i ett av följande avsnitt, baserat på dina krav hello:

    **Lägg till en privat IP-adress**
    
    tooadd en privat IP-adress tooa NIC, måste du skapa en IP-konfiguration med hjälp av hello-kommando som följer. hello statisk IP-adress måste vara en oanvända adress för hello undernät.

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    Skapa så många konfigurationer som du vill använda unika konfigurationsnamn och privata IP-adresser (för konfigurationer med statiska IP-adresser).

    **Lägg till en offentlig IP-adress**
    
    En offentlig IP-adress har lagts till genom att associera den tooeither en ny IP-konfiguration eller en befintlig IP-konfiguration. Slutför hello stegen i ett av hello avsnitten som följer, som du behöver.

    Offentliga IP-adresser har en låg kostnad. Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration. Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.

    - **Associera hello resurs tooa nya IP-konfiguration**
    
        När du lägger till en offentlig IP-adress i en ny IP-konfiguration måste du också lägga en privat IP-adress, eftersom alla IP-konfigurationer måste ha en privat IP-adress. Du kan lägga till en befintlig offentlig IP-adressresurs eller skapa en ny. toocreate en ny ange hello följande kommando:
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        toocreate en ny IP-konfiguration med en statisk privat IP-adress och hello associerade *myPublicIP3* offentlig IP adress resursen måste du ange hello följande kommando:

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - **Associera hello resurs tooan befintliga IP-konfigurationen** en offentlig IP-adressresurs kan bara vara associerad tooan IP-konfiguration som inte redan har en associerad. Du kan avgöra om en IP-konfiguration har en tillhörande offentliga IP-adress genom att ange hello följande kommando:

        ```bash
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        Returnerade utdata:
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        Eftersom hello **PublicIpAddressId** för *IpConfig-3* är tomt i hello utdata, ingen offentlig IP-adressresurs är för närvarande associerad tooit. Du kan lägga till en befintlig offentlig IP-adress resurs tooIpConfig-3 eller ange följande kommando toocreate en hello:

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        Ange följande kommando tooassociate hello offentliga IP-adressen resurs toohello befintliga IP-konfigurationen med namnet hello *IPConfig-3*:
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. Visa hello privata IP-adresser och hello offentliga IP-resurs-ID som tilldelats toohello NIC genom att ange hello följande kommando:

    ```bash
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    Returnerade utdata: <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. Lägg till hello privata IP-adresser som du har lagt till toohello NIC toohello VM operativsystem genom att följa instruktionerna hello i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln. Lägg inte till hello offentliga IP-adresser toohello operativsystem.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
