---
title: "aaaUse a Windows VM felsökning i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tootroubleshoot virtuella problem med Windows i Azure med anslutande hello OS disk tooa recovery VM hello Azure-portalen"
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
ms.openlocfilehash: 396f70338fa39f80bb9adcb9244d3c83f2a233eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a><span data-ttu-id="06f87-103">Felsöka en virtuell Windows-dator genom att koppla hello OS tooa diskåterställning VM med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="06f87-103">Troubleshoot a Windows VM by attaching hello OS disk tooa recovery VM using hello Azure portal</span></span>
<span data-ttu-id="06f87-104">Om din Windows-dator (VM) i Azure påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv.</span><span class="sxs-lookup"><span data-stu-id="06f87-104">If your Windows virtual machine (VM) in Azure encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="06f87-105">Ett vanligt exempel är en programuppdatering för misslyckade som förhindrar hello VM kan tooboot har.</span><span class="sxs-lookup"><span data-stu-id="06f87-105">A common example would be a failed application update that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="06f87-106">Det här artikeln beskriver hur toouse hello Azure portal tooconnect din virtuella hårddisk disk tooanother Windows VM toofix eventuella fel och sedan återskapa den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="06f87-106">This article details how toouse hello Azure portal tooconnect your virtual hard disk tooanother Windows VM toofix any errors, then re-create your original VM.</span></span>

## <a name="recovery-process-overview"></a><span data-ttu-id="06f87-107">Översikt över återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="06f87-107">Recovery process overview</span></span>
<span data-ttu-id="06f87-108">hello felsökningsprocessen är följande:</span><span class="sxs-lookup"><span data-stu-id="06f87-108">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="06f87-109">Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="06f87-109">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="06f87-110">Anslut och montera hello virtuell hårddisk tooanother Windows VM i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="06f87-110">Attach and mount hello virtual hard disk tooanother Windows VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="06f87-111">Ansluta toohello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="06f87-111">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="06f87-112">Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="06f87-112">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="06f87-113">Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="06f87-113">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="06f87-114">Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="06f87-114">Create a VM using hello original virtual hard disk.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="06f87-115">Fastställa startproblem</span><span class="sxs-lookup"><span data-stu-id="06f87-115">Determine boot issues</span></span>
<span data-ttu-id="06f87-116">toodetermine till den virtuella datorn inte är kan tooboot korrekt, granska hello startdiagnostikinställningar VM skärmbild.</span><span class="sxs-lookup"><span data-stu-id="06f87-116">toodetermine why your VM is not able tooboot correctly, examine hello boot diagnostics VM screenshot.</span></span> <span data-ttu-id="06f87-117">Ett vanligt exempel skulle vara en programuppdatering för misslyckade eller en underliggande virtuell hårddisk som tagits bort eller flyttats.</span><span class="sxs-lookup"><span data-stu-id="06f87-117">A common example would be a failed application update, or an underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="06f87-118">Välj den virtuella datorn i hello-portalen och bläddrar sedan nedåt toohello **stöd + felsökning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="06f87-118">Select your VM in hello portal and then scroll down toohello **Support + Troubleshooting** section.</span></span> <span data-ttu-id="06f87-119">Klicka på **starta diagnostik** tooview hello skärmbild.</span><span class="sxs-lookup"><span data-stu-id="06f87-119">Click **Boot diagnostics** tooview hello screenshot.</span></span> <span data-ttu-id="06f87-120">Notera eventuella felmeddelanden eller felkoder toohelp avgöra varför hello VM har uppstått ett problem.</span><span class="sxs-lookup"><span data-stu-id="06f87-120">Note any specific error messages or error codes toohelp determine why hello VM is encountering an issue.</span></span> <span data-ttu-id="06f87-121">hello visas följande exempel en virtuell dator som väntar på att stoppa tjänster:</span><span class="sxs-lookup"><span data-stu-id="06f87-121">hello following example shows a VM waiting on stopping services:</span></span>

![Visa VM konsolen startdiagnostikinställningar loggar](./media/troubleshoot-recovery-disks-portal/screenshot-error.png)

<span data-ttu-id="06f87-123">Du kan också klicka på **skärmbild** toodownload en avbildning av hello VM skärmbild.</span><span class="sxs-lookup"><span data-stu-id="06f87-123">You can also click **Screenshot** toodownload a capture of hello VM screenshot.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="06f87-124">Visa information om befintlig virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="06f87-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="06f87-125">Innan du kan koppla en virtuell hårddisk tooanother VM, måste tooidentify hello namnet på hello virtuell hårddisk (VHD).</span><span class="sxs-lookup"><span data-stu-id="06f87-125">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span> 

<span data-ttu-id="06f87-126">Välj din resursgrupp från hello-portalen och sedan ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="06f87-126">Select your resource group from hello portal, then select your storage account.</span></span> <span data-ttu-id="06f87-127">Klicka på **Blobbar**, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="06f87-127">Click **Blobs**, as in hello following example:</span></span>

![Välj storage-blobbar](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

<span data-ttu-id="06f87-129">Du har vanligtvis en behållare med namnet **virtuella hårddiskar** som lagrar dina virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="06f87-129">Typically you have a container named **vhds** that stores your virtual hard disks.</span></span> <span data-ttu-id="06f87-130">Välj hello behållaren tooview en lista över virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="06f87-130">Select hello container tooview a list of virtual hard disks.</span></span> <span data-ttu-id="06f87-131">Obs hello namnet på den virtuella Hårddisken (hello prefix är vanligtvis hello namnet på den virtuella datorn):</span><span class="sxs-lookup"><span data-stu-id="06f87-131">Note hello name of your VHD (hello prefix is usually hello name of your VM):</span></span>

![Identifiera VHD i lagringsbehållaren](./media/troubleshoot-recovery-disks-portal/storage-container.png)

<span data-ttu-id="06f87-133">Välj en befintlig virtuell hårddisk hello listan och kopiera hello URL: en för användning i hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="06f87-133">Select your existing virtual hard disk from hello list and copy hello URL for use in hello following steps:</span></span>

![Kopiera URL: en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a><span data-ttu-id="06f87-135">Ta bort en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="06f87-135">Delete existing VM</span></span>
<span data-ttu-id="06f87-136">Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="06f87-136">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="06f87-137">En virtuell hårddisk är där hello själva operativsystemet, program och konfigurationer lagras.</span><span class="sxs-lookup"><span data-stu-id="06f87-137">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="06f87-138">hello Virtuella datorn är enbart metadata som definierar hello storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="06f87-138">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="06f87-139">Varje virtuell hårddisk har tilldelats när kopplade tooa VM.</span><span class="sxs-lookup"><span data-stu-id="06f87-139">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="06f87-140">Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort.</span><span class="sxs-lookup"><span data-stu-id="06f87-140">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="06f87-141">hello lån fortsätter tooassociate hello OS-disk med en virtuell dator, även om den virtuella datorn är i tillståndet stoppad och frigjord.</span><span class="sxs-lookup"><span data-stu-id="06f87-141">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="06f87-142">Hej första steg toorecover den virtuella datorn är toodelete hello Virtuella datorresursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="06f87-142">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="06f87-143">Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="06f87-143">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="06f87-144">Efter hello VM tas bort, bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel.</span><span class="sxs-lookup"><span data-stu-id="06f87-144">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="06f87-145">Välj den virtuella datorn i hello portal och klicka sedan på **ta bort**:</span><span class="sxs-lookup"><span data-stu-id="06f87-145">Select your VM in hello portal, then click **Delete**:</span></span>

![VM-Start diagnostik skärmbild som visar start-fel](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

<span data-ttu-id="06f87-147">Vänta tills hello VM har tagits bort innan du kopplar hello virtuell hårddisk tooanother VM.</span><span class="sxs-lookup"><span data-stu-id="06f87-147">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="06f87-148">hello lånet på hello virtuell hårddisk som associeras med hello VM måste släppas innan du kan koppla hello virtuell hårddisk tooanother VM toobe.</span><span class="sxs-lookup"><span data-stu-id="06f87-148">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="06f87-149">Bifoga en befintlig virtuell hårddisk tooanother VM</span><span class="sxs-lookup"><span data-stu-id="06f87-149">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="06f87-150">Hej bredvid för några steg kan du använda en annan virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="06f87-150">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="06f87-151">Du bifogar hello befintlig virtuell hårddisk toothis felsökning VM toobe kan toobrowse och redigera hello disk innehåll.</span><span class="sxs-lookup"><span data-stu-id="06f87-151">You attach hello existing virtual hard disk toothis troubleshooting VM toobe able toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="06f87-152">Den här processen kan toocorrect eventuella fel i programkonfigurationen eller granska ytterligare program eller loggfiler, till exempel.</span><span class="sxs-lookup"><span data-stu-id="06f87-152">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="06f87-153">Välj eller skapa en annan VM toouse för felsökning.</span><span class="sxs-lookup"><span data-stu-id="06f87-153">Choose or create another VM toouse for troubleshooting purposes.</span></span>

1. <span data-ttu-id="06f87-154">Välj din resursgrupp från hello-portalen, och välj sedan den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="06f87-154">Select your resource group from hello portal, then select your troubleshooting VM.</span></span> <span data-ttu-id="06f87-155">Välj **diskar** och klicka sedan på **bifoga befintliga**:</span><span class="sxs-lookup"><span data-stu-id="06f87-155">Select **Disks** and then click **Attach existing**:</span></span>

    ![Bifoga den befintliga disken i hello-portalen](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. <span data-ttu-id="06f87-157">tooselect befintliga virtuella hårddisken, klickar du på **VHD-filen**:</span><span class="sxs-lookup"><span data-stu-id="06f87-157">tooselect your existing virtual hard disk, click **VHD File**:</span></span>

    ![Bläddra till den befintliga virtuella hårddisken](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. <span data-ttu-id="06f87-159">Välj ditt lagringskonto och en behållare och sedan på den befintliga virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="06f87-159">Select your storage account and container, then click your existing VHD.</span></span> <span data-ttu-id="06f87-160">Klicka på hello **Välj** knappen tooconfirm ditt val:</span><span class="sxs-lookup"><span data-stu-id="06f87-160">Click hello **Select** button tooconfirm your choice:</span></span>

    ![Välj din befintliga virtuella hårddisk](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. <span data-ttu-id="06f87-162">Med den virtuella Hårddisken nu markerade, klickar du på **OK** tooattach hello befintlig virtuell hårddisk:</span><span class="sxs-lookup"><span data-stu-id="06f87-162">With your VHD now selected, click **OK** tooattach hello existing virtual hard disk:</span></span>

    ![Bekräfta att bifoga en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. <span data-ttu-id="06f87-164">Efter några sekunder hello **diskar** fönstret för den virtuella datorn visas en befintlig virtuell hårddisk ansluten som en datadisk:</span><span class="sxs-lookup"><span data-stu-id="06f87-164">After a few seconds, hello **Disks** pane for your VM lists your existing virtual hard disk connected as a data disk:</span></span>

    ![Befintlig virtuell hårddisk ansluten som en datadisk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="06f87-166">Montera hello bifogade datadisk</span><span class="sxs-lookup"><span data-stu-id="06f87-166">Mount hello attached data disk</span></span>

1. <span data-ttu-id="06f87-167">Öppna en anslutning till Fjärrskrivbord anslutning tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="06f87-167">Open a Remote Desktop connection tooyour VM.</span></span> <span data-ttu-id="06f87-168">Välj den virtuella datorn i hello portal och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="06f87-168">Select your VM in hello portal and click **Connect**.</span></span> <span data-ttu-id="06f87-169">Ladda ned och öppna hello RDP-anslutningsfil.</span><span class="sxs-lookup"><span data-stu-id="06f87-169">Download and open hello RDP connection file.</span></span> <span data-ttu-id="06f87-170">Ange dina autentiseringsuppgifter toolog i tooyour VM enligt följande:</span><span class="sxs-lookup"><span data-stu-id="06f87-170">Enter your credentials toolog in tooyour VM as follows:</span></span>

    ![Logga in tooyour VM med hjälp av fjärrskrivbord](./media/troubleshoot-recovery-disks-portal/open-remote-desktop.png)

2. <span data-ttu-id="06f87-172">Öppna **Serverhanteraren**och välj **fil- och lagringstjänster**.</span><span class="sxs-lookup"><span data-stu-id="06f87-172">Open **Server Manager**, then select **File and Storage Services**.</span></span> 

    ![Välj fil- och lagringstjänster i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

3. <span data-ttu-id="06f87-174">hello datadisk identifieras automatiskt och ansluten.</span><span class="sxs-lookup"><span data-stu-id="06f87-174">hello data disk is automatically detected and attached.</span></span> <span data-ttu-id="06f87-175">toosee en lista över hello-anslutna diskar, Välj **diskar**.</span><span class="sxs-lookup"><span data-stu-id="06f87-175">toosee a list of hello connected disks, select **Disks**.</span></span> <span data-ttu-id="06f87-176">Du kan välja din data tooview diskvolyminformation, inklusive hello enhetsbeteckning.</span><span class="sxs-lookup"><span data-stu-id="06f87-176">You can select your data disk tooview volume information, including hello drive letter.</span></span> <span data-ttu-id="06f87-177">Hej följande exempel visar hello datadisk som är ansluten och använda **F:**:</span><span class="sxs-lookup"><span data-stu-id="06f87-177">hello following example shows hello data disk attached and using **F:**:</span></span>

    ![Disk som är ansluten och volyminformation i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="06f87-179">Åtgärda problemen på den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="06f87-179">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="06f87-180">Med hello befintlig virtuell hårddisk monterad, kan du nu utföra eventuella underhåll och felsökning efter behov.</span><span class="sxs-lookup"><span data-stu-id="06f87-180">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="06f87-181">När du har åtgärdat hello problem, fortsätter du med hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="06f87-181">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="06f87-182">Demontera och koppla från den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="06f87-182">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="06f87-183">Koppla bort hello befintlig virtuell hårddisk från den virtuella datorn med felsökning när din fel har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="06f87-183">Once your errors are resolved, detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="06f87-184">Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.</span><span class="sxs-lookup"><span data-stu-id="06f87-184">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="06f87-185">Hello RDP-session tooyour VM, öppna **Serverhanteraren**och välj **fil- och lagringstjänster**:</span><span class="sxs-lookup"><span data-stu-id="06f87-185">From hello RDP session tooyour VM, open **Server Manager**, then select **File and Storage Services**:</span></span>

    ![Välj fil- och lagringstjänster i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

2. <span data-ttu-id="06f87-187">Välj **diskar** och välj din datadisk.</span><span class="sxs-lookup"><span data-stu-id="06f87-187">Select **Disks** and then select your data disk.</span></span> <span data-ttu-id="06f87-188">Högerklicka på din datadisk och välj **koppla från**:</span><span class="sxs-lookup"><span data-stu-id="06f87-188">Right-click on your data disk and select **Take Offline**:</span></span>

    ![Ange hello datadisk som offline i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-set-disk-offline.png)

3. <span data-ttu-id="06f87-190">Nu ska du koppla från hello virtuella hårddiskarna från hello VM.</span><span class="sxs-lookup"><span data-stu-id="06f87-190">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="06f87-191">Välj den virtuella datorn i hello Azure-portalen och klicka på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="06f87-191">Select your VM in hello Azure portal and click **Disks**.</span></span> <span data-ttu-id="06f87-192">Välj en befintlig virtuell hårddisk och klicka sedan på **Detach**:</span><span class="sxs-lookup"><span data-stu-id="06f87-192">Select your existing virtual hard disk and then click **Detach**:</span></span>

    ![Koppla från en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    <span data-ttu-id="06f87-194">Vänta tills hello VM har disk har kopplats bort hello data innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="06f87-194">Wait until hello VM has successfully detached hello data disk before continuing.</span></span>

## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="06f87-195">Skapa virtuell dator från den ursprungliga hårddisken</span><span class="sxs-lookup"><span data-stu-id="06f87-195">Create VM from original hard disk</span></span>
<span data-ttu-id="06f87-196">toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="06f87-196">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="06f87-197">hello mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av hello VHD URL från hello tidigare kommandot.</span><span class="sxs-lookup"><span data-stu-id="06f87-197">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="06f87-198">Klicka på hello **distribuera tooAzure** knappen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="06f87-198">Click hello **Deploy tooAzure** button as follows:</span></span>

![Distribuera virtuell dator från mall från Github](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

<span data-ttu-id="06f87-200">hello mallen läses in i hello Azure-portalen för distribution.</span><span class="sxs-lookup"><span data-stu-id="06f87-200">hello template is loaded into hello Azure portal for deployment.</span></span> <span data-ttu-id="06f87-201">Ange hello namn för din nya virtuella datorn och befintliga Azure-resurser och klistra in hello URL tooyour befintlig virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="06f87-201">Enter hello names for your new VM and existing Azure resources, and paste hello URL tooyour existing virtual hard disk.</span></span> <span data-ttu-id="06f87-202">toobegin hello distributionen, klickar du på **inköp**:</span><span class="sxs-lookup"><span data-stu-id="06f87-202">toobegin hello deployment, click **Purchase**:</span></span>

![Distribuera virtuell dator från mall](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="06f87-204">Återaktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="06f87-204">Re-enable boot diagnostics</span></span>
<span data-ttu-id="06f87-205">När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras.</span><span class="sxs-lookup"><span data-stu-id="06f87-205">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="06f87-206">toocheck hello status för startdiagnostikinställningar och starta om det behövs, Välj den virtuella datorn i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="06f87-206">toocheck hello status of boot diagnostics and turn on if needed, select your VM in hello portal.</span></span> <span data-ttu-id="06f87-207">Under **övervakning**, klickar du på **diagnostikinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="06f87-207">Under **Monitoring**, click **Diagnostics settings**.</span></span> <span data-ttu-id="06f87-208">Kontrollera status för hello är **på**, och hello bockmarkering bredvid för**starta diagnostik** är markerad.</span><span class="sxs-lookup"><span data-stu-id="06f87-208">Ensure hello status is **On**, and hello check mark next too**Boot diagnostics** is selected.</span></span> <span data-ttu-id="06f87-209">Om du gör några ändringar klickar du på **spara**:</span><span class="sxs-lookup"><span data-stu-id="06f87-209">If you make any changes, click **Save**:</span></span>

![Uppdatera startdiagnostikinställningar](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="06f87-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06f87-211">Next steps</span></span>
<span data-ttu-id="06f87-212">Om du har problem med anslutning tooyour VM finns [felsökning av RDP-anslutningar tooan Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="06f87-212">If you are having issues connecting tooyour VM, see [Troubleshoot RDP connections tooan Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="06f87-213">Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Windows-VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="06f87-213">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="06f87-214">Mer information om hur du använder Resource Manager finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="06f87-214">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>