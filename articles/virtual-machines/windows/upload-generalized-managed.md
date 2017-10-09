---
title: "aaaCreate en hanterad Azure-VM från en virtuell Hårddisk generaliserad lokalt | Microsoft Docs"
description: "Överför en generaliserad virtuell Hårddisk tooAzure och använda den toocreate nya virtuella datorer i hello Resource Manager-distributionsmodellen."
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
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: 2fd0c0eec922e6ca8af4e712c1bceb1f9466105c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-and-use-it-toocreate-new-vms-in-azure"></a><span data-ttu-id="60094-103">Överföra en generaliserad virtuell Hårddisk och använder den toocreate nya virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="60094-103">Upload a generalized VHD and use it toocreate new VMs in Azure</span></span>

<span data-ttu-id="60094-104">Det här avsnittet vägleder dig genom med hjälp av PowerShell tooupload en VHD från en generaliserad virtuell tooAzure, skapa en avbildning från hello VHD och skapa en ny virtuell dator från den avbildningen.</span><span class="sxs-lookup"><span data-stu-id="60094-104">This topic walks you through using PowerShell tooupload a VHD of a generalized VM tooAzure, create an image from hello VHD and create a new VM from that image.</span></span> <span data-ttu-id="60094-105">Du kan överföra en virtuell Hårddisk som exporteras från ett verktyg för virtualisering av lokalt eller från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="60094-105">You can upload a VHD exported from an on-premises virtualization tool or from another cloud.</span></span> <span data-ttu-id="60094-106">Med hjälp av [hanterade diskar](managed-disks-overview.md) för hello ny virtuell dator förenklar hello VM hantering och ger bättre tillgänglighet när hello VM placeras i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="60094-106">Using [Managed Disks](managed-disks-overview.md) for hello new VM simplifies hello VM managment and provides better availability when hello VM is placed in an availability set.</span></span> 

<span data-ttu-id="60094-107">Om du vill toouse ett exempelskript finns [exempel på skript tooupload en VHD-tooAzure och skapa en ny virtuell dator](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span><span class="sxs-lookup"><span data-stu-id="60094-107">If you want toouse a sample script, see [Sample script tooupload a VHD tooAzure and create a new VM](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="60094-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="60094-108">Before you begin</span></span>

- <span data-ttu-id="60094-109">Du bör följa innan du laddar upp en VHD-tooAzure [förbereda en Windows-VHD eller VHDX tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="60094-109">Before uploading any VHD tooAzure, you should follow [Prepare a Windows VHD or VHDX tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
- <span data-ttu-id="60094-110">Granska [planera för migrering av hello tooManaged diskar](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) innan du påbörjar migreringen för[hanterade diskar](managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="60094-110">Review [Plan for hello migration tooManaged Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) before starting your migration too[Managed Disks](managed-disks-overview.md).</span></span>
- <span data-ttu-id="60094-111">Kontrollera att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="60094-111">Make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="60094-112">Kör följande kommando tooinstall hello den.</span><span class="sxs-lookup"><span data-stu-id="60094-112">Run hello following command tooinstall it.</span></span>

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    <span data-ttu-id="60094-113">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="60094-113">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="60094-114">Generalisera hello virtuell Windows-dator med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="60094-114">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="60094-115">Sysprep tar bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild.</span><span class="sxs-lookup"><span data-stu-id="60094-115">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="60094-116">Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="60094-116">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="60094-117">Kontrollera att hello serverroller som körs på datorn hello stöds av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="60094-117">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="60094-118">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="60094-118">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60094-119">Om du kör Sysprep innan du laddar upp din VHD tooAzure för hello första gången, kontrollera att du har [förberett din virtuella dator](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du kör Sysprep.</span><span class="sxs-lookup"><span data-stu-id="60094-119">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="60094-120">Logga in toohello Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="60094-120">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="60094-121">Öppna hello Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="60094-121">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="60094-122">Ändra hello katalogen för**%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="60094-122">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="60094-123">I hello **systemförberedelseverktyget** dialogrutan **ange System Out of Box Experience (OOBE)**, och se till att hello **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="60094-123">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="60094-124">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="60094-124">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="60094-125">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="60094-125">Click **OK**.</span></span>
   
    ![Starta Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="60094-127">När Sysprep är klar stänger hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="60094-127">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="60094-128">Starta inte om hello VM.</span><span class="sxs-lookup"><span data-stu-id="60094-128">Do not restart hello VM.</span></span>



## <a name="log-in-tooazure"></a><span data-ttu-id="60094-129">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="60094-129">Log in tooAzure</span></span>
<span data-ttu-id="60094-130">Om du inte redan har PowerShell version 1.4 eller senare installerat, läsa [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="60094-130">If you don't already have PowerShell version 1.4 or above installed, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="60094-131">Öppna Azure PowerShell och logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="60094-131">Open Azure PowerShell and sign in tooyour Azure account.</span></span> <span data-ttu-id="60094-132">Ett popup-fönster som öppnas för du tooenter dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="60094-132">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="60094-133">Hämta hello prenumerations-ID: N för din tillgängliga prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="60094-133">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="60094-134">Ange hello korrekt prenumeration med hjälp av hello prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="60094-134">Set hello correct subscription using hello subscription ID.</span></span> <span data-ttu-id="60094-135">Ersätt  *<subscriptionID>*  med hello-ID för hello rätt prenumeration.</span><span class="sxs-lookup"><span data-stu-id="60094-135">Replace *<subscriptionID>* with hello ID of hello correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-hello-storage-account"></a><span data-ttu-id="60094-136">Hämta hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="60094-136">Get hello storage account</span></span>
<span data-ttu-id="60094-137">Du behöver ett lagringskonto i Azure toostore hello upp VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="60094-137">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="60094-138">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="60094-138">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="60094-139">Om du kommer att använda hello VHD toocreate hanterade diskar för en virtuell dator, måste hello lagringskontoplatsen vara samma hello plats där du skapar hello VM.</span><span class="sxs-lookup"><span data-stu-id="60094-139">If you will be using hello VHD toocreate a managed disk for a VM, hello storage account location must be same hello location where you will be creating hello VM.</span></span>

<span data-ttu-id="60094-140">tooshow hello tillgängliga storage-konton, skriver du:</span><span class="sxs-lookup"><span data-stu-id="60094-140">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="60094-141">Om du vill toouse ett befintligt lagringskonto fortsätta toohello [överför hello VM-avbildning](#upload-the-vm-vhd-to-your-storage-account) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="60094-141">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="60094-142">Följ dessa steg om du behöver toocreate ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="60094-142">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="60094-143">Du måste hello namnet på hello resursgruppen där hello storage-konto ska skapas.</span><span class="sxs-lookup"><span data-stu-id="60094-143">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="60094-144">toofind ut hello resursgrupper i din prenumeration, typ:</span><span class="sxs-lookup"><span data-stu-id="60094-144">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="60094-145">en resursgrupp med namnet toocreate **myResourceGroup** i hello **östra USA** region, typ:</span><span class="sxs-lookup"><span data-stu-id="60094-145">toocreate a resource group named **myResourceGroup** in hello **East US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. <span data-ttu-id="60094-146">Skapa ett lagringskonto med namnet **mittlagringskonto** i den här resursgruppen genom att använda hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="60094-146">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    <span data-ttu-id="60094-147">Giltiga värden för - SkuName är:</span><span class="sxs-lookup"><span data-stu-id="60094-147">Valid values for -SkuName are:</span></span>
   
   * <span data-ttu-id="60094-148">**Standard_LRS** -lokalt redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="60094-148">**Standard_LRS** - Locally redundant storage.</span></span> 
   * <span data-ttu-id="60094-149">**Standard_ZRS** -zonen redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="60094-149">**Standard_ZRS** - Zone redundant storage.</span></span>
   * <span data-ttu-id="60094-150">**Standard_GRS** -Geo-redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="60094-150">**Standard_GRS** - Geo redundant storage.</span></span> 
   * <span data-ttu-id="60094-151">**Standard_RAGRS** -geo-redundant lagring med läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="60094-151">**Standard_RAGRS** - Read access geo redundant storage.</span></span> 
   * <span data-ttu-id="60094-152">**Premium_LRS** -Premium lokalt redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="60094-152">**Premium_LRS** - Premium locally redundant storage.</span></span> 

## <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="60094-153">Överför hello VHD tooyour storage-konto</span><span class="sxs-lookup"><span data-stu-id="60094-153">Upload hello VHD tooyour storage account</span></span>

<span data-ttu-id="60094-154">Använd hello [Lägg till AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet tooupload hello VHD tooa behållare på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="60094-154">Use hello [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet tooupload hello VHD tooa container in your storage account.</span></span> <span data-ttu-id="60094-155">Det här exemplet överföringar hello filen *myVHD.vhd* från *”C:\Users\Public\Documents\Virtual hårddiskar\"*  tooa lagringskontonamnet *mittlagringskonto*i hello *myResourceGroup* resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="60094-155">This example uploads hello file *myVHD.vhd* from *"C:\Users\Public\Documents\Virtual hard disks\"* tooa storage account named *mystorageaccount* in hello *myResourceGroup* resource group.</span></span> <span data-ttu-id="60094-156">hello-fil placeras i hello behållare med namnet *minbehållare* och hello nytt filnamn blir *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="60094-156">hello file will be placed into hello container named *mycontainer* and hello new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="60094-157">Om det lyckas, får du ett svar som ser ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="60094-157">If successful, you get a response that looks similar toothis:</span></span>

```powershell
MD5 hash is being calculated for hello file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for hello operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

<span data-ttu-id="60094-158">Det här kommandot kan ta en stund beroende på nätverksanslutningen och hello storleken på VHD-filen, toocomplete</span><span class="sxs-lookup"><span data-stu-id="60094-158">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete</span></span>

<span data-ttu-id="60094-159">Spara hello **mål-URI** sökväg toouse senare om du ska toocreate hanterade diskar eller en ny virtuell dator med hjälp av hello överföra VHD.</span><span class="sxs-lookup"><span data-stu-id="60094-159">Save hello **Destination URI** path toouse later if you are going toocreate a managed disk or a new VM using hello uploaded VHD.</span></span>

### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="60094-160">Andra alternativ för att överföra en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="60094-160">Other options for uploading a VHD</span></span>
 
 
<span data-ttu-id="60094-161">Du kan också ladda upp en VHD tooyour storage-konto med en av följande hello:</span><span class="sxs-lookup"><span data-stu-id="60094-161">You can also upload a VHD tooyour storage account using one of hello following:</span></span>

- [<span data-ttu-id="60094-162">AzCopy</span><span class="sxs-lookup"><span data-stu-id="60094-162">AzCopy</span></span>](http://aka.ms/downloadazcopy)
- [<span data-ttu-id="60094-163">Azure Storage kopiera Blob API</span><span class="sxs-lookup"><span data-stu-id="60094-163">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [<span data-ttu-id="60094-164">Azure Storage Explorer överför Blobbar</span><span class="sxs-lookup"><span data-stu-id="60094-164">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
- [<span data-ttu-id="60094-165">Storage Import/Export Service REST API-referens</span><span class="sxs-lookup"><span data-stu-id="60094-165">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)
-   <span data-ttu-id="60094-166">Du rekommenderas att använda Import/Export Service om överföring tid är längre än 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="60094-166">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="60094-167">Du kan använda [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello tiden från enhet för storlek och överföring av data.</span><span class="sxs-lookup"><span data-stu-id="60094-167">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello time from data size and transfer unit.</span></span> 
    <span data-ttu-id="60094-168">Import/Export kan vara används tooa toocopy standardlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="60094-168">Import/Export can be used toocopy tooa standard storage account.</span></span> <span data-ttu-id="60094-169">Du behöver toocopy från standardlagring toopremium storage-konto med ett verktyg som AzCopy.</span><span class="sxs-lookup"><span data-stu-id="60094-169">You will need toocopy from standard storage toopremium storage account using a tool like AzCopy.</span></span>


## <a name="create-a-managed-image-from-hello-uploaded-vhd"></a><span data-ttu-id="60094-170">Skapa en hanterad avbildning från hello överföra VHD</span><span class="sxs-lookup"><span data-stu-id="60094-170">Create a managed image from hello uploaded VHD</span></span> 

<span data-ttu-id="60094-171">Skapa en hanterad avbildning med hjälp av din generaliserad OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="60094-171">Create a managed image using your generalized OS VHD.</span></span> <span data-ttu-id="60094-172">Ersätt hello värden med din egen information.</span><span class="sxs-lookup"><span data-stu-id="60094-172">Replace hello values with your own information.</span></span>


1.  <span data-ttu-id="60094-173">Ange först hello gemensamma parametrar:</span><span class="sxs-lookup"><span data-stu-id="60094-173">First, set hello common parameters:</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  <span data-ttu-id="60094-174">Skapa hello avbildning med hjälp av din generaliserad OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="60094-174">Create hello image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a><span data-ttu-id="60094-175">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="60094-175">Create a virtual network</span></span>
<span data-ttu-id="60094-176">Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="60094-176">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="60094-177">Skapa hello undernät.</span><span class="sxs-lookup"><span data-stu-id="60094-177">Create hello subnet.</span></span> <span data-ttu-id="60094-178">Det här exemplet skapar ett undernät med namnet *mySubnet* med hello adressprefixet *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="60094-178">This example creates a subnet named *mySubnet* with hello address prefix of *10.0.0.0/24*.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="60094-179">Skapa hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="60094-179">Create hello virtual network.</span></span> <span data-ttu-id="60094-180">Det här exemplet skapar ett virtuellt nätverk med namnet *myVnet* med hello adressprefixet *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="60094-180">This example creates a virtual network named *myVnet* with hello address prefix of *10.0.0.0/16*.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="60094-181">Skapa en offentlig IP-adress och gränssnitt</span><span class="sxs-lookup"><span data-stu-id="60094-181">Create a public IP address and network interface</span></span>

<span data-ttu-id="60094-182">tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="60094-182">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="60094-183">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="60094-183">Create a public IP address.</span></span> <span data-ttu-id="60094-184">Det här exemplet skapas en offentlig IP-adress med namnet *myPip*.</span><span class="sxs-lookup"><span data-stu-id="60094-184">This example creates a public IP address named *myPip*.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="60094-185">Skapa hello NIC.</span><span class="sxs-lookup"><span data-stu-id="60094-185">Create hello NIC.</span></span> <span data-ttu-id="60094-186">Det här exemplet skapar ett nätverkskort med namnet **myNic**.</span><span class="sxs-lookup"><span data-stu-id="60094-186">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="60094-187">Skapa hello nätverkssäkerhetsgruppen och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="60094-187">Create hello network security group and an RDP rule</span></span>

<span data-ttu-id="60094-188">toobe kan toolog i tooyour VM som använder RDP måste toohave en nätverkssäkerhetsregeln (NSG) som gör att RDP-åtkomst på port 3389.</span><span class="sxs-lookup"><span data-stu-id="60094-188">toobe able toolog in tooyour VM using RDP, you need toohave a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="60094-189">Det här exemplet skapas en NSG som heter *myNsg* som innehåller en regel med namnet *myRdpRule* som tillåter RDP-trafik via port 3389.</span><span class="sxs-lookup"><span data-stu-id="60094-189">This example creates an NSG named *myNsg* that contains a rule called *myRdpRule* that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="60094-190">Mer information om NSG: er finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="60094-190">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="60094-191">Skapa en variabel för hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="60094-191">Create a variable for hello virtual network</span></span>

<span data-ttu-id="60094-192">Skapa en variabel för hello slutförts virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="60094-192">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a><span data-ttu-id="60094-193">Hämta hello autentiseringsuppgifter för hello VM</span><span class="sxs-lookup"><span data-stu-id="60094-193">Get hello credentials for hello VM</span></span>

<span data-ttu-id="60094-194">hello öppnar följande cmdlet ett fönster där anger du en ny användare och lösenord toouse som hello lokalt administratörskonto för att få fjärråtkomst till hello VM.</span><span class="sxs-lookup"><span data-stu-id="60094-194">hello following cmdlet will open a window where you will enter a new user name and password toouse as hello local administrator account for remotely accessing hello VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="add-hello-vm-name-and-size-toohello-vm-configuration"></a><span data-ttu-id="60094-195">Lägg till hello namn på virtuell dator och storlek toohello VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="60094-195">Add hello VM name and size toohello VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a><span data-ttu-id="60094-196">Ange hello VM-avbildning som källbilden för hello ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="60094-196">Set hello VM image as source image for hello new VM</span></span>

<span data-ttu-id="60094-197">Ange hello Källavbildningen med hello-ID för hello hanterade VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="60094-197">Set hello source image using hello ID of hello managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a><span data-ttu-id="60094-198">Ange hello Operativsystemets konfiguration och Lägg till hello NIC.</span><span class="sxs-lookup"><span data-stu-id="60094-198">Set hello OS configuration and add hello NIC.</span></span>

<span data-ttu-id="60094-199">Ange hello lagringstyp (PremiumLRS eller StandardLRS) och hello hello OS-diskens storlek.</span><span class="sxs-lookup"><span data-stu-id="60094-199">Enter hello storage type (PremiumLRS or StandardLRS) and hello size of hello OS disk.</span></span> <span data-ttu-id="60094-200">Det här exemplet anger hello kontotyp för*PremiumLRS*, hello diskstorleken för*128 GB* och diskcachelagring för*ReadWrite*.</span><span class="sxs-lookup"><span data-stu-id="60094-200">This example sets hello account type too*PremiumLRS*, hello disk size too*128 GB* and disk caching too*ReadWrite*.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a><span data-ttu-id="60094-201">Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="60094-201">Create hello VM</span></span>

<span data-ttu-id="60094-202">Skapa ny virtuell dator med hello konfiguration lagras i hello hello **$vm** variabeln.</span><span class="sxs-lookup"><span data-stu-id="60094-202">Create hello new VM using hello configuration stored in hello **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="60094-203">Kontrollera att hello VM skapades</span><span class="sxs-lookup"><span data-stu-id="60094-203">Verify that hello VM was created</span></span>
<span data-ttu-id="60094-204">När du är klar bör du se hello nyskapad VM i hello [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande hello PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="60094-204">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="60094-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60094-205">Next steps</span></span>

<span data-ttu-id="60094-206">toosign i tooyour nya virtuella datorn, bläddra toohello VM i hello [portal](https://portal.azure.com), klickar du på **Anslut**, och öppna hello Remote Desktop RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="60094-206">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="60094-207">Använd hello autentiseringsuppgifter för den ursprungliga virtuella toosign i tooyour ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="60094-207">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="60094-208">Mer information finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="60094-208">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

