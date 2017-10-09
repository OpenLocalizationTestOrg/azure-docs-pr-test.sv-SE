---
title: "aaaCreate en virtuell Windows-dator från en särskild virtuell Hårddisk i Azure | Microsoft Docs"
description: "Skapa en ny Windows virtuell dator genom att koppla en särskild hanterade diskar som hello OS-disken med i hello Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: c72c6f4fb650e2664e87cf38ec9be62f0b2209fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a><span data-ttu-id="bfefe-103">Skapa en virtuell Windows-dator från en särskild disk</span><span class="sxs-lookup"><span data-stu-id="bfefe-103">Create a Windows VM from a specialized disk</span></span>

<span data-ttu-id="bfefe-104">Skapa en ny virtuell dator genom att koppla en särskild hanterade diskar som hello OS-disken med hjälp av Powershell.</span><span class="sxs-lookup"><span data-stu-id="bfefe-104">Create a new VM by attaching a specialized managed disk as hello OS disk using Powershell.</span></span> <span data-ttu-id="bfefe-105">En specialiserad disk är en kopia av virtuell hårddisk (VHD) från en befintlig virtuell dator som underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bfefe-105">A specialized disk is a copy of virtual hard disk (VHD) from an existing VM that maintains hello user accounts, applications, and other state data from your original VM.</span></span> 

<span data-ttu-id="bfefe-106">När du använder en särskild VHD toocreate en ny virtuell dator hello hello ny virtuell dator behåller hello datornamnet för ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bfefe-106">When you use a specialized VHD toocreate a new VM, hello new VM retains hello computer name of hello original VM.</span></span> <span data-ttu-id="bfefe-107">Andra datorspecifik information sparas också och i vissa fall kan den här dubblettinformation kan orsaka problem.</span><span class="sxs-lookup"><span data-stu-id="bfefe-107">Other computer-specific information is also be kept and, in some cases, this duplicate information could cause issues.</span></span> <span data-ttu-id="bfefe-108">Ta reda på vilka typer av information om datorn dina program förlitar sig på när du kopierar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bfefe-108">Be aware of what types of computer-specific information your applications rely on when copying a VM.</span></span>

<span data-ttu-id="bfefe-109">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="bfefe-109">You have two options:</span></span>
* [<span data-ttu-id="bfefe-110">Överföra en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="bfefe-110">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="bfefe-111">Kopiera en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="bfefe-111">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

<span data-ttu-id="bfefe-112">Det här avsnittet visar hur toouse hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="bfefe-112">This topic shows you how toouse managed disks.</span></span> <span data-ttu-id="bfefe-113">Om du har en äldre distribution som kräver att du använder ett lagringskonto, se [skapa en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto](sa-create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="bfefe-113">If you have a legacy deployment that requires using a storage account, see [Create a VM from a specialized VHD in a storage account](sa-create-vm-specialized.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="bfefe-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="bfefe-114">Before you begin</span></span>
<span data-ttu-id="bfefe-115">Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="bfefe-115">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="bfefe-116">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bfefe-116">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="bfefe-117">Alternativ 1: Ladda upp en särskild virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="bfefe-117">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="bfefe-118">Du kan överföra hello VHD från en särskild virtuell dator som skapats med ett lokalt virtualisering verktyg, som Hyper-V eller en virtuell dator som har exporterats från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="bfefe-118">You can upload hello VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-hello-vm"></a><span data-ttu-id="bfefe-119">Förbereda hello VM</span><span class="sxs-lookup"><span data-stu-id="bfefe-119">Prepare hello VM</span></span>
<span data-ttu-id="bfefe-120">Om du avser toouse hello VHD som-är toocreate en ny virtuell dator, kontrollera hello följande steg har slutförts.</span><span class="sxs-lookup"><span data-stu-id="bfefe-120">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span> 
  
  * <span data-ttu-id="bfefe-121">[Förbereda en Windows-VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bfefe-121">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="bfefe-122">**Inte** generalisera hello VM med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="bfefe-122">**Do not** generalize hello VM using Sysprep.</span></span>
  * <span data-ttu-id="bfefe-123">Ta bort gäst virtualiseringsverktyg och agenter som installerats på hello VM (till exempel VMware-verktyg).</span><span class="sxs-lookup"><span data-stu-id="bfefe-123">Remove any guest virtualization tools and agents that are installed on hello VM (like VMware tools).</span></span>
  * <span data-ttu-id="bfefe-124">Se till att hello VM är konfigurerade toopull dess IP-adress och DNS-inställningarna via DHCP.</span><span class="sxs-lookup"><span data-stu-id="bfefe-124">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="bfefe-125">Detta säkerställer att hello-servern hämtar en IP-adress inom hello VNet när den startas.</span><span class="sxs-lookup"><span data-stu-id="bfefe-125">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span> 


### <a name="get-hello-storage-account"></a><span data-ttu-id="bfefe-126">Hämta hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="bfefe-126">Get hello storage account</span></span>
<span data-ttu-id="bfefe-127">Du behöver ett konto i Azure toostore hello överföra VHD.</span><span class="sxs-lookup"><span data-stu-id="bfefe-127">You need a storage account in Azure toostore hello uploaded VHD.</span></span> <span data-ttu-id="bfefe-128">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="bfefe-128">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="bfefe-129">tooshow hello tillgängliga storage-konton, skriver du:</span><span class="sxs-lookup"><span data-stu-id="bfefe-129">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="bfefe-130">Om du vill toouse ett befintligt lagringskonto fortsätta toohello [överför hello VHD](#upload-the-vhd-to-your-storage-account) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bfefe-130">If you want toouse an existing storage account, proceed toohello [Upload hello VHD](#upload-the-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="bfefe-131">Följ dessa steg om du behöver toocreate ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="bfefe-131">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="bfefe-132">Du måste hello namnet på hello resursgruppen där hello storage-konto ska skapas.</span><span class="sxs-lookup"><span data-stu-id="bfefe-132">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="bfefe-133">toofind ut hello resursgrupper i din prenumeration, typ:</span><span class="sxs-lookup"><span data-stu-id="bfefe-133">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="bfefe-134">en resursgrupp med namnet toocreate *myResourceGroup* i hello *västra USA* region, typ:</span><span class="sxs-lookup"><span data-stu-id="bfefe-134">toocreate a resource group named *myResourceGroup* in hello *West US* region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="bfefe-135">Skapa ett lagringskonto med namnet *mittlagringskonto* i den här resursgruppen genom att använda hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="bfefe-135">Create a storage account named *mystorageaccount* in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="bfefe-136">Överför hello VHD tooyour storage-konto</span><span class="sxs-lookup"><span data-stu-id="bfefe-136">Upload hello VHD tooyour storage account</span></span> 
<span data-ttu-id="bfefe-137">Använd hello [Lägg till AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello VHD tooa behållare på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bfefe-137">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello VHD tooa container in your storage account.</span></span> <span data-ttu-id="bfefe-138">Det här exemplet överföringar hello filen *myVHD.vhd* från `"C:\Users\Public\Documents\Virtual hard disks\"` tooa lagringskontonamnet *mittlagringskonto* i hello *myResourceGroup* resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="bfefe-138">This example uploads hello file *myVHD.vhd* from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named *mystorageaccount* in hello *myResourceGroup* resource group.</span></span> <span data-ttu-id="bfefe-139">hello lagras i hello behållare med namnet *minbehållare* och hello nytt filnamn blir *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="bfefe-139">hello file is stored in hello container named *mycontainer* and hello new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="bfefe-140">Om det lyckas, får du ett svar som ser ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="bfefe-140">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="bfefe-141">Det här kommandot kan ta en stund beroende på nätverksanslutningen och hello storleken på VHD-filen, toocomplete</span><span class="sxs-lookup"><span data-stu-id="bfefe-141">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete</span></span>

### <a name="create-a-managed-disk-from-hello-vhd"></a><span data-ttu-id="bfefe-142">Skapa en hanterad disk från hello VHD</span><span class="sxs-lookup"><span data-stu-id="bfefe-142">Create a managed disk from hello VHD</span></span>

<span data-ttu-id="bfefe-143">Skapa en hanterad disk från hello specialiserad VHD på din storage-konto med [ny AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="bfefe-143">Create a managed disk from hello specialized VHD in your storage account using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="bfefe-144">Det här exemplet används **myOSDisk1** för hello disknamnet placeringar hello disk i *StandardLRS* lagring och använder *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* som hello URI för hello källan VHD.</span><span class="sxs-lookup"><span data-stu-id="bfefe-144">This example uses **myOSDisk1** for hello disk name, puts hello disk in *StandardLRS* storage, and uses *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* as hello URI for hello source VHD.</span></span>

<span data-ttu-id="bfefe-145">Skapa en ny resursgrupp för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bfefe-145">Create a new resource group for hello new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="bfefe-146">Skapa hello nya OS-disken från hello överföra VHD.</span><span class="sxs-lookup"><span data-stu-id="bfefe-146">Create hello new OS disk from hello uploaded VHD.</span></span> 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a><span data-ttu-id="bfefe-147">Alternativ 2: Kopiera en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="bfefe-147">Option 2: Copy an existing Azure VM</span></span>

<span data-ttu-id="bfefe-148">Du kan skapa en kopia av en virtuell dator som använder hanterade diskar genom att ta en ögonblicksbild av hello VM och sedan med hjälp av den ögonblicksbild toocreate en ny hanterade disken och en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bfefe-148">You can create a copy of a VM that uses managed disks by taking a snapshot of hello VM, then using that snapshot toocreate a new managed disk and a new VM.</span></span>


### <a name="take-a-snapshot-of-hello-os-disk"></a><span data-ttu-id="bfefe-149">Ta en ögonblicksbild av hello OS-disk</span><span class="sxs-lookup"><span data-stu-id="bfefe-149">Take a snapshot of hello OS disk</span></span>

<span data-ttu-id="bfefe-150">Du kan ta en ögonblicksbild av och en hel virtuell dator (inklusive alla diskar) eller bara en enda disk.</span><span class="sxs-lookup"><span data-stu-id="bfefe-150">You can take a snapshot of and entire VM (including all disks) or of just a single disk.</span></span> <span data-ttu-id="bfefe-151">hello följande steg visar hur tootake en ögonblicksbild av bara hello OS-disk för virtuell dator med hjälp av hello [ny AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bfefe-151">hello following steps show you how tootake a snapshot of just hello OS disk of your VM using hello [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet.</span></span> 

<span data-ttu-id="bfefe-152">Ange vissa parametrar.</span><span class="sxs-lookup"><span data-stu-id="bfefe-152">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

<span data-ttu-id="bfefe-153">Hämta hello Virtuella datorobjektet.</span><span class="sxs-lookup"><span data-stu-id="bfefe-153">Get hello VM object.</span></span>

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
<span data-ttu-id="bfefe-154">Hämta hello OS-disknamnet.</span><span class="sxs-lookup"><span data-stu-id="bfefe-154">Get hello OS disk name.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

<span data-ttu-id="bfefe-155">Skapa hello ögonblicksbild konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="bfefe-155">Create hello snapshot configuration.</span></span> 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

<span data-ttu-id="bfefe-156">Ta hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="bfefe-156">Take hello snapshot.</span></span>

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


<span data-ttu-id="bfefe-157">Om du planerar toouse hello ögonblicksbild toocreate en virtuell dator som behöver toobe högpresterande använder hello parametern `-AccountType Premium_LRS` med hello ny AzureRmSnapshot kommando.</span><span class="sxs-lookup"><span data-stu-id="bfefe-157">If you plan toouse hello snapshot toocreate a VM that needs toobe high performing, use hello parameter `-AccountType Premium_LRS` with hello New-AzureRmSnapshot command.</span></span> <span data-ttu-id="bfefe-158">hello-parametern skapar hello ögonblicksbild så att den lagras som en Premium hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="bfefe-158">hello parameter creates hello snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="bfefe-159">Premium-hanterade diskar är dyrare än Standard.</span><span class="sxs-lookup"><span data-stu-id="bfefe-159">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="bfefe-160">Vara säker på att du verkligen behöver Premium innan du använder hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="bfefe-160">So be sure you really need Premium before using hello parameter.</span></span>

### <a name="create-a-new-disk-from-hello-snapshot"></a><span data-ttu-id="bfefe-161">Skapa en ny disk från hello ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="bfefe-161">Create a new disk from hello snapshot</span></span>

<span data-ttu-id="bfefe-162">Skapa en hanterad disk från hello med verktyget [ny AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="bfefe-162">Create a managed disk from hello snapshot using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="bfefe-163">Det här exemplet används *myOSDisk* för hello diskens namn.</span><span class="sxs-lookup"><span data-stu-id="bfefe-163">This example uses *myOSDisk* for hello disk name.</span></span>

<span data-ttu-id="bfefe-164">Skapa en ny resursgrupp för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bfefe-164">Create a new resource group for hello new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="bfefe-165">Ange hello OS-disknamnet.</span><span class="sxs-lookup"><span data-stu-id="bfefe-165">Set hello OS disk name.</span></span> 

```powershell
$osDiskName = 'myOsDisk'
```

<span data-ttu-id="bfefe-166">Skapa hello hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="bfefe-166">Create hello managed disk.</span></span>

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-hello-new-vm"></a><span data-ttu-id="bfefe-167">Skapa hello ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="bfefe-167">Create hello new VM</span></span> 

<span data-ttu-id="bfefe-168">Skapa nätverk och andra toobe för VM-resurser som används av hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bfefe-168">Create networking and other VM resources toobe used by hello new VM.</span></span>

### <a name="create-hello-subnet-and-vnet"></a><span data-ttu-id="bfefe-169">Skapa hello undernät och virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="bfefe-169">Create hello subNet and vNet</span></span>

<span data-ttu-id="bfefe-170">Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bfefe-170">Create hello vNet and subNet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="bfefe-171">Skapa hello undernät.</span><span class="sxs-lookup"><span data-stu-id="bfefe-171">Create hello subNet.</span></span> <span data-ttu-id="bfefe-172">Det här exemplet skapar ett undernät med namnet **mySubNet**, i resursgrupp hello **myDestinationResourceGroup**, och anger hello undernätsprefixet för IP-adress för**10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="bfefe-172">This example creates a subnet named **mySubNet**, in hello resource group **myDestinationResourceGroup**, and sets hello subnet address prefix too**10.0.0.0/24**.</span></span>
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="bfefe-173">Skapa hello vNet.</span><span class="sxs-lookup"><span data-stu-id="bfefe-173">Create hello vNet.</span></span> <span data-ttu-id="bfefe-174">Det här exemplet anger hello virtuella nätverket namnet toobe **myVnetName**, hello plats för**västra USA**, och hello-adressprefix för hello virtuellt nätverk för**10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="bfefe-174">This example sets hello virtual network name toobe **myVnetName**, hello location too**West US**, and hello address prefix for hello virtual network too**10.0.0.0/16**.</span></span> 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="bfefe-175">Skapa hello nätverkssäkerhetsgruppen och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="bfefe-175">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="bfefe-176">toobe kan toolog i tooyour VM som använder RDP måste toohave en säkerhetsregel som tillåter RDP-åtkomst på port 3389.</span><span class="sxs-lookup"><span data-stu-id="bfefe-176">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="bfefe-177">Eftersom hello VHD för hello ny virtuell dator har skapats från en befintlig särskild virtuell dator kan du använda ett konto från hello virtuella källdatorn för RDP.</span><span class="sxs-lookup"><span data-stu-id="bfefe-177">Because hello VHD for hello new VM was created from an existing specialized VM, you can use an account from hello source virtual machine for RDP.</span></span>

<span data-ttu-id="bfefe-178">Det här exemplet anger hello NSG namn för**myNsg** och hello RDP Regelnamn för**myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="bfefe-178">This example sets hello NSG name too**myNsg** and hello RDP rule name too**myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="bfefe-179">Mer information om slutpunkter och NSG-regler finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bfefe-179">For more information about endpoints and NSG rules, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="bfefe-180">Skapa en offentlig IP-adress och ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="bfefe-180">Create a public IP address and NIC</span></span>
<span data-ttu-id="bfefe-181">tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bfefe-181">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

<span data-ttu-id="bfefe-182">Skapa hello offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="bfefe-182">Create hello public IP.</span></span> <span data-ttu-id="bfefe-183">I det här exemplet hello offentliga IP-adressnamn har angetts för**myIP**.</span><span class="sxs-lookup"><span data-stu-id="bfefe-183">In this example, hello public IP address name is set too**myIP**.</span></span>
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

<span data-ttu-id="bfefe-184">Skapa hello NIC.</span><span class="sxs-lookup"><span data-stu-id="bfefe-184">Create hello NIC.</span></span> <span data-ttu-id="bfefe-185">I det här exemplet hello NIC namn har angetts för**myNicName**.</span><span class="sxs-lookup"><span data-stu-id="bfefe-185">In this example, hello NIC name is set too**myNicName**.</span></span>
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-hello-vm-name-and-size"></a><span data-ttu-id="bfefe-186">Ange namn på virtuell hello och storlek</span><span class="sxs-lookup"><span data-stu-id="bfefe-186">Set hello VM name and size</span></span>

<span data-ttu-id="bfefe-187">Det här exemplet anger hello namn på virtuell dator för*myVM* och hello VM-storlek för*Standard_A2*.</span><span class="sxs-lookup"><span data-stu-id="bfefe-187">This example sets hello VM name too*myVM* and hello VM size too*Standard_A2*.</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a><span data-ttu-id="bfefe-188">Lägg till hello NIC</span><span class="sxs-lookup"><span data-stu-id="bfefe-188">Add hello NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-hello-os-disk"></a><span data-ttu-id="bfefe-189">Lägg till hello OS-disk</span><span class="sxs-lookup"><span data-stu-id="bfefe-189">Add hello OS disk</span></span> 

<span data-ttu-id="bfefe-190">Lägg till hello OS disk toohello konfiguration av [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="bfefe-190">Add hello OS disk toohello configuration using [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span> <span data-ttu-id="bfefe-191">Det här exemplet anger hello hello diskens storlek för*128 GB* och bifogar hello hanterade diskar som en *Windows* OS-disken.</span><span class="sxs-lookup"><span data-stu-id="bfefe-191">This example sets hello size of hello disk too*128 GB* and attaches hello managed disk as a *Windows* OS disk.</span></span>
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-hello-vm"></a><span data-ttu-id="bfefe-192">Slutföra hello VM</span><span class="sxs-lookup"><span data-stu-id="bfefe-192">Complete hello VM</span></span> 

<span data-ttu-id="bfefe-193">Skapa hello VM med hjälp av [ny AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello konfigurationer som vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="bfefe-193">Create hello VM using [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello configurations that we just created.</span></span>

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

<span data-ttu-id="bfefe-194">Om det här kommandot lyckades visas utdata som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="bfefe-194">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="bfefe-195">Kontrollera att hello VM skapades</span><span class="sxs-lookup"><span data-stu-id="bfefe-195">Verify that hello VM was created</span></span>
<span data-ttu-id="bfefe-196">Du bör se hello nyskapad VM antingen i hello [Azure-portalen](https://portal.azure.com)under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell hello kommandon:</span><span class="sxs-lookup"><span data-stu-id="bfefe-196">You should see hello newly created VM either in hello [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="bfefe-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bfefe-197">Next steps</span></span>
<span data-ttu-id="bfefe-198">toosign i tooyour nya virtuella datorn, bläddra toohello VM i hello [portal](https://portal.azure.com), klickar du på **Anslut**, och öppna hello Remote Desktop RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="bfefe-198">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="bfefe-199">Använd hello autentiseringsuppgifter för den ursprungliga virtuella toosign i tooyour ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bfefe-199">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="bfefe-200">Mer information finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="bfefe-200">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md).</span></span>

