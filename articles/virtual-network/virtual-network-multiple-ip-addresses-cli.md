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
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-cli-20"></a><span data-ttu-id="45f9d-103">Tilldela flera IP-adresser toovirtual datorer med hjälp av hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="45f9d-103">Assign multiple IP addresses toovirtual machines using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="45f9d-104">Den här artikeln förklarar hur toocreate en virtuell dator (VM) via hello Azure Resource Manager distribution modellen med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="45f9d-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using hello Azure CLI 2.0.</span></span> <span data-ttu-id="45f9d-105">Flera IP-adresser kan inte tilldelas tooresources som skapats via hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="45f9d-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="45f9d-106">Mer om Azure distributionsmodeller läsa hello toolearn [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="45f9d-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="45f9d-107"><a name = "create"></a>Skapa en virtuell dator med flera IP-adresser</span><span class="sxs-lookup"><span data-stu-id="45f9d-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="45f9d-108">Du kan göra detta med hjälp av hello Azure CLI 2.0 (den här artikeln) eller hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="45f9d-108">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md).</span></span> <span data-ttu-id="45f9d-109">Ändra hello värden som är lämpliga för din miljö.</span><span class="sxs-lookup"><span data-stu-id="45f9d-109">Change hello values, as appropriate, for your environment.</span></span> <span data-ttu-id="45f9d-110">hello steg som följer beskrivs hur toocreate exempel VM med flera IP-adresser, enligt beskrivningen i hello scenario.</span><span class="sxs-lookup"><span data-stu-id="45f9d-110">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="45f9d-111">Ändra variabelvärden i ”” och IP-adresstyper som krävs för din implementering.</span><span class="sxs-lookup"><span data-stu-id="45f9d-111">Change variable values in "" and IP address types as required for your implementation.</span></span> 

1. <span data-ttu-id="45f9d-112">Installera hello [Azure CLI 2.0](/cli/azure/install-az-cli2) om du inte redan har installerats.</span><span class="sxs-lookup"><span data-stu-id="45f9d-112">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="45f9d-113">Skapa en SSH offentlig och privat nyckel för virtuella Linux-datorer genom att fylla hello stegen i hello [skapa en SSH offentlig och privat nyckel för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45f9d-113">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="45f9d-114">Från en kommandotolk, logga in med kommandot hello `az login` och välja hello-prenumeration som du använder.</span><span class="sxs-lookup"><span data-stu-id="45f9d-114">From a command shell, login with hello command `az login` and select hello subscription you're using.</span></span>
4. <span data-ttu-id="45f9d-115">Skapa hello VM genom att köra hello-skript som följer på en Linux- eller Mac-dator.</span><span class="sxs-lookup"><span data-stu-id="45f9d-115">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="45f9d-116">hello skriptet skapar en resursgrupp, ett virtuellt nätverk (VNet), ett nätverkskort med tre IP-konfigurationer och en virtuell dator med hello två nätverkskort som är anslutna tooit.</span><span class="sxs-lookup"><span data-stu-id="45f9d-116">hello script creates a resource group, one virtual network (VNet), one NIC with three IP configurations, and a VM with hello two NICs attached tooit.</span></span> <span data-ttu-id="45f9d-117">Hej NIC, offentlig IP-adress, virtuella nätverk och nätverksresurser på Virtuella måste alla finnas i hello samma plats och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="45f9d-117">hello NIC, public IP address, virtual network, and VM resources must all exist in hello same location and subscription.</span></span> <span data-ttu-id="45f9d-118">Även om hello resurser inte alla har tooexist i hello samma resursgrupp i hello följande skript som de gör.</span><span class="sxs-lookup"><span data-stu-id="45f9d-118">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

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

<span data-ttu-id="45f9d-119">Dessutom toocreating en virtuell dator med ett nätverkskort med 3 IP-konfigurationer, hello skriptet skapar:</span><span class="sxs-lookup"><span data-stu-id="45f9d-119">In addition toocreating a VM with a NIC with 3 IP configurations, hello script creates:</span></span>

- <span data-ttu-id="45f9d-120">En enda premium hanterade disken som standard, men du har andra alternativ för hello disktyp som du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="45f9d-120">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="45f9d-121">Läs hello [skapa en Linux VM som använder hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="45f9d-121">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="45f9d-122">Ett virtuellt nätverk med ett undernät och två offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="45f9d-122">A virtual network with one subnet and two public IP addresses.</span></span> <span data-ttu-id="45f9d-123">Du kan också använda *befintliga* virtuellt nätverk, undernät, nätverkskort eller resurserna för offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="45f9d-123">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="45f9d-124">toolearn hur toouse befintliga nätverksresurser i stället för att skapa ytterligare resurser, ange `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="45f9d-124">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

<span data-ttu-id="45f9d-125">Offentliga IP-adresser har en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="45f9d-125">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="45f9d-126">Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.</span><span class="sxs-lookup"><span data-stu-id="45f9d-126">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="45f9d-127">Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="45f9d-127">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="45f9d-128">Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.</span><span class="sxs-lookup"><span data-stu-id="45f9d-128">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

<span data-ttu-id="45f9d-129">När du har skapat hello VM ange hello `az network nic show --name MyNic1 --resource-group myResourceGroup` tooview hello Nätverkskortets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="45f9d-129">After hello VM is created, enter hello `az network nic show --name MyNic1 --resource-group myResourceGroup` command tooview hello NIC configuration.</span></span> <span data-ttu-id="45f9d-130">Ange hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview en lista över hello IP-konfigurationer som är associerade med toohello NIC.</span><span class="sxs-lookup"><span data-stu-id="45f9d-130">Enter hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview a list of hello IP configurations associated toohello NIC.</span></span>

<span data-ttu-id="45f9d-131">Lägg till hello privata IP-adresser toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="45f9d-131">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

## <span data-ttu-id="45f9d-132"><a name="add"></a>Lägg till IP-adresser tooa VM</span><span class="sxs-lookup"><span data-stu-id="45f9d-132"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="45f9d-133">Du kan lägga till ytterligare privata och offentliga IP-adresser tooan befintliga nätverkskort genom att slutföra hello steg som följer.</span><span class="sxs-lookup"><span data-stu-id="45f9d-133">You can add additional private and public IP addresses tooan existing NIC by completing hello steps that follow.</span></span> <span data-ttu-id="45f9d-134">hello exempel bygger på hello [scenariot](#Scenario) beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="45f9d-134">hello examples build upon hello [scenario](#Scenario) described in this article.</span></span>

1. <span data-ttu-id="45f9d-135">Öppna en kommandotolk och fullständig hello återstående stegen i det här avsnittet i en enda session.</span><span class="sxs-lookup"><span data-stu-id="45f9d-135">Open a command shell and complete hello remaining steps in this section within a single session.</span></span> <span data-ttu-id="45f9d-136">Om du inte redan har installerat och konfigurerat Azure-CLI, fullständig hello stegen i hello [installationen Azure CLI 2.0](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel och logga in tooyour Azure-konto med hello `az-login` kommando.</span><span class="sxs-lookup"><span data-stu-id="45f9d-136">If you don't already have Azure CLI installed and configured, complete hello steps in hello [Azure CLI 2.0 installation](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) article and login tooyour Azure account with hello `az-login` command.</span></span>

2. <span data-ttu-id="45f9d-137">Slutför hello stegen i ett av följande avsnitt, baserat på dina krav hello:</span><span class="sxs-lookup"><span data-stu-id="45f9d-137">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    <span data-ttu-id="45f9d-138">**Lägg till en privat IP-adress**</span><span class="sxs-lookup"><span data-stu-id="45f9d-138">**Add a private IP address**</span></span>
    
    <span data-ttu-id="45f9d-139">tooadd en privat IP-adress tooa NIC, måste du skapa en IP-konfiguration med hjälp av hello-kommando som följer.</span><span class="sxs-lookup"><span data-stu-id="45f9d-139">tooadd a private IP address tooa NIC, you must create an IP configuration using hello command that follows.</span></span> <span data-ttu-id="45f9d-140">hello statisk IP-adress måste vara en oanvända adress för hello undernät.</span><span class="sxs-lookup"><span data-stu-id="45f9d-140">hello static IP address must be an unused address for hello subnet.</span></span>

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    <span data-ttu-id="45f9d-141">Skapa så många konfigurationer som du vill använda unika konfigurationsnamn och privata IP-adresser (för konfigurationer med statiska IP-adresser).</span><span class="sxs-lookup"><span data-stu-id="45f9d-141">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    <span data-ttu-id="45f9d-142">**Lägg till en offentlig IP-adress**</span><span class="sxs-lookup"><span data-stu-id="45f9d-142">**Add a public IP address**</span></span>
    
    <span data-ttu-id="45f9d-143">En offentlig IP-adress har lagts till genom att associera den tooeither en ny IP-konfiguration eller en befintlig IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="45f9d-143">A public IP address is added by associating it tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="45f9d-144">Slutför hello stegen i ett av hello avsnitten som följer, som du behöver.</span><span class="sxs-lookup"><span data-stu-id="45f9d-144">Complete hello steps in one of hello sections that follow, as you require.</span></span>

    <span data-ttu-id="45f9d-145">Offentliga IP-adresser har en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="45f9d-145">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="45f9d-146">Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.</span><span class="sxs-lookup"><span data-stu-id="45f9d-146">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="45f9d-147">Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="45f9d-147">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="45f9d-148">Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.</span><span class="sxs-lookup"><span data-stu-id="45f9d-148">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

    - <span data-ttu-id="45f9d-149">**Associera hello resurs tooa nya IP-konfiguration**</span><span class="sxs-lookup"><span data-stu-id="45f9d-149">**Associate hello resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="45f9d-150">När du lägger till en offentlig IP-adress i en ny IP-konfiguration måste du också lägga en privat IP-adress, eftersom alla IP-konfigurationer måste ha en privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="45f9d-150">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="45f9d-151">Du kan lägga till en befintlig offentlig IP-adressresurs eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="45f9d-151">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="45f9d-152">toocreate en ny ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="45f9d-152">toocreate a new one, enter hello following command:</span></span>
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        <span data-ttu-id="45f9d-153">toocreate en ny IP-konfiguration med en statisk privat IP-adress och hello associerade *myPublicIP3* offentlig IP adress resursen måste du ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="45f9d-153">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIP3* public IP address resource, enter hello following command:</span></span>

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - <span data-ttu-id="45f9d-154">**Associera hello resurs tooan befintliga IP-konfigurationen** en offentlig IP-adressresurs kan bara vara associerad tooan IP-konfiguration som inte redan har en associerad.</span><span class="sxs-lookup"><span data-stu-id="45f9d-154">**Associate hello resource tooan existing IP configuration** A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="45f9d-155">Du kan avgöra om en IP-konfiguration har en tillhörande offentliga IP-adress genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="45f9d-155">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```bash
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        <span data-ttu-id="45f9d-156">Returnerade utdata:</span><span class="sxs-lookup"><span data-stu-id="45f9d-156">Returned output:</span></span>
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        <span data-ttu-id="45f9d-157">Eftersom hello **PublicIpAddressId** för *IpConfig-3* är tomt i hello utdata, ingen offentlig IP-adressresurs är för närvarande associerad tooit.</span><span class="sxs-lookup"><span data-stu-id="45f9d-157">Since hello **PublicIpAddressId** column for *IpConfig-3* is blank in hello output, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="45f9d-158">Du kan lägga till en befintlig offentlig IP-adress resurs tooIpConfig-3 eller ange följande kommando toocreate en hello:</span><span class="sxs-lookup"><span data-stu-id="45f9d-158">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        <span data-ttu-id="45f9d-159">Ange följande kommando tooassociate hello offentliga IP-adressen resurs toohello befintliga IP-konfigurationen med namnet hello *IPConfig-3*:</span><span class="sxs-lookup"><span data-stu-id="45f9d-159">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IPConfig-3*:</span></span>
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. <span data-ttu-id="45f9d-160">Visa hello privata IP-adresser och hello offentliga IP-resurs-ID som tilldelats toohello NIC genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="45f9d-160">View hello private IP addresses and hello public IP address resource Ids assigned toohello NIC by entering hello following command:</span></span>

    ```bash
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    <span data-ttu-id="45f9d-161">Returnerade utdata:</span><span class="sxs-lookup"><span data-stu-id="45f9d-161">Returned output:</span></span> <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. <span data-ttu-id="45f9d-162">Lägg till hello privata IP-adresser som du har lagt till toohello NIC toohello VM operativsystem genom att följa instruktionerna hello i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="45f9d-162">Add hello private IP addresses you added toohello NIC toohello VM operating system by following hello instructions in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="45f9d-163">Lägg inte till hello offentliga IP-adresser toohello operativsystem.</span><span class="sxs-lookup"><span data-stu-id="45f9d-163">Do not add hello public IP addresses toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
