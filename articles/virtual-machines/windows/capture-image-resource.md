---
title: aaaCreate en hanterad avbildning i Azure | Microsoft Docs
description: "Skapa en hanterad avbildning av en generaliserad virtuell dator eller virtuell Hårddisk i Azure. Avbildningar kan använda toocreate flera virtuella datorer som använder hanterade diskar."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: cynthn
ms.openlocfilehash: d8cd6c2ce8c5d704de2c845abced85139944d682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a><span data-ttu-id="3133f-104">Skapa en hanterad avbildning av en generaliserad virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="3133f-104">Create a managed image of a generalized VM in Azure</span></span>

<span data-ttu-id="3133f-105">En hanterad avbildning resurs kan skapas från en generaliserad virtuell dator som lagras som en hanterad disk eller en ohanterad disk i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3133f-105">A managed image resource can be created from a generalized VM that is stored as either a managed disk or an unmanaged disk in a storage account.</span></span> <span data-ttu-id="3133f-106">Hej avbildningen kan sedan att använda toocreate flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3133f-106">hello image can then be used toocreate multiple VMs.</span></span> 


## <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="3133f-107">Generalisera hello virtuell Windows-dator med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="3133f-107">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="3133f-108">Sysprep tar bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild.</span><span class="sxs-lookup"><span data-stu-id="3133f-108">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="3133f-109">Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="3133f-109">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="3133f-110">Kontrollera att hello serverroller som körs på datorn hello stöds av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="3133f-110">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="3133f-111">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="3133f-111">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3133f-112">Om du kör Sysprep innan du laddar upp din VHD tooAzure för hello första gången, kontrollera att du har [förberett din virtuella dator](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du kör Sysprep.</span><span class="sxs-lookup"><span data-stu-id="3133f-112">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="3133f-113">Logga in toohello Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="3133f-113">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="3133f-114">Öppna hello Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="3133f-114">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="3133f-115">Ändra hello katalogen för**%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="3133f-115">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="3133f-116">I hello **systemförberedelseverktyget** dialogrutan **ange System Out of Box Experience (OOBE)**, och se till att hello **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="3133f-116">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="3133f-117">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="3133f-117">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="3133f-118">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3133f-118">Click **OK**.</span></span>
   
    ![Starta Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="3133f-120">När Sysprep är klar stänger hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3133f-120">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="3133f-121">Starta inte om hello VM.</span><span class="sxs-lookup"><span data-stu-id="3133f-121">Do not restart hello VM.</span></span>


## <a name="create-a-managed-image-in-hello-portal"></a><span data-ttu-id="3133f-122">Skapa en hanterad avbildning i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="3133f-122">Create a managed image in hello portal</span></span> 

1. <span data-ttu-id="3133f-123">Öppna hello [portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3133f-123">Open hello [portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3133f-124">Klicka på hello plustecknet toocreate en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="3133f-124">Click hello plus sign toocreate a new resource.</span></span>
3. <span data-ttu-id="3133f-125">Skriv i hello filter Sök **bild**.</span><span class="sxs-lookup"><span data-stu-id="3133f-125">In hello filter search, type **Image**.</span></span>
4. <span data-ttu-id="3133f-126">Välj **bild** hello resultaten.</span><span class="sxs-lookup"><span data-stu-id="3133f-126">Select **Image** from hello results.</span></span>
5. <span data-ttu-id="3133f-127">I hello **bild** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3133f-127">In hello **Image** blade, click **Create**.</span></span>
6. <span data-ttu-id="3133f-128">I **namn**, Skriv ett namn för hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="3133f-128">In **Name**, type a name for hello image.</span></span>
7. <span data-ttu-id="3133f-129">Om du har mer än en prenumeration väljer hello rätt en från hello **prenumeration** listrutan.</span><span class="sxs-lookup"><span data-stu-id="3133f-129">If you have more than one subscription, select hello correct one from hello **Subscription** drop-down.</span></span>
7. <span data-ttu-id="3133f-130">I **resursgruppen** Välj antingen **Skapa nytt** och ange ett namn eller välj **från befintliga** och välj en resurs grupp toouse hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="3133f-130">In **Resource Group** either select **Create new** and type in a name, or select **From existing** and select a resource group toouse from hello drop-down list.</span></span>
8. <span data-ttu-id="3133f-131">I **plats**, Välj hello plats för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3133f-131">In **Location**, choose hello location of your resource group.</span></span>
9. <span data-ttu-id="3133f-132">I **OS-typen** Välj hello typ av operativsystem, Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="3133f-132">In **OS type** select hello type of operating system, either Windows or Linux.</span></span>
11. <span data-ttu-id="3133f-133">I **lagringsblob**, klickar du på **Bläddra** toolook för hello VHD i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="3133f-133">In **Storage blob**, click **Browse** toolook for hello VHD in your Azure storage.</span></span>
12. <span data-ttu-id="3133f-134">I **kontotyp** Välj Standard_LRS eller Premium_LRS.</span><span class="sxs-lookup"><span data-stu-id="3133f-134">In **Account type** choose Standard_LRS or Premium_LRS.</span></span> <span data-ttu-id="3133f-135">Standard använder hårddiskar och Premium använder SSD-enheter.</span><span class="sxs-lookup"><span data-stu-id="3133f-135">Standard uses hard-disk drives and Premium uses solid-state drives.</span></span> <span data-ttu-id="3133f-136">Båda använder lokalt redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="3133f-136">Both use locally-redundant storage.</span></span>
13. <span data-ttu-id="3133f-137">I **cachelagring på Disk** Välj hello lämplig disk alternativet för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="3133f-137">In **Disk caching** select hello appropriate disk caching option.</span></span> <span data-ttu-id="3133f-138">hello alternativ är **ingen**, **skrivskyddad** och **läsningen/skrivningen**.</span><span class="sxs-lookup"><span data-stu-id="3133f-138">hello options are **None**, **Read-only** and **Read\write**.</span></span>
14. <span data-ttu-id="3133f-139">Valfritt: Du kan också lägga till en befintlig data toohello diskavbildning genom att klicka på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="3133f-139">Optional: You can also add an existing data disk toohello image by clicking **+ Add data disk**.</span></span>  
15. <span data-ttu-id="3133f-140">När du är klar med dina val klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3133f-140">When you are done making your selections, click **Create**.</span></span>
16. <span data-ttu-id="3133f-141">När hello avbildningen har skapats visas den som en **bild** resurs i hello listan över resurser i hello resursgrupp som du har valt.</span><span class="sxs-lookup"><span data-stu-id="3133f-141">After hello image is created, you will see it as an **Image** resource in hello list of resources in hello resource group you chose.</span></span>



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a><span data-ttu-id="3133f-142">Skapa en hanterad avbildning av en virtuell dator med hjälp av Powershell</span><span class="sxs-lookup"><span data-stu-id="3133f-142">Create a managed image of a VM using Powershell</span></span>

<span data-ttu-id="3133f-143">Skapa en avbildning direkt från hello VM garanterar hello avbildningen innehåller alla hello diskar som är associerade med hello VM, inklusive hello OS-disken och eventuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="3133f-143">Creating an image directly from hello VM ensures that hello image includes all of hello disks associated with hello VM, including hello OS Disk and any data disks.</span></span>


<span data-ttu-id="3133f-144">Innan du börjar bör du kontrollera att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="3133f-144">Before you begin, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="3133f-145">Kör följande kommando tooinstall hello den.</span><span class="sxs-lookup"><span data-stu-id="3133f-145">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="3133f-146">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3133f-146">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


1. <span data-ttu-id="3133f-147">Skapa några variabler.</span><span class="sxs-lookup"><span data-stu-id="3133f-147">Create some variables.</span></span>

    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. <span data-ttu-id="3133f-148">Se till att hello VM har frigjorts.</span><span class="sxs-lookup"><span data-stu-id="3133f-148">Make sure hello VM has been deallocated.</span></span>

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. <span data-ttu-id="3133f-149">Ange hello hello virtuella datorns status för för**generaliserad**.</span><span class="sxs-lookup"><span data-stu-id="3133f-149">Set hello status of hello virtual machine too**Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. <span data-ttu-id="3133f-150">Hämta hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3133f-150">Get hello virtual machine.</span></span> 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. <span data-ttu-id="3133f-151">Skapa hello image-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="3133f-151">Create hello image configuration.</span></span>

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. <span data-ttu-id="3133f-152">Skapa hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="3133f-152">Create hello image.</span></span>

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a><span data-ttu-id="3133f-153">Skapa en hanterad avbildning av en virtuell Hårddisk i PowerShell</span><span class="sxs-lookup"><span data-stu-id="3133f-153">Create a managed image of a VHD in PowerShell</span></span>

<span data-ttu-id="3133f-154">Skapa en hanterad avbildning med hjälp av din generaliserad OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="3133f-154">Create a managed image using your generalized OS VHD.</span></span>


1.  <span data-ttu-id="3133f-155">Ange först hello gemensamma parametrar:</span><span class="sxs-lookup"><span data-stu-id="3133f-155">First, set hello common parameters:</span></span>

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. <span data-ttu-id="3133f-156">Step\deallocate hello VM.</span><span class="sxs-lookup"><span data-stu-id="3133f-156">Step\deallocate hello VM.</span></span>

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. <span data-ttu-id="3133f-157">Markera hello VM som generaliserad.</span><span class="sxs-lookup"><span data-stu-id="3133f-157">Mark hello VM as generalized.</span></span>

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  <span data-ttu-id="3133f-158">Skapa hello avbildning med hjälp av din generaliserad OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="3133f-158">Create hello image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a><span data-ttu-id="3133f-159">Skapa en hanterad avbildning från en ögonblicksbild med hjälp av Powershell</span><span class="sxs-lookup"><span data-stu-id="3133f-159">Create a managed image from a snapshot using Powershell</span></span>

<span data-ttu-id="3133f-160">Du kan också skapa en hanterad avbildning från en ögonblicksbild av hello VHD från en generaliserad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3133f-160">You can also create a managed image from a snapshot of hello VHD from a generalized VM.</span></span>

    
1. <span data-ttu-id="3133f-161">Skapa några variabler.</span><span class="sxs-lookup"><span data-stu-id="3133f-161">Create some variables.</span></span> 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. <span data-ttu-id="3133f-162">Hämta hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="3133f-162">Get hello snapshot.</span></span>

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. <span data-ttu-id="3133f-163">Skapa hello image-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="3133f-163">Create hello image configuration.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. <span data-ttu-id="3133f-164">Skapa hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="3133f-164">Create hello image.</span></span>

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 
    

## <a name="next-steps"></a><span data-ttu-id="3133f-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3133f-165">Next steps</span></span>
- <span data-ttu-id="3133f-166">Nu kan du [skapa en virtuell dator från hello generaliserad hanterad avbildning](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3133f-166">Now you can [create a VM from hello generalized managed image](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>    

