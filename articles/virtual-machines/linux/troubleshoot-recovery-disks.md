---
title: "aaaUse en Linux felsöka VM med hello Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Linux VM problem med anslutning hello OS disk tooa recovery VM hello Azure CLI 2.0"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: iainfou
ms.openlocfilehash: 776d61b61280f46e3699157addcdb1e7dfb6818e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-with-hello-azure-cli-20"></a><span data-ttu-id="77289-103">Felsöka en Linux VM genom att koppla hello OS tooa diskåterställning VM med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="77289-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM with hello Azure CLI 2.0</span></span>
<span data-ttu-id="77289-104">Om Linux-dator (VM) påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv.</span><span class="sxs-lookup"><span data-stu-id="77289-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="77289-105">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab` som förhindrar hello VM kan tooboot har.</span><span class="sxs-lookup"><span data-stu-id="77289-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="77289-106">Det här artikeln beskriver hur toouse hello Azure CLI 2.0 tooconnect din virtuella hårddisk disk tooanother Linux VM toofix eventuella fel och sedan återskapa den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="77289-106">This article details how toouse hello Azure CLI 2.0 tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span> <span data-ttu-id="77289-107">Du kan också utföra dessa steg med hello [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77289-107">You can also perform these steps with hello [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="77289-108">Översikt över återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="77289-108">Recovery process overview</span></span>
<span data-ttu-id="77289-109">hello felsökningsprocessen är följande:</span><span class="sxs-lookup"><span data-stu-id="77289-109">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="77289-110">Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="77289-110">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="77289-111">Anslut och montera hello virtuell hårddisk tooanother Linux VM i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="77289-111">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="77289-112">Ansluta toohello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="77289-112">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="77289-113">Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="77289-113">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="77289-114">Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="77289-114">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="77289-115">Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="77289-115">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="77289-116">tooperform felsökningen, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="77289-116">tooperform these troubleshooting steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="77289-117">Följande exempel Ersätt parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="77289-117">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="77289-118">Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="77289-118">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="77289-119">Fastställa startproblem</span><span class="sxs-lookup"><span data-stu-id="77289-119">Determine boot issues</span></span>
<span data-ttu-id="77289-120">Granska hello seriella utdata toodetermine till den virtuella datorn inte är kan tooboot korrekt.</span><span class="sxs-lookup"><span data-stu-id="77289-120">Examine hello serial output toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="77289-121">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab`, eller hello underliggande virtuell hårddisk som tagits bort eller flyttats.</span><span class="sxs-lookup"><span data-stu-id="77289-121">A common example is an invalid entry in `/etc/fstab`, or hello underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="77289-122">Hämta hello Start loggar med [az vm-startdiagnostikinställningar get-Start-loggen](/cli/azure/vm/boot-diagnostics#get-boot-log).</span><span class="sxs-lookup"><span data-stu-id="77289-122">Get hello boot logs with [az vm boot-diagnostics get-boot-log](/cli/azure/vm/boot-diagnostics#get-boot-log).</span></span> <span data-ttu-id="77289-123">hello följande exempel hämtas hello seriella utdata från hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="77289-123">hello following example gets hello serial output from hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="77289-124">Granska hello seriella utdata toodetermine varför hello VM inte tooboot.</span><span class="sxs-lookup"><span data-stu-id="77289-124">Review hello serial output toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="77289-125">Om hello seriella utdata är inte något indikerar måste du kanske måste tooreview loggfiler i `/var/log` när du har hello virtuell hårddisk ansluten tooa felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="77289-125">If hello serial output isn't providing any indication, you may need tooreview log files in `/var/log` once you have hello virtual hard disk connected tooa troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="77289-126">Visa information om befintlig virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="77289-126">View existing virtual hard disk details</span></span>
<span data-ttu-id="77289-127">Innan du kan koppla en virtuell hårddisk (VHD) tooanother VM, måste tooidentify hello URI för hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="77289-127">Before you can attach your virtual hard disk (VHD) tooanother VM, you need tooidentify hello URI of hello OS disk.</span></span> 

<span data-ttu-id="77289-128">Visa information om den virtuella datorn med [az vm visa](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="77289-128">View information about your VM with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="77289-129">Använd hello `--query` flaggan tooextract hello URI toohello OS-disk.</span><span class="sxs-lookup"><span data-stu-id="77289-129">Use hello `--query` flag tooextract hello URI toohello OS disk.</span></span> <span data-ttu-id="77289-130">hello följande exempel hämtar diskinformation för hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="77289-130">hello following example gets disk information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

<span data-ttu-id="77289-131">hello URI är liknande för**https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span><span class="sxs-lookup"><span data-stu-id="77289-131">hello URI is similar too**https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span></span>

## <a name="delete-existing-vm"></a><span data-ttu-id="77289-132">Ta bort en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="77289-132">Delete existing VM</span></span>
<span data-ttu-id="77289-133">Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="77289-133">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="77289-134">En virtuell hårddisk är där hello själva operativsystemet, program och konfigurationer lagras.</span><span class="sxs-lookup"><span data-stu-id="77289-134">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="77289-135">hello Virtuella datorn är enbart metadata som definierar hello storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="77289-135">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="77289-136">Varje virtuell hårddisk har tilldelats när kopplade tooa VM.</span><span class="sxs-lookup"><span data-stu-id="77289-136">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="77289-137">Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort.</span><span class="sxs-lookup"><span data-stu-id="77289-137">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="77289-138">hello lån fortsätter tooassociate hello OS-disk med en virtuell dator, även om den virtuella datorn är i tillståndet stoppad och frigjord.</span><span class="sxs-lookup"><span data-stu-id="77289-138">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="77289-139">Hej första steg toorecover den virtuella datorn är toodelete hello Virtuella datorresursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="77289-139">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="77289-140">Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="77289-140">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="77289-141">Efter hello VM tas bort, bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel.</span><span class="sxs-lookup"><span data-stu-id="77289-141">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="77289-142">Ta bort hello virtuell dator med [az vm ta bort](/cli/azure/vm#delete).</span><span class="sxs-lookup"><span data-stu-id="77289-142">Delete hello VM with [az vm delete](/cli/azure/vm#delete).</span></span> <span data-ttu-id="77289-143">följande exempel tar bort hello hello virtuella datorn med namnet `myVM` från hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="77289-143">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="77289-144">Vänta tills hello VM har tagits bort innan du kopplar hello virtuell hårddisk tooanother VM.</span><span class="sxs-lookup"><span data-stu-id="77289-144">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="77289-145">hello lånet på hello virtuell hårddisk som associeras med hello VM måste släppas innan du kan koppla hello virtuell hårddisk tooanother VM toobe.</span><span class="sxs-lookup"><span data-stu-id="77289-145">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="77289-146">Bifoga en befintlig virtuell hårddisk tooanother VM</span><span class="sxs-lookup"><span data-stu-id="77289-146">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="77289-147">Hej bredvid för några steg kan du använda en annan virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="77289-147">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="77289-148">Du bifogar hello befintlig virtuell hårddisk toothis felsökning VM toobrowse och redigera hello disk innehåll.</span><span class="sxs-lookup"><span data-stu-id="77289-148">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="77289-149">Den här processen kan toocorrect eventuella fel i programkonfigurationen eller granska ytterligare program eller loggfiler, till exempel.</span><span class="sxs-lookup"><span data-stu-id="77289-149">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="77289-150">Välj eller skapa en annan VM toouse för felsökning.</span><span class="sxs-lookup"><span data-stu-id="77289-150">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="77289-151">Koppla hello befintlig virtuell hårddisk med [az vm ohanterad disk bifoga](/cli/azure/vm/unmanaged-disk#attach).</span><span class="sxs-lookup"><span data-stu-id="77289-151">Attach hello existing virtual hard disk with [az vm unmanaged-disk attach](/cli/azure/vm/unmanaged-disk#attach).</span></span> <span data-ttu-id="77289-152">När du ansluter hello befintlig virtuell hårddisk, ange hello URI toohello disk hämtades i föregående hello `az vm show` kommando.</span><span class="sxs-lookup"><span data-stu-id="77289-152">When you attach hello existing virtual hard disk, specify hello URI toohello disk obtained in hello preceding `az vm show` command.</span></span> <span data-ttu-id="77289-153">hello följande exempel bifogar en befintlig virtuell hårddisk toohello felsökning virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="77289-153">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="77289-154">Montera hello bifogade datadisk</span><span class="sxs-lookup"><span data-stu-id="77289-154">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="77289-155">hello följande exempel innehåller information om hello steg som krävs på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="77289-155">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="77289-156">Om du använder en annan Linux distro, till exempel Red Hat Enterprise Linux eller SUSE, hello loggfilens platser och `mount` kommandon kan vara lite annorlunda.</span><span class="sxs-lookup"><span data-stu-id="77289-156">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="77289-157">Läs toohello dokumentationen för din specifika distro för hello ändringarna i kommandona.</span><span class="sxs-lookup"><span data-stu-id="77289-157">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="77289-158">SSH tooyour felsökning VM som använder hello rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="77289-158">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="77289-159">Om den här disken är hello första data disk ansluten tooyour felsökning VM, hello disk sannolikt ansluten för`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="77289-159">If this disk is hello first data disk attached tooyour troubleshooting VM, hello disk is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="77289-160">Använd `dmseg` tooview anslutna diskar:</span><span class="sxs-lookup"><span data-stu-id="77289-160">Use `dmseg` tooview attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="77289-161">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="77289-161">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="77289-162">I föregående exempel hello, hello OS-disken är på `/dev/sda` och hello diskutrymme för varje virtuell dator är på `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="77289-162">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="77289-163">Om du har flera datadiskar, bör de visas på `/dev/sdd`, `/dev/sde`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="77289-163">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="77289-164">Skapa en katalog toomount en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="77289-164">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="77289-165">hello följande exempel skapas en katalog med namnet `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="77289-165">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="77289-166">Om du har flera partitioner i en befintlig virtuell hårddisk montera hello krävs partition.</span><span class="sxs-lookup"><span data-stu-id="77289-166">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="77289-167">hello följande exempel monterar hello första primära partition på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="77289-167">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="77289-168">Det är bra toomount datadiskar på virtuella datorer i Azure med hjälp av hello universellt Unik identifierare (UUID) för hello virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="77289-168">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="77289-169">Den här korta felsökning scenariot behövs inte montering hello virtuell hårddisk med hello UUID.</span><span class="sxs-lookup"><span data-stu-id="77289-169">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="77289-170">Men vid normal användning, redigering `/etc/fstab` toomount virtuella hårddiskar med enhetsnamn snarare än UUID kan orsaka hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="77289-170">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="77289-171">Åtgärda problemen på den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="77289-171">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="77289-172">Med hello befintlig virtuell hårddisk monterad, kan du nu utföra eventuella underhåll och felsökning efter behov.</span><span class="sxs-lookup"><span data-stu-id="77289-172">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="77289-173">När du har åtgärdat hello problem, fortsätter du med hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="77289-173">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="77289-174">Demontera och koppla från den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="77289-174">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="77289-175">När din fel har åtgärdats, demontera och koppla hello befintlig virtuell hårddisk från den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="77289-175">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="77289-176">Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.</span><span class="sxs-lookup"><span data-stu-id="77289-176">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="77289-177">Demontera från hello SSH-session tooyour felsökning VM, hello befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="77289-177">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="77289-178">Ändra utanför hello överordnade katalogen för din monteringspunkt:</span><span class="sxs-lookup"><span data-stu-id="77289-178">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="77289-179">Nu demontera hello befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="77289-179">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="77289-180">hello följande exempel demonterar hello enhet på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="77289-180">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="77289-181">Nu ska du koppla från hello virtuella hårddiskarna från hello VM.</span><span class="sxs-lookup"><span data-stu-id="77289-181">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="77289-182">Avsluta hello SSH-session tooyour felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="77289-182">Exit hello SSH session tooyour troubleshooting VM.</span></span> <span data-ttu-id="77289-183">Lista hello kopplade data diskar tooyour felsökning av virtuell dator med [az vm ohanterad disk listan](/cli/azure/vm/unmanaged-disk#list).</span><span class="sxs-lookup"><span data-stu-id="77289-183">List hello attached data disks tooyour troubleshooting VM with [az vm unmanaged-disk list](/cli/azure/vm/unmanaged-disk#list).</span></span> <span data-ttu-id="77289-184">hello följande exempel visar hello datadiskar kopplade toohello virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="77289-184">hello following example lists hello data disks attached toohello VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    <span data-ttu-id="77289-185">Observera hello namn för en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="77289-185">Note hello name for your existing virtual hard disk.</span></span> <span data-ttu-id="77289-186">Till exempel hello namnet på en disk med hello URI för **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** är **myVHD**.</span><span class="sxs-lookup"><span data-stu-id="77289-186">For example, hello name of a disk with hello URI of **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** is **myVHD**.</span></span> 

    <span data-ttu-id="77289-187">Koppla från hello datadisk från den virtuella datorn [az vm ohanterad disk frånkoppling](/cli/azure/vm/unmanaged-disk#detach).</span><span class="sxs-lookup"><span data-stu-id="77289-187">Detach hello data disk from your VM [az vm unmanaged-disk detach](/cli/azure/vm/unmanaged-disk#detach).</span></span> <span data-ttu-id="77289-188">hello följande exempel tar bort hello disk med namnet `myVHD` från hello virtuella datorn med namnet `myVMRecovery` i hello `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="77289-188">hello following example detaches hello disk named `myVHD` from hello VM named `myVMRecovery` in hello `myResourceGroup` resource group:</span></span>

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="77289-189">Skapa virtuell dator från den ursprungliga hårddisken</span><span class="sxs-lookup"><span data-stu-id="77289-189">Create VM from original hard disk</span></span>
<span data-ttu-id="77289-190">toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="77289-190">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="77289-191">hello faktiska JSON-mallen finns på hello följande länk:</span><span class="sxs-lookup"><span data-stu-id="77289-191">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="77289-192">https://Raw.githubusercontent.com/Azure/Azure-Quickstart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="77289-192">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="77289-193">hello mallen distribuerar en virtuell dator med hjälp av hello VHD-URI från hello tidigare kommandot.</span><span class="sxs-lookup"><span data-stu-id="77289-193">hello template deploys a VM using hello VHD URI from hello earlier command.</span></span> <span data-ttu-id="77289-194">Distribuera hello mallen med [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="77289-194">Deploy hello template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="77289-195">Ange hello URI tooyour ursprungliga virtuella Hårddisken och anger sedan hello OS-typ, VM-storlek och namn på virtuell dator på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="77289-195">Provide hello URI tooyour original VHD and then specify hello OS type, VM size, and VM name as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="77289-196">Återaktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="77289-196">Re-enable boot diagnostics</span></span>
<span data-ttu-id="77289-197">När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras.</span><span class="sxs-lookup"><span data-stu-id="77289-197">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="77289-198">Aktivera startdiagnostikinställningar med [az vm-startdiagnostikinställningar aktivera](/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="77289-198">Enable boot diagnostics with [az vm boot-diagnostics enable](/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="77289-199">hello följande exempel aktiveras hello diagnostiska tillägg på hello virtuella datorn med namnet `myDeployedVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="77289-199">hello following example enables hello diagnostic extension on hello VM named `myDeployedVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="77289-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77289-200">Next steps</span></span>
<span data-ttu-id="77289-201">Om du har problem med anslutning tooyour VM finns [felsökning av SSH-anslutningar tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77289-201">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="77289-202">Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77289-202">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
