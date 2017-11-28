---
title: "Skapa en hanterad Azure virtuell dator från en virtuell Hårddisk generaliserad lokalt | Microsoft Docs"
description: "Överför en generaliserad virtuell Hårddisk till Azure och använda den för att skapa nya virtuella datorer, i Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: d802ba16ecb4e32e2adb7be3a8e99c72a1625841
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a><span data-ttu-id="92095-103">Överför en generaliserad virtuell Hårddisk och använda den för att skapa nya virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="92095-103">Upload a generalized VHD and use it to create new VMs in Azure</span></span>

<span data-ttu-id="92095-104">Det här avsnittet beskriver hur du använder PowerShell för att överföra en VHD från en generaliserad virtuell dator till Azure, skapa en avbildning från den virtuella Hårddisken och skapa en ny virtuell dator från den avbildningen.</span><span class="sxs-lookup"><span data-stu-id="92095-104">This topic walks you through using PowerShell to upload a VHD of a generalized VM to Azure, create an image from the VHD and create a new VM from that image.</span></span> <span data-ttu-id="92095-105">Du kan överföra en virtuell Hårddisk som exporteras från ett verktyg för virtualisering av lokalt eller från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="92095-105">You can upload a VHD exported from an on-premises virtualization tool or from another cloud.</span></span> <span data-ttu-id="92095-106">Med hjälp av [hanterade diskar](managed-disks-overview.md) för den nya virtuella datorn förenklas VM och ger bättre tillgänglighet när den virtuella datorn placeras i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="92095-106">Using [Managed Disks](managed-disks-overview.md) for the new VM simplifies the VM managment and provides better availability when the VM is placed in an availability set.</span></span> 

<span data-ttu-id="92095-107">Om du vill använda ett exempelskript finns [exempel på skript för att överföra en virtuell Hårddisk till Azure och skapa en ny virtuell dator](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span><span class="sxs-lookup"><span data-stu-id="92095-107">If you want to use a sample script, see [Sample script to upload a VHD to Azure and create a new VM](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="92095-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="92095-108">Before you begin</span></span>

- <span data-ttu-id="92095-109">Du bör följa innan du laddar upp alla VHD till Azure [förbereda en Windows-VHD eller VHDX för att överföra till Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="92095-109">Before uploading any VHD to Azure, you should follow [Prepare a Windows VHD or VHDX to upload to Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
- <span data-ttu-id="92095-110">Granska [planera för migrering till hanterade diskar](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) innan du påbörjar migreringen till [hanterade diskar](managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="92095-110">Review [Plan for the migration to Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) before starting your migration to [Managed Disks](managed-disks-overview.md).</span></span>
- <span data-ttu-id="92095-111">Kontrollera att du har den senaste versionen av AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="92095-111">Make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="92095-112">Kör följande kommando för att installera den.</span><span class="sxs-lookup"><span data-stu-id="92095-112">Run the following command to install it.</span></span>

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    <span data-ttu-id="92095-113">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="92095-113">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="generalize-the-windows-vm-using-sysprep"></a><span data-ttu-id="92095-114">Generalisera Windows VM med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="92095-114">Generalize the Windows VM using Sysprep</span></span>

<span data-ttu-id="92095-115">Sysprep tar bort alla dina personlig information, bland annat och förbereder datorn som ska användas som en bild.</span><span class="sxs-lookup"><span data-stu-id="92095-115">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="92095-116">Mer information om Sysprep finns [så att använda Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="92095-116">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="92095-117">Se till att serverroller som körs på datorn som stöds av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="92095-117">Make sure the server roles running on the machine are supported by Sysprep.</span></span> <span data-ttu-id="92095-118">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="92095-118">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="92095-119">Om du kör Sysprep innan du laddar upp den virtuella Hårddisken till Azure för första gången, kontrollera att du har [förberett din virtuella dator](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du kör Sysprep.</span><span class="sxs-lookup"><span data-stu-id="92095-119">If you are running Sysprep before uploading your VHD to Azure for the first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="92095-120">Logga in på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="92095-120">Sign in to the Windows virtual machine.</span></span>
2. <span data-ttu-id="92095-121">Öppna Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="92095-121">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="92095-122">Ändra katalogen till **%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="92095-122">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="92095-123">I den **systemförberedelseverktyget** dialogrutan **ange System Out of Box Experience (OOBE)**, och se till att den **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="92095-123">In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="92095-124">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="92095-124">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="92095-125">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="92095-125">Click **OK**.</span></span>
   
    ![Starta Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="92095-127">När Sysprep har slutförts stängs den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="92095-127">When Sysprep completes, it shuts down the virtual machine.</span></span> <span data-ttu-id="92095-128">Starta inte om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="92095-128">Do not restart the VM.</span></span>



## <a name="log-in-to-azure"></a><span data-ttu-id="92095-129">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="92095-129">Log in to Azure</span></span>
<span data-ttu-id="92095-130">Om du inte redan har PowerShell version 1.4 eller senare installerat, läsa [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="92095-130">If you don't already have PowerShell version 1.4 or above installed, read [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="92095-131">Öppna Azure PowerShell och logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="92095-131">Open Azure PowerShell and sign in to your Azure account.</span></span> <span data-ttu-id="92095-132">Ett popup-fönster öppnas där du kan ange dina autentiseringsuppgifter för Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="92095-132">A pop-up window opens for you to enter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="92095-133">Hämta ID: N för prenumeration för din tillgängliga prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="92095-133">Get the subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="92095-134">Ange rätt prenumerationen med prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="92095-134">Set the correct subscription using the subscription ID.</span></span> <span data-ttu-id="92095-135">Ersätt  *<subscriptionID>*  med ID korrekt prenumeration.</span><span class="sxs-lookup"><span data-stu-id="92095-135">Replace *<subscriptionID>* with the ID of the correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a><span data-ttu-id="92095-136">Hämta storage-konto</span><span class="sxs-lookup"><span data-stu-id="92095-136">Get the storage account</span></span>
<span data-ttu-id="92095-137">Du behöver ett lagringskonto i Azure för att lagra överförda VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="92095-137">You need a storage account in Azure to store the uploaded VM image.</span></span> <span data-ttu-id="92095-138">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="92095-138">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="92095-139">Om du kommer att använda den virtuella Hårddisken för att skapa en hanterad disk för en virtuell dator, lagringsplats för kontot måste vara samma plats där du skapar den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="92095-139">If you will be using the VHD to create a managed disk for a VM, the storage account location must be same the location where you will be creating the VM.</span></span>

<span data-ttu-id="92095-140">Om du vill visa tillgängliga storage-konton, skriver du:</span><span class="sxs-lookup"><span data-stu-id="92095-140">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="92095-141">Om du vill använda ett befintligt lagringskonto fortsätter du till den [överför den Virtuella datoravbildningen](#upload-the-vm-vhd-to-your-storage-account) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="92095-141">If you want to use an existing storage account, proceed to the [Upload the VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="92095-142">Följ dessa steg om du behöver skapa ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="92095-142">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="92095-143">Du måste namnet på resursgruppen där lagringskontot ska skapas.</span><span class="sxs-lookup"><span data-stu-id="92095-143">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="92095-144">Om du vill ta reda på alla resursgrupper i din prenumeration, skriver du:</span><span class="sxs-lookup"><span data-stu-id="92095-144">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="92095-145">Så här skapar du en resursgrupp med namnet **myResourceGroup** i den **östra USA** region, typ:</span><span class="sxs-lookup"><span data-stu-id="92095-145">To create a resource group named **myResourceGroup** in the **East US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. <span data-ttu-id="92095-146">Skapa ett lagringskonto med namnet **mittlagringskonto** i den här resursgruppen med hjälp av den [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="92095-146">Create a storage account named **mystorageaccount** in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    <span data-ttu-id="92095-147">Giltiga värden för - SkuName är:</span><span class="sxs-lookup"><span data-stu-id="92095-147">Valid values for -SkuName are:</span></span>
   
   * <span data-ttu-id="92095-148">**Standard_LRS** -lokalt redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="92095-148">**Standard_LRS** - Locally redundant storage.</span></span> 
   * <span data-ttu-id="92095-149">**Standard_ZRS** -zonen redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="92095-149">**Standard_ZRS** - Zone redundant storage.</span></span>
   * <span data-ttu-id="92095-150">**Standard_GRS** -Geo-redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="92095-150">**Standard_GRS** - Geo redundant storage.</span></span> 
   * <span data-ttu-id="92095-151">**Standard_RAGRS** -geo-redundant lagring med läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="92095-151">**Standard_RAGRS** - Read access geo redundant storage.</span></span> 
   * <span data-ttu-id="92095-152">**Premium_LRS** -Premium lokalt redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="92095-152">**Premium_LRS** - Premium locally redundant storage.</span></span> 

## <a name="upload-the-vhd-to-your-storage-account"></a><span data-ttu-id="92095-153">Överför den virtuella Hårddisken till ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="92095-153">Upload the VHD to your storage account</span></span>

<span data-ttu-id="92095-154">Använd den [Lägg till AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) för att ladda upp den virtuella Hårddisken till en behållare i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="92095-154">Use the [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet to upload the VHD to a container in your storage account.</span></span> <span data-ttu-id="92095-155">Det här exemplet överför filen *myVHD.vhd* från *”C:\Users\Public\Documents\Virtual hårddiskar\"*  till ett lagringskonto med namnet *mittlagringskonto* i den *myResourceGroup* resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="92095-155">This example uploads the file *myVHD.vhd* from *"C:\Users\Public\Documents\Virtual hard disks\"* to a storage account named *mystorageaccount* in the *myResourceGroup* resource group.</span></span> <span data-ttu-id="92095-156">Filen placeras i behållare med namnet *minbehållare* och det nya namnet kommer att *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="92095-156">The file will be placed into the container named *mycontainer* and the new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="92095-157">Om det lyckas, kan du få ett svar som liknar denna:</span><span class="sxs-lookup"><span data-stu-id="92095-157">If successful, you get a response that looks similar to this:</span></span>

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

<span data-ttu-id="92095-158">Det här kommandot kan ta en stund att slutföra beroende på nätverksanslutningen och storleken på VHD-filen</span><span class="sxs-lookup"><span data-stu-id="92095-158">Depending on your network connection and the size of your VHD file, this command may take a while to complete</span></span>

<span data-ttu-id="92095-159">Spara den **mål-URI** sökväg som ska användas senare om du ska skapa en hanterad disk eller en ny virtuell dator med hjälp av den överförda virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="92095-159">Save the **Destination URI** path to use later if you are going to create a managed disk or a new VM using the uploaded VHD.</span></span>

### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="92095-160">Andra alternativ för att överföra en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="92095-160">Other options for uploading a VHD</span></span>
 
 
<span data-ttu-id="92095-161">Du kan också ladda upp en virtuell Hårddisk till ditt lagringskonto med hjälp av något av följande:</span><span class="sxs-lookup"><span data-stu-id="92095-161">You can also upload a VHD to your storage account using one of the following:</span></span>

- [<span data-ttu-id="92095-162">AzCopy</span><span class="sxs-lookup"><span data-stu-id="92095-162">AzCopy</span></span>](http://aka.ms/downloadazcopy)
- [<span data-ttu-id="92095-163">Azure Storage kopiera Blob API</span><span class="sxs-lookup"><span data-stu-id="92095-163">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [<span data-ttu-id="92095-164">Azure Storage Explorer överför Blobbar</span><span class="sxs-lookup"><span data-stu-id="92095-164">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
- [<span data-ttu-id="92095-165">Storage Import/Export Service REST API-referens</span><span class="sxs-lookup"><span data-stu-id="92095-165">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)
-   <span data-ttu-id="92095-166">Du rekommenderas att använda Import/Export Service om överföring tid är längre än 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="92095-166">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="92095-167">Du kan använda [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) att uppskatta storlek och överför dataenheten tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="92095-167">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) to estimate the time from data size and transfer unit.</span></span> 
    <span data-ttu-id="92095-168">Import/Export kan användas för att kopiera till ett standardlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="92095-168">Import/Export can be used to copy to a standard storage account.</span></span> <span data-ttu-id="92095-169">Du måste kopiera från standardlagring till premium storage-konto med ett verktyg som AzCopy.</span><span class="sxs-lookup"><span data-stu-id="92095-169">You will need to copy from standard storage to premium storage account using a tool like AzCopy.</span></span>


## <a name="create-a-managed-image-from-the-uploaded-vhd"></a><span data-ttu-id="92095-170">Skapa en hanterad avbildning från den överförda virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="92095-170">Create a managed image from the uploaded VHD</span></span> 

<span data-ttu-id="92095-171">Skapa en hanterad avbildning med hjälp av din generaliserad OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="92095-171">Create a managed image using your generalized OS VHD.</span></span> <span data-ttu-id="92095-172">Ersätt värdena med din egen information.</span><span class="sxs-lookup"><span data-stu-id="92095-172">Replace the values with your own information.</span></span>


1.  <span data-ttu-id="92095-173">Innan du kan definiera de gemensamma parametrarna:</span><span class="sxs-lookup"><span data-stu-id="92095-173">First, set the common parameters:</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  <span data-ttu-id="92095-174">Skapa avbildningen med din generaliserad OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="92095-174">Create the image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a><span data-ttu-id="92095-175">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="92095-175">Create a virtual network</span></span>
<span data-ttu-id="92095-176">Skapa vNet och undernät för den [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="92095-176">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="92095-177">Skapa undernätet.</span><span class="sxs-lookup"><span data-stu-id="92095-177">Create the subnet.</span></span> <span data-ttu-id="92095-178">Det här exemplet skapar ett undernät med namnet *mySubnet* med adressprefixet för *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="92095-178">This example creates a subnet named *mySubnet* with the address prefix of *10.0.0.0/24*.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="92095-179">Skapa det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="92095-179">Create the virtual network.</span></span> <span data-ttu-id="92095-180">Det här exemplet skapar ett virtuellt nätverk med namnet *myVnet* med adressprefixet för *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="92095-180">This example creates a virtual network named *myVnet* with the address prefix of *10.0.0.0/16*.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="92095-181">Skapa en offentlig IP-adress och gränssnitt</span><span class="sxs-lookup"><span data-stu-id="92095-181">Create a public IP address and network interface</span></span>

<span data-ttu-id="92095-182">För att upprätta kommunikation med den virtuella datorn i det virtuella nätverket behöver du en [offentlig IP-adress](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="92095-182">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="92095-183">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="92095-183">Create a public IP address.</span></span> <span data-ttu-id="92095-184">Det här exemplet skapas en offentlig IP-adress med namnet *myPip*.</span><span class="sxs-lookup"><span data-stu-id="92095-184">This example creates a public IP address named *myPip*.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="92095-185">Skapa nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="92095-185">Create the NIC.</span></span> <span data-ttu-id="92095-186">Det här exemplet skapar ett nätverkskort med namnet **myNic**.</span><span class="sxs-lookup"><span data-stu-id="92095-186">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="92095-187">Skapa säkerhetsgrupp för nätverk och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="92095-187">Create the network security group and an RDP rule</span></span>

<span data-ttu-id="92095-188">Du måste ha en nätverkssäkerhetsregeln (NSG) som gör att RDP-åtkomst på port 3389 för att kunna logga in på den virtuella datorn med RDP.</span><span class="sxs-lookup"><span data-stu-id="92095-188">To be able to log in to your VM using RDP, you need to have a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="92095-189">Det här exemplet skapas en NSG som heter *myNsg* som innehåller en regel med namnet *myRdpRule* som tillåter RDP-trafik via port 3389.</span><span class="sxs-lookup"><span data-stu-id="92095-189">This example creates an NSG named *myNsg* that contains a rule called *myRdpRule* that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="92095-190">Mer information om NSG: er finns [öppna portar till en virtuell dator i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="92095-190">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

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


## <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="92095-191">Skapa en variabel för det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="92095-191">Create a variable for the virtual network</span></span>

<span data-ttu-id="92095-192">Skapa en variabel för det virtuella nätverket som slutförda.</span><span class="sxs-lookup"><span data-stu-id="92095-192">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a><span data-ttu-id="92095-193">Hämta autentiseringsuppgifterna för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="92095-193">Get the credentials for the VM</span></span>

<span data-ttu-id="92095-194">Följande cmdlet öppnas ett fönster där du ska ange ett nytt användarnamn och lösenord ska användas som ett lokalt administratörskonto för att få fjärråtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="92095-194">The following cmdlet will open a window where you will enter a new user name and password to use as the local administrator account for remotely accessing the VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="add-the-vm-name-and-size-to-the-vm-configuration"></a><span data-ttu-id="92095-195">Lägg till VM-namnet och storleken i VM-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="92095-195">Add the VM name and size to the VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a><span data-ttu-id="92095-196">Ange VM-avbildning som källbilden för den nya virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="92095-196">Set the VM image as source image for the new VM</span></span>

<span data-ttu-id="92095-197">Ange källa avbildningen med ID: T för den hanterade VM-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="92095-197">Set the source image using the ID of the managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a><span data-ttu-id="92095-198">Ange Operativsystemets konfiguration och Lägg till nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="92095-198">Set the OS configuration and add the NIC.</span></span>

<span data-ttu-id="92095-199">Ange lagringstypen av (PremiumLRS eller StandardLRS) och storleken på operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="92095-199">Enter the storage type (PremiumLRS or StandardLRS) and the size of the OS disk.</span></span> <span data-ttu-id="92095-200">Det här exemplet anges kontotypen *PremiumLRS*, diskens storlek till *128 GB* och disk caching *ReadWrite*.</span><span class="sxs-lookup"><span data-stu-id="92095-200">This example sets the account type to *PremiumLRS*, the disk size to *128 GB* and disk caching to *ReadWrite*.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a><span data-ttu-id="92095-201">Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="92095-201">Create the VM</span></span>

<span data-ttu-id="92095-202">Skapa den nya virtuella datorn med hjälp av konfigurationen som lagrats i den **$vm** variabeln.</span><span class="sxs-lookup"><span data-stu-id="92095-202">Create the new VM using the configuration stored in the **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="92095-203">Kontrollera att den virtuella datorn har skapats</span><span class="sxs-lookup"><span data-stu-id="92095-203">Verify that the VM was created</span></span>
<span data-ttu-id="92095-204">När du är klar bör du se den nya virtuella datorn i den [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="92095-204">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="92095-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92095-205">Next steps</span></span>

<span data-ttu-id="92095-206">Om du vill logga in till din nya virtuella datorn, bläddra till den virtuella datorn i den [portal](https://portal.azure.com), klickar du på **Anslut**, öppna fjärrskrivbord RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="92095-206">To sign in to your new virtual machine, browse to the VM in the [portal](https://portal.azure.com), click **Connect**, and open the Remote Desktop RDP file.</span></span> <span data-ttu-id="92095-207">Använda kontouppgifter i den ursprungliga virtuella datorn för att logga in på den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="92095-207">Use the account credentials of your original virtual machine to sign in to your new virtual machine.</span></span> <span data-ttu-id="92095-208">Mer information finns i [ansluta och logga in på en virtuell Azure-dator kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="92095-208">For more information, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

