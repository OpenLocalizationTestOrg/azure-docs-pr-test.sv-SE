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
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-20"></a><span data-ttu-id="064a9-103">Skapa en virtuell dator med flera nätverkskort med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="064a9-103">Create a VM with multiple NICs using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="064a9-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="064a9-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="064a9-105">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](virtual-network-deploy-multinic-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="064a9-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-cli.md).</span></span>
>

## <span data-ttu-id="064a9-106"><a name="create"></a>Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="064a9-106"><a name="create"></a>Create hello VM</span></span>

<span data-ttu-id="064a9-107">Du kan göra detta med hjälp av hello Azure CLI 2.0 (den här artikeln) eller hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="064a9-107">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md).</span></span> <span data-ttu-id="064a9-108">Hej värdena i ”” hello variabler i hello steg som följer skapa resurser med inställningar från hello scenario.</span><span class="sxs-lookup"><span data-stu-id="064a9-108">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="064a9-109">Ändra hello värden som är lämpliga för din miljö.</span><span class="sxs-lookup"><span data-stu-id="064a9-109">Change hello values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="064a9-110">Installera hello [Azure CLI 2.0](/cli/azure/install-az-cli2) om du inte redan har installerats.</span><span class="sxs-lookup"><span data-stu-id="064a9-110">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="064a9-111">Skapa en SSH offentlig och privat nyckel för virtuella Linux-datorer genom att fylla hello stegen i hello [skapa en SSH offentlig och privat nyckel för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="064a9-111">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="064a9-112">Från en kommandotolk, logga in med hello kommandot `az login`.</span><span class="sxs-lookup"><span data-stu-id="064a9-112">From a command shell, login with hello command `az login`.</span></span>
4. <span data-ttu-id="064a9-113">Skapa hello VM genom att köra hello-skript som följer på en Linux- eller Mac-dator.</span><span class="sxs-lookup"><span data-stu-id="064a9-113">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="064a9-114">hello skriptet skapar en resursgrupp, ett virtuellt nätverk (VNet) med två undernät, två nätverkskort och en virtuell dator med hello två nätverkskort anslutna tooit.</span><span class="sxs-lookup"><span data-stu-id="064a9-114">hello script creates a resource group, one virtual network (VNet) with two subnets, two NICs, and a VM with hello two NICs attached tooit.</span></span> <span data-ttu-id="064a9-115">En av hello nätverkskort är anslutet tooone undernät och tilldelas en statisk IP-adress för offentliga och privata.</span><span class="sxs-lookup"><span data-stu-id="064a9-115">One of hello NICs is connected tooone subnet and is assigned a static public and private IP address.</span></span> <span data-ttu-id="064a9-116">hello andra nätverkskortet är anslutet toohello andra undernät och tilldelas en statisk privat IP-adress och ingen offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="064a9-116">hello other NIC is connected toohello other subnet and is assigned a static private IP address and no public IP address.</span></span> <span data-ttu-id="064a9-117">Hej NIC, offentlig IP-adress, virtuella nätverk och nätverksresurser på Virtuella måste alla finnas i hello samma plats och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="064a9-117">hello NIC, public IP address, virtual network, and VM resources must all exist in hello same location and subscription.</span></span> <span data-ttu-id="064a9-118">Även om hello resurser inte alla har tooexist i hello samma resursgrupp i hello följande skript som de gör.</span><span class="sxs-lookup"><span data-stu-id="064a9-118">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

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

<span data-ttu-id="064a9-119">Dessutom toocreating en virtuell dator med två nätverkskort, hello skriptet skapar:</span><span class="sxs-lookup"><span data-stu-id="064a9-119">In addition toocreating a VM with two NICs, hello script creates:</span></span>
- <span data-ttu-id="064a9-120">En enda premium hanterade disken som standard, men du har andra alternativ för hello disktyp som du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="064a9-120">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="064a9-121">Läs hello [skapa en Linux VM som använder hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="064a9-121">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="064a9-122">Ett virtuellt nätverk med två undernät och en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="064a9-122">A virtual network with two subnets and a single public IP address.</span></span> <span data-ttu-id="064a9-123">Du kan också använda *befintliga* virtuellt nätverk, undernät, nätverkskort eller resurserna för offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="064a9-123">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="064a9-124">toolearn hur toouse befintliga nätverksresurser i stället för att skapa ytterligare resurser, ange `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="064a9-124">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="064a9-125"><a name = "validate"></a>Verifiera att skapa en virtuell dator och nätverkskort</span><span class="sxs-lookup"><span data-stu-id="064a9-125"><a name = "validate"></a>Validate VM creation and NICs</span></span>

1. <span data-ttu-id="064a9-126">Ange hello kommando `az resource list --resouce-group Multi-NIC-VM --output table` toosee en lista över hello resurser som skapats av hello skript.</span><span class="sxs-lookup"><span data-stu-id="064a9-126">Enter hello command `az resource list --resouce-group Multi-NIC-VM --output table` toosee a list of hello resources created by hello script.</span></span> <span data-ttu-id="064a9-127">Det bör finnas sex resurser i hello returnerade utdata: två nätverkskort, en disk, en offentlig IP-adress, ett virtuellt nätverk och en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="064a9-127">There should be six resources in hello returned output: Two NICs, one disk, one public IP address, one virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="064a9-128">Ange hello kommando `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`.</span><span class="sxs-lookup"><span data-stu-id="064a9-128">Enter hello command `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`.</span></span> <span data-ttu-id="064a9-129">Anteckna hello värdet i hello returnerade utdata, **IP-adress** och hello värdet av **PublicIpAllocationMethod** är *statiska*.</span><span class="sxs-lookup"><span data-stu-id="064a9-129">In hello returned output, note hello value of **IpAddress** and that hello value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="064a9-130">Ta bort hello <> innan du kör följande kommando hello ersätter *användarnamn* med hello namn som används för hello **användarnamn** variabeln i hello skript och ersätter *ipAddress* med hello **ipAddress** hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="064a9-130">Before running hello following command, remove hello <>, replace *Username* with hello name you used for hello **Username** variable in hello script, and replace *ipAddress* with hello **ipAddress** from hello previous step.</span></span> <span data-ttu-id="064a9-131">Hello kör följande kommando tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="064a9-131">Run hello following command tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 
4. <span data-ttu-id="064a9-132">När du är ansluten toohello VM kör hello `sudo ifconfig` kommandot toosee *eth0* och *eth1* gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="064a9-132">Once connected toohello VM, run hello `sudo ifconfig` command toosee *eth0* and *eth1* interfaces.</span></span> <span data-ttu-id="064a9-133">Varje nätverkskort har tilldelats hello statiska privata IP-adresser som anges i hello skript hello Azure DHCP-servrar.</span><span class="sxs-lookup"><span data-stu-id="064a9-133">Each NIC has been assigned hello static private IP addresses specified in hello script by hello Azure DHCP servers.</span></span> <span data-ttu-id="064a9-134">hello ändras IP- och MAC-adresser som tilldelats toohello nätverkskort inte förrän hello VM tas bort.</span><span class="sxs-lookup"><span data-stu-id="064a9-134">hello IP and MAC addresses assigned toohello NICs do not change until hello VM is deleted.</span></span> <span data-ttu-id="064a9-135">Vi rekommenderar att du inte ändrar IP-adresser inom ett operativsystem som den kan du inaktivera anslutning toohello dator.</span><span class="sxs-lookup"><span data-stu-id="064a9-135">We recommend that you do not change IP addressing within an operating system, as it can disable connectivity toohello computer.</span></span> <span data-ttu-id="064a9-136">Offentliga IP-adresser visas inte i hello operativsystem, som de är network adress översättas tooand från hello privat IP-adress av hello Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="064a9-136">Public IP addresses do not appear within hello operating system, as they are network address translated tooand from hello private IP address by hello Azure infrastructure.</span></span>

## <span data-ttu-id="064a9-137"><a name= "clean-up"></a>Ta bort hello VM och associerade resurser</span><span class="sxs-lookup"><span data-stu-id="064a9-137"><a name= "clean-up"></a>Remove hello VM and associated resources</span></span>

<span data-ttu-id="064a9-138">Vi rekommenderar att du tar bort hello resurser skapas i den här övningen om du inte använder dem i produktion.</span><span class="sxs-lookup"><span data-stu-id="064a9-138">It's recommended that you delete hello resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="064a9-139">VM, offentlig IP-adress och diskresurser avgifter, så länge som de har etablerats.</span><span class="sxs-lookup"><span data-stu-id="064a9-139">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="064a9-140">tooremove hello resurser som skapades under den här övningen fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="064a9-140">tooremove hello resources created during this exercise, complete hello following steps:</span></span>
1. <span data-ttu-id="064a9-141">tooview hello resurser i hello resursgrupp, kör hello `az resource list --resource-group Multi-NIC-VM` kommando.</span><span class="sxs-lookup"><span data-stu-id="064a9-141">tooview hello resources in hello resource group, run hello `az resource list --resource-group Multi-NIC-VM` command.</span></span>
2. <span data-ttu-id="064a9-142">Bekräfta att det finns inga resurser i hello resursgrupp än hello resurser som skapats av hello skriptet i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="064a9-142">Confirm there are no resources in hello resource group, other than hello resources created by hello script in this article.</span></span> 
3. <span data-ttu-id="064a9-143">alla resurser skapas i den här övningen kör hello toodelete `az group delete --name Multi-NIC-VM` kommando.</span><span class="sxs-lookup"><span data-stu-id="064a9-143">toodelete all resources created in this exercise, run hello `az group delete --name Multi-NIC-VM` command.</span></span> <span data-ttu-id="064a9-144">hello-kommando bort hello resursgruppen och alla hello-resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="064a9-144">hello command deletes hello resource group and all hello resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="064a9-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="064a9-145">Next steps</span></span>

<span data-ttu-id="064a9-146">All nätverkstrafik kan flöda tooand från hello skapas den virtuella datorn i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="064a9-146">Any network traffic can flow tooand from hello VM created in this article.</span></span> <span data-ttu-id="064a9-147">Du kan definiera inkommande och utgående regler inom en NSG som begränsar hello trafik som kan flöda tooand från varje nätverksgränssnitt, varje undernät eller båda.</span><span class="sxs-lookup"><span data-stu-id="064a9-147">You can define inbound and outbound rules within an NSG that limit hello traffic that can flow tooand from each network interface, each subnet, or both.</span></span> <span data-ttu-id="064a9-148">Mer om NSG: er, läsa hello toolearn [NSG översikt](virtual-networks-nsg.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="064a9-148">toolearn more about NSGs, read hello [NSG overview](virtual-networks-nsg.md) article.</span></span>
