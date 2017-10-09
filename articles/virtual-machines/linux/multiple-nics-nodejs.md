---
title: "aaaCreate en Linux VM i Azure med flera nätverkskort | Microsoft Docs"
description: "Lär dig hur toocreate en Linux VM med flera nätverkskort anslutna tooit med hello Azure CLI eller Resource Manager-mallar."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 457dab734ceeeefd35cddaf1ebb9ea0a82f4e207
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="e3e2e-103">Skapa en virtuell Linux-dator med flera nätverkskort med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e3e2e-103">Create a Linux virtual machine with multiple NICs using hello Azure CLI 1.0</span></span>
<span data-ttu-id="e3e2e-104">Du kan skapa en virtuell dator (VM) i Azure som har flera virtuella nätverk gränssnitt (NIC) som är anslutna tooit.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached tooit.</span></span> <span data-ttu-id="e3e2e-105">Ett vanligt scenario är toohave olika undernät för frontend och backend-anslutning eller ett nätverk dedikerad tooa övervakning eller lösning för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="e3e2e-106">Den här artikeln innehåller snabb kommandon toocreate en virtuell dator med flera nätverkskort anslutna tooit.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-106">This article provides quick commands toocreate a VM with multiple NICs attached tooit.</span></span> <span data-ttu-id="e3e2e-107">För detaljerad information, inklusive hur toocreate flera nätverkskort i en egen Bash-skript, Läs mer om [distribution av flera nätverkskort VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e3e2e-107">For detailed information, including how toocreate multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="e3e2e-108">Olika [VM-storlekar](sizes.md) stöder olika antal nätverkskort, så därför storlek den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

> [!WARNING]
> <span data-ttu-id="e3e2e-109">När du skapar en virtuell dator – du kan inte lägga till nätverkskort tooan befintlig virtuell dator med hello Azure CLI 1.0 måste du koppla flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-109">You must attach multiple NICs when you create a VM - you cannot add NICs tooan existing VM with hello Azure CLI 1.0.</span></span> <span data-ttu-id="e3e2e-110">Du kan [lägga till nätverkskort tooan befintlig virtuell dator med hello Azure CLI 2.0](multiple-nics.md).</span><span class="sxs-lookup"><span data-stu-id="e3e2e-110">You can [add NICs tooan existing VM with hello Azure CLI 2.0](multiple-nics.md).</span></span> <span data-ttu-id="e3e2e-111">Du kan också [skapa en virtuell dator baserat på hello ursprungliga virtuella diskarna](copy-vm.md) och skapa flera nätverkskort som du distribuerar hello VM.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-111">You can also [create a VM based on hello original virtual disk(s)](copy-vm.md) and create multiple NICs as you deploy hello VM.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="e3e2e-112">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="e3e2e-112">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="e3e2e-113">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-113">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="e3e2e-114">[Azure CLI 1.0](#create-supporting-resources) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="e3e2e-114">[Azure CLI 1.0](#create-supporting-resources) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="e3e2e-115">[Azure CLI 2.0](multiple-nics.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="e3e2e-115">[Azure CLI 2.0](multiple-nics.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="e3e2e-116">Skapa stödresurser</span><span class="sxs-lookup"><span data-stu-id="e3e2e-116">Create supporting resources</span></span>
<span data-ttu-id="e3e2e-117">Se till att du har hello [Azure CLI](../../cli-install-nodejs.md) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-117">Make sure that you have hello [Azure CLI](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="e3e2e-118">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-118">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="e3e2e-119">Exempel parameternamn ingår *myResourceGroup*, *mittlagringskonto*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-119">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="e3e2e-120">Först skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-120">First, create a resource group.</span></span> <span data-ttu-id="e3e2e-121">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-121">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

<span data-ttu-id="e3e2e-122">Skapa en storage-konto toohold dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-122">Create a storage account toohold your VMs.</span></span> <span data-ttu-id="e3e2e-123">hello följande exempel skapas ett lagringskonto med namnet *mittlagringskonto*:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-123">hello following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

<span data-ttu-id="e3e2e-124">Skapa ett virtuellt nätverk tooconnect dina virtuella datorer till.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-124">Create a virtual network tooconnect your VMs to.</span></span> <span data-ttu-id="e3e2e-125">hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med en adressprefixet *192.168.0.0/16*:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-125">hello following example creates a virtual network named *myVnet* with an address prefix of *192.168.0.0/16*:</span></span>

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="e3e2e-126">Skapa två undernät för virtuellt nätverk – ett för frontend-trafik och en för backend-trafik.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-126">Create two virtual network subnets - one for front-end traffic and one for back-end traffic.</span></span> <span data-ttu-id="e3e2e-127">hello följande exempel skapar två undernät, med namnet *mySubnetFrontEnd* och *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-127">hello following example creates two subnets, named *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

```azurecli
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetFrontEnd \
    --address-prefix 192.168.1.0/24
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="e3e2e-128">Skapa och konfigurera flera nätverkskort</span><span class="sxs-lookup"><span data-stu-id="e3e2e-128">Create and configure multiple NICs</span></span>
<span data-ttu-id="e3e2e-129">Du kan läsa mer information om [distribution av flera nätverkskort med hello Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), inklusive skript hello processen att loopa via toocreate alla hello-nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-129">You can read more details about [deploying multiple NICs using hello Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), including scripting hello process of looping through toocreate all hello NICs.</span></span>

<span data-ttu-id="e3e2e-130">hello följande exempel skapar två nätverkskort som heter *myNic1* och *myNic2*, med ett nätverkskort som ansluter tooeach undernät:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-130">hello following example creates two NICs, named *myNic1* and *myNic2*, with one NIC connecting tooeach subnet:</span></span>

```azurecli
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic1 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetFrontEnd
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic2 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetBackEnd
```

<span data-ttu-id="e3e2e-131">Vanligtvis du också skapa en [Nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) eller [belastningsutjämnare](../../load-balancer/load-balancer-overview.md) toohelp hantera och distribuera trafik mellan dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-131">Typically you also create a [Network Security Group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) toohelp manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="e3e2e-132">hello följande exempel skapar en Nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-132">hello following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="e3e2e-133">Binda med dina nätverkskort toohello Nätverkssäkerhetsgruppen `azure network nic set`.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-133">Bind your NICs toohello Network Security Group using `azure network nic set`.</span></span> <span data-ttu-id="e3e2e-134">hello följande exempel Binder *myNic1* och *myNic2* med *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-134">hello following example binds *myNic1* and *myNic2* with *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-hello-nics"></a><span data-ttu-id="e3e2e-135">Skapa en virtuell dator och koppla hello nätverkskort</span><span class="sxs-lookup"><span data-stu-id="e3e2e-135">Create a VM and attach hello NICs</span></span>
<span data-ttu-id="e3e2e-136">När du skapar hello VM kan ange du nu flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-136">When creating hello VM, you now specify multiple NICs.</span></span> <span data-ttu-id="e3e2e-137">I stället använder `--nic-name` tooprovide ett enda nätverkskort i stället använder du `--nic-names` och ange en kommaavgränsad lista över nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-137">Rather using `--nic-name` tooprovide a single NIC, instead you use `--nic-names` and provide a comma-separated list of NICs.</span></span> <span data-ttu-id="e3e2e-138">Du måste också tootake försiktig när du väljer hello VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-138">You also need tootake care when you select hello VM size.</span></span> <span data-ttu-id="e3e2e-139">Det finns begränsningar för hello Totalt antal nätverkskort som du lägger till tooa VM.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-139">There are limits for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="e3e2e-140">Läs mer om [Linux VM-storlekar](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="e3e2e-140">Read more about [Linux VM sizes](sizes.md).</span></span> <span data-ttu-id="e3e2e-141">hello följande exempel visas hur toospecify flera nätverkskort och en virtuell dator storleken som stöds med hjälp av flera nätverkskort (*Standard_DS2_v2*):</span><span class="sxs-lookup"><span data-stu-id="e3e2e-141">hello following example shows how toospecify multiple NICs and then a VM size that supports using multiple NICs (*Standard_DS2_v2*):</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="e3e2e-142">Skapa flera nätverkskort med hjälp av Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="e3e2e-142">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="e3e2e-143">Azure Resource Manager-mallar använda deklarativa JSON filer toodefine din miljö.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-143">Azure Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="e3e2e-144">Du kan läsa ett [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e3e2e-144">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="e3e2e-145">Resource Manager-mallar ger ett sätt toocreate flera instanser av en resurs under distributionen, till exempel skapa flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-145">Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="e3e2e-146">Du använder *kopiera* toospecify hello antal instanser toocreate:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-146">You use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="e3e2e-147">Läs mer om [skapar flera instanser med *kopiera*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="e3e2e-147">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="e3e2e-148">Du kan också använda en `copyIndex()` toothen Lägg till ett nummer tooa resursnamn som gör att du toocreate `myNic1`, `myNic2`, etc. hello följande visar ett exempel på att lägga till hello indexvärde:</span><span class="sxs-lookup"><span data-stu-id="e3e2e-148">You can also use a `copyIndex()` toothen append a number tooa resource name, which allows you toocreate `myNic1`, `myNic2`, etc. hello following shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="e3e2e-149">Du kan läsa en komplett exempel på [skapar flera nätverkskort med hjälp av Resource Manager-mallar](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="e3e2e-149">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3e2e-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3e2e-150">Next steps</span></span>
<span data-ttu-id="e3e2e-151">Se till att tooreview [Linux VM-storlekar](sizes.md) vid toocreating en virtuell dator med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-151">Make sure tooreview [Linux VM sizes](sizes.md) when trying toocreating a VM with multiple NICs.</span></span> <span data-ttu-id="e3e2e-152">Betala uppmärksamhet toohello maximalt antal nätverkskort som har stöd för varje VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-152">Pay attention toohello maximum number of NICs each VM size supports.</span></span> 

<span data-ttu-id="e3e2e-153">Kom ihåg att du kan inte lägga till ytterligare nätverkskort tooan befintlig virtuell dator måste du skapa alla hello nätverkskort när du distribuerar hello VM.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-153">Remember that you cannot add additional NICs tooan existing VM, you must create all hello NICs when you deploy hello VM.</span></span> <span data-ttu-id="e3e2e-154">Vara försiktig när du planerar din distributioner toomake till att du har alla hello krävs nätverksanslutning från hello början.</span><span class="sxs-lookup"><span data-stu-id="e3e2e-154">Take care when planning your deployments toomake sure that you have all hello required network connectivity from hello outset.</span></span>

