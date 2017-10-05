---
title: "Återställer ett lokalt Windows-lösenord utan Azure-agenten | Microsoft Docs"
description: "Hur du återställer lösenordet för ett lokalt Windows-användarkonto när Azure gästagenten inte har installerats eller fungerar på en virtuell dator"
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
ms.openlocfilehash: 880f5e5967298401fc2522124af3746d9906ffa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-local-windows-password-for-azure-vm"></a><span data-ttu-id="8cbd3-103">Hur du återställer lokala Windows-lösenord för Azure VM</span><span class="sxs-lookup"><span data-stu-id="8cbd3-103">How to reset local Windows password for Azure VM</span></span>
<span data-ttu-id="8cbd3-104">Du kan återställa det lokala Windows-lösenordet för en virtuell dator i Azure med hjälp av [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) förutsatt att Azure gästagenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-104">You can reset the local Windows password of a VM in Azure using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) provided the Azure guest agent is installed.</span></span> <span data-ttu-id="8cbd3-105">Den här metoden är det vanligaste sättet att återställa ett lösenord för en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-105">This method is the primary way to reset a password for an Azure VM.</span></span> <span data-ttu-id="8cbd3-106">Om du får problem med Azure gästagenten inte svarar eller inte kunde installeras efter överföring av en anpassad avbildning, du kan manuellt återställa en Windows-lösenord.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-106">If you encounter issues with the Azure guest agent not responding, or failing to install after uploading a custom image, you can manually reset a Windows password.</span></span> <span data-ttu-id="8cbd3-107">Den här artikeln beskriver hur du återställer ett lokalt kontolösenord genom att koppla den virtuella käll-OS-disken till en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-107">This article details how to reset a local account password by attaching the source OS virtual disk to another VM.</span></span> 

> [!WARNING]
> <span data-ttu-id="8cbd3-108">Endast använda den här processen som en sista utväg.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-108">Only use this process as a last resort.</span></span> <span data-ttu-id="8cbd3-109">Alltid ett försök att återställa ett lösenord med hjälp av den [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) första.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-109">Always try to reset a password using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) first.</span></span>
> 
> 

## <a name="overview-of-the-process"></a><span data-ttu-id="8cbd3-110">Översikt över processen</span><span class="sxs-lookup"><span data-stu-id="8cbd3-110">Overview of the process</span></span>
<span data-ttu-id="8cbd3-111">Grundläggande steg för att utföra en lokal återställning av lösenord för en virtuell Windows-dator i Azure när det finns ingen åtkomst till Azure gästagenten är följande:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-111">The core steps for performing a local password reset for a Windows VM in Azure when there is no access to the Azure guest agent is as follows:</span></span>

* <span data-ttu-id="8cbd3-112">Ta bort den Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-112">Delete the source VM.</span></span> <span data-ttu-id="8cbd3-113">Virtuella diskar bevaras.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-113">The virtual disks are retained.</span></span>
* <span data-ttu-id="8cbd3-114">Koppla källa VM OS-disken till en annan virtuell dator på samma plats i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-114">Attach the source VM's OS disk to another VM on the same location within your Azure subscription.</span></span> <span data-ttu-id="8cbd3-115">Den här virtuella datorn kallas felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-115">This VM is referred to as the troubleshooting VM.</span></span>
* <span data-ttu-id="8cbd3-116">Använd felsökning VM för att skapa vissa config-filer på käll-VM OS-disken.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-116">Using the troubleshooting VM, create some config files on the source VM's OS disk.</span></span>
* <span data-ttu-id="8cbd3-117">Koppla bort den virtuella datorn OS-disken från felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-117">Detach the VM's OS disk from the troubleshooting VM.</span></span>
* <span data-ttu-id="8cbd3-118">Använd en Resource Manager-mall för att skapa en virtuell dator med hjälp av den ursprungliga virtuella disken.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-118">Use a Resource Manager template to create a VM, using the original virtual disk.</span></span>
* <span data-ttu-id="8cbd3-119">När den nya virtuella datorn startar uppdatera config-filer som du skapar lösenordet för användaren som krävs.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-119">When the new VM boots, the config files you create update the password of the required user.</span></span>

## <a name="detailed-steps"></a><span data-ttu-id="8cbd3-120">Detaljerade steg</span><span class="sxs-lookup"><span data-stu-id="8cbd3-120">Detailed steps</span></span>
<span data-ttu-id="8cbd3-121">Alltid ett försök att återställa ett lösenord med hjälp av den [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan de försöker med följande steg.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-121">Always try to reset a password using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before trying the following steps.</span></span> <span data-ttu-id="8cbd3-122">Kontrollera att du har en säkerhetskopia av den virtuella datorn innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-122">Make sure you have a backup of your VM before you start.</span></span> 

1. <span data-ttu-id="8cbd3-123">Ta bort de berörda VM i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-123">Delete the affected VM in Azure portal.</span></span> <span data-ttu-id="8cbd3-124">Den virtuella datorn endast tar bort metadata, referensen för den virtuella datorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-124">Deleting the VM only deletes the metadata, the reference of the VM within Azure.</span></span> <span data-ttu-id="8cbd3-125">Virtuella diskar bevaras när den virtuella datorn tas bort:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-125">The virtual disks are retained when the VM is deleted:</span></span>
   
   * <span data-ttu-id="8cbd3-126">Välj den virtuella datorn i Azure-portalen, klicka på *ta bort*:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-126">Select the VM in the Azure portal, click *Delete*:</span></span>
     
     ![Ta bort en befintlig virtuell dator](./media/reset-local-password-without-agent/delete_vm.png)
2. <span data-ttu-id="8cbd3-128">Koppla källa VM OS-disken till Virtuellt datorn felsökning.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-128">Attach the source VM’s OS disk to the troubleshooting VM.</span></span> <span data-ttu-id="8cbd3-129">Felsökning VM måste finnas i samma region som käll-VM OS-disken (som `West US`):</span><span class="sxs-lookup"><span data-stu-id="8cbd3-129">The troubleshooting VM must be in the same region as the source VM's OS disk (such as `West US`):</span></span>
   
   * <span data-ttu-id="8cbd3-130">Välj felsökning VM i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-130">Select the troubleshooting VM in the Azure portal.</span></span> <span data-ttu-id="8cbd3-131">Klicka på *diskar* | *bifoga befintliga*:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-131">Click *Disks* | *Attach existing*:</span></span>
     
     ![Bifoga den befintliga disken](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     <span data-ttu-id="8cbd3-133">Välj *VHD-filen* och välj sedan det lagringskonto som innehåller ditt Virtuella källdatorn:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-133">Select *VHD File* and then select the storage account that contains your source VM:</span></span>
     
     ![Välj lagringskonto](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     <span data-ttu-id="8cbd3-135">Markera behållaren som källa.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-135">Select the source container.</span></span> <span data-ttu-id="8cbd3-136">Käll-behållaren är vanligtvis *virtuella hårddiskar*:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-136">The source container is typically *vhds*:</span></span>
     
     ![Välj lagringsbehållare](./media/reset-local-password-without-agent/disks_select_container.png)
     
     <span data-ttu-id="8cbd3-138">Välj OS-vhd att ansluta.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-138">Select the OS vhd to attach.</span></span> <span data-ttu-id="8cbd3-139">Klicka på *Välj* att slutföra processen:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-139">Click *Select* to complete the process:</span></span>
     
     ![Välj källa för virtuell disk](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. <span data-ttu-id="8cbd3-141">Ansluta till den Virtuella datorn med hjälp av fjärrskrivbord felsökning och säkerställa källa VM OS-disken visas:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-141">Connect to the troubleshooting VM using Remote Desktop and ensure the source VM's OS disk is visible:</span></span>
   
   * <span data-ttu-id="8cbd3-142">Välj felsökning VM i Azure-portalen och klicka på *Anslut*.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-142">Select the troubleshooting VM in the Azure portal and click *Connect*.</span></span>
   * <span data-ttu-id="8cbd3-143">Öppna RDP-filen som laddas ned.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-143">Open the RDP file that downloads.</span></span> <span data-ttu-id="8cbd3-144">Ange användarnamn och lösenord för felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-144">Enter the username and password of the troubleshooting VM.</span></span>
   * <span data-ttu-id="8cbd3-145">I Utforskaren, leta efter datadisk bifogade.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-145">In File Explorer, look for the data disk you attached.</span></span> <span data-ttu-id="8cbd3-146">Om ursprungliga Virtuella datorns VHD finns endast data-disk som är ansluten till Virtuellt datorn felsökning, ska det vara F: enhet:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-146">If the source VM’s VHD is the only data disk attached to the troubleshooting VM, it should be the F: drive:</span></span>
     
     ![Visa bifogade datadisk](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. <span data-ttu-id="8cbd3-148">Skapa `gpt.ini` i `\Windows\System32\GroupPolicy` på käll-VM-enhet (om det finns gpt.ini, Byt namn till gpt.ini.bak):</span><span class="sxs-lookup"><span data-stu-id="8cbd3-148">Create `gpt.ini` in `\Windows\System32\GroupPolicy` on the source VM’s drive (if gpt.ini exists, rename to gpt.ini.bak):</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="8cbd3-149">Kontrollera att du inte oavsiktligt skapar följande filer i C:\Windows OS-enhet för felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-149">Make sure that you do not accidentally create the following files in C:\Windows, the OS drive for the troubleshooting VM.</span></span> <span data-ttu-id="8cbd3-150">Skapa följande filer i OS-enhet för källan virtuell dator som är anslutna som en datadisk.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-150">Create the following files in the OS drive for your source VM that is attached as a data disk.</span></span>
   > 
   > 
   
   * <span data-ttu-id="8cbd3-151">Lägg till följande rader i den `gpt.ini` fil som du skapade:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-151">Add the following lines into the `gpt.ini` file you created:</span></span>
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Skapa gpt.ini](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. <span data-ttu-id="8cbd3-153">Skapa `scripts.ini` i `\Windows\System32\GroupPolicy\Machine\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-153">Create `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`.</span></span> <span data-ttu-id="8cbd3-154">Kontrollera att dolda mappar som visas.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-154">Make sure hidden folders are shown.</span></span> <span data-ttu-id="8cbd3-155">Skapa vid behov på `Machine` eller `Scripts` mappar.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-155">If needed, create the `Machine` or `Scripts` folders.</span></span>
   
   * <span data-ttu-id="8cbd3-156">Lägg till följande rader i `scripts.ini` fil som du skapade:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-156">Add the following lines the `scripts.ini` file you created:</span></span>
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Skapa scripts.ini](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. <span data-ttu-id="8cbd3-158">Skapa `FixAzureVM.cmd` i `\Windows\System32` med följande innehållet och Ersätt `<username>` och `<newpassword>` med egna värden:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-158">Create `FixAzureVM.cmd` in `\Windows\System32` with the following contents, replacing `<username>` and `<newpassword>` with your own values:</span></span>
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![Skapa FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    <span data-ttu-id="8cbd3-160">När du definierar det nya lösenordet måste du uppfylla krav på komplexitet konfigurerat lösenord för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-160">You must meet the configured password complexity requirements for your VM when defining the new password.</span></span>
7. <span data-ttu-id="8cbd3-161">Koppla bort disken från felsökning VM i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-161">In Azure portal, detach the disk from the troubleshooting VM:</span></span>
   
   * <span data-ttu-id="8cbd3-162">Välj felsökning VM i Azure-portalen, klicka på *diskar*.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-162">Select the troubleshooting VM in the Azure portal, click *Disks*.</span></span>
   * <span data-ttu-id="8cbd3-163">Välj datadisken ansluten i steg 2, klicka på *Detach*:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-163">Select the data disk attached in step 2, click *Detach*:</span></span>
     
     ![Koppla bort disk](./media/reset-local-password-without-agent/detach_disk.png)
8. <span data-ttu-id="8cbd3-165">Innan du skapar en virtuell dator måste du hämta URI till din datakälla OS-disk:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-165">Before you create a VM, obtain the URI to your source OS disk:</span></span>
   
   * <span data-ttu-id="8cbd3-166">Välj lagringskonto i Azure-portalen klickar du på *Blobbar*.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-166">Select the storage account in the Azure portal, click *Blobs*.</span></span>
   * <span data-ttu-id="8cbd3-167">Välj behållare.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-167">Select the container.</span></span> <span data-ttu-id="8cbd3-168">Käll-behållaren är vanligtvis *virtuella hårddiskar*:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-168">The source container is typically *vhds*:</span></span>
     
     ![Välj kontot lagringsblob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     <span data-ttu-id="8cbd3-170">Välj källa VM OS VHD och klicka på den *kopiera* knappen bredvid den *URL* namn:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-170">Select your source VM OS VHD and click the *Copy* button next to the *URL* name:</span></span>
     
     ![Kopiera disk URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. <span data-ttu-id="8cbd3-172">Skapa en virtuell dator från käll-VM OS-disken:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-172">Create a VM from the source VM’s OS disk:</span></span>
   
   * <span data-ttu-id="8cbd3-173">Använd [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) att skapa en virtuell dator från en särskild virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-173">Use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) to create a VM from a specialized VHD.</span></span> <span data-ttu-id="8cbd3-174">Klicka på den `Deploy to Azure` knappen för att öppna den Azure-portalen med mallbaserat information fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-174">Click the `Deploy to Azure` button to open the Azure portal with the templated details populated for you.</span></span>
   * <span data-ttu-id="8cbd3-175">Om du vill bevara de tidigare inställningarna för den virtuella datorn, väljer *redigera mallen* att ge dina befintliga VNet, undernät, nätverkskort eller offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-175">If you want to retain all the previous settings for the VM, select *Edit template* to provide your existing VNet, subnet, network adapter, or public IP.</span></span>
   * <span data-ttu-id="8cbd3-176">I den `OSDISKVHDURI` parametern textruta, klistra in URI för käll-VHD hämta i föregående steg:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-176">In the `OSDISKVHDURI` parameter text box, paste the URI of your source VHD obtain in the preceding step:</span></span>
     
     ![Skapa en virtuell dator från mall](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. <span data-ttu-id="8cbd3-178">När den nya virtuella datorn körs, ansluta till den virtuella datorn med hjälp av fjärrskrivbord med det nya lösenordet du angav i den `FixAzureVM.cmd` skript.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-178">After the new VM is running, connect to the VM using Remote Desktop with the new password you specified in the `FixAzureVM.cmd` script.</span></span>
11. <span data-ttu-id="8cbd3-179">Ta bort följande filer att rensa miljön från din fjärrsession till den nya virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="8cbd3-179">From your remote session to the new VM, remove the following files to clean up the environment:</span></span>
    
    * <span data-ttu-id="8cbd3-180">Från %windir%\System32</span><span class="sxs-lookup"><span data-stu-id="8cbd3-180">From %windir%\System32</span></span>
      * <span data-ttu-id="8cbd3-181">ta bort FixAzureVM.cmd</span><span class="sxs-lookup"><span data-stu-id="8cbd3-181">remove FixAzureVM.cmd</span></span>
    * <span data-ttu-id="8cbd3-182">Från %windir%\System32\GroupPolicy\Machine\\</span><span class="sxs-lookup"><span data-stu-id="8cbd3-182">From %windir%\System32\GroupPolicy\Machine\\</span></span>
      * <span data-ttu-id="8cbd3-183">ta bort scripts.ini</span><span class="sxs-lookup"><span data-stu-id="8cbd3-183">remove scripts.ini</span></span>
    * <span data-ttu-id="8cbd3-184">Från %windir%\System32\GroupPolicy</span><span class="sxs-lookup"><span data-stu-id="8cbd3-184">From %windir%\System32\GroupPolicy</span></span>
      * <span data-ttu-id="8cbd3-185">ta bort gpt.ini (om gpt.ini fanns före och du bytt namn till gpt.ini.bak, Byt namn på filen .bak tillbaka till gpt.ini)</span><span class="sxs-lookup"><span data-stu-id="8cbd3-185">remove gpt.ini (if gpt.ini existed before, and you renamed it to gpt.ini.bak, rename the .bak file back to gpt.ini)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8cbd3-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8cbd3-186">Next steps</span></span>
<span data-ttu-id="8cbd3-187">Om du fortfarande inte kan ansluta med hjälp av fjärrskrivbord, finns det [RDP felsökningsguiden](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8cbd3-187">If you still cannot connect using Remote Desktop, see the [RDP troubleshooting guide](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="8cbd3-188">Den [detaljerad RDP felsökningsguide för](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tittar på felsökning metoder i stället för särskilda åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-188">The [detailed RDP troubleshooting guide](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) looks at troubleshooting methods rather than specific steps.</span></span> <span data-ttu-id="8cbd3-189">Du kan också [öppnar du ett Azure supportbegäran](https://azure.microsoft.com/support/options/) för praktiska hjälp.</span><span class="sxs-lookup"><span data-stu-id="8cbd3-189">You can also [open an Azure support request](https://azure.microsoft.com/support/options/) for hands-on assistance.</span></span>

