---
title: "Använda en Windows-felsökning VM i Azure portal | Microsoft Docs"
description: "Lär dig hur du felsöker problem med Windows virtuell dator i Azure genom att ansluta OS-disken till en VM som använder Azure portal för återställning"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 2c1524949931d69d7553d284bb92c550a61c521a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a><span data-ttu-id="ff649-103">Felsöka en virtuell Windows-dator genom att koppla OS-disken till en VM som använder Azure portal för återställning</span><span class="sxs-lookup"><span data-stu-id="ff649-103">Troubleshoot a Windows VM by attaching the OS disk to a recovery VM using the Azure portal</span></span>
<span data-ttu-id="ff649-104">Om din Windows-dator (VM) i Azure påträffar ett fel vid start- eller disk, kan du behöva utför felsökning på den virtuella hårddisken sig själv.</span><span class="sxs-lookup"><span data-stu-id="ff649-104">If your Windows virtual machine (VM) in Azure encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="ff649-105">Ett vanligt exempel är en uppdatering för det program som förhindrar den virtuella datorn ska starta.</span><span class="sxs-lookup"><span data-stu-id="ff649-105">A common example would be a failed application update that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="ff649-106">Den här artikeln beskriver hur du använder Azure-portalen för att ansluta den virtuella hårddisken till en annan Windows virtuell dator och åtgärda eventuella fel och återskapa den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ff649-106">This article details how to use the Azure portal to connect your virtual hard disk to another Windows VM to fix any errors, then re-create your original VM.</span></span>

## <a name="recovery-process-overview"></a><span data-ttu-id="ff649-107">Översikt över återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="ff649-107">Recovery process overview</span></span>
<span data-ttu-id="ff649-108">Så här ser felsökningsprocessen ut:</span><span class="sxs-lookup"><span data-stu-id="ff649-108">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="ff649-109">Ta bort den virtuella datorn får problem, hålla de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="ff649-109">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="ff649-110">Anslut och montera den virtuella hårddisken till en annan Windows virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="ff649-110">Attach and mount the virtual hard disk to another Windows VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="ff649-111">Anslut till den virtuella felsökningsdatorn.</span><span class="sxs-lookup"><span data-stu-id="ff649-111">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="ff649-112">Redigera filer eller köra några verktyg för att åtgärda problem på den ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ff649-112">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="ff649-113">Demontera och koppla från den virtuella hårddisken från den virtuella felsökningsdatorn.</span><span class="sxs-lookup"><span data-stu-id="ff649-113">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="ff649-114">Skapa en virtuell dator med hjälp av den ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ff649-114">Create a VM using the original virtual hard disk.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="ff649-115">Fastställa startproblem</span><span class="sxs-lookup"><span data-stu-id="ff649-115">Determine boot issues</span></span>
<span data-ttu-id="ff649-116">Granska startdiagnostikinställningar skärmbilden Virtuella för att avgöra varför den virtuella datorn kan inte starta korrekt.</span><span class="sxs-lookup"><span data-stu-id="ff649-116">To determine why your VM is not able to boot correctly, examine the boot diagnostics VM screenshot.</span></span> <span data-ttu-id="ff649-117">Ett vanligt exempel skulle vara en programuppdatering för misslyckade eller en underliggande virtuell hårddisk som tagits bort eller flyttats.</span><span class="sxs-lookup"><span data-stu-id="ff649-117">A common example would be a failed application update, or an underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="ff649-118">Välj den virtuella datorn i portalen och rulla ned till den **stöd + felsökning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ff649-118">Select your VM in the portal and then scroll down to the **Support + Troubleshooting** section.</span></span> <span data-ttu-id="ff649-119">Klicka på **starta diagnostik** att visa skärmbilden.</span><span class="sxs-lookup"><span data-stu-id="ff649-119">Click **Boot diagnostics** to view the screenshot.</span></span> <span data-ttu-id="ff649-120">Observera eventuella felmeddelanden eller felkoder för att kunna avgöra varför den virtuella datorn har uppstått ett problem.</span><span class="sxs-lookup"><span data-stu-id="ff649-120">Note any specific error messages or error codes to help determine why the VM is encountering an issue.</span></span> <span data-ttu-id="ff649-121">I följande exempel visas en virtuell dator som väntar på att stoppa tjänster:</span><span class="sxs-lookup"><span data-stu-id="ff649-121">The following example shows a VM waiting on stopping services:</span></span>

![Visa VM konsolen startdiagnostikinställningar loggar](./media/troubleshoot-recovery-disks-portal/screenshot-error.png)

<span data-ttu-id="ff649-123">Du kan också klicka på **skärmbild** att hämta en avbildning av VM-skärmbilden.</span><span class="sxs-lookup"><span data-stu-id="ff649-123">You can also click **Screenshot** to download a capture of the VM screenshot.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="ff649-124">Visa information om befintlig virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="ff649-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="ff649-125">Innan du kan koppla den virtuella hårddisken till en annan virtuell dator, måste du identifiera namnet på den virtuella hårddisken (VHD).</span><span class="sxs-lookup"><span data-stu-id="ff649-125">Before you can attach your virtual hard disk to another VM, you need to identify the name of the virtual hard disk (VHD).</span></span> 

<span data-ttu-id="ff649-126">Välj din resursgrupp från portalen och sedan ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ff649-126">Select your resource group from the portal, then select your storage account.</span></span> <span data-ttu-id="ff649-127">Klicka på **Blobbar**, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="ff649-127">Click **Blobs**, as in the following example:</span></span>

![Välj storage-blobbar](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

<span data-ttu-id="ff649-129">Du har vanligtvis en behållare med namnet **virtuella hårddiskar** som lagrar dina virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="ff649-129">Typically you have a container named **vhds** that stores your virtual hard disks.</span></span> <span data-ttu-id="ff649-130">Välj behållare för att visa en lista över virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="ff649-130">Select the container to view a list of virtual hard disks.</span></span> <span data-ttu-id="ff649-131">Notera namnet på den virtuella Hårddisken (prefixet är vanligtvis namnet på den virtuella datorn):</span><span class="sxs-lookup"><span data-stu-id="ff649-131">Note the name of your VHD (the prefix is usually the name of your VM):</span></span>

![Identifiera VHD i lagringsbehållaren](./media/troubleshoot-recovery-disks-portal/storage-container.png)

<span data-ttu-id="ff649-133">Välj en befintlig virtuell hårddisk i listan och kopiera Webbadressen för användning i följande steg:</span><span class="sxs-lookup"><span data-stu-id="ff649-133">Select your existing virtual hard disk from the list and copy the URL for use in the following steps:</span></span>

![Kopiera URL: en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a><span data-ttu-id="ff649-135">Ta bort en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ff649-135">Delete existing VM</span></span>
<span data-ttu-id="ff649-136">Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="ff649-136">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="ff649-137">En virtuell hårddisk är där själva operativsystemet, program och konfigurationer lagras.</span><span class="sxs-lookup"><span data-stu-id="ff649-137">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="ff649-138">Virtuellt datorn är enbart metadata som definierar storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="ff649-138">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="ff649-139">Varje virtuell hårddisk har tilldelats när ansluten till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ff649-139">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="ff649-140">Datadiskar kan anslutas och kopplas från när den virtuella datorn körs, men operativsystemdisken kan inte kopplas från om inte den virtuella datorresursen tagits bort.</span><span class="sxs-lookup"><span data-stu-id="ff649-140">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="ff649-141">Lånet fortsätter att koppla OS-disken till en virtuell dator även om den virtuella datorn är i tillståndet stoppad och frigjord.</span><span class="sxs-lookup"><span data-stu-id="ff649-141">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="ff649-142">Det första steget att återställa den virtuella datorn är att ta bort den Virtuella datorresursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="ff649-142">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="ff649-143">När du tar bort den virtuella datorn hamnar de virtuella hårddiskarna på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ff649-143">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="ff649-144">När den virtuella datorn har tagits bort, kopplar du den virtuella hårddisken till en annan virtuell dator för att felsöka och lösa problemen.</span><span class="sxs-lookup"><span data-stu-id="ff649-144">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="ff649-145">Välj den virtuella datorn i portalen och klicka sedan på **ta bort**:</span><span class="sxs-lookup"><span data-stu-id="ff649-145">Select your VM in the portal, then click **Delete**:</span></span>

![VM-Start diagnostik skärmbild som visar start-fel](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

<span data-ttu-id="ff649-147">Vänta tills den virtuella datorn har tagits bort innan du kopplar den virtuella hårddisken till en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ff649-147">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="ff649-148">Lånet på den virtuella hårddisken som associeras med den virtuella datorn måste släppas innan du kan koppla den virtuella hårddisken till en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ff649-148">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="ff649-149">Koppla en befintlig virtuell hårddisk till en annan virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ff649-149">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="ff649-150">För nästa steg använder du en annan virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="ff649-150">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="ff649-151">Du bifoga en befintlig virtuell hårddisk för den här felsökningsinformationen VM för att kunna bläddra och redigera dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="ff649-151">You attach the existing virtual hard disk to this troubleshooting VM to be able to browse and edit the disk's content.</span></span> <span data-ttu-id="ff649-152">Den här processen kan du korrigera eventuella fel i programkonfigurationen eller granska ytterligare program eller system loggfiler, till exempel.</span><span class="sxs-lookup"><span data-stu-id="ff649-152">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="ff649-153">Välj eller skapa en annan virtuell dator ska användas för felsökning.</span><span class="sxs-lookup"><span data-stu-id="ff649-153">Choose or create another VM to use for troubleshooting purposes.</span></span>

1. <span data-ttu-id="ff649-154">Välj din resursgrupp från portalen och välj sedan den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="ff649-154">Select your resource group from the portal, then select your troubleshooting VM.</span></span> <span data-ttu-id="ff649-155">Välj **diskar** och klicka sedan på **bifoga befintliga**:</span><span class="sxs-lookup"><span data-stu-id="ff649-155">Select **Disks** and then click **Attach existing**:</span></span>

    ![Bifoga den befintliga disken i portalen](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. <span data-ttu-id="ff649-157">Välj din befintliga virtuella hårddisk genom att klicka på **VHD-fil**:</span><span class="sxs-lookup"><span data-stu-id="ff649-157">To select your existing virtual hard disk, click **VHD File**:</span></span>

    ![Bläddra till den befintliga virtuella hårddisken](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. <span data-ttu-id="ff649-159">Välj ditt lagringskonto och en behållare och sedan på den befintliga virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ff649-159">Select your storage account and container, then click your existing VHD.</span></span> <span data-ttu-id="ff649-160">Klicka på den **Välj** för att bekräfta valet:</span><span class="sxs-lookup"><span data-stu-id="ff649-160">Click the **Select** button to confirm your choice:</span></span>

    ![Välj din befintliga virtuella hårddisk](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. <span data-ttu-id="ff649-162">Med den virtuella Hårddisken nu markerade, klickar du på **OK** att koppla en befintlig virtuell hårddisk:</span><span class="sxs-lookup"><span data-stu-id="ff649-162">With your VHD now selected, click **OK** to attach the existing virtual hard disk:</span></span>

    ![Bekräfta att bifoga en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. <span data-ttu-id="ff649-164">Efter några sekunder den **diskar** fönstret för den virtuella datorn visas en befintlig virtuell hårddisk ansluten som en datadisk:</span><span class="sxs-lookup"><span data-stu-id="ff649-164">After a few seconds, the **Disks** pane for your VM lists your existing virtual hard disk connected as a data disk:</span></span>

    ![Befintlig virtuell hårddisk ansluten som en datadisk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="ff649-166">Bifogade data disken</span><span class="sxs-lookup"><span data-stu-id="ff649-166">Mount the attached data disk</span></span>

1. <span data-ttu-id="ff649-167">Öppna en fjärrskrivbordsanslutning till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ff649-167">Open a Remote Desktop connection to your VM.</span></span> <span data-ttu-id="ff649-168">Välj den virtuella datorn i portalen och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="ff649-168">Select your VM in the portal and click **Connect**.</span></span> <span data-ttu-id="ff649-169">Ladda ned och öppna filen RDP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="ff649-169">Download and open the RDP connection file.</span></span> <span data-ttu-id="ff649-170">Ange dina autentiseringsuppgifter för att logga in på den virtuella datorn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ff649-170">Enter your credentials to log in to your VM as follows:</span></span>

    ![Logga in på den virtuella datorn med hjälp av fjärrskrivbord](./media/troubleshoot-recovery-disks-portal/open-remote-desktop.png)

2. <span data-ttu-id="ff649-172">Öppna **Serverhanteraren**och välj **fil- och lagringstjänster**.</span><span class="sxs-lookup"><span data-stu-id="ff649-172">Open **Server Manager**, then select **File and Storage Services**.</span></span> 

    ![Välj fil- och lagringstjänster i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

3. <span data-ttu-id="ff649-174">Datadisken upptäckt och kopplade automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ff649-174">The data disk is automatically detected and attached.</span></span> <span data-ttu-id="ff649-175">Om du vill se en lista över anslutna diskar, Välj **diskar**.</span><span class="sxs-lookup"><span data-stu-id="ff649-175">To see a list of the connected disks, select **Disks**.</span></span> <span data-ttu-id="ff649-176">Du kan välja din datadisk för att visa volyminformation om, inklusive enhetsbeteckningen.</span><span class="sxs-lookup"><span data-stu-id="ff649-176">You can select your data disk to view volume information, including the drive letter.</span></span> <span data-ttu-id="ff649-177">I följande exempel visas datadisk ansluten och använder **F:**:</span><span class="sxs-lookup"><span data-stu-id="ff649-177">The following example shows the data disk attached and using **F:**:</span></span>

    ![Disk som är ansluten och volyminformation i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="ff649-179">Åtgärda problemen på den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="ff649-179">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="ff649-180">Med den befintliga virtuella hårddisken monteras, kan du nu utföra eventuella underhåll och felsökning efter behov.</span><span class="sxs-lookup"><span data-stu-id="ff649-180">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="ff649-181">När du har åtgärdat problemen fortsätter du med följande steg.</span><span class="sxs-lookup"><span data-stu-id="ff649-181">Once you have addressed the issues, continue with the following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="ff649-182">Demontera och koppla från den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="ff649-182">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="ff649-183">Koppla bort den befintliga virtuella hårddisken från den virtuella datorn med felsökning när din fel har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="ff649-183">Once your errors are resolved, detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="ff649-184">Du kan inte använda din virtuella hårddisk med andra Virtuella förrän lånet bifoga den virtuella hårddisken till Virtuellt datorn felsökning släpps.</span><span class="sxs-lookup"><span data-stu-id="ff649-184">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="ff649-185">RDP-session till den virtuella datorn, öppna **Serverhanteraren**och välj **fil- och lagringstjänster**:</span><span class="sxs-lookup"><span data-stu-id="ff649-185">From the RDP session to your VM, open **Server Manager**, then select **File and Storage Services**:</span></span>

    ![Välj fil- och lagringstjänster i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

2. <span data-ttu-id="ff649-187">Välj **diskar** och välj din datadisk.</span><span class="sxs-lookup"><span data-stu-id="ff649-187">Select **Disks** and then select your data disk.</span></span> <span data-ttu-id="ff649-188">Högerklicka på din datadisk och välj **koppla från**:</span><span class="sxs-lookup"><span data-stu-id="ff649-188">Right-click on your data disk and select **Take Offline**:</span></span>

    ![Ange datadisken som offline i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-set-disk-offline.png)

3. <span data-ttu-id="ff649-190">Nu ska du koppla från den virtuella hårddisken från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ff649-190">Now detach the virtual hard disk from the VM.</span></span> <span data-ttu-id="ff649-191">Välj den virtuella datorn i Azure-portalen och klicka på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="ff649-191">Select your VM in the Azure portal and click **Disks**.</span></span> <span data-ttu-id="ff649-192">Välj en befintlig virtuell hårddisk och klicka sedan på **Detach**:</span><span class="sxs-lookup"><span data-stu-id="ff649-192">Select your existing virtual hard disk and then click **Detach**:</span></span>

    ![Koppla från en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    <span data-ttu-id="ff649-194">Vänta tills den virtuella datorn har kopplats från datadisken innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="ff649-194">Wait until the VM has successfully detached the data disk before continuing.</span></span>

## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="ff649-195">Skapa virtuell dator från den ursprungliga hårddisken</span><span class="sxs-lookup"><span data-stu-id="ff649-195">Create VM from original hard disk</span></span>
<span data-ttu-id="ff649-196">Så här skapar du en virtuell dator från den ursprungliga virtuella hårddisken [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="ff649-196">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="ff649-197">Mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av VHD-Webbadressen från tidigare kommandot.</span><span class="sxs-lookup"><span data-stu-id="ff649-197">The template deploys a VM into an existing virtual network, using the VHD URL from the earlier command.</span></span> <span data-ttu-id="ff649-198">Klicka på den **till Azure** knappen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ff649-198">Click the **Deploy to Azure** button as follows:</span></span>

![Distribuera virtuell dator från mall från Github](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

<span data-ttu-id="ff649-200">Mallen läses in i Azure-portalen för distribution.</span><span class="sxs-lookup"><span data-stu-id="ff649-200">The template is loaded into the Azure portal for deployment.</span></span> <span data-ttu-id="ff649-201">Ange namn för din nya virtuella datorn och befintliga Azure-resurser och klistra in Webbadressen till en befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="ff649-201">Enter the names for your new VM and existing Azure resources, and paste the URL to your existing virtual hard disk.</span></span> <span data-ttu-id="ff649-202">Om du vill starta distributionen, klickar du på **inköp**:</span><span class="sxs-lookup"><span data-stu-id="ff649-202">To begin the deployment, click **Purchase**:</span></span>

![Distribuera virtuell dator från mall](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="ff649-204">Återaktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="ff649-204">Re-enable boot diagnostics</span></span>
<span data-ttu-id="ff649-205">När du skapar den virtuella datorn från den befintliga virtuella hårddisken får startdiagnostikinställningar inte automatiskt aktiveras.</span><span class="sxs-lookup"><span data-stu-id="ff649-205">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="ff649-206">Om du vill kontrollera status för startdiagnostikinställningar och aktivera vid behov, väljer du den virtuella datorn i portalen.</span><span class="sxs-lookup"><span data-stu-id="ff649-206">To check the status of boot diagnostics and turn on if needed, select your VM in the portal.</span></span> <span data-ttu-id="ff649-207">Under **övervakning**, klickar du på **diagnostikinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="ff649-207">Under **Monitoring**, click **Diagnostics settings**.</span></span> <span data-ttu-id="ff649-208">Se till att statusen är **på**, och kryssrutan bredvid **starta diagnostik** är markerad.</span><span class="sxs-lookup"><span data-stu-id="ff649-208">Ensure the status is **On**, and the check mark next to **Boot diagnostics** is selected.</span></span> <span data-ttu-id="ff649-209">Om du gör några ändringar klickar du på **spara**:</span><span class="sxs-lookup"><span data-stu-id="ff649-209">If you make any changes, click **Save**:</span></span>

![Uppdatera startdiagnostikinställningar](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="ff649-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ff649-211">Next steps</span></span>
<span data-ttu-id="ff649-212">Om du har problem med anslutningen till den virtuella datorn finns [felsökning av RDP-anslutningar till en Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff649-212">If you are having issues connecting to your VM, see [Troubleshoot RDP connections to an Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="ff649-213">Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Windows-VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff649-213">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="ff649-214">Mer information om hur du använder Resource Manager finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff649-214">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>