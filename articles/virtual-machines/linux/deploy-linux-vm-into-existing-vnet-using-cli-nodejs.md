---
title: "aaaDeploy virtuella Linux-datorer i befintliga nätverk med Azure CLI 1.0 | Microsoft Docs"
description: "Hur toodeploy en Linux VM till ett befintligt virtuellt nätverk med hjälp av hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: e660f1563d386efc7788bd236f8b067145ea09bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli-10"></a><span data-ttu-id="46b9a-103">Hur toodeploy en Linux-dator i ett befintligt virtuellt nätverk med Azure med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="46b9a-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure CLI 1.0</span></span>

<span data-ttu-id="46b9a-104">Den här artikeln beskrivs hur du toouse Azure CLI 1.0 toodeploy en virtuell dator (VM) till ett befintligt virtuellt nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="46b9a-104">This article shows you how toouse Azure CLI 1.0 toodeploy a virtual machine (VM) into an existing Virtual Network (VNet).</span></span> <span data-ttu-id="46b9a-105">hello kraven är:</span><span class="sxs-lookup"><span data-stu-id="46b9a-105">hello requirements are:</span></span>

- [<span data-ttu-id="46b9a-106">ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="46b9a-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="46b9a-107">offentliga och privata SSH-nyckelfiler</span><span class="sxs-lookup"><span data-stu-id="46b9a-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="46b9a-108">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="46b9a-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="46b9a-109">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="46b9a-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="46b9a-110">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="46b9a-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="46b9a-111">[Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="46b9a-111">[Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="46b9a-112">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="46b9a-112">Quick Commands</span></span>

<span data-ttu-id="46b9a-113">Om du behöver tooquickly utföra hello uppgiften, hello efter avsnittet information hello-kommandon som krävs.</span><span class="sxs-lookup"><span data-stu-id="46b9a-113">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="46b9a-114">Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="46b9a-114">More detailed information and context for each step can be found hello rest of hello document, [starting here](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).</span></span>

<span data-ttu-id="46b9a-115">Krav: Resursgrupp, VNet, NSG med SSH inkommande, undernät.</span><span class="sxs-lookup"><span data-stu-id="46b9a-115">Pre-requirements: Resource Group, VNet, NSG with SSH inbound, Subnet.</span></span> <span data-ttu-id="46b9a-116">Ersätt alla exempel med dina egna inställningar.</span><span class="sxs-lookup"><span data-stu-id="46b9a-116">Replace any examples with your own settings.</span></span>

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="46b9a-117">Distribuera hello VM till hello virtuell nätverksinfrastruktur</span><span class="sxs-lookup"><span data-stu-id="46b9a-117">Deploy hello VM into hello virtual network infrastructure</span></span>

```azurecli
azure vm create myVM \
    -g myResourceGroup \
    -l eastus \
    -y linux \
    -Q Debian \
    -o mystorageaccount \
    -u myAdminUser \
    -M ~/.ssh/id_rsa.pub \
    -n myVM \
    -F myVNet \
    -j mySubnet \
    -N myVNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="46b9a-118">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="46b9a-118">Detailed walkthrough</span></span>

<span data-ttu-id="46b9a-119">Azure tillgångar som hello Vnet och nätverkssäkerhetsgrupper måste vara statiska och resurser som distribueras sällan kvar längre.</span><span class="sxs-lookup"><span data-stu-id="46b9a-119">Azure assets like hello VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="46b9a-120">När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar toohello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="46b9a-120">Once a VNet has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="46b9a-121">Tänk på att ett VNet som en nätverksväxel traditionella maskinvara.</span><span class="sxs-lookup"><span data-stu-id="46b9a-121">Think about a VNet as being a traditional hardware network switch.</span></span> <span data-ttu-id="46b9a-122">Du behöver inte tooconfigure en helt ny maskinvara växel med varje distribution.</span><span class="sxs-lookup"><span data-stu-id="46b9a-122">You would not need tooconfigure a brand new hardware switch with each deployment.</span></span> <span data-ttu-id="46b9a-123">Med en korrekt konfigurerad VNet, kan du fortsätta toodeploy nya servrar i det virtuella nätverket flera gånger med några eventuella ändringar som krävs under hello hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="46b9a-123">With a correctly configured VNet, you can continue toodeploy new servers into that VNet over and over with few, if any, changes required over hello life of hello VNet.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="46b9a-124">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="46b9a-124">Create hello resource group</span></span>

<span data-ttu-id="46b9a-125">Först skapar du en resurs grupp tooorganize allt som du skapar i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="46b9a-125">First, create a resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="46b9a-126">Mer information om resursgrupper finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="46b9a-126">For more information about resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-hello-vnet"></a><span data-ttu-id="46b9a-127">Skapa hello VNet</span><span class="sxs-lookup"><span data-stu-id="46b9a-127">Create hello VNet</span></span>

<span data-ttu-id="46b9a-128">hello första steget är toobuild ett VNet toolaunch hello virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="46b9a-128">hello first step is toobuild a VNet toolaunch hello VMs into.</span></span> <span data-ttu-id="46b9a-129">Hej VNet innehåller ett undernät för den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="46b9a-129">hello VNet contains one subnet for this walkthrough.</span></span> <span data-ttu-id="46b9a-130">Mer information om virtuella Azure-nätverk finns [skapa ett virtuellt nätverk med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)</span><span class="sxs-lookup"><span data-stu-id="46b9a-130">For more information on Azure VNets, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)</span></span>

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="46b9a-131">Skapa hello nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="46b9a-131">Create hello network security group</span></span>

<span data-ttu-id="46b9a-132">hello undernät bygger bakom en befintlig nätverkssäkerhetsgrupp så skapa hello nätverkssäkerhetsgruppen innan hello undernät.</span><span class="sxs-lookup"><span data-stu-id="46b9a-132">hello subnet is built behind an existing network security group so build hello network security group before hello subnet.</span></span> <span data-ttu-id="46b9a-133">Säkerhetsgrupper för Azure-nätverk är likvärdiga tooa brandväggen på hello nätverksnivån.</span><span class="sxs-lookup"><span data-stu-id="46b9a-133">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="46b9a-134">Läs mer på Azure nätverkssäkerhetsgrupper [hur toocreate nätverkssäkerhet grupper i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)</span><span class="sxs-lookup"><span data-stu-id="46b9a-134">For more information on Azure network security groups, see [How toocreate network security groups in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)</span></span>

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="46b9a-135">Lägg till regel för att tillåta en inkommande SSH</span><span class="sxs-lookup"><span data-stu-id="46b9a-135">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="46b9a-136">hello VM behöver åtkomst från hello internet så att en regel som tillåter inkommande port 22 trafik toobe har passerat hello nätverket tooport 22 på hello VM krävs.</span><span class="sxs-lookup"><span data-stu-id="46b9a-136">hello VM needs access from hello internet so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is needed.</span></span>

```azurecli
azure network nsg rule create inboundSSH \
    --resource-group myResourceGroup \
    --nsg-name myNSG \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range 22 \
    --destination-address-prefix 10.10.0.0/24 \
    --destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a><span data-ttu-id="46b9a-137">Lägg till ett undernät toohello VNet</span><span class="sxs-lookup"><span data-stu-id="46b9a-137">Add a subnet toohello VNet</span></span>

<span data-ttu-id="46b9a-138">Virtuella datorer i hello VNet måste finnas i ett undernät.</span><span class="sxs-lookup"><span data-stu-id="46b9a-138">VMs within hello VNet must be located in a subnet.</span></span> <span data-ttu-id="46b9a-139">Varje virtuellt nätverk kan ha flera undernät.</span><span class="sxs-lookup"><span data-stu-id="46b9a-139">Each VNet can have multiple subnets.</span></span> <span data-ttu-id="46b9a-140">Skapa hello undernät och associera med hello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="46b9a-140">Create hello subnet and associate with hello network security group.</span></span>

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

<span data-ttu-id="46b9a-141">hello undernät är nu läggs till i hello virtuella nätverk och som är associerade med hello nätverkssäkerhetsgruppen och regeln.</span><span class="sxs-lookup"><span data-stu-id="46b9a-141">hello Subnet is now added inside hello VNet and associated with hello network security group and rule.</span></span>


## <a name="add-a-vnic-toohello-subnet"></a><span data-ttu-id="46b9a-142">Lägg till ett virtuellt nätverkskort toohello undernät</span><span class="sxs-lookup"><span data-stu-id="46b9a-142">Add a VNic toohello subnet</span></span>

<span data-ttu-id="46b9a-143">Virtuella nätverkskort (VNics) är viktigt eftersom du kan återanvända dem genom att ansluta dem toodifferent virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="46b9a-143">Virtual network cards (VNics) are important as you can reuse them by connecting them toodifferent VMs.</span></span> <span data-ttu-id="46b9a-144">Den här metoden används för att hålla hello VNic som en statisk resurs hello virtuella datorer kan vara tillfälliga.</span><span class="sxs-lookup"><span data-stu-id="46b9a-144">This approach keeps hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="46b9a-145">Skapa ett virtuellt nätverkskort och koppla den till hello undernät som skapats i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="46b9a-145">Create a VNic and associate it with hello subnet created in hello previous step.</span></span>

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="46b9a-146">Distribuera hello VM till hello VNet och NSG</span><span class="sxs-lookup"><span data-stu-id="46b9a-146">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="46b9a-147">Nu har du ett VNet och undernät i det virtuella nätverket och en nätverkssäkerhetsgrupp fungerar tooprotect hello undernät genom att blockera all inkommande trafik utom port 22 för SSH.</span><span class="sxs-lookup"><span data-stu-id="46b9a-147">You now have a VNet and subnet inside that VNet, and a network security group acting tooprotect hello subnet by blocking all inbound traffic except port 22 for SSH.</span></span> <span data-ttu-id="46b9a-148">hello VM kan nu distribueras i det här befintliga nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="46b9a-148">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="46b9a-149">Med hjälp av hello Azure CLI och hello `azure vm create` kommandot hello Linux VM är distribuerade toohello befintliga Azure-resursgrupp, virtuella nätverk, undernät och virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="46b9a-149">Using hello Azure CLI, and hello `azure vm create` command, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span> <span data-ttu-id="46b9a-150">Mer information om hur du använder hello CLI toodeploy fullständig VM finns [skapa en fullständig Linux-miljö med hjälp av hello Azure CLI](create-cli-complete.md)</span><span class="sxs-lookup"><span data-stu-id="46b9a-150">For more information on using hello CLI toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md)</span></span>

```azurecli
azure vm create myVM \
    --resource-group myResourceGroup \
    --location eastus \
    --os-type linux \
    --image-urn Debian \
    --storage-account-name mystorageaccount \
    --admin-username myAdminUser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --vnet-name myVNet \
    --vnet-subnet-name mySubnet \
    --nic-name myVNic
```

<span data-ttu-id="46b9a-151">Med hjälp av hello flaggor CLI toocall befintliga resurser du instruera Azure toodeploy hello VM i hello befintliga nätverk.</span><span class="sxs-lookup"><span data-stu-id="46b9a-151">By using hello CLI flags toocall out existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="46b9a-152">När ett VNet och undernät har distribuerats, kan lämnas som statisk eller permanenta resurser i Azure-region.</span><span class="sxs-lookup"><span data-stu-id="46b9a-152">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="46b9a-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46b9a-153">Next steps</span></span>

* [<span data-ttu-id="46b9a-154">Använda en viss distribution för toocreate en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="46b9a-154">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* [<span data-ttu-id="46b9a-155">Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="46b9a-155">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md)
* [<span data-ttu-id="46b9a-156">Skapa en Linux-VM på Azure med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="46b9a-156">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
