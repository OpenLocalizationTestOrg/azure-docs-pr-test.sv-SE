---
title: "aaaUsing interna DNS för VM-namnmatchning i Azure | Microsoft Docs"
description: "Med hjälp av interna DNS för namnmatchning för virtuell dator på Azure."
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
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.openlocfilehash: 94fd6577aa51ce5db4dc26649b415ddeeb410eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="d2ecf-103">Med hjälp av interna DNS för namnmatchning för virtuell dator på Azure</span><span class="sxs-lookup"><span data-stu-id="d2ecf-103">Using internal DNS for VM name resolution on Azure</span></span>

<span data-ttu-id="d2ecf-104">Den här artikeln visar hur tooset statiska interna DNS-namn för Linux virtuella datorer med hjälp av virtuella nätverkskort (tillval) (VNic) och DNS-etikettnamn.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-104">This article shows how tooset static internal DNS names for Linux VMs using Virtual NIC cards (VNic) and DNS label names.</span></span> <span data-ttu-id="d2ecf-105">Statisk DNS-namn används för permanenta infrastrukturtjänster som en Jenkins build-server, som används för det här dokumentet eller en Git-server.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-105">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="d2ecf-106">hello kraven är:</span><span class="sxs-lookup"><span data-stu-id="d2ecf-106">hello requirements are:</span></span>

* [<span data-ttu-id="d2ecf-107">ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="d2ecf-107">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="d2ecf-108">offentliga och privata SSH-nyckelfiler</span><span class="sxs-lookup"><span data-stu-id="d2ecf-108">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="d2ecf-109">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="d2ecf-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="d2ecf-110">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="d2ecf-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="d2ecf-111">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="d2ecf-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="d2ecf-112">[Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="d2ecf-112">[Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="d2ecf-113">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="d2ecf-113">Quick commands</span></span>

<span data-ttu-id="d2ecf-114">Om du behöver tooquickly utföra hello uppgiften, hello efter avsnittet information hello-kommandon som krävs.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-114">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="d2ecf-115">Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="d2ecf-115">More detailed information and context for each step can be found hello rest of hello document, [starting here](#detailed-walkthrough).</span></span>  

<span data-ttu-id="d2ecf-116">Krav: Resursgrupp, VNet, NSG med SSH inkommande, undernät.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-116">Pre-Requirements: Resource Group, VNet, NSG with SSH inbound, Subnet.</span></span>

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a><span data-ttu-id="d2ecf-117">Skapa ett virtuellt nätverkskort med ett statiskt interna DNS-namn</span><span class="sxs-lookup"><span data-stu-id="d2ecf-117">Create a VNic with a static internal DNS name</span></span>

<span data-ttu-id="d2ecf-118">Hej `-r` cli-flaggan är för inställningen hello DNS-etikett, vilket ger hello statisk DNS-namn för hello VNic.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-118">hello `-r` cli flag is for setting hello DNS label, which provides hello static DNS name for hello VNic.</span></span>

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-hello-vm-into-hello-vnet-nsg-and-connect-hello-vnic"></a><span data-ttu-id="d2ecf-119">Distribuera hello VM till hello VNet, NSG och ansluta hello VNic</span><span class="sxs-lookup"><span data-stu-id="d2ecf-119">Deploy hello VM into hello VNet, NSG and, connect hello VNic</span></span>

<span data-ttu-id="d2ecf-120">Hej `-N` ansluter hello VNic toohello ny virtuell dator under hello distribution tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-120">hello `-N` connects hello VNic toohello new VM during hello deployment tooAzure.</span></span>

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="d2ecf-121">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="d2ecf-121">Detailed walkthrough</span></span>

<span data-ttu-id="d2ecf-122">En fullständig kontinuerlig integrering och kontinuerlig distribution (CiCd) infrastrukturen i Azure kräver vissa toobe statisk eller långlivade servrar.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-122">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers toobe static or long-lived servers.</span></span>  <span data-ttu-id="d2ecf-123">Du rekommenderas att Azure tillgångar som hello virtuella nätverk (Vnet) och Nätverkssäkerhetsgrupper (NSG: er) måste vara statiska och resurser som distribueras sällan kvar längre.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-123">It is recommended that Azure assets like hello Virtual Networks (VNets) and Network Security Groups (NSGs), should be static and long lived resources that are rarely deployed.</span></span>  <span data-ttu-id="d2ecf-124">När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar toohello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-124">Once a VNet has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span>  <span data-ttu-id="d2ecf-125">Lägga till toothis statiska nätverket en Git ger databasen server och en Jenkins automationsserver CiCd tooyour utvecklings- och testmiljöer.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-125">Adding toothis static network a Git repository server and a Jenkins automation server delivers CiCd tooyour development or test environments.</span></span>  

<span data-ttu-id="d2ecf-126">Interna DNS-namn är bara lösas i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-126">Internal DNS names are only resolvable inside an Azure virtual network.</span></span>  <span data-ttu-id="d2ecf-127">Eftersom hello DNS-namn är internt, men de är inte matchas toohello utanför internet, kan ge ytterligare säkerhet toohello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-127">Because hello DNS names are internal, they are not resolvable toohello outside internet, providing additional security toohello infrastructure.</span></span>

<span data-ttu-id="d2ecf-128">_Ersätt alla exempel med egna namn._</span><span class="sxs-lookup"><span data-stu-id="d2ecf-128">_Replace any examples with your own naming._</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="d2ecf-129">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d2ecf-129">Create hello Resource group</span></span>

<span data-ttu-id="d2ecf-130">En resursgrupp är nödvändiga tooorganize allt vi skapa i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-130">A Resource Group is needed tooorganize everything we create in this walkthrough.</span></span>  <span data-ttu-id="d2ecf-131">Läs mer om Azure-resursgrupper [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d2ecf-131">For more information on Azure Resource Groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-hello-vnet"></a><span data-ttu-id="d2ecf-132">Skapa hello VNet</span><span class="sxs-lookup"><span data-stu-id="d2ecf-132">Create hello VNet</span></span>

<span data-ttu-id="d2ecf-133">hello första steget är toobuild ett VNet toolaunch hello virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-133">hello first step is toobuild a VNet toolaunch hello VMs into.</span></span>  <span data-ttu-id="d2ecf-134">Hej VNet innehåller ett undernät för den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-134">hello VNet contains one subnet for this walkthrough.</span></span>  <span data-ttu-id="d2ecf-135">Mer information om virtuella Azure-nätverk finns [skapa ett virtuellt nätverk med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d2ecf-135">For more information on Azure VNets, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-hello-nsg"></a><span data-ttu-id="d2ecf-136">Skapa hello NSG</span><span class="sxs-lookup"><span data-stu-id="d2ecf-136">Create hello NSG</span></span>

<span data-ttu-id="d2ecf-137">hello undernät bygger bakom en befintlig säkerhetsgrupp för nätverk så att vi bygga hello NSG innan hello undernät.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-137">hello Subnet is built behind an existing Network Security Group so we build hello NSG before hello Subnet.</span></span>  <span data-ttu-id="d2ecf-138">Azure NSG: er är likvärdiga tooa brandväggen på hello nätverksnivån.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-138">Azure NSGs are equivalent tooa firewall at hello network layer.</span></span>  <span data-ttu-id="d2ecf-139">Mer information om Azure NSG: er finns [hur toocreate NSG: er i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d2ecf-139">For more information on Azure NSGs, see [How toocreate NSGs in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="d2ecf-140">Lägg till regel för att tillåta en inkommande SSH</span><span class="sxs-lookup"><span data-stu-id="d2ecf-140">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="d2ecf-141">hello Linux VM behöver åtkomst från hello internet så att en regel som tillåter inkommande port 22 trafik toobe har passerat hello nätverket tooport 22 på hello Linux VM krävs.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-141">hello Linux VM needs access from hello internet so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello Linux VM is needed.</span></span>

```azurecli
azure network nsg rule create inboundSSH \
--resource-group myResourceGroup \
--nsg-name myNSG \
--access Allow \
--protocol Tcp \
--direction Inbound \
--priority 100 \
--source-address-prefix * \
--source-port-range * \
--destination-address-prefix 10.10.0.0/24 \
--destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a><span data-ttu-id="d2ecf-142">Lägg till ett undernät toohello VNet</span><span class="sxs-lookup"><span data-stu-id="d2ecf-142">Add a subnet toohello VNet</span></span>

<span data-ttu-id="d2ecf-143">Virtuella datorer i hello VNet måste finnas i ett undernät.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-143">VMs within hello VNet must be located in a subnet.</span></span>  <span data-ttu-id="d2ecf-144">Varje virtuellt nätverk kan ha flera undernät.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-144">Each VNet can have multiple subnets.</span></span>  <span data-ttu-id="d2ecf-145">Skapa hello undernät och associerar hello undernät med hello NSG tooadd ett brandväggen toohello undernät.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-145">Create hello subnet and associate hello subnet with hello NSG tooadd a firewall toohello subnet.</span></span>

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

<span data-ttu-id="d2ecf-146">hello undernät är nu läggs till i hello virtuella nätverk och som är associerade med hello NSG och hello NSG regel.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-146">hello Subnet is now added inside hello VNet and associated with hello NSG and hello NSG rule.</span></span>

## <a name="creating-static-dns-names"></a><span data-ttu-id="d2ecf-147">Skapa statiska DNS-namn</span><span class="sxs-lookup"><span data-stu-id="d2ecf-147">Creating static DNS names</span></span>

<span data-ttu-id="d2ecf-148">Azure är flexibla, men toouse DNS-namn för namnmatchning för virtuella datorer, måste toocreate dem som virtuella nätverkskort (VNics) med hjälp av DNS-etiketter.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-148">Azure is very flexible, but toouse DNS names for VMs name resolution, you need toocreate them as Virtual network cards (VNics) using DNS labeling.</span></span>  <span data-ttu-id="d2ecf-149">VNics är viktigt eftersom du kan återanvända dem genom att ansluta dem toodifferent virtuella datorer, vilket håller hello VNic som en statisk resurs hello virtuella datorer kan vara tillfälliga.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-149">VNics are important as you can reuse them by connecting them toodifferent VMs, which keeps hello VNic as a static resource while hello VMs can be temporary.</span></span>  <span data-ttu-id="d2ecf-150">Med hjälp av DNS-etiketter på hello VNic är vi kan tooenable enkel namnmatchning från andra virtuella datorer i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-150">By using DNS labeling on hello VNic, we are able tooenable simple name resolution from other VMs in hello VNet.</span></span>  <span data-ttu-id="d2ecf-151">Med matchas namn kan andra virtuella datorer tooaccess hello automation-server med DNS-namn för hello `Jenkins` eller hello Git-server som `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-151">Using resolvable names enables other VMs tooaccess hello automation server by hello DNS name `Jenkins` or hello Git server as `gitrepo`.</span></span>  <span data-ttu-id="d2ecf-152">Skapa ett virtuellt nätverkskort och koppla den till hello undernät som skapats i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-152">Create a VNic and associate it with hello Subnet created in hello previous step.</span></span>

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="d2ecf-153">Distribuera hello VM till hello VNet och NSG</span><span class="sxs-lookup"><span data-stu-id="d2ecf-153">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="d2ecf-154">Nu har vi ett VNet, ett undernät i det virtuella nätverket och en NSG som fungerar som en brandvägg tooprotect våra undernät genom att blockera all inkommande trafik utom port 22 för SSH.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-154">We now have a VNet, a subnet inside that VNet, and an NSG acting as a firewall tooprotect our subnet by blocking all inbound traffic except port 22 for SSH.</span></span>  <span data-ttu-id="d2ecf-155">hello VM kan nu distribueras i det här befintliga nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-155">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="d2ecf-156">Med hjälp av hello Azure CLI och hello `azure vm create` kommandot hello Linux VM är distribuerade toohello befintliga Azure-resursgrupp, virtuella nätverk, undernät och virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-156">Using hello Azure CLI, and hello `azure vm create` command, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>  <span data-ttu-id="d2ecf-157">Mer information om hur du använder hello CLI toodeploy fullständig VM finns [skapa en fullständig Linux-miljö med hjälp av hello Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d2ecf-157">For more information on using hello CLI toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

<span data-ttu-id="d2ecf-158">Med hjälp av hello flaggor CLI toocall befintliga resurser vi ber Azure toodeploy hello VM i hello befintliga nätverk.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-158">By using hello CLI flags toocall out existing resources, we instruct Azure toodeploy hello VM inside hello existing network.</span></span>  <span data-ttu-id="d2ecf-159">tooreiterate, när en VNet och undernät har distribuerats, de kan förbli statisk eller permanenta resurser i Azure-region.</span><span class="sxs-lookup"><span data-stu-id="d2ecf-159">tooreiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="d2ecf-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2ecf-160">Next steps</span></span>
* [<span data-ttu-id="d2ecf-161">Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="d2ecf-161">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="d2ecf-162">Skapa en Linux-VM på Azure med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="d2ecf-162">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
