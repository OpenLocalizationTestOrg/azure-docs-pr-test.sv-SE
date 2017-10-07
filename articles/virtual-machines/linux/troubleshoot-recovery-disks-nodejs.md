---
title: "aaaUse en Linux felsöka VM med hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Linux VM problem med anslutning hello OS disk tooa recovery VM hello Azure CLI 1.0"
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
ms.openlocfilehash: 398f681d1149299d444fcfdab20737315db02855
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-cli-10"></a><span data-ttu-id="b55d5-103">Felsöka en Linux VM genom att koppla hello OS tooa diskåterställning VM med hjälp av hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b55d5-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="b55d5-104">Om Linux-dator (VM) påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv.</span><span class="sxs-lookup"><span data-stu-id="b55d5-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="b55d5-105">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab` som förhindrar hello VM kan tooboot har.</span><span class="sxs-lookup"><span data-stu-id="b55d5-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="b55d5-106">Det här artikeln beskriver hur toouse hello Azure CLI 1.0 tooconnect din virtuella hårddisk disk tooanother Linux VM toofix eventuella fel och sedan återskapa den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b55d5-106">This article details how toouse hello Azure CLI 1.0 tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="b55d5-107">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="b55d5-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="b55d5-108">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="b55d5-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="b55d5-109">[Azure CLI 1.0](#recovery-process-overview) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="b55d5-109">[Azure CLI 1.0](#recovery-process-overview) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b55d5-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="b55d5-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="b55d5-111">Översikt över återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="b55d5-111">Recovery process overview</span></span>
<span data-ttu-id="b55d5-112">hello felsökningsprocessen är följande:</span><span class="sxs-lookup"><span data-stu-id="b55d5-112">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="b55d5-113">Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="b55d5-113">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="b55d5-114">Anslut och montera hello virtuell hårddisk tooanother Linux VM i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="b55d5-114">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="b55d5-115">Ansluta toohello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-115">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="b55d5-116">Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="b55d5-116">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="b55d5-117">Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-117">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="b55d5-118">Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="b55d5-118">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="b55d5-119">Se till att du har [hello senaste Azure CLI 1.0](../../cli-install-nodejs.md) installerad och loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="b55d5-119">Make sure that you have [hello latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="b55d5-120">Följande exempel Ersätt parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="b55d5-120">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="b55d5-121">Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="b55d5-121">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="b55d5-122">Fastställa startproblem</span><span class="sxs-lookup"><span data-stu-id="b55d5-122">Determine boot issues</span></span>
<span data-ttu-id="b55d5-123">Granska hello seriella utdata toodetermine till den virtuella datorn inte är kan tooboot korrekt.</span><span class="sxs-lookup"><span data-stu-id="b55d5-123">Examine hello serial output toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="b55d5-124">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab`, eller hello underliggande virtuell hårddisk som tagits bort eller flyttats.</span><span class="sxs-lookup"><span data-stu-id="b55d5-124">A common example is an invalid entry in `/etc/fstab`, or hello underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="b55d5-125">hello följande exempel hämtas hello seriella utdata från hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-125">hello following example gets hello serial output from hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b55d5-126">Granska hello seriella utdata toodetermine varför hello VM inte tooboot.</span><span class="sxs-lookup"><span data-stu-id="b55d5-126">Review hello serial output toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="b55d5-127">Om hello seriella utdata är inte något indikerar måste du kanske måste tooreview loggfiler i `/var/log` när du har hello virtuell hårddisk ansluten tooa felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-127">If hello serial output isn't providing any indication, you may need tooreview log files in `/var/log` once you have hello virtual hard disk connected tooa troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="b55d5-128">Visa information om befintlig virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="b55d5-128">View existing virtual hard disk details</span></span>
<span data-ttu-id="b55d5-129">Innan du kan koppla en virtuell hårddisk tooanother VM, måste tooidentify hello namnet på hello virtuell hårddisk (VHD).</span><span class="sxs-lookup"><span data-stu-id="b55d5-129">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span> 

<span data-ttu-id="b55d5-130">hello följande exempel hämtar information för hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-130">hello following example gets information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b55d5-131">Leta efter `Vhd URI` i hello utdata från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="b55d5-131">Look for `Vhd URI` in hello output from hello preceding command.</span></span> <span data-ttu-id="b55d5-132">hello följande trunkerat exempel visas hello `Vhd URI` på hello sista raden:</span><span class="sxs-lookup"><span data-stu-id="b55d5-132">hello following truncated example output shows hello `Vhd URI` on hello last line:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up hello VM "myVM"
+ Looking up hello NIC "myNic"
+ Looking up hello public ip "myPublicIP"
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


## <a name="delete-existing-vm"></a><span data-ttu-id="b55d5-133">Ta bort en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b55d5-133">Delete existing VM</span></span>
<span data-ttu-id="b55d5-134">Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="b55d5-134">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="b55d5-135">En virtuell hårddisk är där hello själva operativsystemet, program och konfigurationer lagras.</span><span class="sxs-lookup"><span data-stu-id="b55d5-135">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="b55d5-136">hello Virtuella datorn är enbart metadata som definierar hello storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="b55d5-136">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="b55d5-137">Varje virtuell hårddisk har tilldelats när kopplade tooa VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-137">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="b55d5-138">Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort.</span><span class="sxs-lookup"><span data-stu-id="b55d5-138">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="b55d5-139">hello lån fortsätter tooassociate hello OS-disk med en virtuell dator, även om den virtuella datorn är i tillståndet stoppad och frigjord.</span><span class="sxs-lookup"><span data-stu-id="b55d5-139">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="b55d5-140">Hej första steg toorecover den virtuella datorn är toodelete hello Virtuella datorresursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="b55d5-140">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="b55d5-141">Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b55d5-141">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="b55d5-142">Efter hello VM tas bort, bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel.</span><span class="sxs-lookup"><span data-stu-id="b55d5-142">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="b55d5-143">följande exempel tar bort hello hello virtuella datorn med namnet `myVM` från hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-143">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="b55d5-144">Vänta tills hello VM har tagits bort innan du kopplar hello virtuell hårddisk tooanother VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-144">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="b55d5-145">hello lånet på hello virtuell hårddisk som associeras med hello VM måste släppas innan du kan koppla hello virtuell hårddisk tooanother VM toobe.</span><span class="sxs-lookup"><span data-stu-id="b55d5-145">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="b55d5-146">Bifoga en befintlig virtuell hårddisk tooanother VM</span><span class="sxs-lookup"><span data-stu-id="b55d5-146">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="b55d5-147">Hej bredvid för några steg kan du använda en annan virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="b55d5-147">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="b55d5-148">Du bifogar hello befintlig virtuell hårddisk toothis felsökning VM toobrowse och redigera hello disk innehåll.</span><span class="sxs-lookup"><span data-stu-id="b55d5-148">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="b55d5-149">Den här processen kan toocorrect eventuella fel i programkonfigurationen eller granska ytterligare program eller loggfiler, till exempel.</span><span class="sxs-lookup"><span data-stu-id="b55d5-149">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="b55d5-150">Välj eller skapa en annan VM toouse för felsökning.</span><span class="sxs-lookup"><span data-stu-id="b55d5-150">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="b55d5-151">När du ansluter hello befintlig virtuell hårddisk, ange hello URL toohello disk hämtades i föregående hello `azure vm show` kommando.</span><span class="sxs-lookup"><span data-stu-id="b55d5-151">When you attach hello existing virtual hard disk, specify hello URL toohello disk obtained in hello preceding `azure vm show` command.</span></span> <span data-ttu-id="b55d5-152">hello följande exempel bifogar en befintlig virtuell hårddisk toohello felsökning virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-152">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="b55d5-153">Montera hello bifogade datadisk</span><span class="sxs-lookup"><span data-stu-id="b55d5-153">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="b55d5-154">hello följande exempel innehåller information om hello steg som krävs på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-154">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="b55d5-155">Om du använder en annan Linux distro, till exempel Red Hat Enterprise Linux eller SUSE, hello loggfilens platser och `mount` kommandon kan vara lite annorlunda.</span><span class="sxs-lookup"><span data-stu-id="b55d5-155">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="b55d5-156">Läs toohello dokumentationen för din specifika distro för hello ändringarna i kommandona.</span><span class="sxs-lookup"><span data-stu-id="b55d5-156">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="b55d5-157">SSH tooyour felsökning VM som använder hello rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b55d5-157">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="b55d5-158">Om den här disken är hello första data disk ansluten tooyour felsökning VM, hello disk sannolikt ansluten för`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="b55d5-158">If this disk is hello first data disk attached tooyour troubleshooting VM, hello disk is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="b55d5-159">Använd `dmseg` tooview anslutna diskar:</span><span class="sxs-lookup"><span data-stu-id="b55d5-159">Use `dmseg` tooview attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="b55d5-160">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b55d5-160">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="b55d5-161">I föregående exempel hello, hello OS-disken är på `/dev/sda` och hello diskutrymme för varje virtuell dator är på `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="b55d5-161">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="b55d5-162">Om du har flera datadiskar, bör de visas på `/dev/sdd`, `/dev/sde`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="b55d5-162">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="b55d5-163">Skapa en katalog toomount en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b55d5-163">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="b55d5-164">hello följande exempel skapas en katalog med namnet `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-164">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="b55d5-165">Om du har flera partitioner i en befintlig virtuell hårddisk montera hello krävs partition.</span><span class="sxs-lookup"><span data-stu-id="b55d5-165">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="b55d5-166">hello följande exempel monterar hello första primära partition på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-166">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="b55d5-167">Det är bra toomount datadiskar på virtuella datorer i Azure med hjälp av hello universellt Unik identifierare (UUID) för hello virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b55d5-167">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="b55d5-168">Den här korta felsökning scenariot behövs inte montering hello virtuell hårddisk med hello UUID.</span><span class="sxs-lookup"><span data-stu-id="b55d5-168">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="b55d5-169">Men vid normal användning, redigering `/etc/fstab` toomount virtuella hårddiskar med enhetsnamn snarare än UUID kan orsaka hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="b55d5-169">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="b55d5-170">Åtgärda problemen på den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="b55d5-170">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="b55d5-171">Med hello befintlig virtuell hårddisk monterad, kan du nu utföra eventuella underhåll och felsökning efter behov.</span><span class="sxs-lookup"><span data-stu-id="b55d5-171">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="b55d5-172">När du har åtgärdat hello problem, fortsätter du med hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="b55d5-172">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="b55d5-173">Demontera och koppla från den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="b55d5-173">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="b55d5-174">När din fel har åtgärdats, demontera och koppla hello befintlig virtuell hårddisk från den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="b55d5-174">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="b55d5-175">Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.</span><span class="sxs-lookup"><span data-stu-id="b55d5-175">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="b55d5-176">Demontera från hello SSH-session tooyour felsökning VM, hello befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b55d5-176">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="b55d5-177">Ändra utanför hello överordnade katalogen för din monteringspunkt:</span><span class="sxs-lookup"><span data-stu-id="b55d5-177">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="b55d5-178">Nu demontera hello befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b55d5-178">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="b55d5-179">hello följande exempel demonterar hello enhet på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-179">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="b55d5-180">Nu ska du koppla från hello virtuella hårddiskarna från hello VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-180">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="b55d5-181">Avsluta hello SSH-session tooyour felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-181">Exit hello SSH session tooyour troubleshooting VM.</span></span> <span data-ttu-id="b55d5-182">I hello Azure CLI kopplas första listan hello data diskar tooyour felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="b55d5-182">In hello Azure CLI, first list hello attached data disks tooyour troubleshooting VM.</span></span> <span data-ttu-id="b55d5-183">hello följande exempel visar hello datadiskar kopplade toohello virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-183">hello following example lists hello data disks attached toohello VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    <span data-ttu-id="b55d5-184">Obs hello `Lun` värde för en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b55d5-184">Note hello `Lun` value for your existing virtual hard disk.</span></span> <span data-ttu-id="b55d5-185">hello visar kommandoutdata i följande exempel hello befintlig virtuell disk som är ansluten på LUN 0:</span><span class="sxs-lookup"><span data-stu-id="b55d5-185">hello following example command output shows hello existing virtual disk attached at LUN 0:</span></span>

    ```azurecli
    info:    Executing command vm disk list
    + Looking up hello VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    <span data-ttu-id="b55d5-186">Koppla från hello datadisk från den virtuella datorn med hjälp av hello tillämpliga `Lun` värde:</span><span class="sxs-lookup"><span data-stu-id="b55d5-186">Detach hello data disk from your VM using hello applicable `Lun` value:</span></span>

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="b55d5-187">Skapa virtuell dator från den ursprungliga hårddisken</span><span class="sxs-lookup"><span data-stu-id="b55d5-187">Create VM from original hard disk</span></span>
<span data-ttu-id="b55d5-188">toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="b55d5-188">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="b55d5-189">hello faktiska JSON-mallen finns på hello följande länk:</span><span class="sxs-lookup"><span data-stu-id="b55d5-189">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="b55d5-190">https://Raw.githubusercontent.com/Azure/Azure-Quickstart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b55d5-190">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="b55d5-191">hello mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av hello VHD URL från hello tidigare kommandot.</span><span class="sxs-lookup"><span data-stu-id="b55d5-191">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="b55d5-192">hello följande exempel distribuerar hello mallen toohello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-192">hello following example deploys hello template toohello resource group named `myResourceGroup`:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

<span data-ttu-id="b55d5-193">Svaret hello efterfrågar hello mall som namn på virtuell dator (`myDeployedVM` hello följande exempel), OS-typen (`Linux`), och VM-storlek (`Standard_DS1_v2`).</span><span class="sxs-lookup"><span data-stu-id="b55d5-193">Answer hello prompts for hello template such as VM name (`myDeployedVM` hello following example), OS type (`Linux`), and VM size (`Standard_DS1_v2`).</span></span> <span data-ttu-id="b55d5-194">Hej `osDiskVhdUri` är hello densamma som används när du ansluter hello befintlig virtuell hårddisk toohello felsökning VM som tidigare.</span><span class="sxs-lookup"><span data-stu-id="b55d5-194">hello `osDiskVhdUri` is hello same as previously used when attaching hello existing virtual hard disk toohello troubleshooting VM.</span></span> <span data-ttu-id="b55d5-195">Ett exempel på utdata från kommandot hello och anvisningarna är följande:</span><span class="sxs-lookup"><span data-stu-id="b55d5-195">An example of hello command output and prompts is as follows:</span></span>

```azurecli
info:    Executing command group deployment create
info:    Supply values for hello following parameters
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
+ Waiting for deployment toocomplete
+
```


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="b55d5-196">Återaktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="b55d5-196">Re-enable boot diagnostics</span></span>

<span data-ttu-id="b55d5-197">När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras.</span><span class="sxs-lookup"><span data-stu-id="b55d5-197">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="b55d5-198">hello följande exempel aktiveras hello diagnostiska tillägg på hello virtuella datorn med namnet `myDeployedVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b55d5-198">hello following example enables hello diagnostic extension on hello VM named `myDeployedVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="b55d5-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b55d5-199">Next steps</span></span>
<span data-ttu-id="b55d5-200">Om du har problem med anslutning tooyour VM finns [felsökning av SSH-anslutningar tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b55d5-200">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="b55d5-201">Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b55d5-201">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
