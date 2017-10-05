---
title: "Använda interna DNS för namnmatchning för virtuell dator med Azure CLI 2.0 | Microsoft Docs"
description: "Skapa ett virtuellt nätverk nätverkskort och använda interna DNS för namnmatchning för virtuell dator på Azure med Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: 992920adb1ae3736d43cc5f0bbb2081a20a1674d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="8136c-103">Skapa virtuella nätverkskort och använda interna DNS för namnmatchning för virtuell dator på Azure</span><span class="sxs-lookup"><span data-stu-id="8136c-103">Create virtual network interface cards and use internal DNS for VM name resolution on Azure</span></span>
<span data-ttu-id="8136c-104">Den här artikeln visar hur du ställer in statiska interna DNS-namn för Linux virtuella datorer med Azure CLI 2.0 virtuella nätverkskort (vNics) och DNS-etikettnamn.</span><span class="sxs-lookup"><span data-stu-id="8136c-104">This article shows you how to set static internal DNS names for Linux VMs using virtual network interface cards (vNics) and DNS label names with the Azure CLI 2.0.</span></span> <span data-ttu-id="8136c-105">Du kan också utföra dessa steg med [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8136c-105">You can also perform these steps with the [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="8136c-106">Statisk DNS-namn används för permanenta infrastrukturtjänster som en Jenkins build-server, som används för det här dokumentet eller en Git-server.</span><span class="sxs-lookup"><span data-stu-id="8136c-106">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="8136c-107">Kraven är:</span><span class="sxs-lookup"><span data-stu-id="8136c-107">The requirements are:</span></span>

* [<span data-ttu-id="8136c-108">ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="8136c-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="8136c-109">offentliga och privata SSH-nyckelfiler</span><span class="sxs-lookup"><span data-stu-id="8136c-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="8136c-110">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="8136c-110">Quick commands</span></span>
<span data-ttu-id="8136c-111">Om du behöver utföra aktiviteten i följande avsnitt finns information de kommandon som krävs.</span><span class="sxs-lookup"><span data-stu-id="8136c-111">If you need to quickly accomplish the task, the following section details the commands needed.</span></span> <span data-ttu-id="8136c-112">Mer detaljerad information och kontext för varje steg i resten av dokumentet [startar här](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="8136c-112">More detailed information and context for each step can be found in the rest of the document, [starting here](#detailed-walkthrough).</span></span> <span data-ttu-id="8136c-113">Om du vill utföra dessa steg behöver du senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="8136c-113">To perform these steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="8136c-114">Krav: Resursgrupp, virtuella nätverk och undernät, Nätverkssäkerhetsgrupp med SSH inkommande.</span><span class="sxs-lookup"><span data-stu-id="8136c-114">Pre-Requirements: Resource Group, virtual network and subnet, Network Security Group with SSH inbound.</span></span>

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a><span data-ttu-id="8136c-115">Skapa ett virtuellt nätverkskort med ett statiskt interna DNS-namn</span><span class="sxs-lookup"><span data-stu-id="8136c-115">Create a virtual network interface card with a static internal DNS name</span></span>
<span data-ttu-id="8136c-116">Skapa vNic med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="8136c-116">Create the vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="8136c-117">Den `--internal-dns-name` CLI-flaggan är för att ställa in DNS-etikett, vilket ger statisk DNS-namn för det virtuella nätverksgränssnittskortet (vNic).</span><span class="sxs-lookup"><span data-stu-id="8136c-117">The `--internal-dns-name` CLI flag is for setting the DNS label, which provides the static DNS name for the virtual network interface card (vNic).</span></span> <span data-ttu-id="8136c-118">I följande exempel skapas ett virtuellt nätverkskort med namnet `myNic`, ansluter den till den `myVnet` virtuella nätverk och skapar en intern DNS-namnpost kallas `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="8136c-118">The following example creates a vNic named `myNic`, connects it to the `myVnet` virtual network, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-the-vnic"></a><span data-ttu-id="8136c-119">Distribuera en virtuell dator och ansluta vNic</span><span class="sxs-lookup"><span data-stu-id="8136c-119">Deploy a VM and connect the vNic</span></span>
<span data-ttu-id="8136c-120">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="8136c-120">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="8136c-121">Den `--nics` flaggan ansluter vNic till den virtuella datorn under distributionen till Azure.</span><span class="sxs-lookup"><span data-stu-id="8136c-121">The `--nics` flag connects the vNic to the VM during the deployment to Azure.</span></span> <span data-ttu-id="8136c-122">I följande exempel skapas en virtuell dator med namnet `myVM` med Azure hanterade diskar och bifogar vNic med namnet `myNic` från föregående steg:</span><span class="sxs-lookup"><span data-stu-id="8136c-122">The following example creates a VM named `myVM` with Azure Managed Disks and attaches the vNic named `myNic` from the preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="8136c-123">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="8136c-123">Detailed walkthrough</span></span>

<span data-ttu-id="8136c-124">En fullständig kontinuerlig integrering och kontinuerlig distribution (CiCd) infrastrukturen i Azure kräver vissa servrar vara statiska eller långlivade servrar.</span><span class="sxs-lookup"><span data-stu-id="8136c-124">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers to be static or long-lived servers.</span></span> <span data-ttu-id="8136c-125">Du rekommenderas att Azure tillgångar som den virtuella nätverk och Nätverkssäkerhetsgrupper är statiska och resurser som distribueras sällan kvar längre.</span><span class="sxs-lookup"><span data-stu-id="8136c-125">It is recommended that Azure assets like the virtual networks and Network Security Groups are static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="8136c-126">När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar i infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="8136c-126">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects to the infrastructure.</span></span> <span data-ttu-id="8136c-127">Du kan senare lägga till en Git-lagringsplatsen server eller en Jenkins automation-server ger CiCd till det här virtuella nätverket för utvecklings- eller testmiljöer.</span><span class="sxs-lookup"><span data-stu-id="8136c-127">You can later add a Git repository server or a Jenkins automation server delivers CiCd to this virtual network for your development or test environments.</span></span>  

<span data-ttu-id="8136c-128">Interna DNS-namn är bara lösas i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="8136c-128">Internal DNS names are only resolvable inside an Azure virtual network.</span></span> <span data-ttu-id="8136c-129">Eftersom DNS-namn är internt, är de inte matchas till utanför internet, vilket ger ytterligare säkerhet i infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="8136c-129">Because the DNS names are internal, they are not resolvable to the outside internet, providing additional security to the infrastructure.</span></span>

<span data-ttu-id="8136c-130">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="8136c-130">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="8136c-131">Exempel parameternamn inkluderar `myResourceGroup`, `myNic`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="8136c-131">Example parameter names include `myResourceGroup`, `myNic`, and `myVM`.</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="8136c-132">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="8136c-132">Create the resource group</span></span>
<span data-ttu-id="8136c-133">Börja med att skapa resursgruppen med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="8136c-133">First, create the resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="8136c-134">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `westus` plats:</span><span class="sxs-lookup"><span data-stu-id="8136c-134">The following example creates a resource group named `myResourceGroup` in the `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-the-virtual-network"></a><span data-ttu-id="8136c-135">Skapa virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="8136c-135">Create the virtual network</span></span>

<span data-ttu-id="8136c-136">Nästa steg är att skapa ett virtuellt nätverk för att starta de virtuella datorerna till.</span><span class="sxs-lookup"><span data-stu-id="8136c-136">The next step is to build a virtual network to launch the VMs into.</span></span> <span data-ttu-id="8136c-137">Det virtuella nätverket innehåller ett undernät för den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="8136c-137">The virtual network contains one subnet for this walkthrough.</span></span> <span data-ttu-id="8136c-138">Mer information om virtuella Azure-nätverk finns [skapa ett virtuellt nätverk med hjälp av Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8136c-138">For more information on Azure virtual networks, see [Create a virtual network by using the Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="8136c-139">Skapa ett virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="8136c-139">Create the virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="8136c-140">I följande exempel skapas ett virtuellt nätverk med namnet `myVnet` och undernät med namnet `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="8136c-140">The following example creates a virtual network named `myVnet` and subnet named `mySubnet`:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-the-network-security-group"></a><span data-ttu-id="8136c-141">Skapa Nätverkssäkerhetsgruppen</span><span class="sxs-lookup"><span data-stu-id="8136c-141">Create the Network Security Group</span></span>
<span data-ttu-id="8136c-142">Azure Nätverkssäkerhetsgrupper är likvärdiga med en brandvägg på nätverksnivå.</span><span class="sxs-lookup"><span data-stu-id="8136c-142">Azure Network Security Groups are equivalent to a firewall at the network layer.</span></span> <span data-ttu-id="8136c-143">Läs mer om Nätverkssäkerhetsgrupper [hur du skapar NSG: er i Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8136c-143">For more information about Network Security Groups, see [How to create NSGs in the Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="8136c-144">Skapa nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="8136c-144">Create the network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="8136c-145">I följande exempel skapas en nätverkssäkerhetsgrupp som heter `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="8136c-145">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-to-allow-ssh"></a><span data-ttu-id="8136c-146">Lägg till en inkommande regel för att tillåta SSH</span><span class="sxs-lookup"><span data-stu-id="8136c-146">Add an inbound rule to allow SSH</span></span>
<span data-ttu-id="8136c-147">Lägg till en inkommande regel för nätverkssäkerhetsgruppen med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="8136c-147">Add an inbound rule for the network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="8136c-148">I följande exempel skapas en regel med namnet `myRuleAllowSSH`:</span><span class="sxs-lookup"><span data-stu-id="8136c-148">The following example creates a rule named `myRuleAllowSSH`:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-the-subnet-with-the-network-security-group"></a><span data-ttu-id="8136c-149">Associera undernätet med Nätverkssäkerhetsgruppen</span><span class="sxs-lookup"><span data-stu-id="8136c-149">Associate the subnet with the Network Security Group</span></span>
<span data-ttu-id="8136c-150">Om du vill associera undernätet med Nätverkssäkerhetsgruppen använder [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update).</span><span class="sxs-lookup"><span data-stu-id="8136c-150">To associate the subnet with the Network Security Group, use [az network vnet subnet update](/cli/azure/network/vnet/subnet#update).</span></span> <span data-ttu-id="8136c-151">I följande exempel associerar undernätnamnet `mySubnet` med Nätverkssäkerhetsgruppen med namnet `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="8136c-151">The following example associates the subnet name `mySubnet` with the Network Security Group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-the-virtual-network-interface-card-and-static-dns-names"></a><span data-ttu-id="8136c-152">Skapa virtuella nätverkskort och statisk DNS-namn</span><span class="sxs-lookup"><span data-stu-id="8136c-152">Create the virtual network interface card and static DNS names</span></span>
<span data-ttu-id="8136c-153">Azure är flexibla, men om du vill använda DNS-namn för VM-namnmatchning, måste du skapa virtuellt nätverkskort (vNics) som innehåller en DNS-etikett.</span><span class="sxs-lookup"><span data-stu-id="8136c-153">Azure is very flexible, but to use DNS names for VM name resolution, you need to create virtual network interface cards (vNics) that include a DNS label.</span></span> <span data-ttu-id="8136c-154">vNics är viktigt eftersom du kan återanvända dem genom att ansluta dem till olika virtuella datorer under hela livscykeln för infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="8136c-154">vNics are important as you can reuse them by connecting them to different VMs over the infrastructure lifecycle.</span></span> <span data-ttu-id="8136c-155">Den här metoden används för att hålla vNic som en statisk resurs medan de virtuella datorerna kan vara tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="8136c-155">This approach keeps the vNic as a static resource while the VMs can be temporary.</span></span> <span data-ttu-id="8136c-156">Med hjälp av DNS-etiketter på vNic, kommer du att aktivera enkel namnmatchning från andra virtuella datorer i VNet.</span><span class="sxs-lookup"><span data-stu-id="8136c-156">By using DNS labeling on the vNic, we are able to enable simple name resolution from other VMs in the VNet.</span></span> <span data-ttu-id="8136c-157">Med matchas namn kan andra virtuella datorer att ansluta till automatiseringsservern med DNS-namn `Jenkins` eller Git-server som `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="8136c-157">Using resolvable names enables other VMs to access the automation server by the DNS name `Jenkins` or the Git server as `gitrepo`.</span></span>  

<span data-ttu-id="8136c-158">Skapa vNic med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="8136c-158">Create the vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="8136c-159">I följande exempel skapas ett virtuellt nätverkskort med namnet `myNic`, ansluter den till den `myVnet` virtuellt nätverk med namnet `myVnet`, och skapar en intern DNS-namnpost kallas `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="8136c-159">The following example creates a vNic named `myNic`, connects it to the `myVnet` virtual network named `myVnet`, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a><span data-ttu-id="8136c-160">Distribuera den virtuella datorn till den virtuella nätverksinfrastrukturen</span><span class="sxs-lookup"><span data-stu-id="8136c-160">Deploy the VM into the virtual network infrastructure</span></span>
<span data-ttu-id="8136c-161">Nu har vi ett virtuellt nätverk och undernät, en Nätverkssäkerhetsgrupp fungerar som en brandvägg för att skydda våra undernät genom att blockera all inkommande trafik utom port 22 för SSH- och ett virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="8136c-161">We now have a virtual network and subnet, a Network Security Group acting as a firewall to protect our subnet by blocking all inbound traffic except port 22 for SSH, and a vNic.</span></span> <span data-ttu-id="8136c-162">Du kan nu distribuera en virtuell dator i den här befintliga nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="8136c-162">You can now deploy a VM inside this existing network infrastructure.</span></span>

<span data-ttu-id="8136c-163">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="8136c-163">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="8136c-164">I följande exempel skapas en virtuell dator med namnet `myVM` med Azure hanterade diskar och bifogar vNic med namnet `myNic` från föregående steg:</span><span class="sxs-lookup"><span data-stu-id="8136c-164">The following example creates a VM named `myVM` with Azure Managed Disks and attaches the vNic named `myNic` from the preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="8136c-165">Med hjälp av CLI-flaggor för att anropa befintliga resurser, ber vi Azure för att distribuera den virtuella datorn i det befintliga nätverket.</span><span class="sxs-lookup"><span data-stu-id="8136c-165">By using the CLI flags to call out existing resources, we instruct Azure to deploy the VM inside the existing network.</span></span> <span data-ttu-id="8136c-166">Om du vill upprepar när ett VNet och undernät som har distribuerats, kan de förbli statisk eller permanenta resurser i Azure-region.</span><span class="sxs-lookup"><span data-stu-id="8136c-166">To reiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="8136c-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8136c-167">Next steps</span></span>
* [<span data-ttu-id="8136c-168">Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="8136c-168">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="8136c-169">Skapa en Linux-VM på Azure med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="8136c-169">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
