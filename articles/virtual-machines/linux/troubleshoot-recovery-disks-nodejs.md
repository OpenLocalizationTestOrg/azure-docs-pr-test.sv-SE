---
title: "Använda en Linux felsökning av virtuell dator med Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du felsöker problem med Linux VM genom att ansluta OS-disken till återställning av en virtuell dator med hjälp av Azure CLI 1.0"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: d817358211f123c96d899c5cff88cc47aeb5c9c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-cli-10"></a><span data-ttu-id="b4c5a-103">Felsöka en Linux VM genom att koppla OS-disken till återställning av en virtuell dator med hjälp av Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b4c5a-103">Troubleshoot a Linux VM by attaching the OS disk to a recovery VM using the Azure CLI 1.0</span></span>
<span data-ttu-id="b4c5a-104">Om Linux-dator (VM) påträffar ett fel vid start- eller disk, kan du behöva utför felsökning på den virtuella hårddisken sig själv.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="b4c5a-105">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab` som förhindrar att den virtuella datorn kan starta korrekt.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-105">A common example would be an invalid entry in `/etc/fstab` that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="b4c5a-106">Den här artikeln beskriver hur du använder Azure CLI 1.0 för att ansluta den virtuella hårddisken till en annan Linux VM att åtgärda eventuella fel och återskapa den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-106">This article details how to use the Azure CLI 1.0 to connect your virtual hard disk to another Linux VM to fix any errors, then re-create your original VM.</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="b4c5a-107">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="b4c5a-107">CLI versions to complete the task</span></span>
<span data-ttu-id="b4c5a-108">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-108">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="b4c5a-109">[Azure CLI 1.0](#recovery-process-overview) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="b4c5a-109">[Azure CLI 1.0](#recovery-process-overview) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b4c5a-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="b4c5a-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="b4c5a-111">Översikt över återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="b4c5a-111">Recovery process overview</span></span>
<span data-ttu-id="b4c5a-112">Så här ser felsökningsprocessen ut:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-112">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="b4c5a-113">Ta bort den virtuella datorn får problem, hålla de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-113">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="b4c5a-114">Anslut och montera den virtuella hårddisken till en annan Linux VM i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-114">Attach and mount the virtual hard disk to another Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="b4c5a-115">Anslut till den virtuella felsökningsdatorn.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-115">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="b4c5a-116">Redigera filer eller köra några verktyg för att åtgärda problem på den ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-116">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="b4c5a-117">Demontera och koppla från den virtuella hårddisken från den virtuella felsökningsdatorn.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-117">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="b4c5a-118">Skapa en virtuell dator med hjälp av den ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-118">Create a VM using the original virtual hard disk.</span></span>

<span data-ttu-id="b4c5a-119">Se till att du har [senaste Azure CLI 1.0](../../cli-install-nodejs.md) installerad och loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-119">Make sure that you have [the latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="b4c5a-120">Ersätt parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-120">In the following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="b4c5a-121">Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-121">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="b4c5a-122">Fastställa startproblem</span><span class="sxs-lookup"><span data-stu-id="b4c5a-122">Determine boot issues</span></span>
<span data-ttu-id="b4c5a-123">Granska seriella utdata för att avgöra varför den virtuella datorn kan inte starta korrekt.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-123">Examine the serial output to determine why your VM is not able to boot correctly.</span></span> <span data-ttu-id="b4c5a-124">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab`, eller den underliggande virtuella hårddisken som tagits bort eller flyttats.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-124">A common example is an invalid entry in `/etc/fstab`, or the underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="b4c5a-125">I följande exempel hämtas seriella utdata från den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-125">The following example gets the serial output from the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b4c5a-126">Granska seriella utdata för att avgöra varför den virtuella datorn inte kan starta.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-126">Review the serial output to determine why the VM is failing to boot.</span></span> <span data-ttu-id="b4c5a-127">Om seriella utdata är inte något indikerar måste du kan behöva Granska loggfilerna i `/var/log` när du har den virtuella hårddisken är ansluten till en felsökning virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-127">If the serial output isn't providing any indication, you may need to review log files in `/var/log` once you have the virtual hard disk connected to a troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="b4c5a-128">Visa information om befintlig virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="b4c5a-128">View existing virtual hard disk details</span></span>
<span data-ttu-id="b4c5a-129">Innan du kan koppla den virtuella hårddisken till en annan virtuell dator, måste du identifiera namnet på den virtuella hårddisken (VHD).</span><span class="sxs-lookup"><span data-stu-id="b4c5a-129">Before you can attach your virtual hard disk to another VM, you need to identify the name of the virtual hard disk (VHD).</span></span> 

<span data-ttu-id="b4c5a-130">I följande exempel hämtar information för den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-130">The following example gets information for the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b4c5a-131">Leta efter `Vhd URI` i utdata från kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-131">Look for `Vhd URI` in the output from the preceding command.</span></span> <span data-ttu-id="b4c5a-132">Följande trunkeras exempel utdata visar de `Vhd URI` på den sista raden:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-132">The following truncated example output shows the `Vhd URI` on the last line:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up the VM "myVM"
+ Looking up the NIC "myNic"
+ Looking up the public ip "myPublicIP"
...
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :myVM
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
```


## <a name="delete-existing-vm"></a><span data-ttu-id="b4c5a-133">Ta bort en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b4c5a-133">Delete existing VM</span></span>
<span data-ttu-id="b4c5a-134">Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-134">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="b4c5a-135">En virtuell hårddisk är där själva operativsystemet, program och konfigurationer lagras.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-135">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="b4c5a-136">Virtuellt datorn är enbart metadata som definierar storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="b4c5a-136">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="b4c5a-137">Varje virtuell hårddisk har tilldelats när ansluten till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-137">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="b4c5a-138">Datadiskar kan anslutas och kopplas från när den virtuella datorn körs, men operativsystemdisken kan inte kopplas från om inte den virtuella datorresursen tagits bort.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-138">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="b4c5a-139">Lånet fortsätter att koppla OS-disken till en virtuell dator även om den virtuella datorn är i tillståndet stoppad och frigjord.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-139">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="b4c5a-140">Det första steget att återställa den virtuella datorn är att ta bort den Virtuella datorresursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-140">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="b4c5a-141">När du tar bort den virtuella datorn hamnar de virtuella hårddiskarna på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-141">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="b4c5a-142">När den virtuella datorn har tagits bort, kopplar du den virtuella hårddisken till en annan virtuell dator för att felsöka och lösa problemen.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-142">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="b4c5a-143">I följande exempel tar bort den virtuella datorn med namnet `myVM` från resursgruppen med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-143">The following example deletes the VM named `myVM` from the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="b4c5a-144">Vänta tills den virtuella datorn har tagits bort innan du kopplar den virtuella hårddisken till en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-144">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="b4c5a-145">Lånet på den virtuella hårddisken som associeras med den virtuella datorn måste släppas innan du kan koppla den virtuella hårddisken till en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-145">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="b4c5a-146">Koppla en befintlig virtuell hårddisk till en annan virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b4c5a-146">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="b4c5a-147">För nästa steg använder du en annan virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-147">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="b4c5a-148">Du bifoga en befintlig virtuell hårddisk för den här felsökningsinformationen VM att bläddra och redigera dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-148">You attach the existing virtual hard disk to this troubleshooting VM to browse and edit the disk's content.</span></span> <span data-ttu-id="b4c5a-149">Den här processen kan du korrigera eventuella fel i programkonfigurationen eller granska ytterligare program eller system loggfiler, till exempel.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-149">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="b4c5a-150">Välj eller skapa en annan virtuell dator ska användas för felsökning.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-150">Choose or create another VM to use for troubleshooting purposes.</span></span>

<span data-ttu-id="b4c5a-151">När du ansluter en befintlig virtuell hårddisk, ange URL till den disk som hämtades i föregående `azure vm show` kommando.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-151">When you attach the existing virtual hard disk, specify the URL to the disk obtained in the preceding `azure vm show` command.</span></span> <span data-ttu-id="b4c5a-152">I följande exempel bifogar en befintlig virtuell hårddisk till den Virtuella datorn med namnet felsökning `myVMRecovery` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-152">The following example attaches an existing virtual hard disk to the troubleshooting VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="b4c5a-153">Bifogade data disken</span><span class="sxs-lookup"><span data-stu-id="b4c5a-153">Mount the attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="b4c5a-154">I följande exempel innehåller information om de steg som krävs på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-154">The following examples detail the steps required on an Ubuntu VM.</span></span> <span data-ttu-id="b4c5a-155">Om du använder en annan Linux distro, till exempel Red Hat Enterprise Linux eller SUSE, loggen filplatser och `mount` kommandon kan vara lite annorlunda.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-155">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, the log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="b4c5a-156">Finns i dokumentationen för din specifika distro för ändringarna i kommandona.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-156">Refer to the documentation for your specific distro for the appropriate changes in commands.</span></span>

1. <span data-ttu-id="b4c5a-157">SSH till den felsökning virtuella datorn med rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-157">SSH to your troubleshooting VM using the appropriate credentials.</span></span> <span data-ttu-id="b4c5a-158">Om den här disken är den första datadisk som är kopplade till den virtuella datorn med felsökning, disken sannolikt är ansluten till `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-158">If this disk is the first data disk attached to your troubleshooting VM, the disk is likely connected to `/dev/sdc`.</span></span> <span data-ttu-id="b4c5a-159">Använd `dmseg` att visa anslutna diskar:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-159">Use `dmseg` to view attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="b4c5a-160">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-160">The output is similar to the following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="b4c5a-161">I föregående exempel är OS-disken på `/dev/sda` och tillfällig disketten för varje virtuell dator är på `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-161">In the preceding example, the OS disk is at `/dev/sda` and the temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="b4c5a-162">Om du har flera datadiskar, bör de visas på `/dev/sdd`, `/dev/sde`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-162">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="b4c5a-163">Skapa en katalog om du vill montera en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-163">Create a directory to mount your existing virtual hard disk.</span></span> <span data-ttu-id="b4c5a-164">I följande exempel skapas en katalog med namnet `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-164">The following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="b4c5a-165">Om du har flera partitioner i en befintlig virtuell hårddisk kan du montera partitionen som krävs.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-165">If you have multiple partitions on your existing virtual hard disk, mount the required partition.</span></span> <span data-ttu-id="b4c5a-166">I följande exempel monterar den första primära partitionen på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-166">The following example mounts the first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="b4c5a-167">Det är bra att montera hårddiskar på virtuella datorer i Azure med hjälp av universellt Unik identifierare (UUID) för den virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-167">Best practice is to mount data disks on VMs in Azure using the universally unique identifier (UUID) of the virtual hard disk.</span></span> <span data-ttu-id="b4c5a-168">Den här korta felsökning scenariot är montera den virtuella hårddisken med hjälp av UUID inte nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-168">For this short troubleshooting scenario, mounting the virtual hard disk using the UUID is not necessary.</span></span> <span data-ttu-id="b4c5a-169">Men vid normal användning, redigering `/etc/fstab` för att montera den virtuella hårddiskar med enhetens namn i stället för UUID kanske den virtuella datorn inte startar.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-169">However, under normal use, editing `/etc/fstab` to mount virtual hard disks using device name rather than UUID may cause the VM to fail to boot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="b4c5a-170">Åtgärda problemen på den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="b4c5a-170">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="b4c5a-171">Med den befintliga virtuella hårddisken monteras, kan du nu utföra eventuella underhåll och felsökning efter behov.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-171">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="b4c5a-172">När du har åtgärdat problemen fortsätter du med följande steg.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-172">Once you have addressed the issues, continue with the following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="b4c5a-173">Demontera och koppla från den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="b4c5a-173">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="b4c5a-174">När din felen är löst kan du demontera och koppla från den befintliga virtuella hårddisken från den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-174">Once your errors are resolved, you unmount and detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="b4c5a-175">Du kan inte använda din virtuella hårddisk med andra Virtuella förrän lånet bifoga den virtuella hårddisken till Virtuellt datorn felsökning släpps.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-175">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="b4c5a-176">Demontera den befintliga virtuella hårddisken från SSH-session till den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-176">From the SSH session to your troubleshooting VM, unmount the existing virtual hard disk.</span></span> <span data-ttu-id="b4c5a-177">Ändra utanför den överordnade katalogen för din monteringspunkt:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-177">Change out of the parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="b4c5a-178">Nu demontera befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-178">Now unmount the existing virtual hard disk.</span></span> <span data-ttu-id="b4c5a-179">I följande exempel demonterar enheten på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-179">The following example unmounts the device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="b4c5a-180">Nu ska du koppla från den virtuella hårddisken från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-180">Now detach the virtual hard disk from the VM.</span></span> <span data-ttu-id="b4c5a-181">Avsluta SSH-session till den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-181">Exit the SSH session to your troubleshooting VM.</span></span> <span data-ttu-id="b4c5a-182">I Azure CLI, först visa bifogade datadiskar till den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-182">In the Azure CLI, first list the attached data disks to your troubleshooting VM.</span></span> <span data-ttu-id="b4c5a-183">I följande exempel visar datadiskar som är kopplade till den virtuella datorn med namnet `myVMRecovery` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-183">The following example lists the data disks attached to the VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    <span data-ttu-id="b4c5a-184">Observera den `Lun` värde för en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-184">Note the `Lun` value for your existing virtual hard disk.</span></span> <span data-ttu-id="b4c5a-185">Följande visas exempel kommandot de befintliga virtuella disk som är ansluten på LUN 0:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-185">The following example command output shows the existing virtual disk attached at LUN 0:</span></span>

    ```azurecli
    info:    Executing command vm disk list
    + Looking up the VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    <span data-ttu-id="b4c5a-186">Koppla från datadisk från den virtuella datorn med hjälp av den tillämpliga `Lun` värde:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-186">Detach the data disk from your VM using the applicable `Lun` value:</span></span>

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="b4c5a-187">Skapa virtuell dator från den ursprungliga hårddisken</span><span class="sxs-lookup"><span data-stu-id="b4c5a-187">Create VM from original hard disk</span></span>
<span data-ttu-id="b4c5a-188">Så här skapar du en virtuell dator från den ursprungliga virtuella hårddisken [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="b4c5a-188">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="b4c5a-189">Den faktiska JSON-mallen finns på följande länk:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-189">The actual JSON template is at the following link:</span></span>

- <span data-ttu-id="b4c5a-190">https://Raw.githubusercontent.com/Azure/Azure-Quickstart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b4c5a-190">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="b4c5a-191">Mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av VHD-Webbadressen från tidigare kommandot.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-191">The template deploys a VM into an existing virtual network, using the VHD URL from the earlier command.</span></span> <span data-ttu-id="b4c5a-192">I följande exempel distribuerar mallen till resursgruppen med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-192">The following example deploys the template to the resource group named `myResourceGroup`:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

<span data-ttu-id="b4c5a-193">Besvara anvisningarna för mallen som namn på virtuell dator (`myDeployedVM` i följande exempel), OS-typen (`Linux`), och VM-storlek (`Standard_DS1_v2`).</span><span class="sxs-lookup"><span data-stu-id="b4c5a-193">Answer the prompts for the template such as VM name (`myDeployedVM` the following example), OS type (`Linux`), and VM size (`Standard_DS1_v2`).</span></span> <span data-ttu-id="b4c5a-194">Den `osDiskVhdUri` är samma som tidigare använt när kopplar befintlig virtuell hårddisk till Virtuellt datorn felsökning.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-194">The `osDiskVhdUri` is the same as previously used when attaching the existing virtual hard disk to the troubleshooting VM.</span></span> <span data-ttu-id="b4c5a-195">Ett exempel på utdata från kommandot och anvisningarna är följande:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-195">An example of the command output and prompts is as follows:</span></span>

```azurecli
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName:  myDeployedVM
osType:  Linux
osDiskVhdUri:  https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
vmSize:  Standard_DS1_v2
existingVirtualNetworkName:  myVnet
existingVirtualNetworkResourceGroup:  myResourceGroup
subnetName:  mySubnet
dnsNameForPublicIP:  mypublicipdeployed
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "mydeployment"
+ Waiting for deployment to complete
+
```


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="b4c5a-196">Återaktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="b4c5a-196">Re-enable boot diagnostics</span></span>

<span data-ttu-id="b4c5a-197">När du skapar den virtuella datorn från den befintliga virtuella hårddisken får startdiagnostikinställningar inte automatiskt aktiveras.</span><span class="sxs-lookup"><span data-stu-id="b4c5a-197">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="b4c5a-198">I följande exempel aktiveras diagnostiska tillägget på den virtuella datorn med namnet `myDeployedVM` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b4c5a-198">The following example enables the diagnostic extension on the VM named `myDeployedVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="b4c5a-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b4c5a-199">Next steps</span></span>
<span data-ttu-id="b4c5a-200">Om du har problem med anslutningen till den virtuella datorn finns [felsökning av SSH-anslutningar till en Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b4c5a-200">If you are having issues connecting to your VM, see [Troubleshoot SSH connections to an Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="b4c5a-201">Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b4c5a-201">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>