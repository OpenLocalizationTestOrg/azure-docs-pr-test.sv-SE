---
title: "aaaUse a Windows felsökning av virtuell dator med Azure PowerShell | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Windows VM problem i Azure genom att ansluta hello OS tooa diskåterställning VM med hjälp av Azure PowerShell"
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
ms.openlocfilehash: 7a6a76f64824fe5d06dc4286cb1d87ab8bb794e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-azure-powershell"></a><span data-ttu-id="4d005-103">Felsöka en virtuell Windows-dator genom att koppla hello OS tooa diskåterställning VM med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d005-103">Troubleshoot a Windows VM by attaching hello OS disk tooa recovery VM using Azure PowerShell</span></span>
<span data-ttu-id="4d005-104">Om din Windows-dator (VM) i Azure påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv.</span><span class="sxs-lookup"><span data-stu-id="4d005-104">If your Windows virtual machine (VM) in Azure encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="4d005-105">Ett vanligt exempel är en programuppdatering för misslyckade som förhindrar hello VM kan tooboot har.</span><span class="sxs-lookup"><span data-stu-id="4d005-105">A common example would be a failed application update that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="4d005-106">Den här artikeln information om hur toouse Azure PowerShell tooconnect din virtuella hårddisk tooanother Windows VM toofix eventuella fel sedan återskapa den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4d005-106">This article details how toouse Azure PowerShell tooconnect your virtual hard disk tooanother Windows VM toofix any errors, then re-create your original VM.</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="4d005-107">Översikt över återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="4d005-107">Recovery process overview</span></span>
<span data-ttu-id="4d005-108">hello felsökningsprocessen är följande:</span><span class="sxs-lookup"><span data-stu-id="4d005-108">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="4d005-109">Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="4d005-109">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="4d005-110">Anslut och montera hello virtuell hårddisk tooanother Windows VM i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="4d005-110">Attach and mount hello virtual hard disk tooanother Windows VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="4d005-111">Ansluta toohello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="4d005-111">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="4d005-112">Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="4d005-112">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="4d005-113">Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.</span><span class="sxs-lookup"><span data-stu-id="4d005-113">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="4d005-114">Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="4d005-114">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="4d005-115">Se till att du har [hello senaste Azure PowerShell](/powershell/azure/overview) installerad och loggas i tooyour prenumerationen:</span><span class="sxs-lookup"><span data-stu-id="4d005-115">Make sure that you have [hello latest Azure PowerShell](/powershell/azure/overview) installed and logged in tooyour subscription:</span></span>

```powershell
Login-AzureRMAccount
```

<span data-ttu-id="4d005-116">Följande exempel Ersätt parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="4d005-116">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="4d005-117">Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="4d005-117">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="4d005-118">Fastställa startproblem</span><span class="sxs-lookup"><span data-stu-id="4d005-118">Determine boot issues</span></span>
<span data-ttu-id="4d005-119">Du kan visa en skärmbild av den virtuella datorn i Azure toohelp felsöka startproblem.</span><span class="sxs-lookup"><span data-stu-id="4d005-119">You can view a screenshot of your VM in Azure toohelp troubleshoot boot issues.</span></span> <span data-ttu-id="4d005-120">Den här skärmbilden kan hjälpa dig att identifiera varför en virtuell dator misslyckas tooboot.</span><span class="sxs-lookup"><span data-stu-id="4d005-120">This screenshot can help identify why a VM fails tooboot.</span></span> <span data-ttu-id="4d005-121">hello följande exempel hämtas hello skärmbild från hello virtuell Windows-dator med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="4d005-121">hello following example gets hello screenshot from hello Windows VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

<span data-ttu-id="4d005-122">Granska hello skärmbild toodetermine varför hello VM inte tooboot.</span><span class="sxs-lookup"><span data-stu-id="4d005-122">Review hello screenshot toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="4d005-123">Observera eventuella felmeddelanden eller felkoderna som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="4d005-123">Note any specific error messages or error codes provided.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="4d005-124">Visa information om befintlig virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="4d005-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="4d005-125">Innan du kan koppla en virtuell hårddisk tooanother VM, måste tooidentify hello namnet på hello virtuell hårddisk (VHD).</span><span class="sxs-lookup"><span data-stu-id="4d005-125">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span>

<span data-ttu-id="4d005-126">hello följande exempel hämtar information för hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="4d005-126">hello following example gets information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="4d005-127">Leta efter `Vhd URI` inom hello `StorageProfile` avsnitt från hello utdata i hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="4d005-127">Look for `Vhd URI` within hello `StorageProfile` section from hello output of hello preceding command.</span></span> <span data-ttu-id="4d005-128">hello följande trunkerat exempel visas hello `Vhd URI` hello utgången av hello kodblock:</span><span class="sxs-lookup"><span data-stu-id="4d005-128">hello following truncated example output shows hello `Vhd URI` towards hello end of hello code block:</span></span>

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a><span data-ttu-id="4d005-129">Ta bort en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="4d005-129">Delete existing VM</span></span>
<span data-ttu-id="4d005-130">Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="4d005-130">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="4d005-131">En virtuell hårddisk är där hello själva operativsystemet, program och konfigurationer lagras.</span><span class="sxs-lookup"><span data-stu-id="4d005-131">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="4d005-132">hello Virtuella datorn är enbart metadata som definierar hello storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="4d005-132">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="4d005-133">Varje virtuell hårddisk har tilldelats när kopplade tooa VM.</span><span class="sxs-lookup"><span data-stu-id="4d005-133">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="4d005-134">Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort.</span><span class="sxs-lookup"><span data-stu-id="4d005-134">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="4d005-135">hello lån fortsätter tooassociate hello OS-disk med en virtuell dator, även om den virtuella datorn är i tillståndet stoppad och frigjord.</span><span class="sxs-lookup"><span data-stu-id="4d005-135">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="4d005-136">Hej första steg toorecover den virtuella datorn är toodelete hello Virtuella datorresursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="4d005-136">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="4d005-137">Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="4d005-137">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="4d005-138">Efter hello VM tas bort, bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel.</span><span class="sxs-lookup"><span data-stu-id="4d005-138">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="4d005-139">följande exempel tar bort hello hello virtuella datorn med namnet `myVM` från hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="4d005-139">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="4d005-140">Vänta tills hello VM har tagits bort innan du kopplar hello virtuell hårddisk tooanother VM.</span><span class="sxs-lookup"><span data-stu-id="4d005-140">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="4d005-141">hello lånet på hello virtuell hårddisk som associeras med hello VM måste släppas innan du kan koppla hello virtuell hårddisk tooanother VM toobe.</span><span class="sxs-lookup"><span data-stu-id="4d005-141">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="4d005-142">Bifoga en befintlig virtuell hårddisk tooanother VM</span><span class="sxs-lookup"><span data-stu-id="4d005-142">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="4d005-143">Hej bredvid för några steg kan du använda en annan virtuell dator i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="4d005-143">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="4d005-144">Du bifogar hello befintlig virtuell hårddisk toothis felsökning VM toobrowse och redigera hello disk innehåll.</span><span class="sxs-lookup"><span data-stu-id="4d005-144">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="4d005-145">Den här processen kan toocorrect eventuella fel i programkonfigurationen eller granska ytterligare program eller loggfiler, till exempel.</span><span class="sxs-lookup"><span data-stu-id="4d005-145">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="4d005-146">Välj eller skapa en annan VM toouse för felsökning.</span><span class="sxs-lookup"><span data-stu-id="4d005-146">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="4d005-147">När du ansluter hello befintlig virtuell hårddisk, ange hello URL toohello disk hämtades i föregående hello `Get-AzureRmVM` kommando.</span><span class="sxs-lookup"><span data-stu-id="4d005-147">When you attach hello existing virtual hard disk, specify hello URL toohello disk obtained in hello preceding `Get-AzureRmVM` command.</span></span> <span data-ttu-id="4d005-148">hello följande exempel bifogar en befintlig virtuell hårddisk toohello felsökning virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="4d005-148">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> <span data-ttu-id="4d005-149">Lägga till en disk måste du toospecify hello diskens hello storlek.</span><span class="sxs-lookup"><span data-stu-id="4d005-149">Adding a disk requires you toospecify hello size of hello disk.</span></span> <span data-ttu-id="4d005-150">Som vi bifoga en befintlig disk hello `-DiskSizeInGB` har angetts som `$null`.</span><span class="sxs-lookup"><span data-stu-id="4d005-150">As we attach an existing disk, hello `-DiskSizeInGB` is specified as `$null`.</span></span> <span data-ttu-id="4d005-151">Det här värdet säkerställer hello datadisk är korrekt ansluten och utan hello behöver toodetermine, som hello true storleken på datadisk.</span><span class="sxs-lookup"><span data-stu-id="4d005-151">This value ensures hello data disk is correctly attached, and without hello need toodetermine hello true size of data disk.</span></span>


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="4d005-152">Montera hello bifogade datadisk</span><span class="sxs-lookup"><span data-stu-id="4d005-152">Mount hello attached data disk</span></span>

1. <span data-ttu-id="4d005-153">RDP-tooyour felsökning VM som använder hello rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4d005-153">RDP tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="4d005-154">hello följande exempel hämtar hello RDP-anslutningsfil för hello virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`, och laddar ned för`C:\Users\ops\Documents`”</span><span class="sxs-lookup"><span data-stu-id="4d005-154">hello following example downloads hello RDP connection file for hello VM named `myVMRecovery` in hello resource group named `myResourceGroup`, and downloads it too`C:\Users\ops\Documents`"</span></span>

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. <span data-ttu-id="4d005-155">hello datadisk identifieras automatiskt och ansluten.</span><span class="sxs-lookup"><span data-stu-id="4d005-155">hello data disk is automatically detected and attached.</span></span> <span data-ttu-id="4d005-156">Visa hello lista över anslutna volymer toodetermine hello enhetsbeteckning på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4d005-156">View hello list of attached volumes toodetermine hello drive letter as follows:</span></span>

    ```powershell
    Get-Disk
    ```

    <span data-ttu-id="4d005-157">hello följande exempel på utdata som visar hello virtuell hårddisk ansluten en disk **2**.</span><span class="sxs-lookup"><span data-stu-id="4d005-157">hello following example output shows hello virtual hard disk connected a disk **2**.</span></span> <span data-ttu-id="4d005-158">(Du kan också använda `Get-Volume` tooview hello enhetsbeteckning):</span><span class="sxs-lookup"><span data-stu-id="4d005-158">(You can also use `Get-Volume` tooview hello drive letter):</span></span>

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="4d005-159">Åtgärda problemen på den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="4d005-159">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="4d005-160">Med hello befintlig virtuell hårddisk monterad, kan du nu utföra eventuella underhåll och felsökning efter behov.</span><span class="sxs-lookup"><span data-stu-id="4d005-160">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="4d005-161">När du har åtgärdat hello problem, fortsätter du med hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="4d005-161">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="4d005-162">Demontera och koppla från den ursprungliga virtuella hårddisken</span><span class="sxs-lookup"><span data-stu-id="4d005-162">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="4d005-163">När din fel har åtgärdats, demontera och koppla hello befintlig virtuell hårddisk från den virtuella datorn med felsökning.</span><span class="sxs-lookup"><span data-stu-id="4d005-163">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="4d005-164">Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.</span><span class="sxs-lookup"><span data-stu-id="4d005-164">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="4d005-165">Demontera hello datadisk på den Virtuella återställningsdatorn i RDP-session från.</span><span class="sxs-lookup"><span data-stu-id="4d005-165">From within your RDP session, unmount hello data disk on your recovery VM.</span></span> <span data-ttu-id="4d005-166">Du behöver hello disknumret från hello tidigare `Get-Disk` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4d005-166">You need hello disk number from hello previous `Get-Disk` cmdlet.</span></span> <span data-ttu-id="4d005-167">Använd sedan `Set-Disk` tooset hello disken som:</span><span class="sxs-lookup"><span data-stu-id="4d005-167">Then, use `Set-Disk` tooset hello disk as offline:</span></span>

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    <span data-ttu-id="4d005-168">Bekräfta hello disk nu anges som offline med `Get-Disk` igen.</span><span class="sxs-lookup"><span data-stu-id="4d005-168">Confirm hello disk is now set as offline using `Get-Disk` again.</span></span> <span data-ttu-id="4d005-169">hello visas följande exempel hello disk nu anges som offline:</span><span class="sxs-lookup"><span data-stu-id="4d005-169">hello following example output shows hello disk is now set as offline:</span></span>

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. <span data-ttu-id="4d005-170">Avsluta RDP-session.</span><span class="sxs-lookup"><span data-stu-id="4d005-170">Exit your RDP session.</span></span> <span data-ttu-id="4d005-171">Ta bort hello virtuella hårddisken från hello felsökning VM från Azure PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="4d005-171">From your Azure PowerShell session, remove hello virtual hard disk from hello troubleshooting VM.</span></span>

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="4d005-172">Skapa virtuell dator från den ursprungliga hårddisken</span><span class="sxs-lookup"><span data-stu-id="4d005-172">Create VM from original hard disk</span></span>
<span data-ttu-id="4d005-173">toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="4d005-173">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="4d005-174">hello faktiska JSON-mallen finns på hello följande länk:</span><span class="sxs-lookup"><span data-stu-id="4d005-174">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="4d005-175">https://Raw.githubusercontent.com/Azure/Azure-Quickstart-Templates/Master/201-VM-Specialized-VHD-existing-vnet/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="4d005-175">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json</span></span>

<span data-ttu-id="4d005-176">hello mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av hello VHD URL från hello tidigare kommandot.</span><span class="sxs-lookup"><span data-stu-id="4d005-176">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="4d005-177">hello följande exempel distribuerar hello mallen toohello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="4d005-177">hello following example deploys hello template toohello resource group named `myResourceGroup`:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

<span data-ttu-id="4d005-178">Svaret hello efterfrågar hello mall som namn på virtuell dator, OS-typen och VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="4d005-178">Answer hello prompts for hello template such as VM name, OS type, and VM size.</span></span> <span data-ttu-id="4d005-179">Hej `osDiskVhdUri` är hello densamma som används när du ansluter hello befintlig virtuell hårddisk toohello felsökning VM som tidigare.</span><span class="sxs-lookup"><span data-stu-id="4d005-179">hello `osDiskVhdUri` is hello same as previously used when attaching hello existing virtual hard disk toohello troubleshooting VM.</span></span>


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="4d005-180">Återaktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="4d005-180">Re-enable boot diagnostics</span></span>

<span data-ttu-id="4d005-181">När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras.</span><span class="sxs-lookup"><span data-stu-id="4d005-181">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="4d005-182">hello följande exempel aktiveras hello diagnostiska tillägg på hello virtuella datorn med namnet `myVMDeployed` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="4d005-182">hello following example enables hello diagnostic extension on hello VM named `myVMDeployed` in hello resource group named `myResourceGroup`:</span></span>

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a><span data-ttu-id="4d005-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d005-183">Next steps</span></span>
<span data-ttu-id="4d005-184">Om du har problem med anslutning tooyour VM finns [felsökning av RDP-anslutningar tooan Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d005-184">If you are having issues connecting tooyour VM, see [Troubleshoot RDP connections tooan Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="4d005-185">Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Windows-VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d005-185">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="4d005-186">Mer information om hur du använder Resource Manager finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d005-186">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
