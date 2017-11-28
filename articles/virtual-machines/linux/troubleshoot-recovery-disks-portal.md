---
title: "Använda en Linux felsökning VM i Azure portal | Microsoft Docs"
description: "Lär dig hur du felsöker problem för Linux virtuella datorer genom att ansluta OS-disken till en VM som använder Azure portal för återställning"
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
ms.openlocfilehash: c96ff625c3e83f6fc9057f1163c877e8e0aed5e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a><span data-ttu-id="ea6b6-103">Felsöka en Linux VM genom att koppla OS-disken till en VM som använder Azure portal för återställning</span><span class="sxs-lookup"><span data-stu-id="ea6b6-103">Troubleshoot a Linux VM by attaching the OS disk to a recovery VM using the Azure portal</span></span>
<span data-ttu-id="ea6b6-104">Om Linux-dator (VM) påträffar ett fel vid start- eller disk, kan du behöva utför felsökning på den virtuella hårddisken sig själv.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="ea6b6-105">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab` som förhindrar att den virtuella datorn kan starta korrekt.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-105">A common example would be an invalid entry in `/etc/fstab` that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="ea6b6-106">Den här artikeln beskriver hur du använder Azure-portalen för att ansluta den virtuella hårddisken till en annan Linux VM att åtgärda eventuella fel och återskapa den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-106">This article details how to use the Azure portal to connect your virtual hard disk to another Linux VM to fix any errors, then re-create your original VM.</span></span>

## <a name="recovery-process-overview"></a><span data-ttu-id="ea6b6-107">Översikt över återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="ea6b6-107">Recovery process overview</span></span>
<span data-ttu-id="ea6b6-108">Så här ser felsökningsprocessen ut:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-108">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="ea6b6-109">Ta bort den virtuella datorn får problem, hålla de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-109">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="ea6b6-110">Anslut och montera den virtuella hårddisken till en annan Linux VM i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-110">Attach and mount the virtual hard disk to another Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="ea6b6-111">Anslut till den virtuella felsökningsdatorn.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-111">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="ea6b6-112">Redigera filer eller köra några verktyg för att åtgärda problem på den ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-112">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="ea6b6-113">Demontera och koppla från den virtuella hårddisken från den virtuella felsökningsdatorn.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-113">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="ea6b6-114">Skapa en virtuell dator med hjälp av den ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-114">Create a VM using the original virtual hard disk.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="ea6b6-115">Fastställa startproblem</span><span class="sxs-lookup"><span data-stu-id="ea6b6-115">Determine boot issues</span></span>
<span data-ttu-id="ea6b6-116">Granska startdiagnostikinställningar och skärmbilden Virtuella att avgöra varför den virtuella datorn kan inte starta korrekt.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-116">Examine the boot diagnostics and VM screenshot to determine why your VM is not able to boot correctly.</span></span> <span data-ttu-id="ea6b6-117">Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab`, eller en underliggande virtuell hårddisk som tagits bort eller flyttats.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-117">A common example would be an invalid entry in `/etc/fstab`, or an underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="ea6b6-118">Välj den virtuella datorn i portalen och rulla ned till den **stöd + felsökning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-118">Select your VM in the portal and then scroll down to the **Support + Troubleshooting** section.</span></span> <span data-ttu-id="ea6b6-119">Klicka på **starta diagnostik** visa console-meddelanden som strömmas från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-119">Click **Boot diagnostics** to view the console messages streamed from your VM.</span></span> <span data-ttu-id="ea6b6-120">Granska loggarna för konsolen för att se om du kan fastställa varför den virtuella datorn har uppstått ett problem.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-120">Review the console logs to see if you can determine why the VM is encountering an issue.</span></span> <span data-ttu-id="ea6b6-121">I följande exempel visas en virtuell dator som fastnat i underhållsläge som kräver manuell interaktion:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-121">The following example shows a VM stuck in maintenance mode that requires manual interaction:</span></span>

![Visa VM konsolen startdiagnostikinställningar loggar](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

<span data-ttu-id="ea6b6-123">Du kan också klicka på **skärmbild** längst upp i boot diagnostik loggen att hämta en avbildning av VM-skärmbilden.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-123">You can also click **Screenshot** across the top of the boot diagnostics log to download a capture of the VM screenshot.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="ea6b6-124">Visa information om befintlig virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="ea6b6-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="ea6b6-125">Innan du kan koppla den virtuella hårddisken till en annan virtuell dator, måste du identifiera namnet på den virtuella hårddisken (VHD).</span><span class="sxs-lookup"><span data-stu-id="ea6b6-125">Before you can attach your virtual hard disk to another VM, you need to identify the name of the virtual hard disk (VHD).</span></span> 

<span data-ttu-id="ea6b6-126">Välj din resursgrupp från portalen och sedan ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-126">Select your resource group from the portal, then select your storage account.</span></span> <span data-ttu-id="ea6b6-127">Klicka på **Blobbar**, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-127">Click **Blobs**, as in the following example:</span></span>

![Välj storage-blobbar](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

<span data-ttu-id="ea6b6-129">Du har vanligtvis en behållare med namnet **virtuella hårddiskar** som lagrar dina virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-129">Typically you have a container named **vhds** that stores your virtual hard disks.</span></span> <span data-ttu-id="ea6b6-130">Välj behållare för att visa en lista över virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-130">Select the container to view a list of virtual hard disks.</span></span> <span data-ttu-id="ea6b6-131">Notera namnet på den virtuella Hårddisken (prefixet är vanligtvis namnet på den virtuella datorn):</span><span class="sxs-lookup"><span data-stu-id="ea6b6-131">Note the name of your VHD (the prefix is usually the name of your VM):</span></span>

![Identifiera VHD i lagringsbehållaren](./media/troubleshoot-recovery-disks-portal/storage-container.png)

<span data-ttu-id="ea6b6-133">Välj en befintlig virtuell hårddisk i listan och kopiera Webbadressen för användning i följande steg:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-133">Select your existing virtual hard disk from the list and copy the URL for use in the following steps:</span></span>

![Kopiera URL: en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a><span data-ttu-id="ea6b6-135">Ta bort en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ea6b6-135">Delete existing VM</span></span>
<span data-ttu-id="ea6b6-136">Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-136">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="ea6b6-137">En virtuell hårddisk är där själva operativsystemet, program och konfigurationer lagras.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-137">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="ea6b6-138">Virtuellt datorn är enbart metadata som definierar storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="ea6b6-138">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="ea6b6-139">Varje virtuell hårddisk har tilldelats när ansluten till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-139">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="ea6b6-140">Datadiskar kan anslutas och kopplas från när den virtuella datorn körs, men operativsystemdisken kan inte kopplas från om inte den virtuella datorresursen tagits bort.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-140">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="ea6b6-141">Lånet fortsätter att koppla OS-disken till en virtuell dator även om den virtuella datorn är i tillståndet stoppad och frigjord.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-141">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="ea6b6-142">Det första steget att återställa den virtuella datorn är att ta bort den Virtuella datorresursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-142">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="ea6b6-143">När du tar bort den virtuella datorn hamnar de virtuella hårddiskarna på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-143">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="ea6b6-144">När den virtuella datorn har tagits bort, kopplar du den virtuella hårddisken till en annan virtuell dator för att felsöka och lösa problemen.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-144">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="ea6b6-145">Välj den virtuella datorn i portalen och klicka sedan på **ta bort**:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-145">Select your VM in the portal, then click **Delete**:</span></span>

![VM-Start diagnostik skärmbild som visar start-fel](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

<span data-ttu-id="ea6b6-147">Vänta tills den virtuella datorn har tagits bort innan du kopplar den virtuella hårddisken till en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-147">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="ea6b6-148">Lånet på den virtuella hårddisken som associeras med den virtuella datorn måste släppas innan du kan koppla den virtuella hårddisken till en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-148">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="ea6b6-149">Koppla en befintlig virtuell hårddisk till en annan virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ea6b6-149">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="ea6b6-150">För nästa steg använder du en annan virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-150">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="ea6b6-151">Du bifoga en befintlig virtuell hårddisk för den här felsökningsinformationen VM för att kunna bläddra och redigera dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-151">You attach the existing virtual hard disk to this troubleshooting VM to be able to browse and edit the disk's content.</span></span> <span data-ttu-id="ea6b6-152">Den här processen kan du korrigera eventuella fel i programkonfigurationen eller granska ytterligare program eller system loggfiler, till exempel.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-152">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="ea6b6-153">Välj eller skapa en annan virtuell dator ska användas för felsökning.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-153">Choose or create another VM to use for troubleshooting purposes.</span></span>

1. <span data-ttu-id="ea6b6-154">Välj din resursgrupp från portalen och välj sedan den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-154">Select your resource group from the portal, then select your troubleshooting VM.</span></span> <span data-ttu-id="ea6b6-155">Välj **diskar** och klicka sedan på **bifoga befintliga**:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-155">Select **Disks** and then click **Attach existing**:</span></span>

    ![Bifoga den befintliga disken i portalen](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. <span data-ttu-id="ea6b6-157">Välj din befintliga virtuella hårddisk genom att klicka på **VHD-fil**:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-157">To select your existing virtual hard disk, click **VHD File**:</span></span>

    ![Bläddra till den befintliga virtuella hårddisken](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. <span data-ttu-id="ea6b6-159">Välj ditt lagringskonto och en behållare och sedan på den befintliga virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-159">Select your storage account and container, then click your existing VHD.</span></span> <span data-ttu-id="ea6b6-160">Klicka på den **Välj** för att bekräfta valet:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-160">Click the **Select** button to confirm your choice:</span></span>

    ![Välj din befintliga virtuella hårddisk](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. <span data-ttu-id="ea6b6-162">Med den virtuella Hårddisken nu markerade, klickar du på **OK** att koppla en befintlig virtuell hårddisk:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-162">With your VHD now selected, click **OK** to attach the existing virtual hard disk:</span></span>

    ![Bekräfta att bifoga en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. <span data-ttu-id="ea6b6-164">Efter några sekunder den **diskar** fönstret för den virtuella datorn visas en befintlig virtuell hårddisk ansluten som en datadisk:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-164">After a few seconds, the **Disks** pane for your VM lists your existing virtual hard disk connected as a data disk:</span></span>

    ![Befintlig virtuell hårddisk ansluten som en datadisk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="ea6b6-166">Bifogade data disken</span><span class="sxs-lookup"><span data-stu-id="ea6b6-166">Mount the attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="ea6b6-167">I följande exempel innehåller information om de steg som krävs på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-167">The following examples detail the steps required on an Ubuntu VM.</span></span> <span data-ttu-id="ea6b6-168">Om du använder en annan Linux distro, till exempel Red Hat Enterprise Linux eller SUSE, loggen filplatser och `mount` kommandon kan vara lite annorlunda.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-168">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, the log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="ea6b6-169">Finns i dokumentationen för din specifika distro för ändringarna i kommandona.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-169">Refer to the documentation for your specific distro for the appropriate changes in commands.</span></span>

1. <span data-ttu-id="ea6b6-170">SSH till den felsökning virtuella datorn med rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-170">SSH to your troubleshooting VM using the appropriate credentials.</span></span> <span data-ttu-id="ea6b6-171">Om den här disken är den första datadisk som är kopplade till den virtuella datorn med felsökning, det förmodligen är anslutet till `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-171">If this disk is the first data disk attached to your troubleshooting VM, it is likely connected to `/dev/sdc`.</span></span> <span data-ttu-id="ea6b6-172">Använd `dmseg` att visa anslutna diskar:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-172">Use `dmseg` to list attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```
    <span data-ttu-id="ea6b6-173">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-173">The output is similar to the following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="ea6b6-174">I föregående exempel är OS-disken på `/dev/sda` och tillfällig disketten för varje virtuell dator är på `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-174">In the preceding example, the OS disk is at `/dev/sda` and the temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="ea6b6-175">Om du har flera datadiskar, bör de visas på `/dev/sdd`, `/dev/sde`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-175">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="ea6b6-176">Skapa en katalog om du vill montera en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-176">Create a directory to mount your existing virtual hard disk.</span></span> <span data-ttu-id="ea6b6-177">I följande exempel skapas en katalog med namnet `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-177">The following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="ea6b6-178">Om du har flera partitioner i en befintlig virtuell hårddisk kan du montera partitionen som krävs.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-178">If you have multiple partitions on your existing virtual hard disk, mount the required partition.</span></span> <span data-ttu-id="ea6b6-179">I följande exempel monterar den första primära partitionen på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-179">The following example mounts the first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="ea6b6-180">Det är bra att montera hårddiskar på virtuella datorer i Azure med hjälp av universellt Unik identifierare (UUID) för den virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-180">Best practice is to mount data disks on VMs in Azure using the universally unique identifier (UUID) of the virtual hard disk.</span></span> <span data-ttu-id="ea6b6-181">Den här korta felsökning scenariot är montera den virtuella hårddisken med hjälp av UUID inte nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-181">For this short troubleshooting scenario, mounting the virtual hard disk using the UUID is not necessary.</span></span> <span data-ttu-id="ea6b6-182">Men vid normal användning, redigering `/etc/fstab` för att montera den virtuella hårddiskar med enhetens namn i stället för UUID kanske den virtuella datorn inte startar.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-182">However, under normal use, editing `/etc/fstab` to mount virtual hard disks using device name rather than UUID may cause the VM to fail to boot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="ea6b6-183">Åtgärda problemen på den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="ea6b6-183">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="ea6b6-184">Med den befintliga virtuella hårddisken monteras, kan du nu utföra eventuella underhåll och felsökning efter behov.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-184">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="ea6b6-185">När du har åtgärdat problemen fortsätter du med följande steg.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-185">Once you have addressed the issues, continue with the following steps.</span></span>

## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="ea6b6-186">Demontera och koppla från den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="ea6b6-186">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="ea6b6-187">Koppla bort den befintliga virtuella hårddisken från den virtuella datorn med felsökning när din fel har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-187">Once your errors are resolved, detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="ea6b6-188">Du kan inte använda din virtuella hårddisk med andra Virtuella förrän lånet bifoga den virtuella hårddisken till Virtuellt datorn felsökning släpps.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-188">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="ea6b6-189">Demontera den befintliga virtuella hårddisken från SSH-session till den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-189">From the SSH session to your troubleshooting VM, unmount the existing virtual hard disk.</span></span> <span data-ttu-id="ea6b6-190">Ändra utanför den överordnade katalogen för din monteringspunkt:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-190">Change out of the parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="ea6b6-191">Nu demontera befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-191">Now unmount the existing virtual hard disk.</span></span> <span data-ttu-id="ea6b6-192">I följande exempel demonterar enheten på `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-192">The following example unmounts the device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="ea6b6-193">Nu ska du koppla från den virtuella hårddisken från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-193">Now detach the virtual hard disk from the VM.</span></span> <span data-ttu-id="ea6b6-194">Välj den virtuella datorn i portalen och klicka på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-194">Select your VM in the portal and click **Disks**.</span></span> <span data-ttu-id="ea6b6-195">Välj en befintlig virtuell hårddisk och klicka sedan på **Detach**:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-195">Select your existing virtual hard disk and then click **Detach**:</span></span>

    ![Koppla från en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    <span data-ttu-id="ea6b6-197">Vänta tills den virtuella datorn har kopplats från datadisken innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-197">Wait until the VM has successfully detached the data disk before continuing.</span></span>

## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="ea6b6-198">Skapa virtuell dator från den ursprungliga hårddisken</span><span class="sxs-lookup"><span data-stu-id="ea6b6-198">Create VM from original hard disk</span></span>
<span data-ttu-id="ea6b6-199">Så här skapar du en virtuell dator från den ursprungliga virtuella hårddisken [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="ea6b6-199">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="ea6b6-200">Mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av VHD-Webbadressen från tidigare kommandot.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-200">The template deploys a VM into an existing virtual network, using the VHD URL from the earlier command.</span></span> <span data-ttu-id="ea6b6-201">Klicka på den **till Azure** knappen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-201">Click the **Deploy to Azure** button as follows:</span></span>

![Distribuera virtuell dator från mall från GitHub](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

<span data-ttu-id="ea6b6-203">Mallen läses in i Azure-portalen för distribution.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-203">The template is loaded into the Azure portal for deployment.</span></span> <span data-ttu-id="ea6b6-204">Ange namn för din nya virtuella datorn och befintliga Azure-resurser och klistra in Webbadressen till en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-204">Enter the names for your new VM and existing Azure resources, and paste the URL to your existing virtual hard disk.</span></span> <span data-ttu-id="ea6b6-205">Om du vill starta distributionen, klickar du på **inköp**:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-205">To begin the deployment, click **Purchase**:</span></span>

![Distribuera virtuell dator från mall](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="ea6b6-207">Återaktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="ea6b6-207">Re-enable boot diagnostics</span></span>
<span data-ttu-id="ea6b6-208">När du skapar den virtuella datorn från den befintliga virtuella hårddisken får startdiagnostikinställningar inte automatiskt aktiveras.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-208">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="ea6b6-209">Om du vill kontrollera status för startdiagnostikinställningar och aktivera vid behov, väljer du den virtuella datorn i portalen.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-209">To check the status of boot diagnostics and turn on if needed, select your VM in the portal.</span></span> <span data-ttu-id="ea6b6-210">Under **övervakning**, klickar du på **diagnostikinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-210">Under **Monitoring**, click **Diagnostics settings**.</span></span> <span data-ttu-id="ea6b6-211">Se till att statusen är **på**, och kryssrutan bredvid **starta diagnostik** är markerad.</span><span class="sxs-lookup"><span data-stu-id="ea6b6-211">Ensure the status is **On**, and the check mark next to **Boot diagnostics** is selected.</span></span> <span data-ttu-id="ea6b6-212">Om du gör några ändringar klickar du på **spara**:</span><span class="sxs-lookup"><span data-stu-id="ea6b6-212">If you make any changes, click **Save**:</span></span>

![Uppdatera startdiagnostikinställningar](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="ea6b6-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ea6b6-214">Next steps</span></span>
<span data-ttu-id="ea6b6-215">Om du har problem med anslutningen till den virtuella datorn finns [felsökning av SSH-anslutningar till en Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ea6b6-215">If you are having issues connecting to your VM, see [Troubleshoot SSH connections to an Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="ea6b6-216">Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ea6b6-216">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="ea6b6-217">Mer information om hur du använder Resource Manager finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ea6b6-217">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
