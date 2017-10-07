---
title: "aaaCopy en Linux VM med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocreate en kopia av din virtuella Azure Linux-datorn med hjälp av Azure CLI 2.0 och hanterade diskar."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/10/2017
ms.author: cynthn
ms.openlocfilehash: ee34a4259dd0c1e7bf49312fe3fe3ba809bf69e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a><span data-ttu-id="3e8cb-103">Skapa en kopia av en Linux-VM med hjälp av Azure CLI 2.0 och hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="3e8cb-103">Create a copy of a Linux VM by using Azure CLI 2.0 and Managed Disks</span></span>


<span data-ttu-id="3e8cb-104">Den här artikeln visar hur toocreate en kopia av din Azure virtuell dator (VM kör Linux med hjälp av) hello Azure CLI 2.0 och hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="3e8cb-104">This article shows you how toocreate a copy of your Azure virtual machine (VM) running Linux using hello Azure CLI 2.0 and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="3e8cb-105">Du kan också utföra dessa steg med hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-105">You can also perform these steps with hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="3e8cb-106">Du kan också [överför och skapa en virtuell dator från en virtuell Hårddisk](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-106">You can also [upload and create a VM from a VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e8cb-107">Krav</span><span class="sxs-lookup"><span data-stu-id="3e8cb-107">Prerequisites</span></span>


-   <span data-ttu-id="3e8cb-108">Installera [Azure CLI 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="3e8cb-108">Install [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

-   <span data-ttu-id="3e8cb-109">Logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-109">Sign in tooan Azure account with [az login](/cli/azure/#login).</span></span>

-   <span data-ttu-id="3e8cb-110">Ha en Azure VM toouse som hello källa för din kopia.</span><span class="sxs-lookup"><span data-stu-id="3e8cb-110">Have an Azure VM toouse as hello source for your copy.</span></span>

## <a name="step-1-stop-hello-source-vm"></a><span data-ttu-id="3e8cb-111">Steg 1: Stoppa hello källa VM</span><span class="sxs-lookup"><span data-stu-id="3e8cb-111">Step 1: Stop hello source VM</span></span>


<span data-ttu-id="3e8cb-112">Frigöra Virtuella hello källdatorn med hjälp av [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-112">Deallocate hello source VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span>
<span data-ttu-id="3e8cb-113">hello följande exempel tar bort hello virtuella datorn med namnet **myVM** i hello resursgruppen **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="3e8cb-113">hello following example deallocates hello VM named **myVM** in hello resource group **myResourceGroup**:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-hello-source-vm"></a><span data-ttu-id="3e8cb-114">Steg 2: Hello kopieringskälla VM</span><span class="sxs-lookup"><span data-stu-id="3e8cb-114">Step 2: Copy hello source VM</span></span>


<span data-ttu-id="3e8cb-115">toocopy en virtuell dator, skapa en kopia av hello underliggande virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="3e8cb-115">toocopy a VM, you create a copy of hello underlying virtual hard disk.</span></span> <span data-ttu-id="3e8cb-116">Den här processen skapar en särskild virtuell Hårddisk som en hanterad Disk som innehåller hello inställningar som hello datakällan VM.</span><span class="sxs-lookup"><span data-stu-id="3e8cb-116">This process creates a specialized VHD as a Managed Disk that contains hello same configuration and settings as hello source VM.</span></span>

<span data-ttu-id="3e8cb-117">Mer information om Azure Managed Disks finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-117">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span> 

1.  <span data-ttu-id="3e8cb-118">Visa en lista över varje virtuell dator och hello namnet på dess OS-disken med [az vm listan](/cli/azure/vm#list).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-118">List each VM and hello name of its OS disk with [az vm list](/cli/azure/vm#list).</span></span> <span data-ttu-id="3e8cb-119">hello följande exempel visar en lista över alla virtuella datorer i resursgruppen med namnet **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="3e8cb-119">hello following example lists all VMs in the resource group named **myResourceGroup**:</span></span>
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    <span data-ttu-id="3e8cb-120">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3e8cb-120">hello output is similar toohello following example:</span></span>

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  <span data-ttu-id="3e8cb-121">Kopiera hello disk genom att skapa en ny hanteras disk med hjälp av [az disk skapa](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-121">Copy hello disk by creating a new managed disk using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="3e8cb-122">hello följande exempel skapas en disk med namnet **myCopiedDisk** från hello hanteras disk med namnet **myDisk**:</span><span class="sxs-lookup"><span data-stu-id="3e8cb-122">hello following example creates a disk named **myCopiedDisk** from hello managed disk named **myDisk**:</span></span>

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  <span data-ttu-id="3e8cb-123">Kontrollera diskar hello hanteras nu i resursgruppen med hjälp av [az Disklista](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-123">Verify hello managed disks now in your resource group by using [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="3e8cb-124">hello följande exempel visar hello hanterade diskar i hello resursgrupp med namnet **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="3e8cb-124">hello following example lists hello managed disks in hello resource group named **myResourceGroup**:</span></span>

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  <span data-ttu-id="3e8cb-125">Hoppa över för[”steg 3: konfigurera ett virtuellt nätverk”](#step-3-set-up-a-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-125">Skip too["Step 3: Set up a virtual network"](#step-3-set-up-a-virtual-network).</span></span>


## <a name="step-3-set-up-a-virtual-network"></a><span data-ttu-id="3e8cb-126">Steg 3: Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="3e8cb-126">Step 3: Set up a virtual network</span></span>


<span data-ttu-id="3e8cb-127">hello skapa följande valfria steg ett nytt virtuellt nätverk, undernät, offentliga IP-adressen och virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-127">hello following optional steps create a new virtual network, subnet, public IP address, and virtual network interface card (NIC).</span></span>

<span data-ttu-id="3e8cb-128">Om du kopierar en virtuell dator för att felsöka ändamål eller ytterligare distributioner kan du inte vill toouse en virtuell dator i ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="3e8cb-128">If you are copying a VM for troubleshooting purposes or additional deployments, you might not want toouse a VM in an existing virtual network.</span></span>

<span data-ttu-id="3e8cb-129">Om du vill toocreate infrastruktur för ett virtuellt nätverk för de kopierade virtuella datorerna, hello Följ nästa steg.</span><span class="sxs-lookup"><span data-stu-id="3e8cb-129">If you want toocreate a virtual network infrastructure for your copied VMs, follow hello next few steps.</span></span> <span data-ttu-id="3e8cb-130">Om du inte vill toocreate ett virtuellt nätverk kan du hoppa över för[steg 4: skapa en virtuell dator](#step-4-create-a-vm).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-130">If you don't want toocreate a virtual network, skip too[Step 4: Create a VM](#step-4-create-a-vm).</span></span>

1.  <span data-ttu-id="3e8cb-131">Skapa hello virtuellt nätverk med hjälp av [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-131">Create hello virtual network by using [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="3e8cb-132">hello följande exempel skapas ett virtuellt nätverk med namnet **myVnet** och ett undernät med namnet **mySubnet**:</span><span class="sxs-lookup"><span data-stu-id="3e8cb-132">hello following example creates a virtual network named **myVnet** and a subnet named **mySubnet**:</span></span>

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  <span data-ttu-id="3e8cb-133">Skapa en offentlig IP-adress med hjälp av [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-133">Create a public IP by using [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="3e8cb-134">hello följande exempel skapas en offentlig IP-adress med namnet **myPublicIP** med hello DNS-namnet för **mypublicdns**.</span><span class="sxs-lookup"><span data-stu-id="3e8cb-134">hello following example creates a public IP named **myPublicIP** with hello DNS name of **mypublicdns**.</span></span> <span data-ttu-id="3e8cb-135">(hello DNS-namn måste vara unikt, så ange ett unikt namn)</span><span class="sxs-lookup"><span data-stu-id="3e8cb-135">(hello DNS name must be unique, so provide a unique name.)</span></span>

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  <span data-ttu-id="3e8cb-136">Skapa hello nätverkskortet använder [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-136">Create hello NIC using [az network nic create](/cli/azure/network/nic#create).</span></span>
    <span data-ttu-id="3e8cb-137">hello följande exempel skapas ett nätverkskort med namnet **myNic** som bifogade toothe **mySubnet** undernät:</span><span class="sxs-lookup"><span data-stu-id="3e8cb-137">hello following example creates a NIC named **myNic** that's attached toothe **mySubnet** subnet:</span></span>

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a><span data-ttu-id="3e8cb-138">Steg 4: Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3e8cb-138">Step 4: Create a VM</span></span>

<span data-ttu-id="3e8cb-139">Nu kan du skapa en virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-139">You can now create a VM by using [az vm create](/cli/azure/vm#create).</span></span>

<span data-ttu-id="3e8cb-140">Ange hello kopieras hanterade diskar toouse som hello OS-disk (--bifoga-os-disk), enligt följande:</span><span class="sxs-lookup"><span data-stu-id="3e8cb-140">Specify hello copied managed disk toouse as hello OS disk (--attach-os-disk), as follows:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a><span data-ttu-id="3e8cb-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e8cb-141">Next steps</span></span>

<span data-ttu-id="3e8cb-142">toolearn hur toouse Azure CLI toomanage din nya VM finns [Azure CLI-kommandona för hello Azure Resource Manager](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="3e8cb-142">toolearn how toouse Azure CLI toomanage your new VM, see [Azure CLI commands for hello Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>
