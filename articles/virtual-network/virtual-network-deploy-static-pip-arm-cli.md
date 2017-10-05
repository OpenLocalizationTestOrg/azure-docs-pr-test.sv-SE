---
title: Skapa en virtuell dator med en statisk offentlig IP-adress - Azure CLI 2.0 | Microsoft Docs
description: "Lär dig hur du skapar en virtuell dator med en statisk offentlig IP-adress med hjälp av Azure-kommandoradsgränssnittet (CLI) 2.0."
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
ms.openlocfilehash: a4c32694949880037f01bb2b6b9779d2cbb9809c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli-20"></a><span data-ttu-id="59770-103">Skapa en virtuell dator med en statisk offentlig IP-adress använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="59770-103">Create a VM with a static public IP address using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="59770-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="59770-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="59770-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="59770-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="59770-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="59770-106">Azure CLI 2.0</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="59770-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="59770-107">Azure CLI 1.0</span></span>](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [<span data-ttu-id="59770-108">Mall</span><span class="sxs-lookup"><span data-stu-id="59770-108">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="59770-109">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="59770-109">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

<span data-ttu-id="59770-110">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59770-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="59770-111">Den här artikeln täcker distributionsmodell hanteraren för filserverresurser, som Microsoft rekommenderar för de flesta nya distributioner i stället för den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="59770-111">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <span data-ttu-id="59770-112"><a name = "create"></a>Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="59770-112"><a name = "create"></a>Create the VM</span></span>

<span data-ttu-id="59770-113">Du kan göra detta med hjälp av Azure CLI 2.0 (den här artikeln) eller [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="59770-113">You can complete this task using the Azure CLI 2.0 (this article) or the [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span></span> <span data-ttu-id="59770-114">Värdena i ”” för variabler i de steg som följer skapa resurser med inställningar för scenariot.</span><span class="sxs-lookup"><span data-stu-id="59770-114">The values in "" for the variables in the steps that follow create resources with settings from the scenario.</span></span> <span data-ttu-id="59770-115">Ändra värdena för din miljö.</span><span class="sxs-lookup"><span data-stu-id="59770-115">Change the values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="59770-116">Installera den [Azure CLI 2.0](/cli/azure/install-az-cli2) om du inte redan har installerats.</span><span class="sxs-lookup"><span data-stu-id="59770-116">Install the [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="59770-117">Skapa en SSH offentlig och privat nyckel för Linux virtuella datorer genom att slutföra stegen i den [skapa en SSH offentlig och privat nyckel för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59770-117">Create an SSH public and private key pair for Linux VMs by completing the steps in the [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="59770-118">Från en kommandotolk, logga in med kommandot `az login`.</span><span class="sxs-lookup"><span data-stu-id="59770-118">From a command shell, login with the command `az login`.</span></span>
4. <span data-ttu-id="59770-119">Skapa den virtuella datorn genom att köra skriptet som följer på en Linux- eller Mac-dator.</span><span class="sxs-lookup"><span data-stu-id="59770-119">Create the VM by executing the script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="59770-120">Azure offentlig IP-adress, virtuella nätverk, nätverksgränssnitt och Virtuella resurser finnas på samma plats.</span><span class="sxs-lookup"><span data-stu-id="59770-120">The Azure public IP address, virtual network, network interface, and VM resources must all exist in the same location.</span></span> <span data-ttu-id="59770-121">Även om resurserna inte alla finns i samma resursgrupp kan i följande skript gör de.</span><span class="sxs-lookup"><span data-stu-id="59770-121">Though the resources don't all have to exist in the same resource group, in the following script they do.</span></span>

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using the --allocation-method Static option.
# If you do not specify this option, the address is allocated dynamically. The address is assigned to the
# resource from a pool of IP adresses unique to each Azure region. The DnsName must be unique within the
# Azure location it's created in. Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists the ranges for each region.

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

# Create a network interface connected to the VNet with a static private IP address and associate the public IP address
# resource to the NIC.

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

# Create a new VM with the NIC

VmName="WEB1"

# Replace the value for the VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace the value for the OsImage variable with a value for *urn* from the output returned by entering
# the `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace the following value with the path to your public key file.
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
# If creating a Windows VM, remove the previous line and you'll be prompted for the password you want to configure for the VM.
```

<span data-ttu-id="59770-122">Förutom att skapa en virtuell dator skapar skriptet:</span><span class="sxs-lookup"><span data-stu-id="59770-122">In addition to creating a VM, the script creates:</span></span>
- <span data-ttu-id="59770-123">En enda premium hanterade disken som standard, men du har andra alternativ för den typ av disk som du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="59770-123">A single premium managed disk by default, but you have other options for the disk type you can create.</span></span> <span data-ttu-id="59770-124">Läs den [skapa en Linux VM som använder Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="59770-124">Read the [Create a Linux VM using the Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="59770-125">Virtuella nätverk, undernät, nätverkskort och offentliga IP-adress-resurser.</span><span class="sxs-lookup"><span data-stu-id="59770-125">Virtual network, subnet, NIC, and public IP address resources.</span></span> <span data-ttu-id="59770-126">Du kan också använda *befintliga* virtuellt nätverk, undernät, nätverkskort eller resurserna för offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="59770-126">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="59770-127">Information om hur du använder befintliga nätverksresurser i stället för att skapa ytterligare resurser, ange `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="59770-127">To learn how to use existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="59770-128"><a name = "validate"></a>Validera skapa Virtuella och den offentliga IP-adress</span><span class="sxs-lookup"><span data-stu-id="59770-128"><a name = "validate"></a>Validate VM creation and public IP address</span></span>

1. <span data-ttu-id="59770-129">Ange kommandot `az resource list --resouce-group IaaSStory --output table` att se en lista över de resurser som skapats av skriptet.</span><span class="sxs-lookup"><span data-stu-id="59770-129">Enter the command `az resource list --resouce-group IaaSStory --output table` to see a list of the resources created by the script.</span></span> <span data-ttu-id="59770-130">Det bör finnas fem resurser i returnerade utdata: network interface, disk, offentlig IP-adress, virtuella nätverk och en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="59770-130">There should be five resources in the returned output: network interface, disk, public IP address, virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="59770-131">Ange kommandot `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span><span class="sxs-lookup"><span data-stu-id="59770-131">Enter the command `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span></span> <span data-ttu-id="59770-132">Returnerade utdata och anteckna värdet för **IpAddress** och att värdet för **PublicIpAllocationMethod** är *statiska*.</span><span class="sxs-lookup"><span data-stu-id="59770-132">In the returned output, note the value of **IpAddress** and that the value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="59770-133">Ta bort <> innan du kör följande kommando ersätter *användarnamn* med namn som du använde för den **användarnamn** variabeln i skript och ersätter *IP-adress* med den **IP-adress** från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="59770-133">Before executing the following command, remove the <>, replace *Username* with the name you used for the **Username** variable in the script, and replace *ipAddress* with the **ipAddress** from the previous step.</span></span> <span data-ttu-id="59770-134">Kör följande kommando för att ansluta till den virtuella datorn: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="59770-134">Run the following command to connect to the VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 

## <span data-ttu-id="59770-135"><a name= "clean-up"></a>Ta bort den virtuella datorn och associerade resurser</span><span class="sxs-lookup"><span data-stu-id="59770-135"><a name= "clean-up"></a>Remove the VM and associated resources</span></span>

<span data-ttu-id="59770-136">Vi rekommenderar att du tar bort de resurser som skapades i den här övningen om du inte använder dem i produktion.</span><span class="sxs-lookup"><span data-stu-id="59770-136">It's recommended that you delete the resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="59770-137">VM, offentlig IP-adress och diskresurser avgifter, så länge som de har etablerats.</span><span class="sxs-lookup"><span data-stu-id="59770-137">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="59770-138">Om du vill ta bort de resurser som skapades under den här övningen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="59770-138">To remove the resources created during this exercise, complete the following steps:</span></span>

1. <span data-ttu-id="59770-139">Om du vill visa resurserna i resursgruppen kör den `az resource list --resource-group IaaSStory` kommando.</span><span class="sxs-lookup"><span data-stu-id="59770-139">To view the resources in the resource group, run the `az resource list --resource-group IaaSStory` command.</span></span>
2. <span data-ttu-id="59770-140">Bekräfta att det finns inga resurser i resursgrupp än de resurser som skapades av skriptet i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="59770-140">Confirm there are no resources in the resource group, other than the resources created by the script in this article.</span></span> 
3. <span data-ttu-id="59770-141">Om du vill ta bort alla resurser som har skapats i den här övningen kör den `az group delete -n IaaSStory` kommando.</span><span class="sxs-lookup"><span data-stu-id="59770-141">To delete all resources created in this exercise, run the `az group delete -n IaaSStory` command.</span></span> <span data-ttu-id="59770-142">Kommandot tar bort resursgruppen och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="59770-142">The command deletes the resource group and all the resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59770-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59770-143">Next steps</span></span>

<span data-ttu-id="59770-144">All nätverkstrafik kan flöda till och från den virtuella datorn skapas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="59770-144">Any network traffic can flow to and from the VM created in this article.</span></span> <span data-ttu-id="59770-145">Du kan definiera inkommande och utgående regler inom en NSG som begränsar trafiken kan flöda till och från nätverksgränssnittet undernätet eller båda.</span><span class="sxs-lookup"><span data-stu-id="59770-145">You can define inbound and outbound rules within an NSG that limit the traffic that can flow to and from the network interface, the subnet, or both.</span></span> <span data-ttu-id="59770-146">Mer information om NSG: er i [NSG översikt](virtual-networks-nsg.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="59770-146">To learn more about NSGs, read the [NSG overview](virtual-networks-nsg.md) article.</span></span>