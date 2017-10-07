---
title: "aaaExpand virtuella hårddiskar på en Linux-VM i Azure | Microsoft Docs"
description: "Lär dig hur tooexpand virtuella hårddiskar på en Linux-VM med hello Azure CLI 2.0"
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
ms.openlocfilehash: 7c09a682cb4322c027e57667640e8f1f8e6612f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooexpand-virtual-hard-disks-on-a-linux-vm-with-hello-azure-cli"></a><span data-ttu-id="0ed76-103">Hur tooexpand virtuella hårddiskar på en Linux-VM med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0ed76-103">How tooexpand virtual hard disks on a Linux VM with hello Azure CLI</span></span>
<span data-ttu-id="0ed76-104">hello virtuell hårddisk standardstorlek för hello operativsystem (OS) är vanligtvis 30 GB på en Linux-dator (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="0ed76-104">hello default virtual hard disk size for hello operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="0ed76-105">Du kan [lägga till datadiskar](add-disk.md) tooprovide för ytterligare lagringsutrymme, men du kan också tooexpand en befintlig datadisk.</span><span class="sxs-lookup"><span data-stu-id="0ed76-105">You can [add data disks](add-disk.md) tooprovide for additional storage space, but you may also wish tooexpand an existing data disk.</span></span> <span data-ttu-id="0ed76-106">Den här artikeln beskrivs hur tooexpand hanterade diskar för en Linux-VM med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="0ed76-106">This article details how tooexpand managed disks for a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="0ed76-107">Du kan också expandera hello ohanterad OS-disken med hello [Azure CLI 1.0](expand-disks-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="0ed76-107">You can also expand hello unmanaged OS disk with hello [Azure CLI 1.0](expand-disks-nodejs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="0ed76-108">Kontrollera alltid att du säkerhetskopierar dina data innan du utför disk ändra storlek på åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0ed76-108">Always make sure that you back up your data before you perform disk resize operations.</span></span> <span data-ttu-id="0ed76-109">Mer information finns i [säkerhetskopiera virtuella Linux-datorer i Azure](tutorial-backup-vms.md).</span><span class="sxs-lookup"><span data-stu-id="0ed76-109">For more information, see [Back up Linux VMs in Azure](tutorial-backup-vms.md).</span></span>

## <a name="expand-disk"></a><span data-ttu-id="0ed76-110">Expandera disk</span><span class="sxs-lookup"><span data-stu-id="0ed76-110">Expand disk</span></span>
<span data-ttu-id="0ed76-111">Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="0ed76-111">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="0ed76-112">Den här artikeln kräver en befintlig virtuell dator i Azure med minst en datadisk ansluten och förberetts.</span><span class="sxs-lookup"><span data-stu-id="0ed76-112">This article requires an existing VM in Azure with at least one data disk attached and prepared.</span></span> <span data-ttu-id="0ed76-113">Om du inte redan har en virtuell dator som du kan använda finns [skapa och förbereda en virtuell dator med datadiskar](tutorial-manage-disks.md#create-and-attach-disks).</span><span class="sxs-lookup"><span data-stu-id="0ed76-113">If you do not already have a VM that you can use, see [Create and prepare a VM with data disks](tutorial-manage-disks.md#create-and-attach-disks).</span></span>

<span data-ttu-id="0ed76-114">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="0ed76-114">In hello following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="0ed76-115">Exempel parameternamn inkluderar *myResourceGroup* och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="0ed76-115">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

1. <span data-ttu-id="0ed76-116">Går inte att utföra åtgärder på virtuella hårddiskar med hello VM körs.</span><span class="sxs-lookup"><span data-stu-id="0ed76-116">Operations on virtual hard disks cannot be performed with hello VM running.</span></span> <span data-ttu-id="0ed76-117">Frigör den virtuella datorn med [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="0ed76-117">Deallocate your VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="0ed76-118">hello följande exempel tar bort hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="0ed76-118">hello following example deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="0ed76-119">`az vm stop`Frigör inte hello beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="0ed76-119">`az vm stop` does not release hello compute resources.</span></span> <span data-ttu-id="0ed76-120">toorelease beräkningsresurser använder `az vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="0ed76-120">toorelease compute resources, use `az vm deallocate`.</span></span> <span data-ttu-id="0ed76-121">hello VM att frigöra tooexpand hello virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="0ed76-121">hello VM must be deallocated tooexpand hello virtual hard disk.</span></span>

2. <span data-ttu-id="0ed76-122">Visa en lista över hanterade diskar i en resursgrupp med [az Disklista](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="0ed76-122">View a list of managed disks in a resource group with [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="0ed76-123">hello följande exempel visar en lista över hanterade diskar i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="0ed76-123">hello following example displays a list of managed disks in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    <span data-ttu-id="0ed76-124">Expandera hello diskutrymme med [az disk uppdatering](/cli/azure/disk#update).</span><span class="sxs-lookup"><span data-stu-id="0ed76-124">Expand hello required disk with [az disk update](/cli/azure/disk#update).</span></span> <span data-ttu-id="0ed76-125">hello följande exempel expanderas hello hanterade disk med namnet *myDataDisk* toobe *200*Gb i storlek:</span><span class="sxs-lookup"><span data-stu-id="0ed76-125">hello following example expands hello managed disk named *myDataDisk* toobe *200*Gb in size:</span></span>

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > <span data-ttu-id="0ed76-126">När du expanderar en hanterad disk är hello uppdateras storleken mappade toohello närmsta hanterade diskstorleken.</span><span class="sxs-lookup"><span data-stu-id="0ed76-126">When you expand a managed disk, hello updated size is mapped toohello nearest managed disk size.</span></span> <span data-ttu-id="0ed76-127">En tabell med hello tillgängliga hanterade diskstorlekar och nivåer finns [hanterade diskar översikt över Azure - priser och fakturering](../windows/managed-disks-overview.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="0ed76-127">For a table of hello available managed disk sizes and tiers, see [Azure Managed Disks Overview - Pricing and Billing](../windows/managed-disks-overview.md#pricing-and-billing).</span></span>

3. <span data-ttu-id="0ed76-128">Starta den virtuella datorn med [az vm start](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="0ed76-128">Start your VM with [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="0ed76-129">följande exempel startar hello hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="0ed76-129">hello following example starts hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="0ed76-130">SSH tooyour VM med hello rätt behörighet.</span><span class="sxs-lookup"><span data-stu-id="0ed76-130">SSH tooyour VM with hello appropriate credentials.</span></span> <span data-ttu-id="0ed76-131">Du kan hämta hello offentliga IP-adressen för den virtuella datorn med [az vm visa](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="0ed76-131">You can obtain hello public IP address of your VM with [az vm show](/cli/azure/vm#show):</span></span>

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. <span data-ttu-id="0ed76-132">toouse hello expanderas disk måste du tooexpand hello underliggande partition och filsystem.</span><span class="sxs-lookup"><span data-stu-id="0ed76-132">toouse hello expanded disk, you need tooexpand hello underlying partition and filesystem.</span></span>

    <span data-ttu-id="0ed76-133">a.</span><span class="sxs-lookup"><span data-stu-id="0ed76-133">a.</span></span> <span data-ttu-id="0ed76-134">Om du redan är monterad demontera hello disk:</span><span class="sxs-lookup"><span data-stu-id="0ed76-134">If already mounted, unmount hello disk:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

    <span data-ttu-id="0ed76-135">b.</span><span class="sxs-lookup"><span data-stu-id="0ed76-135">b.</span></span> <span data-ttu-id="0ed76-136">Använd `parted` tooview disk information och ändra storlek på hello-partition:</span><span class="sxs-lookup"><span data-stu-id="0ed76-136">Use `parted` tooview disk information and resize hello partition:</span></span>

    ```bash
    sudo parted /dev/sdc
    ```

    <span data-ttu-id="0ed76-137">Visa information om hello befintliga partitionslayout med `print`.</span><span class="sxs-lookup"><span data-stu-id="0ed76-137">View information about hello existing partition layout with `print`.</span></span> <span data-ttu-id="0ed76-138">hello utdata är liknande toohello följande exempel som visar hello underliggande disk är 215Gb i storlek:</span><span class="sxs-lookup"><span data-stu-id="0ed76-138">hello output is similar toohello following example, which shows hello underlying disk is 215Gb in size:</span></span>

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome tooGNU Parted! Type 'help' tooview a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    <span data-ttu-id="0ed76-139">c.</span><span class="sxs-lookup"><span data-stu-id="0ed76-139">c.</span></span> <span data-ttu-id="0ed76-140">Expandera hello partitionen med `resizepart`.</span><span class="sxs-lookup"><span data-stu-id="0ed76-140">Expand hello partition with `resizepart`.</span></span> <span data-ttu-id="0ed76-141">Ange hello partitionsnumret *1*, och en storlek för hello ny partition:</span><span class="sxs-lookup"><span data-stu-id="0ed76-141">Enter hello partition number, *1*, and a size for hello new partition:</span></span>

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    <span data-ttu-id="0ed76-142">d.</span><span class="sxs-lookup"><span data-stu-id="0ed76-142">d.</span></span> <span data-ttu-id="0ed76-143">tooexit, ange`quit`</span><span class="sxs-lookup"><span data-stu-id="0ed76-143">tooexit, enter `quit`</span></span>

5. <span data-ttu-id="0ed76-144">Hello partition storlek, verifiera hello partition konsekvens med `e2fsck`:</span><span class="sxs-lookup"><span data-stu-id="0ed76-144">With hello partition resized, verify hello partition consistency with `e2fsck`:</span></span>

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. <span data-ttu-id="0ed76-145">Nu ändra storlek på hello filsystem med `resize2fs`:</span><span class="sxs-lookup"><span data-stu-id="0ed76-145">Now resize hello filesystem with `resize2fs`:</span></span>

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. <span data-ttu-id="0ed76-146">Montera hello partition toohello önskad plats, t.ex `/datadrive`:</span><span class="sxs-lookup"><span data-stu-id="0ed76-146">Mount hello partition toohello desired location, such as `/datadrive`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. <span data-ttu-id="0ed76-147">storleken har tooverify hello OS-disk, Använd `df -h`.</span><span class="sxs-lookup"><span data-stu-id="0ed76-147">tooverify hello OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="0ed76-148">hello följande exempel på utdata som visar hello dataenhet */dev/sdc1*, är nu 200 GB:</span><span class="sxs-lookup"><span data-stu-id="0ed76-148">hello following example output shows hello data drive, */dev/sdc1*, is now 200 GB:</span></span>

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a><span data-ttu-id="0ed76-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ed76-149">Next steps</span></span>
<span data-ttu-id="0ed76-150">Om du behöver ytterligare lagringsutrymme du också [lägga till data diskar tooa Linux VM](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="0ed76-150">If you need additional storage, you also [add data disks tooa Linux VM](add-disk.md).</span></span> <span data-ttu-id="0ed76-151">Mer information om diskkryptering finns [kryptera diskar på en Linux VM som använder hello Azure CLI](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="0ed76-151">For more information about disk encryption, see [Encrypt disks on a Linux VM using hello Azure CLI](encrypt-disks.md).</span></span>
