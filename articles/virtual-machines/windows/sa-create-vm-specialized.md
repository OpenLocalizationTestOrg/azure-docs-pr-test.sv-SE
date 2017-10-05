---
title: "Skapa virtuell dator från en särskild disk i Azure | Microsoft Docs"
description: "Skapa en ny virtuell dator genom att koppla en särskild ohanterade disk i Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: 974d89aa96cba94fedfd1acbaf4f1d30ac8e6257
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a><span data-ttu-id="955ce-103">Skapa en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="955ce-103">Create a VM from a specialized VHD in a storage account</span></span>

<span data-ttu-id="955ce-104">Skapa en ny virtuell dator genom att koppla en särskild ohanterade disk som OS-disk med hjälp av Powershell.</span><span class="sxs-lookup"><span data-stu-id="955ce-104">Create a new VM by attaching a specialized unmanaged disk as the OS disk using Powershell.</span></span> <span data-ttu-id="955ce-105">En specialiserad disk är en kopia av VHD från en befintlig virtuell dator som hanterar användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="955ce-105">A specialized disk is a copy of VHD from an existing VM that maintains the user accounts, applications and other state data from your original VM.</span></span> 

<span data-ttu-id="955ce-106">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="955ce-106">You have two options:</span></span>
* [<span data-ttu-id="955ce-107">Överföra en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="955ce-107">Upload a VHD</span></span>](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="955ce-108">Kopiera VHD från en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="955ce-108">Copy the VHD of an existing Azure VM</span></span>](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a><span data-ttu-id="955ce-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="955ce-109">Before you begin</span></span>
<span data-ttu-id="955ce-110">Om du använder PowerShell kan du kontrollera att du har den senaste versionen av AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="955ce-110">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="955ce-111">Kör följande kommando för att installera den.</span><span class="sxs-lookup"><span data-stu-id="955ce-111">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute 
```
<span data-ttu-id="955ce-112">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="955ce-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="955ce-113">Alternativ 1: Ladda upp en särskild virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="955ce-113">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="955ce-114">Du kan överföra den virtuella Hårddisken från en särskild virtuell dator som skapats med ett lokalt virtualisering verktyg, som Hyper-V eller en virtuell dator som har exporterats från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="955ce-114">You can upload the VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-the-vm"></a><span data-ttu-id="955ce-115">Förbereda den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="955ce-115">Prepare the VM</span></span>
<span data-ttu-id="955ce-116">Du kan ladda upp en särskild virtuell Hårddisk som har skapats med en lokal dator eller en virtuell Hårddisk som exporterats från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="955ce-116">You can upload a specialized VHD that was created using an on-premises VM or a VHD exported from another cloud.</span></span> <span data-ttu-id="955ce-117">En särskild virtuell Hårddisk har användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="955ce-117">A specialized VHD maintains the user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="955ce-118">Om du tänker använda den virtuella Hårddisken som – är att skapa en ny virtuell dator och se till att följande åtgärder har slutförts.</span><span class="sxs-lookup"><span data-stu-id="955ce-118">If you intend to use the VHD as-is to create a new VM, ensure the following steps are completed.</span></span> 
  
  * <span data-ttu-id="955ce-119">[Förbereda en Windows-VHD att överföra till Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="955ce-119">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="955ce-120">**Inte** generalisera den virtuella datorn med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="955ce-120">**Do not** generalize the VM using Sysprep.</span></span>
  * <span data-ttu-id="955ce-121">Ta bort gäst virtualiseringsverktyg och agenter som installerats på den virtuella datorn (d.v.s. VMware-verktyg).</span><span class="sxs-lookup"><span data-stu-id="955ce-121">Remove any guest virtualization tools and agents that are installed on the VM (i.e. VMware tools).</span></span>
  * <span data-ttu-id="955ce-122">Se till att den virtuella datorn är konfigurerad för att hämta dess IP-adress och DNS-inställningarna via DHCP.</span><span class="sxs-lookup"><span data-stu-id="955ce-122">Ensure the VM is configured to pull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="955ce-123">Detta säkerställer att servern erhåller en IP-adress inom VNet när den startas.</span><span class="sxs-lookup"><span data-stu-id="955ce-123">This ensures that the server obtains an IP address within the VNet when it starts up.</span></span> 


### <a name="get-the-storage-account"></a><span data-ttu-id="955ce-124">Hämta storage-konto</span><span class="sxs-lookup"><span data-stu-id="955ce-124">Get the storage account</span></span>
<span data-ttu-id="955ce-125">Du behöver ett lagringskonto i Azure för att lagra överförda VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="955ce-125">You need a storage account in Azure to store the uploaded VM image.</span></span> <span data-ttu-id="955ce-126">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="955ce-126">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="955ce-127">Om du vill visa tillgängliga storage-konton, skriver du:</span><span class="sxs-lookup"><span data-stu-id="955ce-127">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="955ce-128">Om du vill använda ett befintligt lagringskonto fortsätter du till den [överför den Virtuella datoravbildningen](#upload-the-vm-vhd-to-your-storage-account) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="955ce-128">If you want to use an existing storage account, proceed to the [Upload the VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="955ce-129">Följ dessa steg om du behöver skapa ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="955ce-129">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="955ce-130">Du måste namnet på resursgruppen där lagringskontot ska skapas.</span><span class="sxs-lookup"><span data-stu-id="955ce-130">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="955ce-131">Om du vill ta reda på alla resursgrupper i din prenumeration, skriver du:</span><span class="sxs-lookup"><span data-stu-id="955ce-131">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="955ce-132">Så här skapar du en resursgrupp med namnet **myResourceGroup** i den **västra USA** region, typ:</span><span class="sxs-lookup"><span data-stu-id="955ce-132">To create a resource group named **myResourceGroup** in the **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="955ce-133">Skapa ett lagringskonto med namnet **mittlagringskonto** i den här resursgruppen med hjälp av den [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="955ce-133">Create a storage account named **mystorageaccount** in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
### <a name="upload-the-vhd-to-your-storage-account"></a><span data-ttu-id="955ce-134">Överför den virtuella Hårddisken till ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="955ce-134">Upload the VHD to your storage account</span></span>
<span data-ttu-id="955ce-135">Använd den [Lägg till AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) för att ladda upp avbildningen till en behållare i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="955ce-135">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the image to a container in your storage account.</span></span> <span data-ttu-id="955ce-136">Det här exemplet överför filen **myVHD.vhd** från `"C:\Users\Public\Documents\Virtual hard disks\"` till ett lagringskonto med namnet **mittlagringskonto** i den **myResourceGroup** resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="955ce-136">This example uploads the file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named **mystorageaccount** in the **myResourceGroup** resource group.</span></span> <span data-ttu-id="955ce-137">Filen placeras i behållare med namnet **minbehållare** och det nya namnet kommer att **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="955ce-137">The file will be placed into the container named **mycontainer** and the new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="955ce-138">Om det lyckas, kan du få ett svar som liknar denna:</span><span class="sxs-lookup"><span data-stu-id="955ce-138">If successful, you get a response that looks similar to this:</span></span>

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

<span data-ttu-id="955ce-139">Det här kommandot kan ta en stund att slutföra beroende på nätverksanslutningen och storleken på VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="955ce-139">Depending on your network connection and the size of your VHD file, this command may take a while to complete.</span></span>


## <a name="option-2-copy-the-vhd-from-an-existing-azure-vm"></a><span data-ttu-id="955ce-140">Alternativ 2: Kopiera den virtuella Hårddisken från en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="955ce-140">Option 2: Copy the VHD from an existing Azure VM</span></span>

<span data-ttu-id="955ce-141">Du kan kopiera en virtuell Hårddisk till en annan storage-konto som ska användas när du skapar en ny, dubbla virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="955ce-141">You can copy a VHD to another storage account to use when creating a new, duplicate VM.</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="955ce-142">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="955ce-142">Before you begin</span></span>
<span data-ttu-id="955ce-143">Se till att du:</span><span class="sxs-lookup"><span data-stu-id="955ce-143">Make sure that you:</span></span>

* <span data-ttu-id="955ce-144">Innehåller information om den **käll- och storage-konton**.</span><span class="sxs-lookup"><span data-stu-id="955ce-144">Have information about the **source and destination storage accounts**.</span></span> <span data-ttu-id="955ce-145">Du måste ha storage-konto och en behållare namn för den Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="955ce-145">For the source VM, you need to have the storage account and container names.</span></span> <span data-ttu-id="955ce-146">Vanligtvis behållarens namn kommer att **virtuella hårddiskar**.</span><span class="sxs-lookup"><span data-stu-id="955ce-146">Usually, the container name will be **vhds**.</span></span> <span data-ttu-id="955ce-147">Du måste också ha en mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="955ce-147">You also need to have a destination storage account.</span></span> <span data-ttu-id="955ce-148">Om du inte redan har en, du kan skapa en med hjälp av antingen portalen (**fler tjänster** > lagringskonton > Lägg till) eller med hjälp av den [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="955ce-148">If you don't already have one, you can create one using either the portal (**More Services** > Storage accounts > Add) or using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet.</span></span> 
* <span data-ttu-id="955ce-149">Har hämtat och installerat den [AzCopy verktyget](../../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="955ce-149">Have downloaded and installed the [AzCopy tool](../../storage/common/storage-use-azcopy.md).</span></span> 

### <a name="deallocate-the-vm"></a><span data-ttu-id="955ce-150">Frigör den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="955ce-150">Deallocate the VM</span></span>
<span data-ttu-id="955ce-151">Frigör den virtuella datorn, vilket frigör den virtuella Hårddisken ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="955ce-151">Deallocate the VM, which frees up the VHD to be copied.</span></span> 

* <span data-ttu-id="955ce-152">**Portalen**: Klicka på **virtuella datorer** > **myVM** > Stoppa</span><span class="sxs-lookup"><span data-stu-id="955ce-152">**Portal**: Click **Virtual machines** > **myVM** > Stop</span></span>
* <span data-ttu-id="955ce-153">**PowerShell**: Använd [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) att stoppa (frigöra) den virtuella datorn med namnet **myVM** i resursgruppen **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="955ce-153">**Powershell**: Use [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) to stop (deallocate) the VM named **myVM** in resource group **myResourceGroup**.</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="955ce-154">Den **Status** för den virtuella datorn i Azure portal ändras från **stoppad** till **Stoppad (frigjord)**.</span><span class="sxs-lookup"><span data-stu-id="955ce-154">The **Status** for the VM in the Azure portal changes from **Stopped** to **Stopped (deallocated)**.</span></span>

### <a name="get-the-storage-account-urls"></a><span data-ttu-id="955ce-155">Hämta URL: er för storage-konto</span><span class="sxs-lookup"><span data-stu-id="955ce-155">Get the storage account URLs</span></span>
<span data-ttu-id="955ce-156">Du behöver URL: er för de käll- och storage-kontona.</span><span class="sxs-lookup"><span data-stu-id="955ce-156">You need the URLs of the source and destination storage accounts.</span></span> <span data-ttu-id="955ce-157">URL: er se ut: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span><span class="sxs-lookup"><span data-stu-id="955ce-157">The URLs look like: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span></span> <span data-ttu-id="955ce-158">Du kan bara ersätta information mellan hakparenteser för att skapa din URL Om du redan vet lagernamnet konto och en behållare.</span><span class="sxs-lookup"><span data-stu-id="955ce-158">If you already know the storage account and container name, you can just replace the information between the brackets to create your URL.</span></span> 

<span data-ttu-id="955ce-159">Du kan använda Azure-portalen eller Azure Powershell för att hämta Webbadress:</span><span class="sxs-lookup"><span data-stu-id="955ce-159">You can use the Azure portal or Azure Powershell to get the URL:</span></span>

* <span data-ttu-id="955ce-160">**Portalen**: Klicka på den  **>**  för **fler tjänster** > **lagringskonton**  >   *lagringskontot* > **Blobbar** och käll-VHD-filen är förmodligen i den **virtuella hårddiskar** behållare.</span><span class="sxs-lookup"><span data-stu-id="955ce-160">**Portal**: Click the **>** for **More services** > **Storage accounts** > *storage account* > **Blobs** and your source VHD file is probably in the **vhds** container.</span></span> <span data-ttu-id="955ce-161">Klicka på **egenskaper** för behållaren och kopiera texten märkta **URL**.</span><span class="sxs-lookup"><span data-stu-id="955ce-161">Click **Properties** for the container, and copy the text labeled **URL**.</span></span> <span data-ttu-id="955ce-162">Du behöver URL: er för både käll- och behållare.</span><span class="sxs-lookup"><span data-stu-id="955ce-162">You'll need the URLs of both the source and destination containers.</span></span> 
* <span data-ttu-id="955ce-163">**PowerShell**: Använd [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) att hämta information för den virtuella datorn med namnet **myVM** i resursgruppen **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="955ce-163">**Powershell**: Use [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) to get the information for VM named **myVM** in the resource group **myResourceGroup**.</span></span> <span data-ttu-id="955ce-164">I resultaten, titta i den **lagringsprofil** avsnittet för den **Vhd-Uri**.</span><span class="sxs-lookup"><span data-stu-id="955ce-164">In the results, look in the **Storage profile** section for the **Vhd Uri**.</span></span> <span data-ttu-id="955ce-165">Den första delen av URI: N är Webbadressen till behållaren och den sista delen är virtuella Hårddiskens OS-namn för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="955ce-165">The first part of the Uri is the URL to the container and the last part is the OS VHD name for the VM.</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-the-storage-access-keys"></a><span data-ttu-id="955ce-166">Hämta åtkomstnycklar för lagring</span><span class="sxs-lookup"><span data-stu-id="955ce-166">Get the storage access keys</span></span>
<span data-ttu-id="955ce-167">Hitta åtkomstnycklarna för käll- och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="955ce-167">Find the access keys for the source and destination storage accounts.</span></span> <span data-ttu-id="955ce-168">Läs mer om åtkomstnycklarna [om Azure storage-konton](../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="955ce-168">For more information about access keys, see [About Azure storage accounts](../../storage/common/storage-create-storage-account.md).</span></span>

* <span data-ttu-id="955ce-169">**Portalen**: Klicka på **fler tjänster** > **lagringskonton** > *lagringskonto*  >  **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="955ce-169">**Portal**: Click **More services** > **Storage accounts** > *storage account* > **Access keys**.</span></span> <span data-ttu-id="955ce-170">Kopiera den nyckel som är märkta som **key1**.</span><span class="sxs-lookup"><span data-stu-id="955ce-170">Copy the key labeled as **key1**.</span></span>
* <span data-ttu-id="955ce-171">**PowerShell**: Använd [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) att hämta lagringsnyckeln för lagringskontot **mittlagringskonto** i resursgruppen **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="955ce-171">**Powershell**: Use [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) to get the storage key for the storage account **mystorageaccount** in the resource group **myResourceGroup**.</span></span> <span data-ttu-id="955ce-172">Kopiera den nyckel som är märkta **key1**.</span><span class="sxs-lookup"><span data-stu-id="955ce-172">Copy the key labeled **key1**.</span></span>

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-the-vhd"></a><span data-ttu-id="955ce-173">Kopiera den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="955ce-173">Copy the VHD</span></span>
<span data-ttu-id="955ce-174">Du kan kopiera filer mellan storage-konton med hjälp av AzCopy.</span><span class="sxs-lookup"><span data-stu-id="955ce-174">You can copy files between storage accounts using AzCopy.</span></span> <span data-ttu-id="955ce-175">För Målbehållaren om den angivna behållaren inte finns, kommer den att skapas för dig.</span><span class="sxs-lookup"><span data-stu-id="955ce-175">For the destination container, if the specified container doesn't exist, it will be created for you.</span></span> 

<span data-ttu-id="955ce-176">Du använder AzCopy, öppna Kommandotolken på den lokala datorn och navigera till mappen där AzCopy är installerat.</span><span class="sxs-lookup"><span data-stu-id="955ce-176">To use AzCopy, open a command prompt on your local machine and navigate to the folder where AzCopy is installed.</span></span> <span data-ttu-id="955ce-177">Den kommer att likna *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="955ce-177">It will be similar to *C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy*.</span></span> 

<span data-ttu-id="955ce-178">Om du vill kopiera alla filer i en behållare som du använder den **/S** växla.</span><span class="sxs-lookup"><span data-stu-id="955ce-178">To copy all of the files within a container, you use the **/S** switch.</span></span> <span data-ttu-id="955ce-179">Detta kan användas för att kopiera den OS virtuella Hårddisken och alla datadiskar om de finns i samma behållare.</span><span class="sxs-lookup"><span data-stu-id="955ce-179">This can be used to copy the OS VHD and all of the data disks if they are in the same container.</span></span> <span data-ttu-id="955ce-180">Det här exemplet illustrerar hur du kopierar alla filer i behållaren **mysourcecontainer** i lagringskonto **mysourcestorageaccount** till behållaren **mydestinationcontainer**i den **mydestinationstorageaccount** storage-konto.</span><span class="sxs-lookup"><span data-stu-id="955ce-180">This example shows how to copy all of the files in the container **mysourcecontainer** in storage account **mysourcestorageaccount** to the container **mydestinationcontainer** in the **mydestinationstorageaccount** storage account.</span></span> <span data-ttu-id="955ce-181">Ersätta namnen på storage-konton och behållare med din egen.</span><span class="sxs-lookup"><span data-stu-id="955ce-181">Replace the names of the storage accounts and containers with your own.</span></span> <span data-ttu-id="955ce-182">Ersätt `<sourceStorageAccountKey1>` och `<destinationStorageAccountKey1>` med egna nycklar.</span><span class="sxs-lookup"><span data-stu-id="955ce-182">Replace `<sourceStorageAccountKey1>` and `<destinationStorageAccountKey1>` with your own keys.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

<span data-ttu-id="955ce-183">Om du bara vill kopiera en viss VHD i en behållare med flera filer kan du också ange namnet på filen med hjälp av växeln /Pattern.</span><span class="sxs-lookup"><span data-stu-id="955ce-183">If you only want to copy a specific VHD in a container with multiple files, you can also specify the file name using the /Pattern switch.</span></span> <span data-ttu-id="955ce-184">I detta exempel på filen som heter **myFileName.vhd** ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="955ce-184">In this example, only the file named **myFileName.vhd** will be copied.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


<span data-ttu-id="955ce-185">När den är klar, visas ett meddelande som ser ut ungefär så:</span><span class="sxs-lookup"><span data-stu-id="955ce-185">When it is finished, you will get a message that looks something like:</span></span>

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a><span data-ttu-id="955ce-186">Felsökning</span><span class="sxs-lookup"><span data-stu-id="955ce-186">Troubleshooting</span></span>
* <span data-ttu-id="955ce-187">När du använder AZCopy, om du ser felet ”servern gick inte att autentisera begäran”, se till att är värdet för Authorization-huvud formaterad korrekt inklusive signaturen.</span><span class="sxs-lookup"><span data-stu-id="955ce-187">When you use AZCopy, if you see the error "Server failed to authenticate the request", make sure the value of the Authorization header is formed correctly including the signature.</span></span> <span data-ttu-id="955ce-188">Om du använder nyckel 2 eller sekundära lagringsplatsen nyckeln, försök med den primära eller 1-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="955ce-188">If you are using Key 2 or the secondary storage key, try using the primary or 1st storage key.</span></span>

## <a name="create-the-new-vm"></a><span data-ttu-id="955ce-189">Skapa ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="955ce-189">Create the new VM</span></span> 

<span data-ttu-id="955ce-190">Du måste skapa nätverk och andra VM resurser som ska användas av den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="955ce-190">You need to create networking and other VM resources to be used by the new VM.</span></span>

### <a name="create-the-subnet-and-vnet"></a><span data-ttu-id="955ce-191">Skapa undernät och virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="955ce-191">Create the subNet and vNet</span></span>

<span data-ttu-id="955ce-192">Skapa vNet och undernät för den [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="955ce-192">Create the vNet and subNet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="955ce-193">Skapa undernätet.</span><span class="sxs-lookup"><span data-stu-id="955ce-193">Create the subNet.</span></span> <span data-ttu-id="955ce-194">Det här exemplet skapar ett undernät med namnet **mySubNet**, i resursgrupp **myResourceGroup**, och anger adressprefixet undernät till **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="955ce-194">This example creates a subnet named **mySubNet**, in the resource group **myResourceGroup**, and sets the subnet address prefix to **10.0.0.0/24**.</span></span>
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="955ce-195">Skapa vNet.</span><span class="sxs-lookup"><span data-stu-id="955ce-195">Create the vNet.</span></span> <span data-ttu-id="955ce-196">Det här exemplet anger det virtuella nätverksnamnet ska **myVnetName**, platsen för **västra USA**, och adressprefixet för det virtuella nätverket till **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="955ce-196">This example sets the virtual network name to be **myVnetName**, the location to **West US**, and the address prefix for the virtual network to **10.0.0.0/16**.</span></span> 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="955ce-197">Skapa en offentlig IP-adress och ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="955ce-197">Create a public IP address and NIC</span></span>
<span data-ttu-id="955ce-198">För att upprätta kommunikation med den virtuella datorn i det virtuella nätverket behöver du en [offentlig IP-adress](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="955ce-198">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="955ce-199">Skapa offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="955ce-199">Create the public IP.</span></span> <span data-ttu-id="955ce-200">I det här exemplet anges det offentliga IP-adress namnet till **myIP**.</span><span class="sxs-lookup"><span data-stu-id="955ce-200">In this example, the public IP address name is set to **myIP**.</span></span>
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="955ce-201">Skapa nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="955ce-201">Create the NIC.</span></span> <span data-ttu-id="955ce-202">I det här exemplet NIC-namnet har angetts **myNicName**.</span><span class="sxs-lookup"><span data-stu-id="955ce-202">In this example, the NIC name is set to **myNicName**.</span></span>
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="955ce-203">Skapa säkerhetsgrupp för nätverk och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="955ce-203">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="955ce-204">Du måste ha en säkerhetsregel som tillåter RDP-åtkomst på port 3389 för att kunna logga in på den virtuella datorn med RDP.</span><span class="sxs-lookup"><span data-stu-id="955ce-204">To be able to log in to your VM using RDP, you need to have an security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="955ce-205">Eftersom den virtuella Hårddisken för den nya virtuella datorn har skapats från en befintlig kan särskild virtuell dator när den virtuella datorn skapas du använda ett befintligt konto från den virtuella källdatorn som har behörighet att logga in med RDP.</span><span class="sxs-lookup"><span data-stu-id="955ce-205">Because the VHD for the new VM was created from an existing specialized VM, after the VM is created you can use an existing account from the source virtual machine that had permission to log on using RDP.</span></span>
<span data-ttu-id="955ce-206">Det här exemplet anger NSG namn **myNsg** och Regelnamnet RDP till **myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="955ce-206">This example sets the NSG name to **myNsg** and the RDP rule name to **myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="955ce-207">Mer information om slutpunkter och NSG-regler finns [öppna portar till en virtuell dator i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="955ce-207">For more information about endpoints and NSG rules, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="set-the-vm-name-and-size"></a><span data-ttu-id="955ce-208">Ange namn på virtuell dator och storlek</span><span class="sxs-lookup"><span data-stu-id="955ce-208">Set the VM name and size</span></span>

<span data-ttu-id="955ce-209">Det här exemplet anger VM-namn till ”myVM” och VM-storlek till ”Standard_A2”.</span><span class="sxs-lookup"><span data-stu-id="955ce-209">This example sets the VM name to "myVM" and the VM size to "Standard_A2".</span></span>
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a><span data-ttu-id="955ce-210">Lägg till nätverkskortet</span><span class="sxs-lookup"><span data-stu-id="955ce-210">Add the NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-the-os-disk"></a><span data-ttu-id="955ce-211">Konfigurera OS-disk</span><span class="sxs-lookup"><span data-stu-id="955ce-211">Configure the OS disk</span></span>

1. <span data-ttu-id="955ce-212">Ange URI för den virtuella Hårddisken som du laddat upp eller kopieras.</span><span class="sxs-lookup"><span data-stu-id="955ce-212">Set the URI for the VHD that you uploaded or copied.</span></span> <span data-ttu-id="955ce-213">I det här exemplet VHD-filen med namnet **myOsDisk.vhd** sparas i ett lagringskonto med namnet **Mittlagringskonto** i en behållare med namnet **Minbehållare**.</span><span class="sxs-lookup"><span data-stu-id="955ce-213">In this example, the VHD file named **myOsDisk.vhd** is kept in a storage account named **myStorageAccount** in a container named **myContainer**.</span></span>

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. <span data-ttu-id="955ce-214">Lägg till OS-disk.</span><span class="sxs-lookup"><span data-stu-id="955ce-214">Add the OS disk.</span></span> <span data-ttu-id="955ce-215">I det här exemplet är termen ”osDisk” appened till VM-namn för att skapa OS-disknamnet när OS-disken skapas.</span><span class="sxs-lookup"><span data-stu-id="955ce-215">In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name.</span></span> <span data-ttu-id="955ce-216">Det här exemplet anger också att den här Windows-baserade virtuella Hårddisken ska kopplas till den virtuella datorn som OS-disk.</span><span class="sxs-lookup"><span data-stu-id="955ce-216">This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.</span></span>
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

<span data-ttu-id="955ce-217">Valfritt: Om du har datadiskar som måste vara kopplad till den virtuella datorn, Lägg till datadiskar med hjälp av URL: er för data virtuella hårddiskar och den lämpliga logiska enhetsnummer (Lun).</span><span class="sxs-lookup"><span data-stu-id="955ce-217">Optional: If you have data disks that need to be attached to the VM, add the data disks by using the URLs of data VHDs and the appropriate Logical Unit Number (Lun).</span></span>

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

<span data-ttu-id="955ce-218">När du använder ett lagringskonto, data och operativsystemet disk webbadresser ut ungefär så här: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span><span class="sxs-lookup"><span data-stu-id="955ce-218">When using a storage account, the data and operating system disk URLs look something like this: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span></span> <span data-ttu-id="955ce-219">Du hittar det på portalen genom att bläddra till målbehållare för lagring, klicka på operativsystemet eller data VHD som har kopierats och sedan kopiera innehållet i URL: en.</span><span class="sxs-lookup"><span data-stu-id="955ce-219">You can find this on the portal by browsing to the target storage container, clicking the operating system or data VHD that was copied, and then copying the contents of the URL.</span></span>


### <a name="complete-the-vm"></a><span data-ttu-id="955ce-220">Slutför den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="955ce-220">Complete the VM</span></span> 

<span data-ttu-id="955ce-221">Skapa den virtuella datorn med hjälp av de konfigurationer som vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="955ce-221">Create the VM using the configurations that we just created.</span></span>

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

<span data-ttu-id="955ce-222">Om det här kommandot lyckades visas utdata som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="955ce-222">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="955ce-223">Kontrollera att den virtuella datorn har skapats</span><span class="sxs-lookup"><span data-stu-id="955ce-223">Verify that the VM was created</span></span>
<span data-ttu-id="955ce-224">Du bör se den nyligen skapade Virtuellt antingen i den [Azure-portalen](https://portal.azure.com)under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="955ce-224">You should see the newly created VM either in the [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="955ce-225">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="955ce-225">Next steps</span></span>
<span data-ttu-id="955ce-226">Om du vill logga in till din nya virtuella datorn, bläddra till den virtuella datorn i den [portal](https://portal.azure.com), klickar du på **Anslut**, öppna fjärrskrivbord RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="955ce-226">To sign in to your new virtual machine, browse to the VM in the [portal](https://portal.azure.com), click **Connect**, and open the Remote Desktop RDP file.</span></span> <span data-ttu-id="955ce-227">Använda kontouppgifter i den ursprungliga virtuella datorn för att logga in på den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="955ce-227">Use the account credentials of your original virtual machine to sign in to your new virtual machine.</span></span> <span data-ttu-id="955ce-228">Mer information finns i [ansluta och logga in på en virtuell Azure-dator kör Windows](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="955ce-228">For more information, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md).</span></span>

