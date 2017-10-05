---
title: "Skapa en virtuell Windows-dator från en särskild virtuell Hårddisk i Azure | Microsoft Docs"
description: "Skapa en ny Windows virtuell dator genom att koppla en särskild hanterade diskar som OS-disk i Resource Manager-distributionsmodellen."
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
ms.date: 06/29/2017
ms.author: cynthn
ms.openlocfilehash: fa952286a9ceca8b3b2c7efe2cc4867a2728c477
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a><span data-ttu-id="551d8-103">Skapa en virtuell Windows-dator från en särskild disk</span><span class="sxs-lookup"><span data-stu-id="551d8-103">Create a Windows VM from a specialized disk</span></span>

<span data-ttu-id="551d8-104">Skapa en ny virtuell dator genom att koppla en särskild hanterade diskar som OS-disk med hjälp av Powershell.</span><span class="sxs-lookup"><span data-stu-id="551d8-104">Create a new VM by attaching a specialized managed disk as the OS disk using Powershell.</span></span> <span data-ttu-id="551d8-105">En specialiserad disk är en kopia av virtuell hårddisk (VHD) från en befintlig virtuell dator som hanterar användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="551d8-105">A specialized disk is a copy of virtual hard disk (VHD) from an existing VM that maintains the user accounts, applications, and other state data from your original VM.</span></span> 

<span data-ttu-id="551d8-106">När du använder en särskild virtuell Hårddisk för att skapa en ny virtuell dator, behåller datornamnet för den ursprungliga virtuella datorn i den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="551d8-106">When you use a specialized VHD to create a new VM, the new VM retains the computer name of the original VM.</span></span> <span data-ttu-id="551d8-107">Andra datorspecifik information sparas också och i vissa fall kan den här dubblettinformation kan orsaka problem.</span><span class="sxs-lookup"><span data-stu-id="551d8-107">Other computer-specific information is also be kept and, in some cases, this duplicate information could cause issues.</span></span> <span data-ttu-id="551d8-108">Ta reda på vilka typer av information om datorn dina program förlitar sig på när du kopierar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="551d8-108">Be aware of what types of computer-specific information your applications rely on when copying a VM.</span></span>

<span data-ttu-id="551d8-109">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="551d8-109">You have two options:</span></span>
* [<span data-ttu-id="551d8-110">Överföra en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="551d8-110">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="551d8-111">Kopiera en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="551d8-111">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

<span data-ttu-id="551d8-112">Det här avsnittet visar hur du använder hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="551d8-112">This topic shows you how to use managed disks.</span></span> <span data-ttu-id="551d8-113">Om du har en äldre distribution som kräver att du använder ett lagringskonto, se [skapa en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto](sa-create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="551d8-113">If you have a legacy deployment that requires using a storage account, see [Create a VM from a specialized VHD in a storage account](sa-create-vm-specialized.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="551d8-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="551d8-114">Before you begin</span></span>
<span data-ttu-id="551d8-115">Om du använder PowerShell kan du kontrollera att du har den senaste versionen av AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="551d8-115">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="551d8-116">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="551d8-116">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="551d8-117">Alternativ 1: Ladda upp en särskild virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="551d8-117">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="551d8-118">Du kan överföra den virtuella Hårddisken från en särskild virtuell dator som skapats med ett lokalt virtualisering verktyg, som Hyper-V eller en virtuell dator som har exporterats från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="551d8-118">You can upload the VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-the-vm"></a><span data-ttu-id="551d8-119">Förbereda den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="551d8-119">Prepare the VM</span></span>
<span data-ttu-id="551d8-120">Om du tänker använda den virtuella Hårddisken som – är att skapa en ny virtuell dator och se till att följande åtgärder har slutförts.</span><span class="sxs-lookup"><span data-stu-id="551d8-120">If you intend to use the VHD as-is to create a new VM, ensure the following steps are completed.</span></span> 
  
  * <span data-ttu-id="551d8-121">[Förbereda en Windows-VHD att överföra till Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="551d8-121">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="551d8-122">**Inte** generalisera den virtuella datorn med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="551d8-122">**Do not** generalize the VM using Sysprep.</span></span>
  * <span data-ttu-id="551d8-123">Ta bort gäst virtualiseringsverktyg och agenter som installerats på den virtuella datorn (till exempel VMware-verktyg).</span><span class="sxs-lookup"><span data-stu-id="551d8-123">Remove any guest virtualization tools and agents that are installed on the VM (like VMware tools).</span></span>
  * <span data-ttu-id="551d8-124">Se till att den virtuella datorn är konfigurerad för att hämta dess IP-adress och DNS-inställningarna via DHCP.</span><span class="sxs-lookup"><span data-stu-id="551d8-124">Ensure the VM is configured to pull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="551d8-125">Detta säkerställer att servern erhåller en IP-adress inom VNet när den startas.</span><span class="sxs-lookup"><span data-stu-id="551d8-125">This ensures that the server obtains an IP address within the VNet when it starts up.</span></span> 


### <a name="get-the-storage-account"></a><span data-ttu-id="551d8-126">Hämta storage-konto</span><span class="sxs-lookup"><span data-stu-id="551d8-126">Get the storage account</span></span>
<span data-ttu-id="551d8-127">Du behöver ett lagringskonto i Azure för att lagra den överförda virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="551d8-127">You need a storage account in Azure to store the uploaded VHD.</span></span> <span data-ttu-id="551d8-128">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="551d8-128">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="551d8-129">Om du vill visa tillgängliga storage-konton, skriver du:</span><span class="sxs-lookup"><span data-stu-id="551d8-129">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="551d8-130">Om du vill använda ett befintligt lagringskonto fortsätter du till den [överför den virtuella Hårddisken](#upload-the-vhd-to-your-storage-account) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="551d8-130">If you want to use an existing storage account, proceed to the [Upload the VHD](#upload-the-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="551d8-131">Följ dessa steg om du behöver skapa ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="551d8-131">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="551d8-132">Du måste namnet på resursgruppen där lagringskontot ska skapas.</span><span class="sxs-lookup"><span data-stu-id="551d8-132">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="551d8-133">Om du vill ta reda på alla resursgrupper i din prenumeration, skriver du:</span><span class="sxs-lookup"><span data-stu-id="551d8-133">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="551d8-134">Så här skapar du en resursgrupp med namnet *myResourceGroup* i den *västra USA* region, typ:</span><span class="sxs-lookup"><span data-stu-id="551d8-134">To create a resource group named *myResourceGroup* in the *West US* region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="551d8-135">Skapa ett lagringskonto med namnet *mittlagringskonto* i den här resursgruppen med hjälp av den [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="551d8-135">Create a storage account named *mystorageaccount* in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a><span data-ttu-id="551d8-136">Överför den virtuella Hårddisken till ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="551d8-136">Upload the VHD to your storage account</span></span> 
<span data-ttu-id="551d8-137">Använd den [Lägg till AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) för att ladda upp den virtuella Hårddisken till en behållare i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="551d8-137">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the VHD to a container in your storage account.</span></span> <span data-ttu-id="551d8-138">Det här exemplet överför filen *myVHD.vhd* från `"C:\Users\Public\Documents\Virtual hard disks\"` till ett lagringskonto med namnet *mittlagringskonto* i den *myResourceGroup* resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="551d8-138">This example uploads the file *myVHD.vhd* from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named *mystorageaccount* in the *myResourceGroup* resource group.</span></span> <span data-ttu-id="551d8-139">Filen lagras i behållare med namnet *minbehållare* och det nya namnet kommer att *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="551d8-139">The file is stored in the container named *mycontainer* and the new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="551d8-140">Om det lyckas, kan du få ett svar som liknar denna:</span><span class="sxs-lookup"><span data-stu-id="551d8-140">If successful, you get a response that looks similar to this:</span></span>

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

<span data-ttu-id="551d8-141">Det här kommandot kan ta en stund att slutföra beroende på nätverksanslutningen och storleken på VHD-filen</span><span class="sxs-lookup"><span data-stu-id="551d8-141">Depending on your network connection and the size of your VHD file, this command may take a while to complete</span></span>

### <a name="create-a-managed-disk-from-the-vhd"></a><span data-ttu-id="551d8-142">Skapa en hanterade diskar från den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="551d8-142">Create a managed disk from the VHD</span></span>

<span data-ttu-id="551d8-143">Skapa en hanterad disk från särskilda VHD: N i din storage-konto med [ny AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="551d8-143">Create a managed disk from the specialized VHD in your storage account using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="551d8-144">Det här exemplet används **myOSDisk1** för disk-namn, placerar disken i *StandardLRS* lagring och använder *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* som URI för källan VHD.</span><span class="sxs-lookup"><span data-stu-id="551d8-144">This example uses **myOSDisk1** for the disk name, puts the disk in *StandardLRS* storage, and uses *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* as the URI for the source VHD.</span></span>

<span data-ttu-id="551d8-145">Skapa en ny resursgrupp för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="551d8-145">Create a new resource group for the new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="551d8-146">Skapa den nya OS-disken från den överförda virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="551d8-146">Create the new OS disk from the uploaded VHD.</span></span> 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a><span data-ttu-id="551d8-147">Alternativ 2: Kopiera en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="551d8-147">Option 2: Copy an existing Azure VM</span></span>

<span data-ttu-id="551d8-148">Du kan skapa en kopia av en virtuell dator som använder hanterade diskar genom att ta en ögonblicksbild av den virtuella datorn och sedan med den ögonblicksbilden för att skapa en ny hanterade disken och en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="551d8-148">You can create a copy of a VM that uses managed disks by taking a snapshot of the VM, then using that snapshot to create a new managed disk and a new VM.</span></span>


### <a name="take-a-snapshot-of-the-os-disk"></a><span data-ttu-id="551d8-149">Ta en ögonblicksbild av OS-disk</span><span class="sxs-lookup"><span data-stu-id="551d8-149">Take a snapshot of the OS disk</span></span>

<span data-ttu-id="551d8-150">Du kan ta en ögonblicksbild av och en hel virtuell dator (inklusive alla diskar) eller bara en enda disk.</span><span class="sxs-lookup"><span data-stu-id="551d8-150">You can take a snapshot of and entire VM (including all disks) or of just a single disk.</span></span> <span data-ttu-id="551d8-151">Följande steg visar hur du kan ta en ögonblicksbild av bara OS-disk för virtuell dator med hjälp av den [ny AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="551d8-151">The following steps show you how to take a snapshot of just the OS disk of your VM using the [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet.</span></span> 

<span data-ttu-id="551d8-152">Ange vissa parametrar.</span><span class="sxs-lookup"><span data-stu-id="551d8-152">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

<span data-ttu-id="551d8-153">Hämta det Virtuella datorobjektet.</span><span class="sxs-lookup"><span data-stu-id="551d8-153">Get the VM object.</span></span>

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
<span data-ttu-id="551d8-154">Hämta OS-diskens namn.</span><span class="sxs-lookup"><span data-stu-id="551d8-154">Get the OS disk name.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

<span data-ttu-id="551d8-155">Skapa en ögonblicksbild av konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="551d8-155">Create the snapshot configuration.</span></span> 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

<span data-ttu-id="551d8-156">Ta ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="551d8-156">Take the snapshot.</span></span>

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


<span data-ttu-id="551d8-157">Om du planerar att använda ögonblicksbilden för att skapa en virtuell dator som måste vara höga prestanda, använder du parametern `-AccountType Premium_LRS` med kommandot Ny AzureRmSnapshot.</span><span class="sxs-lookup"><span data-stu-id="551d8-157">If you plan to use the snapshot to create a VM that needs to be high performing, use the parameter `-AccountType Premium_LRS` with the New-AzureRmSnapshot command.</span></span> <span data-ttu-id="551d8-158">Parametern skapar ögonblicksbilden så att den lagras som en Premium hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="551d8-158">The parameter creates the snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="551d8-159">Premium-hanterade diskar är dyrare än Standard.</span><span class="sxs-lookup"><span data-stu-id="551d8-159">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="551d8-160">Vara säker på att du verkligen behöver Premium innan du använder parametern.</span><span class="sxs-lookup"><span data-stu-id="551d8-160">So be sure you really need Premium before using the parameter.</span></span>

### <a name="create-a-new-disk-from-the-snapshot"></a><span data-ttu-id="551d8-161">Skapa en ny disk från ögonblicksbilden</span><span class="sxs-lookup"><span data-stu-id="551d8-161">Create a new disk from the snapshot</span></span>

<span data-ttu-id="551d8-162">Skapa en hanterad disk från en ögonblicksbild med hjälp av [ny AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="551d8-162">Create a managed disk from the snapshot using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="551d8-163">Det här exemplet används *myOSDisk* för diskens namn.</span><span class="sxs-lookup"><span data-stu-id="551d8-163">This example uses *myOSDisk* for the disk name.</span></span>

<span data-ttu-id="551d8-164">Skapa en ny resursgrupp för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="551d8-164">Create a new resource group for the new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="551d8-165">Ange namnet på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="551d8-165">Set the OS disk name.</span></span> 

```powershell
$osDiskName = 'myOsDisk'
```

<span data-ttu-id="551d8-166">Skapa den hantera disken.</span><span class="sxs-lookup"><span data-stu-id="551d8-166">Create the managed disk.</span></span>

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a><span data-ttu-id="551d8-167">Skapa ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="551d8-167">Create the new VM</span></span> 

<span data-ttu-id="551d8-168">Skapa nätverk och andra VM resurser som ska användas av den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="551d8-168">Create networking and other VM resources to be used by the new VM.</span></span>

### <a name="create-the-subnet-and-vnet"></a><span data-ttu-id="551d8-169">Skapa undernät och virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="551d8-169">Create the subNet and vNet</span></span>

<span data-ttu-id="551d8-170">Skapa vNet och undernät för den [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="551d8-170">Create the vNet and subNet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="551d8-171">Skapa undernätet.</span><span class="sxs-lookup"><span data-stu-id="551d8-171">Create the subNet.</span></span> <span data-ttu-id="551d8-172">Det här exemplet skapar ett undernät med namnet **mySubNet**, i resursgrupp **myDestinationResourceGroup**, och anger adressprefixet undernät till **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="551d8-172">This example creates a subnet named **mySubNet**, in the resource group **myDestinationResourceGroup**, and sets the subnet address prefix to **10.0.0.0/24**.</span></span>
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="551d8-173">Skapa vNet.</span><span class="sxs-lookup"><span data-stu-id="551d8-173">Create the vNet.</span></span> <span data-ttu-id="551d8-174">Det här exemplet anger det virtuella nätverksnamnet ska **myVnetName**, platsen för **västra USA**, och adressprefixet för det virtuella nätverket till **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="551d8-174">This example sets the virtual network name to be **myVnetName**, the location to **West US**, and the address prefix for the virtual network to **10.0.0.0/16**.</span></span> 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="551d8-175">Skapa säkerhetsgrupp för nätverk och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="551d8-175">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="551d8-176">Du måste ha en säkerhetsregel som tillåter RDP-åtkomst på port 3389 för att kunna logga in på den virtuella datorn med RDP.</span><span class="sxs-lookup"><span data-stu-id="551d8-176">To be able to log in to your VM using RDP, you need to have a security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="551d8-177">Eftersom den virtuella Hårddisken för den nya virtuella datorn har skapats från en befintlig särskild virtuell dator kan du använda ett konto från den virtuella källdatorn för RDP.</span><span class="sxs-lookup"><span data-stu-id="551d8-177">Because the VHD for the new VM was created from an existing specialized VM, you can use an account from the source virtual machine for RDP.</span></span>

<span data-ttu-id="551d8-178">Det här exemplet anger NSG namn **myNsg** och Regelnamnet RDP till **myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="551d8-178">This example sets the NSG name to **myNsg** and the RDP rule name to **myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="551d8-179">Mer information om slutpunkter och NSG-regler finns [öppna portar till en virtuell dator i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="551d8-179">For more information about endpoints and NSG rules, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="551d8-180">Skapa en offentlig IP-adress och ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="551d8-180">Create a public IP address and NIC</span></span>
<span data-ttu-id="551d8-181">För att upprätta kommunikation med den virtuella datorn i det virtuella nätverket behöver du en [offentlig IP-adress](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="551d8-181">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

<span data-ttu-id="551d8-182">Skapa offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="551d8-182">Create the public IP.</span></span> <span data-ttu-id="551d8-183">I det här exemplet anges det offentliga IP-adress namnet till **myIP**.</span><span class="sxs-lookup"><span data-stu-id="551d8-183">In this example, the public IP address name is set to **myIP**.</span></span>
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

<span data-ttu-id="551d8-184">Skapa nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="551d8-184">Create the NIC.</span></span> <span data-ttu-id="551d8-185">I det här exemplet NIC-namnet har angetts **myNicName**.</span><span class="sxs-lookup"><span data-stu-id="551d8-185">In this example, the NIC name is set to **myNicName**.</span></span>
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a><span data-ttu-id="551d8-186">Ange namn på virtuell dator och storlek</span><span class="sxs-lookup"><span data-stu-id="551d8-186">Set the VM name and size</span></span>

<span data-ttu-id="551d8-187">Det här exemplet anger namnet på virtuella datorn till *myVM* och VM-storlek till *Standard_A2*.</span><span class="sxs-lookup"><span data-stu-id="551d8-187">This example sets the VM name to *myVM* and the VM size to *Standard_A2*.</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a><span data-ttu-id="551d8-188">Lägg till nätverkskortet</span><span class="sxs-lookup"><span data-stu-id="551d8-188">Add the NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a><span data-ttu-id="551d8-189">Lägg till OS-disk</span><span class="sxs-lookup"><span data-stu-id="551d8-189">Add the OS disk</span></span> 

<span data-ttu-id="551d8-190">Lägg till OS-disk i en konfiguration med hjälp av [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="551d8-190">Add the OS disk to the configuration using [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span> <span data-ttu-id="551d8-191">Det här exemplet anger storleken på disken för att *128 GB* och bifogar hanterade disken som en *Windows* OS-disken.</span><span class="sxs-lookup"><span data-stu-id="551d8-191">This example sets the size of the disk to *128 GB* and attaches the managed disk as a *Windows* OS disk.</span></span>
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a><span data-ttu-id="551d8-192">Slutför den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="551d8-192">Complete the VM</span></span> 

<span data-ttu-id="551d8-193">Skapa en virtuell dator med hjälp av [ny AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)de konfigurationer som vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="551d8-193">Create the VM using [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)the configurations that we just created.</span></span>

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

<span data-ttu-id="551d8-194">Om det här kommandot lyckades visas utdata som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="551d8-194">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="551d8-195">Kontrollera att den virtuella datorn har skapats</span><span class="sxs-lookup"><span data-stu-id="551d8-195">Verify that the VM was created</span></span>
<span data-ttu-id="551d8-196">Du bör se den nyligen skapade Virtuellt antingen i den [Azure-portalen](https://portal.azure.com)under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="551d8-196">You should see the newly created VM either in the [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="551d8-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="551d8-197">Next steps</span></span>
<span data-ttu-id="551d8-198">Om du vill logga in till din nya virtuella datorn, bläddra till den virtuella datorn i den [portal](https://portal.azure.com), klickar du på **Anslut**, öppna fjärrskrivbord RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="551d8-198">To sign in to your new virtual machine, browse to the VM in the [portal](https://portal.azure.com), click **Connect**, and open the Remote Desktop RDP file.</span></span> <span data-ttu-id="551d8-199">Använda kontouppgifter i den ursprungliga virtuella datorn för att logga in på den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="551d8-199">Use the account credentials of your original virtual machine to sign in to your new virtual machine.</span></span> <span data-ttu-id="551d8-200">Mer information finns i [ansluta och logga in på en virtuell Azure-dator kör Windows](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="551d8-200">For more information, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md).</span></span>

