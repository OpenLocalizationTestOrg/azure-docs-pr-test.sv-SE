---
title: "aaaReset ett lokalt Windows-lösenord utan Azure-agenten | Microsoft Docs"
description: "Hur tooreset hello lösenord för lokala Windows-användarkonto när hello Azure gästagenten inte har installerats eller fungerar på en virtuell dator"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/07/2017
ms.author: iainfou
ms.openlocfilehash: c559c31ea142f9cf50d2c5b6182c5355fec9bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-windows-password-for-azure-vm"></a><span data-ttu-id="cf7c9-103">Hur tooreset lokala Windows-lösenord för Azure VM</span><span class="sxs-lookup"><span data-stu-id="cf7c9-103">How tooreset local Windows password for Azure VM</span></span>
<span data-ttu-id="cf7c9-104">Du kan återställa hello lokala Windows-lösenord för en virtuell dator i Azure med hjälp av hello [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) angivna hello Azure gäst-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-104">You can reset hello local Windows password of a VM in Azure using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) provided hello Azure guest agent is installed.</span></span> <span data-ttu-id="cf7c9-105">Den här metoden är hello primära sätt tooreset ett lösenord för en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-105">This method is hello primary way tooreset a password for an Azure VM.</span></span> <span data-ttu-id="cf7c9-106">Om du får problem med hello Azure gästagent inte svarar eller misslyckas tooinstall efter överföring av en anpassad avbildning måste återställa du manuellt en Windows-lösenord.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-106">If you encounter issues with hello Azure guest agent not responding, or failing tooinstall after uploading a custom image, you can manually reset a Windows password.</span></span> <span data-ttu-id="cf7c9-107">Den här artikeln beskrivs hur tooreset ett lokalt kontolösenord genom att koppla hello källa OS virtuell disk tooanother VM.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-107">This article details how tooreset a local account password by attaching hello source OS virtual disk tooanother VM.</span></span> 

> [!WARNING]
> <span data-ttu-id="cf7c9-108">Endast använda den här processen som en sista utväg.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-108">Only use this process as a last resort.</span></span> <span data-ttu-id="cf7c9-109">Försök tooreset alltid ett lösenord med hello [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) första.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-109">Always try tooreset a password using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) first.</span></span>
> 
> 

## <a name="overview-of-hello-process"></a><span data-ttu-id="cf7c9-110">Översikt över hello-processen</span><span class="sxs-lookup"><span data-stu-id="cf7c9-110">Overview of hello process</span></span>
<span data-ttu-id="cf7c9-111">hello core stegen för att utföra en lokal återställning av lösenord för en virtuell Windows-dator i Azure när det finns ingen åtkomst toohello Azure gästagent är följande:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-111">hello core steps for performing a local password reset for a Windows VM in Azure when there is no access toohello Azure guest agent is as follows:</span></span>

* <span data-ttu-id="cf7c9-112">Ta bort Virtuella hello källdatorn.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-112">Delete hello source VM.</span></span> <span data-ttu-id="cf7c9-113">hello virtuella diskar bevaras.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-113">hello virtual disks are retained.</span></span>
* <span data-ttu-id="cf7c9-114">Koppla hello källa VM OS disk tooanother VM på hello samma plats i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-114">Attach hello source VM's OS disk tooanother VM on hello same location within your Azure subscription.</span></span> <span data-ttu-id="cf7c9-115">Den här virtuella datorn är refererad tooas hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-115">This VM is referred tooas hello troubleshooting VM.</span></span>
* <span data-ttu-id="cf7c9-116">Använd hello felsökning VM för att skapa vissa config-filer på hello källa VM OS-disken.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-116">Using hello troubleshooting VM, create some config files on hello source VM's OS disk.</span></span>
* <span data-ttu-id="cf7c9-117">Koppla bort hello VM OS-disken från hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-117">Detach hello VM's OS disk from hello troubleshooting VM.</span></span>
* <span data-ttu-id="cf7c9-118">Använd en virtuell dator med hello ursprungliga virtuella disken för toocreate en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-118">Use a Resource Manager template toocreate a VM, using hello original virtual disk.</span></span>
* <span data-ttu-id="cf7c9-119">När hello nya VM Starter hello konfigurationsfiler skapar du uppdateringen hello lösenordet för användaren som hello krävs.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-119">When hello new VM boots, hello config files you create update hello password of hello required user.</span></span>

## <a name="detailed-steps"></a><span data-ttu-id="cf7c9-120">Detaljerade steg</span><span class="sxs-lookup"><span data-stu-id="cf7c9-120">Detailed steps</span></span>
<span data-ttu-id="cf7c9-121">Försök tooreset alltid ett lösenord med hello [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du försöker hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-121">Always try tooreset a password using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before trying hello following steps.</span></span> <span data-ttu-id="cf7c9-122">Kontrollera att du har en säkerhetskopia av den virtuella datorn innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-122">Make sure you have a backup of your VM before you start.</span></span> 

1. <span data-ttu-id="cf7c9-123">Ta bort hello påverkas VM i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-123">Delete hello affected VM in Azure portal.</span></span> <span data-ttu-id="cf7c9-124">Ta bort hello VM tar bara bort hello metadata, hello referens för hello VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-124">Deleting hello VM only deletes hello metadata, hello reference of hello VM within Azure.</span></span> <span data-ttu-id="cf7c9-125">hello virtuella diskar bevaras när hello VM tas bort:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-125">hello virtual disks are retained when hello VM is deleted:</span></span>
   
   * <span data-ttu-id="cf7c9-126">Välj hello VM i hello Azure-portalen klickar du på *ta bort*:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-126">Select hello VM in hello Azure portal, click *Delete*:</span></span>
     
     ![Ta bort en befintlig virtuell dator](./media/reset-local-password-without-agent/delete_vm.png)
2. <span data-ttu-id="cf7c9-128">Koppla hello källa VM OS disk toohello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-128">Attach hello source VM’s OS disk toohello troubleshooting VM.</span></span> <span data-ttu-id="cf7c9-129">hello felsökning VM måste vara i hello samma region som hello källa VM OS-disken (som `West US`):</span><span class="sxs-lookup"><span data-stu-id="cf7c9-129">hello troubleshooting VM must be in hello same region as hello source VM's OS disk (such as `West US`):</span></span>
   
   * <span data-ttu-id="cf7c9-130">Välj hello felsökning VM i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-130">Select hello troubleshooting VM in hello Azure portal.</span></span> <span data-ttu-id="cf7c9-131">Klicka på *diskar* | *bifoga befintliga*:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-131">Click *Disks* | *Attach existing*:</span></span>
     
     ![Bifoga den befintliga disken](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     <span data-ttu-id="cf7c9-133">Välj *VHD-filen* och välj sedan hello storage-konto som innehåller ditt Virtuella källdatorn:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-133">Select *VHD File* and then select hello storage account that contains your source VM:</span></span>
     
     ![Välj lagringskonto](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     <span data-ttu-id="cf7c9-135">Välj hello käll-behållare.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-135">Select hello source container.</span></span> <span data-ttu-id="cf7c9-136">hello källa behållare är vanligtvis *virtuella hårddiskar*:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-136">hello source container is typically *vhds*:</span></span>
     
     ![Välj lagringsbehållare](./media/reset-local-password-without-agent/disks_select_container.png)
     
     <span data-ttu-id="cf7c9-138">Välj hello OS vhd tooattach.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-138">Select hello OS vhd tooattach.</span></span> <span data-ttu-id="cf7c9-139">Klicka på *Välj* toocomplete hello processen:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-139">Click *Select* toocomplete hello process:</span></span>
     
     ![Välj källa för virtuell disk](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. <span data-ttu-id="cf7c9-141">Ansluta toohello felsökning av virtuell dator med hjälp av fjärrskrivbord och säkerställa hello källa VM OS-disken visas:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-141">Connect toohello troubleshooting VM using Remote Desktop and ensure hello source VM's OS disk is visible:</span></span>
   
   * <span data-ttu-id="cf7c9-142">Välj hello felsökning VM i hello Azure-portalen och klicka på *Anslut*.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-142">Select hello troubleshooting VM in hello Azure portal and click *Connect*.</span></span>
   * <span data-ttu-id="cf7c9-143">Öppna hello RDP-filen som hämtas.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-143">Open hello RDP file that downloads.</span></span> <span data-ttu-id="cf7c9-144">Ange hello användarnamnet och lösenordet för hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-144">Enter hello username and password of hello troubleshooting VM.</span></span>
   * <span data-ttu-id="cf7c9-145">I Utforskaren, leta efter hello datadisk bifogade.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-145">In File Explorer, look for hello data disk you attached.</span></span> <span data-ttu-id="cf7c9-146">Om hello källa Virtuella datorns virtuella Hårddisken är hello endast data disk ansluten toohello felsökning VM, bör det vara hello F: enhet:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-146">If hello source VM’s VHD is hello only data disk attached toohello troubleshooting VM, it should be hello F: drive:</span></span>
     
     ![Visa bifogade datadisk](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. <span data-ttu-id="cf7c9-148">Skapa `gpt.ini` i `\Windows\System32\GroupPolicy` på hello källa VM-enhet (om det finns gpt.ini, byta namn på toogpt.ini.bak):</span><span class="sxs-lookup"><span data-stu-id="cf7c9-148">Create `gpt.ini` in `\Windows\System32\GroupPolicy` on hello source VM’s drive (if gpt.ini exists, rename toogpt.ini.bak):</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="cf7c9-149">Se till att du inte oavsiktligt skapa hello följande filer i C:\Windows, hello OS-enhet för hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-149">Make sure that you do not accidentally create hello following files in C:\Windows, hello OS drive for hello troubleshooting VM.</span></span> <span data-ttu-id="cf7c9-150">Skapa följande filer i hello OS-enhet för källan virtuell dator som är anslutna som en datadisk hello.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-150">Create hello following files in hello OS drive for your source VM that is attached as a data disk.</span></span>
   > 
   > 
   
   * <span data-ttu-id="cf7c9-151">Lägg till följande rader i hello hello `gpt.ini` fil som du skapade:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-151">Add hello following lines into hello `gpt.ini` file you created:</span></span>
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Skapa gpt.ini](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. <span data-ttu-id="cf7c9-153">Skapa `scripts.ini` i `\Windows\System32\GroupPolicy\Machine\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-153">Create `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`.</span></span> <span data-ttu-id="cf7c9-154">Kontrollera att dolda mappar som visas.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-154">Make sure hidden folders are shown.</span></span> <span data-ttu-id="cf7c9-155">Om det behövs, skapa hello `Machine` eller `Scripts` mappar.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-155">If needed, create hello `Machine` or `Scripts` folders.</span></span>
   
   * <span data-ttu-id="cf7c9-156">Lägg till följande rader hello hello `scripts.ini` fil som du skapade:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-156">Add hello following lines hello `scripts.ini` file you created:</span></span>
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Skapa scripts.ini](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. <span data-ttu-id="cf7c9-158">Skapa `FixAzureVM.cmd` i `\Windows\System32` med hello efter innehållet, ersätter `<username>` och `<newpassword>` med egna värden:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-158">Create `FixAzureVM.cmd` in `\Windows\System32` with hello following contents, replacing `<username>` and `<newpassword>` with your own values:</span></span>
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![Skapa FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    <span data-ttu-id="cf7c9-160">När du definierar hello nytt lösenord måste du uppfylla kraven på lösenordskomplexitet hello som konfigurerats för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-160">You must meet hello configured password complexity requirements for your VM when defining hello new password.</span></span>
7. <span data-ttu-id="cf7c9-161">Koppla bort hello disk från hello felsökning VM i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-161">In Azure portal, detach hello disk from hello troubleshooting VM:</span></span>
   
   * <span data-ttu-id="cf7c9-162">Välj hello felsökning VM i hello Azure-portalen, klicka på *diskar*.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-162">Select hello troubleshooting VM in hello Azure portal, click *Disks*.</span></span>
   * <span data-ttu-id="cf7c9-163">Välj hello datadisk kopplade i steg 2, klickar du på *Detach*:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-163">Select hello data disk attached in step 2, click *Detach*:</span></span>
     
     ![Koppla bort disk](./media/reset-local-password-without-agent/detach_disk.png)
8. <span data-ttu-id="cf7c9-165">Innan du skapar en virtuell dator måste du hämta hello URI tooyour källa OS-disken:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-165">Before you create a VM, obtain hello URI tooyour source OS disk:</span></span>
   
   * <span data-ttu-id="cf7c9-166">Välj hello storage-konto i hello Azure-portalen klickar du på *Blobbar*.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-166">Select hello storage account in hello Azure portal, click *Blobs*.</span></span>
   * <span data-ttu-id="cf7c9-167">Välj hello-behållare.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-167">Select hello container.</span></span> <span data-ttu-id="cf7c9-168">hello källa behållare är vanligtvis *virtuella hårddiskar*:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-168">hello source container is typically *vhds*:</span></span>
     
     ![Välj kontot lagringsblob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     <span data-ttu-id="cf7c9-170">Välj källa VM OS VHD och klicka på hello *kopiera* knappen Nästa toohello *URL* namn:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-170">Select your source VM OS VHD and click hello *Copy* button next toohello *URL* name:</span></span>
     
     ![Kopiera disk URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. <span data-ttu-id="cf7c9-172">Skapa en virtuell dator från hello källa VM OS-disken:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-172">Create a VM from hello source VM’s OS disk:</span></span>
   
   * <span data-ttu-id="cf7c9-173">Använd [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate en virtuell dator från en särskild virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-173">Use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate a VM from a specialized VHD.</span></span> <span data-ttu-id="cf7c9-174">Klicka på hello `Deploy tooAzure` knappen tooopen hello Azure-portalen med hello mallbaserat information fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-174">Click hello `Deploy tooAzure` button tooopen hello Azure portal with hello templated details populated for you.</span></span>
   * <span data-ttu-id="cf7c9-175">Om du vill tooretain alla tidigare hello-inställningarna för hello VM, Välj *redigera mallen* tooprovide din befintliga VNet, undernät, nätverkskort eller offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-175">If you want tooretain all hello previous settings for hello VM, select *Edit template* tooprovide your existing VNet, subnet, network adapter, or public IP.</span></span>
   * <span data-ttu-id="cf7c9-176">I hello `OSDISKVHDURI` klistra in hello käll-VHD-URI hämta i hello föregående steg i textrutan parameter:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-176">In hello `OSDISKVHDURI` parameter text box, paste hello URI of your source VHD obtain in hello preceding step:</span></span>
     
     ![Skapa en virtuell dator från mall](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. <span data-ttu-id="cf7c9-178">Efter hello nya virtuella datorn körs, ansluta toohello virtuella datorn med Fjärrskrivbord med hello nya lösenordet du angav i hello `FixAzureVM.cmd` skript.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-178">After hello new VM is running, connect toohello VM using Remote Desktop with hello new password you specified in hello `FixAzureVM.cmd` script.</span></span>
11. <span data-ttu-id="cf7c9-179">Från din fjärrsession toohello filer nya virtuella datorn, ta bort hello följande tooclean in hello miljö:</span><span class="sxs-lookup"><span data-stu-id="cf7c9-179">From your remote session toohello new VM, remove hello following files tooclean up hello environment:</span></span>
    
    * <span data-ttu-id="cf7c9-180">Från %windir%\System32</span><span class="sxs-lookup"><span data-stu-id="cf7c9-180">From %windir%\System32</span></span>
      * <span data-ttu-id="cf7c9-181">ta bort FixAzureVM.cmd</span><span class="sxs-lookup"><span data-stu-id="cf7c9-181">remove FixAzureVM.cmd</span></span>
    * <span data-ttu-id="cf7c9-182">Från %windir%\System32\GroupPolicy\Machine\\</span><span class="sxs-lookup"><span data-stu-id="cf7c9-182">From %windir%\System32\GroupPolicy\Machine\\</span></span>
      * <span data-ttu-id="cf7c9-183">ta bort scripts.ini</span><span class="sxs-lookup"><span data-stu-id="cf7c9-183">remove scripts.ini</span></span>
    * <span data-ttu-id="cf7c9-184">Från %windir%\System32\GroupPolicy</span><span class="sxs-lookup"><span data-stu-id="cf7c9-184">From %windir%\System32\GroupPolicy</span></span>
      * <span data-ttu-id="cf7c9-185">ta bort gpt.ini (om gpt.ini fanns före och du byta namn på den toogpt.ini.bak, Byt namn på hello .bak-filen tillbaka toogpt.ini)</span><span class="sxs-lookup"><span data-stu-id="cf7c9-185">remove gpt.ini (if gpt.ini existed before, and you renamed it toogpt.ini.bak, rename hello .bak file back toogpt.ini)</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf7c9-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cf7c9-186">Next steps</span></span>
<span data-ttu-id="cf7c9-187">Om du fortfarande inte kan ansluta med hjälp av fjärrskrivbord, se hello [RDP felsökningsguiden](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cf7c9-187">If you still cannot connect using Remote Desktop, see hello [RDP troubleshooting guide](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="cf7c9-188">Hej [detaljerad RDP felsökningsguide för](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tittar på felsökning metoder i stället för särskilda åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-188">hello [detailed RDP troubleshooting guide](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) looks at troubleshooting methods rather than specific steps.</span></span> <span data-ttu-id="cf7c9-189">Du kan också [öppnar du ett Azure supportbegäran](https://azure.microsoft.com/support/options/) för praktiska hjälp.</span><span class="sxs-lookup"><span data-stu-id="cf7c9-189">You can also [open an Azure support request](https://azure.microsoft.com/support/options/) for hands-on assistance.</span></span>

