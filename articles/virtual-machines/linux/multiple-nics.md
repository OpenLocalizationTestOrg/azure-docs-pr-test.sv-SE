---
title: "aaaCreate en Linux VM i Azure med flera nätverkskort | Microsoft Docs"
description: "Lär dig hur toocreate en Linux VM med flera nätverkskort anslutna tooit med hello Azure CLI 2.0 eller Resource Manager-mallar."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 2723405914777a5dce4354d4f5d8413e357f58e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a><span data-ttu-id="9bbe8-103">Hur toocreate en Linux-dator i Azure med flera nätverkskort</span><span class="sxs-lookup"><span data-stu-id="9bbe8-103">How toocreate a Linux virtual machine in Azure with multiple network interface cards</span></span>
<span data-ttu-id="9bbe8-104">Du kan skapa en virtuell dator (VM) i Azure som har flera virtuella nätverk gränssnitt (NIC) som är anslutna tooit.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached tooit.</span></span> <span data-ttu-id="9bbe8-105">Ett vanligt scenario är toohave olika undernät för frontend och backend-anslutning eller ett nätverk dedikerad tooa övervakning eller lösning för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="9bbe8-106">Den här artikeln beskrivs hur toocreate en virtuell dator med flera nätverkskort anslutna tooit och tooadd eller ta bort nätverkskort från en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-106">This article details how toocreate a VM with multiple NICs attached tooit and how tooadd or remove NICs from an existing VM.</span></span> <span data-ttu-id="9bbe8-107">För detaljerad information, inklusive hur toocreate flera nätverkskort i en egen Bash-skript, Läs mer om [distribution av flera nätverkskort VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-107">For detailed information, including how toocreate multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="9bbe8-108">Olika [VM-storlekar](sizes.md) stöder olika antal nätverkskort, så därför storlek den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="9bbe8-109">Den här artikeln beskrivs hur toocreate en virtuell dator med flera nätverkskort med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-109">This article details how toocreate a VM with multiple NICs with hello Azure CLI 2.0.</span></span> <span data-ttu-id="9bbe8-110">Du kan också utföra dessa steg med hello [Azure CLI 1.0](multiple-nics-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-110">You can also perform these steps with hello [Azure CLI 1.0](multiple-nics-nodejs.md).</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="9bbe8-111">Skapa stödresurser</span><span class="sxs-lookup"><span data-stu-id="9bbe8-111">Create supporting resources</span></span>
<span data-ttu-id="9bbe8-112">Installera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-112">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="9bbe8-113">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="9bbe8-114">Exempel parameternamn ingår *myResourceGroup*, *mittlagringskonto*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-114">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="9bbe8-115">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-115">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="9bbe8-116">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="9bbe8-117">Skapa hello virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-117">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="9bbe8-118">hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* och undernät med namnet *mySubnetFrontEnd*:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-118">hello following example creates a virtual network named *myVnet* and subnet named *mySubnetFrontEnd*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="9bbe8-119">Skapa ett undernät för hello backend-trafik med [az undernät för virtuellt nätverk skapa](/cli/azure/network/vnet/subnet#create).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-119">Create a subnet for hello back-end traffic with [az network vnet subnet create](/cli/azure/network/vnet/subnet#create).</span></span> <span data-ttu-id="9bbe8-120">hello följande exempel skapas ett undernät med namnet *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-120">hello following example creates a subnet named *mySubnetBackEnd*:</span></span>

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

<span data-ttu-id="9bbe8-121">Skapa en nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-121">Create a network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="9bbe8-122">hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-122">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="9bbe8-123">Skapa och konfigurera flera nätverkskort</span><span class="sxs-lookup"><span data-stu-id="9bbe8-123">Create and configure multiple NICs</span></span>
<span data-ttu-id="9bbe8-124">Skapa två nätverkskort med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-124">Create two NICs with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="9bbe8-125">hello följande exempel skapar två nätverkskort som heter *myNic1* och *myNic2*, ansluten hello säkerhetsgrupp för nätverk, med ett nätverkskort som ansluter tooeach undernät:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-125">hello following example creates two NICs, named *myNic1* and *myNic2*, connected hello network security group, with one NIC connecting tooeach subnet:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-hello-nics"></a><span data-ttu-id="9bbe8-126">Skapa en virtuell dator och koppla hello nätverkskort</span><span class="sxs-lookup"><span data-stu-id="9bbe8-126">Create a VM and attach hello NICs</span></span>
<span data-ttu-id="9bbe8-127">När du skapar hello VM, ange hello nätverkskort du skapade med `--nics`.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-127">When you create hello VM, specify hello NICs you created with `--nics`.</span></span> <span data-ttu-id="9bbe8-128">Du måste också tootake försiktig när du väljer hello VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-128">You also need tootake care when you select hello VM size.</span></span> <span data-ttu-id="9bbe8-129">Det finns begränsningar för hello Totalt antal nätverkskort som du lägger till tooa VM.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-129">There are limits for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="9bbe8-130">Läs mer om [Linux VM-storlekar](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-130">Read more about [Linux VM sizes](sizes.md).</span></span> 

<span data-ttu-id="9bbe8-131">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-131">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="9bbe8-132">hello följande exempel skapas en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-132">hello following example creates a VM named *myVM*:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

## <a name="add-a-nic-tooa-vm"></a><span data-ttu-id="9bbe8-133">Lägg till NIC-tooa VM</span><span class="sxs-lookup"><span data-stu-id="9bbe8-133">Add a NIC tooa VM</span></span>
<span data-ttu-id="9bbe8-134">hello föregående steg skapas en virtuell dator med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-134">hello previous steps created a VM with multiple NICs.</span></span> <span data-ttu-id="9bbe8-135">Du kan också lägga till nätverkskort tooan befintlig virtuell dator med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-135">You can also add NICs tooan existing VM with hello Azure CLI 2.0.</span></span> 

<span data-ttu-id="9bbe8-136">Skapa ett annat nätverkskort med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-136">Create another NIC with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="9bbe8-137">hello följande exempel skapas ett nätverkskort med namnet *myNic3* anslutna toohello backend-undernät och nätverkssäkerhetsgruppen skapade i föregående steg i hello:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-137">hello following example creates a NIC named *myNic3* connected toohello back-end subnet and network security group created in hello previous steps:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="9bbe8-138">tooadd ett NIC-tooan befintliga VM först frigöra hello virtuell dator med [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-138">tooadd a NIC tooan existing VM, first deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="9bbe8-139">hello följande exempel tar bort hello virtuella datorn med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-139">hello following example deallocates hello VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="9bbe8-140">Lägg till hello nätverkskortet med [az vm nic lägga till](/cli/azure/vm/nic#add).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-140">Add hello NIC with [az vm nic add](/cli/azure/vm/nic#add).</span></span> <span data-ttu-id="9bbe8-141">hello följande exempel läggs *myNic3* för*myVM*:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-141">hello following example adds *myNic3* too*myVM*:</span></span>

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

<span data-ttu-id="9bbe8-142">Starta virtuell dator med hello [az vm start](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="9bbe8-142">Start hello VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a><span data-ttu-id="9bbe8-143">Ta bort ett nätverkskort från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9bbe8-143">Remove a NIC from a VM</span></span>
<span data-ttu-id="9bbe8-144">tooremove ett nätverkskort från en befintlig virtuell dator först frigöra hello VM med [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-144">tooremove a NIC from an existing VM, first deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="9bbe8-145">hello följande exempel tar bort hello virtuella datorn med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-145">hello following example deallocates hello VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="9bbe8-146">Ta bort hello nätverkskortet med [az vm nic ta bort](/cli/azure/vm/nic#remove).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-146">Remove hello NIC with [az vm nic remove](/cli/azure/vm/nic#remove).</span></span> <span data-ttu-id="9bbe8-147">hello följande exempel tar bort *myNic3* från *myVM*:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-147">hello following example removes *myNic3* from *myVM*:</span></span>

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

<span data-ttu-id="9bbe8-148">Starta virtuell dator med hello [az vm start](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="9bbe8-148">Start hello VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="9bbe8-149">Skapa flera nätverkskort med hjälp av Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="9bbe8-149">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="9bbe8-150">Azure Resource Manager-mallar använda deklarativa JSON filer toodefine din miljö.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-150">Azure Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="9bbe8-151">Du kan läsa ett [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-151">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="9bbe8-152">Resource Manager-mallar ger ett sätt toocreate flera instanser av en resurs under distributionen, till exempel skapa flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-152">Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="9bbe8-153">Du använder *kopiera* toospecify hello antal instanser toocreate:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-153">You use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="9bbe8-154">Läs mer om [skapar flera instanser med *kopiera*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-154">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="9bbe8-155">Du kan också använda en `copyIndex()` toothen Lägg till ett nummer tooa resursnamn som gör att du toocreate `myNic1`, `myNic2`, etc. hello följande visar ett exempel på att lägga till hello indexvärde:</span><span class="sxs-lookup"><span data-stu-id="9bbe8-155">You can also use a `copyIndex()` toothen append a number tooa resource name, which allows you toocreate `myNic1`, `myNic2`, etc. hello following shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="9bbe8-156">Du kan läsa en komplett exempel på [skapar flera nätverkskort med hjälp av Resource Manager-mallar](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="9bbe8-156">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9bbe8-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9bbe8-157">Next steps</span></span>
<span data-ttu-id="9bbe8-158">Granska [Linux VM-storlekar](sizes.md) vid toocreating en virtuell dator med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-158">Review [Linux VM sizes](sizes.md) when trying toocreating a VM with multiple NICs.</span></span> <span data-ttu-id="9bbe8-159">Betala uppmärksamhet toohello maximalt antal nätverkskort som har stöd för varje VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="9bbe8-159">Pay attention toohello maximum number of NICs each VM size supports.</span></span> 
