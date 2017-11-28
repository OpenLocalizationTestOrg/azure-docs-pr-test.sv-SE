---
title: "aaaUse interna DNS för VM-namnmatchning med hello Azure CLI 2.0 | Microsoft Docs"
description: "Hur toocreate virtuellt nätverkskort och använder interna DNS för namnmatchning för virtuell dator på Azure med hello Azure CLI 2.0"
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
ms.openlocfilehash: b3c4bfd3ab698f7b25d763ba9e60dd7984f6269d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="59745-103">Skapa virtuella nätverkskort och använda interna DNS för namnmatchning för virtuell dator på Azure</span><span class="sxs-lookup"><span data-stu-id="59745-103">Create virtual network interface cards and use internal DNS for VM name resolution on Azure</span></span>
<span data-ttu-id="59745-104">Den här artikeln visar hur tooset statiska interna DNS-namn för Linux virtuella datorer med virtuella nätverk nätverkskort (vNics) och DNS-etikettnamn med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="59745-104">This article shows you how tooset static internal DNS names for Linux VMs using virtual network interface cards (vNics) and DNS label names with hello Azure CLI 2.0.</span></span> <span data-ttu-id="59745-105">Du kan också utföra dessa steg med hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59745-105">You can also perform these steps with hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="59745-106">Statisk DNS-namn används för permanenta infrastrukturtjänster som en Jenkins build-server, som används för det här dokumentet eller en Git-server.</span><span class="sxs-lookup"><span data-stu-id="59745-106">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="59745-107">hello kraven är:</span><span class="sxs-lookup"><span data-stu-id="59745-107">hello requirements are:</span></span>

* [<span data-ttu-id="59745-108">ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="59745-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="59745-109">offentliga och privata SSH-nyckelfiler</span><span class="sxs-lookup"><span data-stu-id="59745-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="59745-110">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="59745-110">Quick commands</span></span>
<span data-ttu-id="59745-111">Om du behöver tooquickly utföra hello uppgiften, hello efter avsnittet information hello-kommandon som krävs.</span><span class="sxs-lookup"><span data-stu-id="59745-111">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="59745-112">Mer detaljerad information och kontext för varje steg i hello resten av dokumentet hello [startar här](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="59745-112">More detailed information and context for each step can be found in hello rest of hello document, [starting here](#detailed-walkthrough).</span></span> <span data-ttu-id="59745-113">tooperform dessa steg, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="59745-113">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="59745-114">Krav: Resursgrupp, virtuella nätverk och undernät, Nätverkssäkerhetsgrupp med SSH inkommande.</span><span class="sxs-lookup"><span data-stu-id="59745-114">Pre-Requirements: Resource Group, virtual network and subnet, Network Security Group with SSH inbound.</span></span>

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a><span data-ttu-id="59745-115">Skapa ett virtuellt nätverkskort med ett statiskt interna DNS-namn</span><span class="sxs-lookup"><span data-stu-id="59745-115">Create a virtual network interface card with a static internal DNS name</span></span>
<span data-ttu-id="59745-116">Skapa hello vNic med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="59745-116">Create hello vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="59745-117">Hej `--internal-dns-name` CLI-flaggan är för inställningen hello DNS-etikett, vilket ger hello statisk DNS-namn för hello virtuella nätverksgränssnittskortet (vNic).</span><span class="sxs-lookup"><span data-stu-id="59745-117">hello `--internal-dns-name` CLI flag is for setting hello DNS label, which provides hello static DNS name for hello virtual network interface card (vNic).</span></span> <span data-ttu-id="59745-118">hello följande exempel skapas ett virtuellt nätverkskort med namnet `myNic`, ansluter den toohello `myVnet` virtuella nätverk och skapar en intern DNS-namnpost kallas `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="59745-118">hello following example creates a vNic named `myNic`, connects it toohello `myVnet` virtual network, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-hello-vnic"></a><span data-ttu-id="59745-119">Distribuera en virtuell dator och Anslut hello vNic</span><span class="sxs-lookup"><span data-stu-id="59745-119">Deploy a VM and connect hello vNic</span></span>
<span data-ttu-id="59745-120">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="59745-120">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="59745-121">Hej `--nics` flaggan ansluter hello vNic toohello VM under hello distribution tooAzure.</span><span class="sxs-lookup"><span data-stu-id="59745-121">hello `--nics` flag connects hello vNic toohello VM during hello deployment tooAzure.</span></span> <span data-ttu-id="59745-122">hello följande exempel skapas en virtuell dator med namnet `myVM` med Azure hanterade diskar och bifogar hello vNic med namnet `myNic` från hello föregående steg:</span><span class="sxs-lookup"><span data-stu-id="59745-122">hello following example creates a VM named `myVM` with Azure Managed Disks and attaches hello vNic named `myNic` from hello preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="59745-123">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="59745-123">Detailed walkthrough</span></span>

<span data-ttu-id="59745-124">En fullständig kontinuerlig integrering och kontinuerlig distribution (CiCd) infrastrukturen i Azure kräver vissa toobe statisk eller långlivade servrar.</span><span class="sxs-lookup"><span data-stu-id="59745-124">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers toobe static or long-lived servers.</span></span> <span data-ttu-id="59745-125">Du rekommenderas att Azure tillgångar som hello virtuella nätverk och Nätverkssäkerhetsgrupper är statiska och resurser som distribueras sällan kvar längre.</span><span class="sxs-lookup"><span data-stu-id="59745-125">It is recommended that Azure assets like hello virtual networks and Network Security Groups are static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="59745-126">När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar toohello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="59745-126">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="59745-127">Du kan senare lägga till en Git-lagringsplatsen server eller en Jenkins automation-server ger CiCd toothis virtuellt nätverk för utvecklings- och testmiljöer.</span><span class="sxs-lookup"><span data-stu-id="59745-127">You can later add a Git repository server or a Jenkins automation server delivers CiCd toothis virtual network for your development or test environments.</span></span>  

<span data-ttu-id="59745-128">Interna DNS-namn är bara lösas i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="59745-128">Internal DNS names are only resolvable inside an Azure virtual network.</span></span> <span data-ttu-id="59745-129">Eftersom hello DNS-namn är internt, men de är inte matchas toohello utanför internet, kan ge ytterligare säkerhet toohello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="59745-129">Because hello DNS names are internal, they are not resolvable toohello outside internet, providing additional security toohello infrastructure.</span></span>

<span data-ttu-id="59745-130">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="59745-130">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="59745-131">Exempel parameternamn inkluderar `myResourceGroup`, `myNic`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="59745-131">Example parameter names include `myResourceGroup`, `myNic`, and `myVM`.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="59745-132">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="59745-132">Create hello resource group</span></span>
<span data-ttu-id="59745-133">Först skapar hello resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="59745-133">First, create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="59745-134">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westus` plats:</span><span class="sxs-lookup"><span data-stu-id="59745-134">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-hello-virtual-network"></a><span data-ttu-id="59745-135">Skapa hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="59745-135">Create hello virtual network</span></span>

<span data-ttu-id="59745-136">hello nästa steg är toobuild ett virtuellt nätverk toolaunch hello virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="59745-136">hello next step is toobuild a virtual network toolaunch hello VMs into.</span></span> <span data-ttu-id="59745-137">hello virtuellt nätverk innehåller ett undernät för den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="59745-137">hello virtual network contains one subnet for this walkthrough.</span></span> <span data-ttu-id="59745-138">Mer information om virtuella Azure-nätverk finns [skapa ett virtuellt nätverk med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59745-138">For more information on Azure virtual networks, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="59745-139">Skapa hello virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="59745-139">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="59745-140">hello följande exempel skapas ett virtuellt nätverk med namnet `myVnet` och undernät med namnet `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="59745-140">hello following example creates a virtual network named `myVnet` and subnet named `mySubnet`:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="59745-141">Skapa hello Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="59745-141">Create hello Network Security Group</span></span>
<span data-ttu-id="59745-142">Säkerhetsgrupper för Azure nätverket är likvärdiga tooa brandväggen på nätverksnivå hello.</span><span class="sxs-lookup"><span data-stu-id="59745-142">Azure Network Security Groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="59745-143">Läs mer om Nätverkssäkerhetsgrupper [hur toocreate NSG: er i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59745-143">For more information about Network Security Groups, see [How toocreate NSGs in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="59745-144">Skapa hello nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="59745-144">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="59745-145">hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="59745-145">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-tooallow-ssh"></a><span data-ttu-id="59745-146">Lägg till en regel för inkommande trafik tooallow SSH</span><span class="sxs-lookup"><span data-stu-id="59745-146">Add an inbound rule tooallow SSH</span></span>
<span data-ttu-id="59745-147">Lägg till en inkommande regel för hello nätverkssäkerhetsgrupp med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="59745-147">Add an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="59745-148">hello följande exempel skapas en regel med namnet `myRuleAllowSSH`:</span><span class="sxs-lookup"><span data-stu-id="59745-148">hello following example creates a rule named `myRuleAllowSSH`:</span></span>

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

## <a name="associate-hello-subnet-with-hello-network-security-group"></a><span data-ttu-id="59745-149">Associera hello undernät med hello Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="59745-149">Associate hello subnet with hello Network Security Group</span></span>
<span data-ttu-id="59745-150">tooassociate hello undernät med hello säkerhetsgrupp för nätverk använder [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update).</span><span class="sxs-lookup"><span data-stu-id="59745-150">tooassociate hello subnet with hello Network Security Group, use [az network vnet subnet update](/cli/azure/network/vnet/subnet#update).</span></span> <span data-ttu-id="59745-151">hello följande exempel associerar hello undernätsnamn `mySubnet` med hello säkerhetsgrupp för nätverk med namnet `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="59745-151">hello following example associates hello subnet name `mySubnet` with hello Network Security Group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-hello-virtual-network-interface-card-and-static-dns-names"></a><span data-ttu-id="59745-152">Skapa hello virtuella nätverkskort och statisk DNS-namn</span><span class="sxs-lookup"><span data-stu-id="59745-152">Create hello virtual network interface card and static DNS names</span></span>
<span data-ttu-id="59745-153">Azure är flexibla, men toouse DNS-namn för VM-namnmatchning, behöver du toocreate virtuella nätverkskort (vNics) som innehåller en DNS-etikett.</span><span class="sxs-lookup"><span data-stu-id="59745-153">Azure is very flexible, but toouse DNS names for VM name resolution, you need toocreate virtual network interface cards (vNics) that include a DNS label.</span></span> <span data-ttu-id="59745-154">vNics är viktigt eftersom du kan återanvända dem genom att ansluta dem toodifferent virtuella datorer över hello infrastruktur livscykel.</span><span class="sxs-lookup"><span data-stu-id="59745-154">vNics are important as you can reuse them by connecting them toodifferent VMs over hello infrastructure lifecycle.</span></span> <span data-ttu-id="59745-155">Den här metoden används för att hålla hello vNic som en statisk resurs hello virtuella datorer kan vara tillfälliga.</span><span class="sxs-lookup"><span data-stu-id="59745-155">This approach keeps hello vNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="59745-156">Vi kan kan tooenable enkel namnmatchning från andra virtuella datorer i hello VNet med hjälp av DNS-etiketter på hello virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="59745-156">By using DNS labeling on hello vNic, we are able tooenable simple name resolution from other VMs in hello VNet.</span></span> <span data-ttu-id="59745-157">Med matchas namn kan andra virtuella datorer tooaccess hello automation-server med DNS-namn för hello `Jenkins` eller hello Git-server som `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="59745-157">Using resolvable names enables other VMs tooaccess hello automation server by hello DNS name `Jenkins` or hello Git server as `gitrepo`.</span></span>  

<span data-ttu-id="59745-158">Skapa hello vNic med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="59745-158">Create hello vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="59745-159">hello följande exempel skapas ett virtuellt nätverkskort med namnet `myNic`, ansluter den toohello `myVnet` virtuellt nätverk med namnet `myVnet`, och skapar en intern DNS-namnpost kallas `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="59745-159">hello following example creates a vNic named `myNic`, connects it toohello `myVnet` virtual network named `myVnet`, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="59745-160">Distribuera hello VM till hello virtuell nätverksinfrastruktur</span><span class="sxs-lookup"><span data-stu-id="59745-160">Deploy hello VM into hello virtual network infrastructure</span></span>
<span data-ttu-id="59745-161">Nu har vi ett virtuellt nätverk och undernät, en Nätverkssäkerhetsgrupp som fungerar som en brandvägg tooprotect våra undernät genom att blockera all inkommande trafik utom port 22 för SSH- och ett virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="59745-161">We now have a virtual network and subnet, a Network Security Group acting as a firewall tooprotect our subnet by blocking all inbound traffic except port 22 for SSH, and a vNic.</span></span> <span data-ttu-id="59745-162">Du kan nu distribuera en virtuell dator i den här befintliga nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="59745-162">You can now deploy a VM inside this existing network infrastructure.</span></span>

<span data-ttu-id="59745-163">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="59745-163">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="59745-164">hello följande exempel skapas en virtuell dator med namnet `myVM` med Azure hanterade diskar och bifogar hello vNic med namnet `myNic` från hello föregående steg:</span><span class="sxs-lookup"><span data-stu-id="59745-164">hello following example creates a VM named `myVM` with Azure Managed Disks and attaches hello vNic named `myNic` from hello preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="59745-165">Med hjälp av hello flaggor CLI toocall befintliga resurser vi ber Azure toodeploy hello VM i hello befintliga nätverk.</span><span class="sxs-lookup"><span data-stu-id="59745-165">By using hello CLI flags toocall out existing resources, we instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="59745-166">tooreiterate, när en VNet och undernät har distribuerats, de kan förbli statisk eller permanenta resurser i Azure-region.</span><span class="sxs-lookup"><span data-stu-id="59745-166">tooreiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="59745-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59745-167">Next steps</span></span>
* [<span data-ttu-id="59745-168">Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="59745-168">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="59745-169">Skapa en Linux-VM på Azure med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="59745-169">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
