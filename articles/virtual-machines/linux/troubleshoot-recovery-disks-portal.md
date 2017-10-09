---
title: "aaaUse en Linux felsökning VM i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Linux virtuella problem med anslutning hello OS disk tooa recovery VM hello Azure-portalen"
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
ms.date: 11/14/2016
ms.author: iainfou
ms.openlocfilehash: 794daa06d7436215af84a61ab9088524254c47df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a><span data-ttu-id="f48fb-103">Felsöka en Linux VM genom att koppla hello OS tooa diskåterställning VM med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f48fb-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM using hello Azure portal</span></span>
<span data-ttu-id="f48fb-104">Om Linux-dator (VM) påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv.</span><span class="sxs-lookup"><span data-stu-id="f48fb-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="f48fb-105">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab` som förhindrar hello VM kan tooboot har.</span><span class="sxs-lookup"><span data-stu-id="f48fb-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="f48fb-106">Det här artikeln beskriver hur toouse hello Azure portal tooconnect din virtuella hårddisk disk tooanother Linux VM toofix eventuella fel och sedan återskapa den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f48fb-106">This article details how toouse hello Azure portal tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span>

## <a name="recovery-process-overview"></a><span data-ttu-id="f48fb-107">Översikt över återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="f48fb-107">Recovery process overview</span></span>
<span data-ttu-id="f48fb-108">hello felsökningsprocessen är följande:</span><span class="sxs-lookup"><span data-stu-id="f48fb-108">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="f48fb-109">Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="f48fb-109">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="f48fb-110">Anslut och montera hello virtuell hårddisk tooanother Linux VM i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="f48fb-110">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="f48fb-111">Ansluta toohello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="f48fb-111">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="f48fb-112">Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="f48fb-112">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="f48fb-113">Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="f48fb-113">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="f48fb-114">Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="f48fb-114">Create a VM using hello original virtual hard disk.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="f48fb-115">Fastställa startproblem</span><span class="sxs-lookup"><span data-stu-id="f48fb-115">Determine boot issues</span></span>
<span data-ttu-id="f48fb-116">Granska hello startdiagnostikinställningar och VM skärmbild toodetermine till den virtuella datorn inte är kan tooboot korrekt.</span><span class="sxs-lookup"><span data-stu-id="f48fb-116">Examine hello boot diagnostics and VM screenshot toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="f48fb-117">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab`, eller en underliggande virtuell hårddisk som tagits bort eller flyttats.</span><span class="sxs-lookup"><span data-stu-id="f48fb-117">A common example would be an invalid entry in `/etc/fstab`, or an underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="f48fb-118">Välj den virtuella datorn i hello-portalen och bläddrar sedan nedåt toohello **stöd + felsökning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f48fb-118">Select your VM in hello portal and then scroll down toohello **Support + Troubleshooting** section.</span></span> <span data-ttu-id="f48fb-119">Klicka på **starta diagnostik** tooview hälsningsmeddelande konsolen strömmas från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f48fb-119">Click **Boot diagnostics** tooview hello console messages streamed from your VM.</span></span> <span data-ttu-id="f48fb-120">Granska hello konsolloggar toosee om du kan fastställa varför hello VM har uppstått ett problem.</span><span class="sxs-lookup"><span data-stu-id="f48fb-120">Review hello console logs toosee if you can determine why hello VM is encountering an issue.</span></span> <span data-ttu-id="f48fb-121">hello följande exempel visas en virtuell dator som fastnat i underhållsläge som kräver manuell interaktion:</span><span class="sxs-lookup"><span data-stu-id="f48fb-121">hello following example shows a VM stuck in maintenance mode that requires manual interaction:</span></span>

![Visa VM konsolen startdiagnostikinställningar loggar](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

<span data-ttu-id="f48fb-123">Du kan också klicka på **skärmbild** över hello överkant hello Start diagnostik loggen toodownload en avbildning av hello VM skärmbild.</span><span class="sxs-lookup"><span data-stu-id="f48fb-123">You can also click **Screenshot** across hello top of hello boot diagnostics log toodownload a capture of hello VM screenshot.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="f48fb-124">Visa information om befintlig virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="f48fb-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="f48fb-125">Innan du kan koppla en virtuell hårddisk tooanother VM, måste tooidentify hello namnet på hello virtuell hårddisk (VHD).</span><span class="sxs-lookup"><span data-stu-id="f48fb-125">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span> 

<span data-ttu-id="f48fb-126">Välj din resursgrupp från hello-portalen och sedan ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f48fb-126">Select your resource group from hello portal, then select your storage account.</span></span> <span data-ttu-id="f48fb-127">Klicka på **Blobbar**, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="f48fb-127">Click **Blobs**, as in hello following example:</span></span>

![Välj storage-blobbar](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

<span data-ttu-id="f48fb-129">Du har vanligtvis en behållare med namnet **virtuella hårddiskar** som lagrar dina virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="f48fb-129">Typically you have a container named **vhds** that stores your virtual hard disks.</span></span> <span data-ttu-id="f48fb-130">Välj hello behållaren tooview en lista över virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="f48fb-130">Select hello container tooview a list of virtual hard disks.</span></span> <span data-ttu-id="f48fb-131">Obs hello namnet på den virtuella Hårddisken (hello prefix är vanligtvis hello namnet på den virtuella datorn):</span><span class="sxs-lookup"><span data-stu-id="f48fb-131">Note hello name of your VHD (hello prefix is usually hello name of your VM):</span></span>

![Identifiera VHD i lagringsbehållaren](./media/troubleshoot-recovery-disks-portal/storage-container.png)

<span data-ttu-id="f48fb-133">Välj en befintlig virtuell hårddisk hello listan och kopiera hello URL: en för användning i hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f48fb-133">Select your existing virtual hard disk from hello list and copy hello URL for use in hello following steps:</span></span>

![Kopiera URL: en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a><span data-ttu-id="f48fb-135">Ta bort en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f48fb-135">Delete existing VM</span></span>
<span data-ttu-id="f48fb-136">Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="f48fb-136">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="f48fb-137">En virtuell hårddisk är där hello själva operativsystemet, program och konfigurationer lagras.</span><span class="sxs-lookup"><span data-stu-id="f48fb-137">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="f48fb-138">hello Virtuella datorn är enbart metadata som definierar hello storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="f48fb-138">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="f48fb-139">Varje virtuell hårddisk har tilldelats när kopplade tooa VM.</span><span class="sxs-lookup"><span data-stu-id="f48fb-139">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="f48fb-140">Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort.</span><span class="sxs-lookup"><span data-stu-id="f48fb-140">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="f48fb-141">hello lån fortsätter tooassociate hello OS-disk med en virtuell dator, även om den virtuella datorn är i tillståndet stoppad och frigjord.</span><span class="sxs-lookup"><span data-stu-id="f48fb-141">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="f48fb-142">Hej första steg toorecover den virtuella datorn är toodelete hello Virtuella datorresursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="f48fb-142">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="f48fb-143">Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f48fb-143">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="f48fb-144">Efter hello VM tas bort, bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel.</span><span class="sxs-lookup"><span data-stu-id="f48fb-144">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="f48fb-145">Välj den virtuella datorn i hello portal och klicka sedan på **ta bort**:</span><span class="sxs-lookup"><span data-stu-id="f48fb-145">Select your VM in hello portal, then click **Delete**:</span></span>

![VM-Start diagnostik skärmbild som visar start-fel](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

<span data-ttu-id="f48fb-147">Vänta tills hello VM har tagits bort innan du kopplar hello virtuell hårddisk tooanother VM.</span><span class="sxs-lookup"><span data-stu-id="f48fb-147">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="f48fb-148">hello lånet på hello virtuell hårddisk som associeras med hello VM måste släppas innan du kan koppla hello virtuell hårddisk tooanother VM toobe.</span><span class="sxs-lookup"><span data-stu-id="f48fb-148">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="f48fb-149">Bifoga en befintlig virtuell hårddisk tooanother VM</span><span class="sxs-lookup"><span data-stu-id="f48fb-149">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="f48fb-150">Hej bredvid för några steg kan du använda en annan virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="f48fb-150">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="f48fb-151">Du bifogar hello befintlig virtuell hårddisk toothis felsökning VM toobe kan toobrowse och redigera hello disk innehåll.</span><span class="sxs-lookup"><span data-stu-id="f48fb-151">You attach hello existing virtual hard disk toothis troubleshooting VM toobe able toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="f48fb-152">Den här processen kan toocorrect eventuella fel i programkonfigurationen eller granska ytterligare program eller loggfiler, till exempel.</span><span class="sxs-lookup"><span data-stu-id="f48fb-152">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="f48fb-153">Välj eller skapa en annan VM toouse för felsökning.</span><span class="sxs-lookup"><span data-stu-id="f48fb-153">Choose or create another VM toouse for troubleshooting purposes.</span></span>

1. <span data-ttu-id="f48fb-154">Välj din resursgrupp från hello-portalen, och välj sedan den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="f48fb-154">Select your resource group from hello portal, then select your troubleshooting VM.</span></span> <span data-ttu-id="f48fb-155">Välj **diskar** och klicka sedan på **bifoga befintliga**:</span><span class="sxs-lookup"><span data-stu-id="f48fb-155">Select **Disks** and then click **Attach existing**:</span></span>

    ![Bifoga den befintliga disken i hello-portalen](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. <span data-ttu-id="f48fb-157">tooselect befintliga virtuella hårddisken, klickar du på **VHD-filen**:</span><span class="sxs-lookup"><span data-stu-id="f48fb-157">tooselect your existing virtual hard disk, click **VHD File**:</span></span>

    ![Bläddra till den befintliga virtuella hårddisken](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. <span data-ttu-id="f48fb-159">Välj ditt lagringskonto och en behållare och sedan på den befintliga virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="f48fb-159">Select your storage account and container, then click your existing VHD.</span></span> <span data-ttu-id="f48fb-160">Klicka på hello **Välj** knappen tooconfirm ditt val:</span><span class="sxs-lookup"><span data-stu-id="f48fb-160">Click hello **Select** button tooconfirm your choice:</span></span>

    ![Välj din befintliga virtuella hårddisk](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. <span data-ttu-id="f48fb-162">Med den virtuella Hårddisken nu markerade, klickar du på **OK** tooattach hello befintlig virtuell hårddisk:</span><span class="sxs-lookup"><span data-stu-id="f48fb-162">With your VHD now selected, click **OK** tooattach hello existing virtual hard disk:</span></span>

    ![Bekräfta att bifoga en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. <span data-ttu-id="f48fb-164">Efter några sekunder hello **diskar** fönstret för den virtuella datorn visas en befintlig virtuell hårddisk ansluten som en datadisk:</span><span class="sxs-lookup"><span data-stu-id="f48fb-164">After a few seconds, hello **Disks** pane for your VM lists your existing virtual hard disk connected as a data disk:</span></span>

    ![Befintlig virtuell hårddisk ansluten som en datadisk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="f48fb-166">Montera hello bifogade datadisk</span><span class="sxs-lookup"><span data-stu-id="f48fb-166">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="f48fb-167">hello följande exempel innehåller information om hello steg som krävs på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="f48fb-167">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="f48fb-168">Om du använder en annan Linux distro, till exempel Red Hat Enterprise Linux eller SUSE, hello loggfilens platser och `mount` kommandon kan vara lite annorlunda.</span><span class="sxs-lookup"><span data-stu-id="f48fb-168">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="f48fb-169">Läs toohello dokumentationen för din specifika distro för hello ändringarna i kommandona.</span><span class="sxs-lookup"><span data-stu-id="f48fb-169">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="f48fb-170">SSH tooyour felsökning VM som använder hello rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f48fb-170">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="f48fb-171">Om den här disken är hello första data disk ansluten tooyour felsökning VM, är det sannolikt anslutet för`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="f48fb-171">If this disk is hello first data disk attached tooyour troubleshooting VM, it is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="f48fb-172">Använd `dmseg` toolist anslutna diskar:</span><span class="sxs-lookup"><span data-stu-id="f48fb-172">Use `dmseg` toolist attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```
    <span data-ttu-id="f48fb-173">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="f48fb-173">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="f48fb-174">I föregående exempel hello, hello OS-disken är på `/dev/sda` och hello diskutrymme för varje virtuell dator är på `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="f48fb-174">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="f48fb-175">Om du har flera datadiskar, bör de visas på `/dev/sdd`, `/dev/sde`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="f48fb-175">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="f48fb-176">Skapa en katalog toomount en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="f48fb-176">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="f48fb-177">hello följande exempel skapas en katalog med namnet `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="f48fb-177">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="f48fb-178">Om du har flera partitioner i en befintlig virtuell hårddisk montera hello krävs partition.</span><span class="sxs-lookup"><span data-stu-id="f48fb-178">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="f48fb-179">hello följande exempel monterar hello första primära partition på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="f48fb-179">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="f48fb-180">Det är bra toomount datadiskar på virtuella datorer i Azure med hjälp av hello universellt Unik identifierare (UUID) för hello virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="f48fb-180">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="f48fb-181">Den här korta felsökning scenariot behövs inte montering hello virtuell hårddisk med hello UUID.</span><span class="sxs-lookup"><span data-stu-id="f48fb-181">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="f48fb-182">Men vid normal användning, redigering `/etc/fstab` toomount virtuella hårddiskar med enhetsnamn snarare än UUID kan orsaka hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="f48fb-182">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="f48fb-183">Åtgärda problemen på den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="f48fb-183">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="f48fb-184">Med hello befintlig virtuell hårddisk monterad, kan du nu utföra eventuella underhåll och felsökning efter behov.</span><span class="sxs-lookup"><span data-stu-id="f48fb-184">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="f48fb-185">När du har åtgärdat hello problem, fortsätter du med hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="f48fb-185">Once you have addressed hello issues, continue with hello following steps.</span></span>

## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="f48fb-186">Demontera och koppla från den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="f48fb-186">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="f48fb-187">Koppla bort hello befintlig virtuell hårddisk från den virtuella datorn med felsökning när din fel har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="f48fb-187">Once your errors are resolved, detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="f48fb-188">Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.</span><span class="sxs-lookup"><span data-stu-id="f48fb-188">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="f48fb-189">Demontera från hello SSH-session tooyour felsökning VM, hello befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="f48fb-189">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="f48fb-190">Ändra utanför hello överordnade katalogen för din monteringspunkt:</span><span class="sxs-lookup"><span data-stu-id="f48fb-190">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="f48fb-191">Nu demontera hello befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="f48fb-191">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="f48fb-192">hello följande exempel demonterar hello enhet på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="f48fb-192">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="f48fb-193">Nu ska du koppla från hello virtuella hårddiskarna från hello VM.</span><span class="sxs-lookup"><span data-stu-id="f48fb-193">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="f48fb-194">Välj den virtuella datorn i hello portal och klicka på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="f48fb-194">Select your VM in hello portal and click **Disks**.</span></span> <span data-ttu-id="f48fb-195">Välj en befintlig virtuell hårddisk och klicka sedan på **Detach**:</span><span class="sxs-lookup"><span data-stu-id="f48fb-195">Select your existing virtual hard disk and then click **Detach**:</span></span>

    ![Koppla från en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    <span data-ttu-id="f48fb-197">Vänta tills hello VM har disk har kopplats bort hello data innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="f48fb-197">Wait until hello VM has successfully detached hello data disk before continuing.</span></span>

## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="f48fb-198">Skapa virtuell dator från den ursprungliga hårddisken</span><span class="sxs-lookup"><span data-stu-id="f48fb-198">Create VM from original hard disk</span></span>
<span data-ttu-id="f48fb-199">toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="f48fb-199">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="f48fb-200">hello mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av hello VHD URL från hello tidigare kommandot.</span><span class="sxs-lookup"><span data-stu-id="f48fb-200">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="f48fb-201">Klicka på hello **distribuera tooAzure** knappen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f48fb-201">Click hello **Deploy tooAzure** button as follows:</span></span>

![Distribuera virtuell dator från mall från GitHub](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

<span data-ttu-id="f48fb-203">hello mallen läses in i hello Azure-portalen för distribution.</span><span class="sxs-lookup"><span data-stu-id="f48fb-203">hello template is loaded into hello Azure portal for deployment.</span></span> <span data-ttu-id="f48fb-204">Ange hello namn för din nya virtuella datorn och befintliga Azure-resurser och klistra in hello URL tooyour befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="f48fb-204">Enter hello names for your new VM and existing Azure resources, and paste hello URL tooyour existing virtual hard disk.</span></span> <span data-ttu-id="f48fb-205">toobegin hello distributionen, klickar du på **inköp**:</span><span class="sxs-lookup"><span data-stu-id="f48fb-205">toobegin hello deployment, click **Purchase**:</span></span>

![Distribuera virtuell dator från mall](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="f48fb-207">Återaktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="f48fb-207">Re-enable boot diagnostics</span></span>
<span data-ttu-id="f48fb-208">När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras.</span><span class="sxs-lookup"><span data-stu-id="f48fb-208">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="f48fb-209">toocheck hello status för startdiagnostikinställningar och starta om det behövs, Välj den virtuella datorn i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f48fb-209">toocheck hello status of boot diagnostics and turn on if needed, select your VM in hello portal.</span></span> <span data-ttu-id="f48fb-210">Under **övervakning**, klickar du på **diagnostikinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="f48fb-210">Under **Monitoring**, click **Diagnostics settings**.</span></span> <span data-ttu-id="f48fb-211">Kontrollera status för hello är **på**, och hello bockmarkering bredvid för**starta diagnostik** är markerad.</span><span class="sxs-lookup"><span data-stu-id="f48fb-211">Ensure hello status is **On**, and hello check mark next too**Boot diagnostics** is selected.</span></span> <span data-ttu-id="f48fb-212">Om du gör några ändringar klickar du på **spara**:</span><span class="sxs-lookup"><span data-stu-id="f48fb-212">If you make any changes, click **Save**:</span></span>

![Uppdatera startdiagnostikinställningar](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="f48fb-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f48fb-214">Next steps</span></span>
<span data-ttu-id="f48fb-215">Om du har problem med anslutning tooyour VM finns [felsökning av SSH-anslutningar tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f48fb-215">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f48fb-216">Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f48fb-216">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f48fb-217">Mer information om hur du använder Resource Manager finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f48fb-217">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
