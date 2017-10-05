---
title: "Expandera virtuella hårddiskar på en Linux-VM i Azure | Microsoft Docs"
description: "Lär dig hur du expandera virtuella hårddiskar på en Linux-VM med Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: b82cc0473c003da767ee230ab485c69b233977d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a><span data-ttu-id="95b7f-103">Så här expanderar virtuella hårddiskar på en Linux-VM med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="95b7f-103">How to expand virtual hard disks on a Linux VM with the Azure CLI</span></span>
<span data-ttu-id="95b7f-104">Standardstorleken för virtuell hårddisk för operativsystem (OS) är vanligtvis 30 GB på en Linux-dator (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="95b7f-104">The default virtual hard disk size for the operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="95b7f-105">Du kan [lägga till datadiskar](add-disk.md) att tillhandahålla för ytterligare lagringsutrymme, men du kan också expandera en befintlig datadisk.</span><span class="sxs-lookup"><span data-stu-id="95b7f-105">You can [add data disks](add-disk.md) to provide for additional storage space, but you may also wish to expand an existing data disk.</span></span> <span data-ttu-id="95b7f-106">Den här artikeln beskriver hur du expandera hanterade diskar för en Linux-VM med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="95b7f-106">This article details how to expand managed disks for a Linux VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="95b7f-107">Du kan också expandera ohanterade OS-disk med den [Azure CLI 1.0](expand-disks-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="95b7f-107">You can also expand the unmanaged OS disk with the [Azure CLI 1.0](expand-disks-nodejs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="95b7f-108">Kontrollera alltid att du säkerhetskopierar dina data innan du utför disk ändra storlek på åtgärder.</span><span class="sxs-lookup"><span data-stu-id="95b7f-108">Always make sure that you back up your data before you perform disk resize operations.</span></span> <span data-ttu-id="95b7f-109">Mer information finns i [säkerhetskopiera virtuella Linux-datorer i Azure](tutorial-backup-vms.md).</span><span class="sxs-lookup"><span data-stu-id="95b7f-109">For more information, see [Back up Linux VMs in Azure](tutorial-backup-vms.md).</span></span>

## <a name="expand-disk"></a><span data-ttu-id="95b7f-110">Expandera disk</span><span class="sxs-lookup"><span data-stu-id="95b7f-110">Expand disk</span></span>
<span data-ttu-id="95b7f-111">Se till att du har senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="95b7f-111">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="95b7f-112">Den här artikeln kräver en befintlig virtuell dator i Azure med minst en datadisk ansluten och förberetts.</span><span class="sxs-lookup"><span data-stu-id="95b7f-112">This article requires an existing VM in Azure with at least one data disk attached and prepared.</span></span> <span data-ttu-id="95b7f-113">Om du inte redan har en virtuell dator som du kan använda finns [skapa och förbereda en virtuell dator med datadiskar](tutorial-manage-disks.md#create-and-attach-disks).</span><span class="sxs-lookup"><span data-stu-id="95b7f-113">If you do not already have a VM that you can use, see [Create and prepare a VM with data disks](tutorial-manage-disks.md#create-and-attach-disks).</span></span>

<span data-ttu-id="95b7f-114">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="95b7f-114">In the following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="95b7f-115">Exempel parameternamn inkluderar *myResourceGroup* och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="95b7f-115">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

1. <span data-ttu-id="95b7f-116">Det går inte att utföra åtgärder på virtuella hårddiskar med den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="95b7f-116">Operations on virtual hard disks cannot be performed with the VM running.</span></span> <span data-ttu-id="95b7f-117">Frigör den virtuella datorn med [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="95b7f-117">Deallocate your VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="95b7f-118">I följande exempel tar bort den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="95b7f-118">The following example deallocates the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="95b7f-119">`az vm stop`Frigör inte beräkningsresurserna.</span><span class="sxs-lookup"><span data-stu-id="95b7f-119">`az vm stop` does not release the compute resources.</span></span> <span data-ttu-id="95b7f-120">Använd om du vill frigöra beräkningsresurser `az vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="95b7f-120">To release compute resources, use `az vm deallocate`.</span></span> <span data-ttu-id="95b7f-121">Den virtuella datorn måste frigöras för att expandera den virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="95b7f-121">The VM must be deallocated to expand the virtual hard disk.</span></span>

2. <span data-ttu-id="95b7f-122">Visa en lista över hanterade diskar i en resursgrupp med [az Disklista](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="95b7f-122">View a list of managed disks in a resource group with [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="95b7f-123">I följande exempel visas en lista över hanterade diskar i resursgruppen med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="95b7f-123">The following example displays a list of managed disks in the resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    <span data-ttu-id="95b7f-124">Expandera diskutrymme med [az disk uppdatering](/cli/azure/disk#update).</span><span class="sxs-lookup"><span data-stu-id="95b7f-124">Expand the required disk with [az disk update](/cli/azure/disk#update).</span></span> <span data-ttu-id="95b7f-125">I följande exempel expanderas hanterade disken med namnet *myDataDisk* ska *200*Gb i storlek:</span><span class="sxs-lookup"><span data-stu-id="95b7f-125">The following example expands the managed disk named *myDataDisk* to be *200*Gb in size:</span></span>

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > <span data-ttu-id="95b7f-126">När du expanderar en hanterad disk är uppdaterade storleken mappad till den närmaste hantera diskstorleken.</span><span class="sxs-lookup"><span data-stu-id="95b7f-126">When you expand a managed disk, the updated size is mapped to the nearest managed disk size.</span></span> <span data-ttu-id="95b7f-127">En tabell med tillgängliga hanterade diskstorlekar och nivåer finns [hanterade diskar översikt över Azure - priser och fakturering](../windows/managed-disks-overview.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="95b7f-127">For a table of the available managed disk sizes and tiers, see [Azure Managed Disks Overview - Pricing and Billing](../windows/managed-disks-overview.md#pricing-and-billing).</span></span>

3. <span data-ttu-id="95b7f-128">Starta den virtuella datorn med [az vm start](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="95b7f-128">Start your VM with [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="95b7f-129">Följande exempel startar den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="95b7f-129">The following example starts the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="95b7f-130">SSH till den virtuella datorn med rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="95b7f-130">SSH to your VM with the appropriate credentials.</span></span> <span data-ttu-id="95b7f-131">Du kan hämta den offentliga IP-adressen på den virtuella datorn med [az vm visa](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="95b7f-131">You can obtain the public IP address of your VM with [az vm show](/cli/azure/vm#show):</span></span>

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. <span data-ttu-id="95b7f-132">Om du vill använda den expanderade disken måste du expandera den underliggande partitionen och filsystem.</span><span class="sxs-lookup"><span data-stu-id="95b7f-132">To use the expanded disk, you need to expand the underlying partition and filesystem.</span></span>

    <span data-ttu-id="95b7f-133">a.</span><span class="sxs-lookup"><span data-stu-id="95b7f-133">a.</span></span> <span data-ttu-id="95b7f-134">Om du redan är monterad demontera disken:</span><span class="sxs-lookup"><span data-stu-id="95b7f-134">If already mounted, unmount the disk:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

    <span data-ttu-id="95b7f-135">b.</span><span class="sxs-lookup"><span data-stu-id="95b7f-135">b.</span></span> <span data-ttu-id="95b7f-136">Använd `parted` att visa information och ändra storlek på partitionen:</span><span class="sxs-lookup"><span data-stu-id="95b7f-136">Use `parted` to view disk information and resize the partition:</span></span>

    ```bash
    sudo parted /dev/sdc
    ```

    <span data-ttu-id="95b7f-137">Visa information om befintliga partitionslayout med `print`.</span><span class="sxs-lookup"><span data-stu-id="95b7f-137">View information about the existing partition layout with `print`.</span></span> <span data-ttu-id="95b7f-138">Utdata liknar följande exempel som visar den underliggande disken är 215Gb i storlek:</span><span class="sxs-lookup"><span data-stu-id="95b7f-138">The output is similar to the following example, which shows the underlying disk is 215Gb in size:</span></span>

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    <span data-ttu-id="95b7f-139">c.</span><span class="sxs-lookup"><span data-stu-id="95b7f-139">c.</span></span> <span data-ttu-id="95b7f-140">Expandera partitionen med `resizepart`.</span><span class="sxs-lookup"><span data-stu-id="95b7f-140">Expand the partition with `resizepart`.</span></span> <span data-ttu-id="95b7f-141">Ange partitionsnumret *1*, och en storlek för den nya partitionen:</span><span class="sxs-lookup"><span data-stu-id="95b7f-141">Enter the partition number, *1*, and a size for the new partition:</span></span>

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    <span data-ttu-id="95b7f-142">d.</span><span class="sxs-lookup"><span data-stu-id="95b7f-142">d.</span></span> <span data-ttu-id="95b7f-143">Om du vill avsluta, ange`quit`</span><span class="sxs-lookup"><span data-stu-id="95b7f-143">To exit, enter `quit`</span></span>

5. <span data-ttu-id="95b7f-144">Partitionen storlek, verifiera partition konsekvens med `e2fsck`:</span><span class="sxs-lookup"><span data-stu-id="95b7f-144">With the partition resized, verify the partition consistency with `e2fsck`:</span></span>

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. <span data-ttu-id="95b7f-145">Nu ändra storlek på filsystemet med `resize2fs`:</span><span class="sxs-lookup"><span data-stu-id="95b7f-145">Now resize the filesystem with `resize2fs`:</span></span>

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. <span data-ttu-id="95b7f-146">Montera partitionen till önskad plats som `/datadrive`:</span><span class="sxs-lookup"><span data-stu-id="95b7f-146">Mount the partition to the desired location, such as `/datadrive`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. <span data-ttu-id="95b7f-147">Använd för att kontrollera att OS-disken har ändrats, `df -h`.</span><span class="sxs-lookup"><span data-stu-id="95b7f-147">To verify the OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="95b7f-148">Följande exempel visas dataenheten, */dev/sdc1*, är nu 200 GB:</span><span class="sxs-lookup"><span data-stu-id="95b7f-148">The following example output shows the data drive, */dev/sdc1*, is now 200 GB:</span></span>

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a><span data-ttu-id="95b7f-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95b7f-149">Next steps</span></span>
<span data-ttu-id="95b7f-150">Om du behöver ytterligare lagringsutrymme du också [lägga till diskar till en Linux-VM](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="95b7f-150">If you need additional storage, you also [add data disks to a Linux VM](add-disk.md).</span></span> <span data-ttu-id="95b7f-151">Mer information om diskkryptering finns [kryptera diskar på en Linux VM som använder Azure CLI](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="95b7f-151">For more information about disk encryption, see [Encrypt disks on a Linux VM using the Azure CLI](encrypt-disks.md).</span></span>
