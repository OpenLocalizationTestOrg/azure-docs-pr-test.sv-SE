---
title: "Kopiera en Linux VM med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur du skapar en kopia av den virtuella datorn Linux Azure med Azure CLI 2.0 och hanterade diskar."
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
ms.openlocfilehash: 7983061a933370803669480296d7625106e1360c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a><span data-ttu-id="00e8e-103">Skapa en kopia av en Linux-VM med hjälp av Azure CLI 2.0 och hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="00e8e-103">Create a copy of a Linux VM by using Azure CLI 2.0 and Managed Disks</span></span>


<span data-ttu-id="00e8e-104">Den här artikeln visar hur du skapar en kopia av din Azure virtuell dator (VM kör Linux med hjälp av Azure CLI 2.0 och Azure Resource Manager-distributionsmodellen).</span><span class="sxs-lookup"><span data-stu-id="00e8e-104">This article shows you how to create a copy of your Azure virtual machine (VM) running Linux using the Azure CLI 2.0 and the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="00e8e-105">Du kan också utföra dessa steg med [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="00e8e-105">You can also perform these steps with the [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="00e8e-106">Du kan också [överför och skapa en virtuell dator från en virtuell Hårddisk](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="00e8e-106">You can also [upload and create a VM from a VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00e8e-107">Krav</span><span class="sxs-lookup"><span data-stu-id="00e8e-107">Prerequisites</span></span>


-   <span data-ttu-id="00e8e-108">Installera [Azure CLI 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="00e8e-108">Install [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

-   <span data-ttu-id="00e8e-109">Logga in på ett Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="00e8e-109">Sign in to an Azure account with [az login](/cli/azure/#login).</span></span>

-   <span data-ttu-id="00e8e-110">Ha en Azure VM ska användas som källa för din kopia.</span><span class="sxs-lookup"><span data-stu-id="00e8e-110">Have an Azure VM to use as the source for your copy.</span></span>

## <a name="step-1-stop-the-source-vm"></a><span data-ttu-id="00e8e-111">Steg 1: Stoppa den Virtuella källdatorn</span><span class="sxs-lookup"><span data-stu-id="00e8e-111">Step 1: Stop the source VM</span></span>


<span data-ttu-id="00e8e-112">Frigör den Virtuella källdatorn med hjälp av [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="00e8e-112">Deallocate the source VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span>
<span data-ttu-id="00e8e-113">I följande exempel tar bort den virtuella datorn med namnet **myVM** i resursgruppen **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="00e8e-113">The following example deallocates the VM named **myVM** in the resource group **myResourceGroup**:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-the-source-vm"></a><span data-ttu-id="00e8e-114">Steg 2: Kopiera den Virtuella källdatorn</span><span class="sxs-lookup"><span data-stu-id="00e8e-114">Step 2: Copy the source VM</span></span>


<span data-ttu-id="00e8e-115">Om du vill kopiera en virtuell dator kan du skapa en kopia av den underliggande virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="00e8e-115">To copy a VM, you create a copy of the underlying virtual hard disk.</span></span> <span data-ttu-id="00e8e-116">Den här processen skapar en särskild virtuell Hårddisk som en hanterad Disk som innehåller samma konfiguration och inställningar som den Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="00e8e-116">This process creates a specialized VHD as a Managed Disk that contains the same configuration and settings as the source VM.</span></span>

<span data-ttu-id="00e8e-117">Mer information om Azure Managed Disks finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="00e8e-117">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span> 

1.  <span data-ttu-id="00e8e-118">Lista över varje virtuell dator och namnet på dess OS-disken med [az vm listan](/cli/azure/vm#list).</span><span class="sxs-lookup"><span data-stu-id="00e8e-118">List each VM and the name of its OS disk with [az vm list](/cli/azure/vm#list).</span></span> <span data-ttu-id="00e8e-119">I följande exempel visar en lista över alla virtuella datorer i resursgruppen med namnet **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="00e8e-119">The following example lists all VMs in the resource group named **myResourceGroup**:</span></span>
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    <span data-ttu-id="00e8e-120">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="00e8e-120">The output is similar to the following example:</span></span>

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  <span data-ttu-id="00e8e-121">Kopiera disken genom att skapa en ny hanteras disk med hjälp av [az disk skapa](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="00e8e-121">Copy the disk by creating a new managed disk using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="00e8e-122">I följande exempel skapas en disk med namnet **myCopiedDisk** från hanterade disken med namnet **myDisk**:</span><span class="sxs-lookup"><span data-stu-id="00e8e-122">The following example creates a disk named **myCopiedDisk** from the managed disk named **myDisk**:</span></span>

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  <span data-ttu-id="00e8e-123">Kontrollera hanterade diskar nu i resursgruppen med hjälp av [az Disklista](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="00e8e-123">Verify the managed disks now in your resource group by using [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="00e8e-124">I följande exempel visar de hantera diskarna i resursgruppen med namnet **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="00e8e-124">The following example lists the managed disks in the resource group named **myResourceGroup**:</span></span>

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  <span data-ttu-id="00e8e-125">Gå vidare till [”steg 3: konfigurera ett virtuellt nätverk”](#step-3-set-up-a-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="00e8e-125">Skip to ["Step 3: Set up a virtual network"](#step-3-set-up-a-virtual-network).</span></span>


## <a name="step-3-set-up-a-virtual-network"></a><span data-ttu-id="00e8e-126">Steg 3: Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="00e8e-126">Step 3: Set up a virtual network</span></span>


<span data-ttu-id="00e8e-127">Följande valfria steg skapar du ett nytt virtuellt nätverk, undernät, offentliga IP-adressen och virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="00e8e-127">The following optional steps create a new virtual network, subnet, public IP address, and virtual network interface card (NIC).</span></span>

<span data-ttu-id="00e8e-128">Om du kopierar en virtuell dator för att felsöka ändamål eller ytterligare distributioner kan du inte vill använda en virtuell dator i ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="00e8e-128">If you are copying a VM for troubleshooting purposes or additional deployments, you might not want to use a VM in an existing virtual network.</span></span>

<span data-ttu-id="00e8e-129">Om du vill skapa en virtuell nätverksinfrastruktur för de kopierade virtuella datorerna, följer du följande steg.</span><span class="sxs-lookup"><span data-stu-id="00e8e-129">If you want to create a virtual network infrastructure for your copied VMs, follow the next few steps.</span></span> <span data-ttu-id="00e8e-130">Om du inte vill skapa ett virtuellt nätverk går du till [steg 4: skapa en virtuell dator](#step-4-create-a-vm).</span><span class="sxs-lookup"><span data-stu-id="00e8e-130">If you don't want to create a virtual network, skip to [Step 4: Create a VM](#step-4-create-a-vm).</span></span>

1.  <span data-ttu-id="00e8e-131">Skapa det virtuella nätverket med hjälp av [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="00e8e-131">Create the virtual network by using [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="00e8e-132">I följande exempel skapas ett virtuellt nätverk med namnet **myVnet** och ett undernät med namnet **mySubnet**:</span><span class="sxs-lookup"><span data-stu-id="00e8e-132">The following example creates a virtual network named **myVnet** and a subnet named **mySubnet**:</span></span>

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  <span data-ttu-id="00e8e-133">Skapa en offentlig IP-adress med hjälp av [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="00e8e-133">Create a public IP by using [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="00e8e-134">I följande exempel skapas en offentlig IP-adress med namnet **myPublicIP** med DNS-namnet på **mypublicdns**.</span><span class="sxs-lookup"><span data-stu-id="00e8e-134">The following example creates a public IP named **myPublicIP** with the DNS name of **mypublicdns**.</span></span> <span data-ttu-id="00e8e-135">(DNS-namnet måste vara unikt, så ange ett unikt namn)</span><span class="sxs-lookup"><span data-stu-id="00e8e-135">(The DNS name must be unique, so provide a unique name.)</span></span>

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  <span data-ttu-id="00e8e-136">Skapa nätverkskortet använder [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="00e8e-136">Create the NIC using [az network nic create](/cli/azure/network/nic#create).</span></span>
    <span data-ttu-id="00e8e-137">I följande exempel skapas ett nätverkskort med namnet **myNic** som är kopplad till den **mySubnet** undernät:</span><span class="sxs-lookup"><span data-stu-id="00e8e-137">The following example creates a NIC named **myNic** that's attached to the **mySubnet** subnet:</span></span>

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a><span data-ttu-id="00e8e-138">Steg 4: Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="00e8e-138">Step 4: Create a VM</span></span>

<span data-ttu-id="00e8e-139">Nu kan du skapa en virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="00e8e-139">You can now create a VM by using [az vm create](/cli/azure/vm#create).</span></span>

<span data-ttu-id="00e8e-140">Ange den kopierade hantera disken ska användas som OS-disken (--bifoga-os-disk), enligt följande:</span><span class="sxs-lookup"><span data-stu-id="00e8e-140">Specify the copied managed disk to use as the OS disk (--attach-os-disk), as follows:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a><span data-ttu-id="00e8e-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00e8e-141">Next steps</span></span>

<span data-ttu-id="00e8e-142">Information om hur du använder Azure CLI för att hantera den nya virtuella datorn finns [Azure CLI-kommandona för Azure Resource Manager](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="00e8e-142">To learn how to use Azure CLI to manage your new VM, see [Azure CLI commands for the Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>
