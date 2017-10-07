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
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-cli-20"></a><span data-ttu-id="5e18a-103">Skapa en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5e18a-103">Create a VM with a static public IP address using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e18a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5e18a-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="5e18a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e18a-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="5e18a-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5e18a-106">Azure CLI 2.0</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="5e18a-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="5e18a-107">Azure CLI 1.0</span></span>](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [<span data-ttu-id="5e18a-108">Mall</span><span class="sxs-lookup"><span data-stu-id="5e18a-108">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="5e18a-109">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="5e18a-109">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

<span data-ttu-id="5e18a-110">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5e18a-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="5e18a-111">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="5e18a-111">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <span data-ttu-id="5e18a-112"><a name = "create"></a>Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="5e18a-112"><a name = "create"></a>Create hello VM</span></span>

<span data-ttu-id="5e18a-113">Du kan göra detta med hjälp av hello Azure CLI 2.0 (den här artikeln) eller hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5e18a-113">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span></span> <span data-ttu-id="5e18a-114">Hej värdena i ”” hello variabler i hello steg som följer skapa resurser med inställningar från hello scenario.</span><span class="sxs-lookup"><span data-stu-id="5e18a-114">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="5e18a-115">Ändra hello värden som är lämpliga för din miljö.</span><span class="sxs-lookup"><span data-stu-id="5e18a-115">Change hello values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="5e18a-116">Installera hello [Azure CLI 2.0](/cli/azure/install-az-cli2) om du inte redan har installerats.</span><span class="sxs-lookup"><span data-stu-id="5e18a-116">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="5e18a-117">Skapa en SSH offentlig och privat nyckel för virtuella Linux-datorer genom att fylla hello stegen i hello [skapa en SSH offentlig och privat nyckel för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5e18a-117">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="5e18a-118">Från en kommandotolk, logga in med hello kommandot `az login`.</span><span class="sxs-lookup"><span data-stu-id="5e18a-118">From a command shell, login with hello command `az login`.</span></span>
4. <span data-ttu-id="5e18a-119">Skapa hello VM genom att köra hello-skript som följer på en Linux- eller Mac-dator.</span><span class="sxs-lookup"><span data-stu-id="5e18a-119">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="5e18a-120">hello Azure offentlig IP-adress, virtuella nätverk, nätverksgränssnitt och Virtuella resurser måste alla finnas i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="5e18a-120">hello Azure public IP address, virtual network, network interface, and VM resources must all exist in hello same location.</span></span> <span data-ttu-id="5e18a-121">Även om hello resurser inte alla har tooexist i hello samma resursgrupp i hello följande skript som de gör.</span><span class="sxs-lookup"><span data-stu-id="5e18a-121">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

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

<span data-ttu-id="5e18a-122">Dessutom toocreating en virtuell dator, hello skriptet skapar:</span><span class="sxs-lookup"><span data-stu-id="5e18a-122">In addition toocreating a VM, hello script creates:</span></span>
- <span data-ttu-id="5e18a-123">En enda premium hanterade disken som standard, men du har andra alternativ för hello disktyp som du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="5e18a-123">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="5e18a-124">Läs hello [skapa en Linux VM som använder hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="5e18a-124">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="5e18a-125">Virtuella nätverk, undernät, nätverkskort och offentliga IP-adress-resurser.</span><span class="sxs-lookup"><span data-stu-id="5e18a-125">Virtual network, subnet, NIC, and public IP address resources.</span></span> <span data-ttu-id="5e18a-126">Du kan också använda *befintliga* virtuellt nätverk, undernät, nätverkskort eller resurserna för offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5e18a-126">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="5e18a-127">toolearn hur toouse befintliga nätverksresurser i stället för att skapa ytterligare resurser, ange `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="5e18a-127">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="5e18a-128"><a name = "validate"></a>Validera skapa Virtuella och den offentliga IP-adress</span><span class="sxs-lookup"><span data-stu-id="5e18a-128"><a name = "validate"></a>Validate VM creation and public IP address</span></span>

1. <span data-ttu-id="5e18a-129">Ange hello kommando `az resource list --resouce-group IaaSStory --output table` toosee en lista över hello resurser som skapats av hello skript.</span><span class="sxs-lookup"><span data-stu-id="5e18a-129">Enter hello command `az resource list --resouce-group IaaSStory --output table` toosee a list of hello resources created by hello script.</span></span> <span data-ttu-id="5e18a-130">Det bör finnas fem resurser i hello returnerade utdata: network interface, disk, offentlig IP-adress, virtuella nätverk och en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5e18a-130">There should be five resources in hello returned output: network interface, disk, public IP address, virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="5e18a-131">Ange hello kommando `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span><span class="sxs-lookup"><span data-stu-id="5e18a-131">Enter hello command `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span></span> <span data-ttu-id="5e18a-132">Anteckna hello värdet i hello returnerade utdata, **IP-adress** och hello värdet av **PublicIpAllocationMethod** är *statiska*.</span><span class="sxs-lookup"><span data-stu-id="5e18a-132">In hello returned output, note hello value of **IpAddress** and that hello value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="5e18a-133">Ta bort hello <> innan du kör följande kommando hello ersätter *användarnamn* med hello namn som används för hello **användarnamn** variabeln i hello skript och ersätter *ipAddress* med hello **ipAddress** hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="5e18a-133">Before executing hello following command, remove hello <>, replace *Username* with hello name you used for hello **Username** variable in hello script, and replace *ipAddress* with hello **ipAddress** from hello previous step.</span></span> <span data-ttu-id="5e18a-134">Hello kör följande kommando tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="5e18a-134">Run hello following command tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 

## <span data-ttu-id="5e18a-135"><a name= "clean-up"></a>Ta bort hello VM och associerade resurser</span><span class="sxs-lookup"><span data-stu-id="5e18a-135"><a name= "clean-up"></a>Remove hello VM and associated resources</span></span>

<span data-ttu-id="5e18a-136">Vi rekommenderar att du tar bort hello resurser skapas i den här övningen om du inte använder dem i produktion.</span><span class="sxs-lookup"><span data-stu-id="5e18a-136">It's recommended that you delete hello resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="5e18a-137">VM, offentlig IP-adress och diskresurser avgifter, så länge som de har etablerats.</span><span class="sxs-lookup"><span data-stu-id="5e18a-137">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="5e18a-138">tooremove hello resurser som skapades under den här övningen fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5e18a-138">tooremove hello resources created during this exercise, complete hello following steps:</span></span>

1. <span data-ttu-id="5e18a-139">tooview hello resurser i hello resursgrupp, kör hello `az resource list --resource-group IaaSStory` kommando.</span><span class="sxs-lookup"><span data-stu-id="5e18a-139">tooview hello resources in hello resource group, run hello `az resource list --resource-group IaaSStory` command.</span></span>
2. <span data-ttu-id="5e18a-140">Bekräfta att det finns inga resurser i hello resursgrupp än hello resurser som skapats av hello skriptet i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5e18a-140">Confirm there are no resources in hello resource group, other than hello resources created by hello script in this article.</span></span> 
3. <span data-ttu-id="5e18a-141">alla resurser skapas i den här övningen kör hello toodelete `az group delete -n IaaSStory` kommando.</span><span class="sxs-lookup"><span data-stu-id="5e18a-141">toodelete all resources created in this exercise, run hello `az group delete -n IaaSStory` command.</span></span> <span data-ttu-id="5e18a-142">hello-kommando bort hello resursgruppen och alla hello-resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="5e18a-142">hello command deletes hello resource group and all hello resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e18a-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e18a-143">Next steps</span></span>

<span data-ttu-id="5e18a-144">All nätverkstrafik kan flöda tooand från hello skapas den virtuella datorn i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5e18a-144">Any network traffic can flow tooand from hello VM created in this article.</span></span> <span data-ttu-id="5e18a-145">Du kan definiera inkommande och utgående regler inom en NSG som begränsar hello trafik som kan flöda tooand från hello nätverksgränssnitt, hello undernät eller båda.</span><span class="sxs-lookup"><span data-stu-id="5e18a-145">You can define inbound and outbound rules within an NSG that limit hello traffic that can flow tooand from hello network interface, hello subnet, or both.</span></span> <span data-ttu-id="5e18a-146">Mer om NSG: er, läsa hello toolearn [NSG översikt](virtual-networks-nsg.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="5e18a-146">toolearn more about NSGs, read hello [NSG overview](virtual-networks-nsg.md) article.</span></span>
