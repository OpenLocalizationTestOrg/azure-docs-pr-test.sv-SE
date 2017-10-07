---
title: "aaaCreate virtuell dator från en särskild disk i Azure | Microsoft Docs"
description: "Skapa en ny virtuell dator genom att koppla en särskild ohanterade disk i hello Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: c88f213b6629a6c1d6ff5845e76c2f7719672714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a><span data-ttu-id="1b8c5-103">Skapa en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="1b8c5-103">Create a VM from a specialized VHD in a storage account</span></span>

<span data-ttu-id="1b8c5-104">Skapa en ny virtuell dator genom att koppla en särskild ohanterade disk som hello OS-disken med hjälp av Powershell.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-104">Create a new VM by attaching a specialized unmanaged disk as hello OS disk using Powershell.</span></span> <span data-ttu-id="1b8c5-105">En specialiserad disk är en kopia av VHD från en befintlig virtuell dator som underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-105">A specialized disk is a copy of VHD from an existing VM that maintains hello user accounts, applications and other state data from your original VM.</span></span> 

<span data-ttu-id="1b8c5-106">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-106">You have two options:</span></span>
* [<span data-ttu-id="1b8c5-107">Överföra en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="1b8c5-107">Upload a VHD</span></span>](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="1b8c5-108">Kopiera hello VHD från en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="1b8c5-108">Copy hello VHD of an existing Azure VM</span></span>](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a><span data-ttu-id="1b8c5-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1b8c5-109">Before you begin</span></span>
<span data-ttu-id="1b8c5-110">Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-110">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="1b8c5-111">Kör följande kommando tooinstall hello den.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-111">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute 
```
<span data-ttu-id="1b8c5-112">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="1b8c5-113">Alternativ 1: Ladda upp en särskild virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="1b8c5-113">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="1b8c5-114">Du kan överföra hello VHD från en särskild virtuell dator som skapats med ett lokalt virtualisering verktyg, som Hyper-V eller en virtuell dator som har exporterats från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-114">You can upload hello VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-hello-vm"></a><span data-ttu-id="1b8c5-115">Förbereda hello VM</span><span class="sxs-lookup"><span data-stu-id="1b8c5-115">Prepare hello VM</span></span>
<span data-ttu-id="1b8c5-116">Du kan ladda upp en särskild virtuell Hårddisk som har skapats med en lokal dator eller en virtuell Hårddisk som exporterats från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-116">You can upload a specialized VHD that was created using an on-premises VM or a VHD exported from another cloud.</span></span> <span data-ttu-id="1b8c5-117">En specialiserad VHD underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-117">A specialized VHD maintains hello user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="1b8c5-118">Om du avser toouse hello VHD som-är toocreate en ny virtuell dator, kontrollera hello följande steg har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-118">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span> 
  
  * <span data-ttu-id="1b8c5-119">[Förbereda en Windows-VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-119">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="1b8c5-120">**Inte** generalisera hello VM med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-120">**Do not** generalize hello VM using Sysprep.</span></span>
  * <span data-ttu-id="1b8c5-121">Ta bort gäst virtualiseringsverktyg och agenter som installerats på hello VM (d.v.s. VMware-verktyg).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-121">Remove any guest virtualization tools and agents that are installed on hello VM (i.e. VMware tools).</span></span>
  * <span data-ttu-id="1b8c5-122">Se till att hello VM är konfigurerade toopull dess IP-adress och DNS-inställningarna via DHCP.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-122">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="1b8c5-123">Detta säkerställer att hello-servern hämtar en IP-adress inom hello VNet när den startas.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-123">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span> 


### <a name="get-hello-storage-account"></a><span data-ttu-id="1b8c5-124">Hämta hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="1b8c5-124">Get hello storage account</span></span>
<span data-ttu-id="1b8c5-125">Du behöver ett lagringskonto i Azure toostore hello upp VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-125">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="1b8c5-126">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-126">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="1b8c5-127">tooshow hello tillgängliga storage-konton, skriver du:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-127">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="1b8c5-128">Om du vill toouse ett befintligt lagringskonto fortsätta toohello [överför hello VM-avbildning](#upload-the-vm-vhd-to-your-storage-account) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-128">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="1b8c5-129">Följ dessa steg om du behöver toocreate ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-129">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="1b8c5-130">Du måste hello namnet på hello resursgruppen där hello storage-konto ska skapas.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-130">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="1b8c5-131">toofind ut hello resursgrupper i din prenumeration, typ:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-131">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="1b8c5-132">en resursgrupp med namnet toocreate **myResourceGroup** i hello **västra USA** region, typ:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-132">toocreate a resource group named **myResourceGroup** in hello **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="1b8c5-133">Skapa ett lagringskonto med namnet **mittlagringskonto** i den här resursgruppen genom att använda hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-133">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
### <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="1b8c5-134">Överför hello VHD tooyour storage-konto</span><span class="sxs-lookup"><span data-stu-id="1b8c5-134">Upload hello VHD tooyour storage account</span></span>
<span data-ttu-id="1b8c5-135">Använd hello [Lägg till AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello avbildningen tooa behållare på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-135">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello image tooa container in your storage account.</span></span> <span data-ttu-id="1b8c5-136">Det här exemplet överföringar hello filen **myVHD.vhd** från `"C:\Users\Public\Documents\Virtual hard disks\"` tooa lagringskontonamnet **mittlagringskonto** i hello **myResourceGroup** resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-136">This example uploads hello file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named **mystorageaccount** in hello **myResourceGroup** resource group.</span></span> <span data-ttu-id="1b8c5-137">hello-fil placeras i hello behållare med namnet **minbehållare** och hello nytt filnamn blir **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-137">hello file will be placed into hello container named **mycontainer** and hello new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="1b8c5-138">Om det lyckas, får du ett svar som ser ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-138">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="1b8c5-139">Det här kommandot kan ta en stund beroende på nätverksanslutningen och hello storleken på VHD-filen, toocomplete.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-139">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete.</span></span>


## <a name="option-2-copy-hello-vhd-from-an-existing-azure-vm"></a><span data-ttu-id="1b8c5-140">Alternativ 2: Kopiera hello VHD från en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="1b8c5-140">Option 2: Copy hello VHD from an existing Azure VM</span></span>

<span data-ttu-id="1b8c5-141">Du kan kopiera en VHD tooanother storage-konto toouse när du skapar en ny, dubbla virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-141">You can copy a VHD tooanother storage account toouse when creating a new, duplicate VM.</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="1b8c5-142">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1b8c5-142">Before you begin</span></span>
<span data-ttu-id="1b8c5-143">Se till att du:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-143">Make sure that you:</span></span>

* <span data-ttu-id="1b8c5-144">Innehåller information om hello **käll- och storage-konton**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-144">Have information about hello **source and destination storage accounts**.</span></span> <span data-ttu-id="1b8c5-145">För hello källa VM behöver du toohave hello storage-konto och en behållare namn.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-145">For hello source VM, you need toohave hello storage account and container names.</span></span> <span data-ttu-id="1b8c5-146">Vanligtvis hello behållarens namn kommer att **virtuella hårddiskar**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-146">Usually, hello container name will be **vhds**.</span></span> <span data-ttu-id="1b8c5-147">Du måste också toohave en mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-147">You also need toohave a destination storage account.</span></span> <span data-ttu-id="1b8c5-148">Om du inte redan har ett kan du skapa en med hjälp av antingen hello-portal (**fler tjänster** > lagringskonton > Lägg till) eller med hjälp av hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-148">If you don't already have one, you can create one using either hello portal (**More Services** > Storage accounts > Add) or using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet.</span></span> 
* <span data-ttu-id="1b8c5-149">Har hämtat och installerat hello [AzCopy verktyget](../../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-149">Have downloaded and installed hello [AzCopy tool](../../storage/common/storage-use-azcopy.md).</span></span> 

### <a name="deallocate-hello-vm"></a><span data-ttu-id="1b8c5-150">Frigöra hello VM</span><span class="sxs-lookup"><span data-stu-id="1b8c5-150">Deallocate hello VM</span></span>
<span data-ttu-id="1b8c5-151">Frigöra hello VM, vilket frigör hello VHD toobe kopieras.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-151">Deallocate hello VM, which frees up hello VHD toobe copied.</span></span> 

* <span data-ttu-id="1b8c5-152">**Portalen**: Klicka på **virtuella datorer** > **myVM** > Stoppa</span><span class="sxs-lookup"><span data-stu-id="1b8c5-152">**Portal**: Click **Virtual machines** > **myVM** > Stop</span></span>
* <span data-ttu-id="1b8c5-153">**PowerShell**: Använd [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (frigöra) hello virtuella datorn med namnet **myVM** i resursgruppen **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-153">**Powershell**: Use [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (deallocate) hello VM named **myVM** in resource group **myResourceGroup**.</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="1b8c5-154">Hej **Status** för hello VM i hello Azure portal ändras från **stoppad** för**Stoppad (frigjord)**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-154">hello **Status** for hello VM in hello Azure portal changes from **Stopped** too**Stopped (deallocated)**.</span></span>

### <a name="get-hello-storage-account-urls"></a><span data-ttu-id="1b8c5-155">Hämta URL: er för hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="1b8c5-155">Get hello storage account URLs</span></span>
<span data-ttu-id="1b8c5-156">Du måste hello URL: er för hello käll- och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-156">You need hello URLs of hello source and destination storage accounts.</span></span> <span data-ttu-id="1b8c5-157">hello URL: er se ut: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-157">hello URLs look like: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span></span> <span data-ttu-id="1b8c5-158">Om du redan vet hello namn på kontot och en behållare kan du bara ersätta hello information mellan hello hakparenteser toocreate URL: en.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-158">If you already know hello storage account and container name, you can just replace hello information between hello brackets toocreate your URL.</span></span> 

<span data-ttu-id="1b8c5-159">Du kan använda hello Azure-portalen eller Azure Powershell tooget hello URL:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-159">You can use hello Azure portal or Azure Powershell tooget hello URL:</span></span>

* <span data-ttu-id="1b8c5-160">**Portalen**: Klicka på hello  **>**  för **fler tjänster** > **lagringskonton**  >   *lagringskontot* > **Blobbar** och käll-VHD-filen är förmodligen i hello **virtuella hårddiskar** behållare.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-160">**Portal**: Click hello **>** for **More services** > **Storage accounts** > *storage account* > **Blobs** and your source VHD file is probably in hello **vhds** container.</span></span> <span data-ttu-id="1b8c5-161">Klicka på **egenskaper** för hello-behållaren och kopiera hello text med etiketten **URL**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-161">Click **Properties** for hello container, and copy hello text labeled **URL**.</span></span> <span data-ttu-id="1b8c5-162">Du behöver hello URL: er för hello käll- och behållare.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-162">You'll need hello URLs of both hello source and destination containers.</span></span> 
* <span data-ttu-id="1b8c5-163">**PowerShell**: Använd [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello information för den virtuella datorn med namnet **myVM** i hello resursgruppen **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-163">**Powershell**: Use [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello information for VM named **myVM** in hello resource group **myResourceGroup**.</span></span> <span data-ttu-id="1b8c5-164">Hello resultat, titta i hello **lagringsprofil** avsnittet hello **Vhd-Uri**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-164">In hello results, look in hello **Storage profile** section for hello **Vhd Uri**.</span></span> <span data-ttu-id="1b8c5-165">hello första delen av hello Uri är hello URL toohello behållare och hello sista delen är hello hello VM OS VHD namn.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-165">hello first part of hello Uri is hello URL toohello container and hello last part is hello OS VHD name for hello VM.</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-hello-storage-access-keys"></a><span data-ttu-id="1b8c5-166">Hämta hello åtkomstnycklar för lagring</span><span class="sxs-lookup"><span data-stu-id="1b8c5-166">Get hello storage access keys</span></span>
<span data-ttu-id="1b8c5-167">Hitta hello snabbtangenter för hello käll- och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-167">Find hello access keys for hello source and destination storage accounts.</span></span> <span data-ttu-id="1b8c5-168">Läs mer om åtkomstnycklarna [om Azure storage-konton](../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-168">For more information about access keys, see [About Azure storage accounts](../../storage/common/storage-create-storage-account.md).</span></span>

* <span data-ttu-id="1b8c5-169">**Portalen**: Klicka på **fler tjänster** > **lagringskonton** > *lagringskonto*  >  **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-169">**Portal**: Click **More services** > **Storage accounts** > *storage account* > **Access keys**.</span></span> <span data-ttu-id="1b8c5-170">Kopiera hello nyckel som är märkta som **key1**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-170">Copy hello key labeled as **key1**.</span></span>
* <span data-ttu-id="1b8c5-171">**PowerShell**: Använd [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello lagringsnyckel för hello lagringskontot **mittlagringskonto** i hello resursgruppen  **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-171">**Powershell**: Use [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello storage key for hello storage account **mystorageaccount** in hello resource group **myResourceGroup**.</span></span> <span data-ttu-id="1b8c5-172">Kopiera hello nyckel med namnet **key1**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-172">Copy hello key labeled **key1**.</span></span>

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-hello-vhd"></a><span data-ttu-id="1b8c5-173">Kopiera hello VHD</span><span class="sxs-lookup"><span data-stu-id="1b8c5-173">Copy hello VHD</span></span>
<span data-ttu-id="1b8c5-174">Du kan kopiera filer mellan storage-konton med hjälp av AzCopy.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-174">You can copy files between storage accounts using AzCopy.</span></span> <span data-ttu-id="1b8c5-175">För hello målbehållare om hello angivna behållaren inte finns, kommer den att skapas för dig.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-175">For hello destination container, if hello specified container doesn't exist, it will be created for you.</span></span> 

<span data-ttu-id="1b8c5-176">toouse AzCopy, öppna Kommandotolken på den lokala datorn och navigera toohello mappen där AzCopy är installerat.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-176">toouse AzCopy, open a command prompt on your local machine and navigate toohello folder where AzCopy is installed.</span></span> <span data-ttu-id="1b8c5-177">Det ser ut ungefär för*C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-177">It will be similar too*C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy*.</span></span> 

<span data-ttu-id="1b8c5-178">toocopy alla hello filer i en behållare, använda hello **/S** växla.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-178">toocopy all of hello files within a container, you use hello **/S** switch.</span></span> <span data-ttu-id="1b8c5-179">Detta kan vara används toocopy hello OS VHD och alla data hello diskar om de finns i hello samma behållare.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-179">This can be used toocopy hello OS VHD and all of hello data disks if they are in hello same container.</span></span> <span data-ttu-id="1b8c5-180">Det här exemplet illustrerar hur toocopy alla hello filer i hello behållaren **mysourcecontainer** i lagringskonto **mysourcestorageaccount** toohello behållare **mydestinationcontainer**  i hello **mydestinationstorageaccount** storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-180">This example shows how toocopy all of hello files in hello container **mysourcecontainer** in storage account **mysourcestorageaccount** toohello container **mydestinationcontainer** in hello **mydestinationstorageaccount** storage account.</span></span> <span data-ttu-id="1b8c5-181">Ersätt hello namnen på hello storage-konton och behållare med din egen.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-181">Replace hello names of hello storage accounts and containers with your own.</span></span> <span data-ttu-id="1b8c5-182">Ersätt `<sourceStorageAccountKey1>` och `<destinationStorageAccountKey1>` med egna nycklar.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-182">Replace `<sourceStorageAccountKey1>` and `<destinationStorageAccountKey1>` with your own keys.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

<span data-ttu-id="1b8c5-183">Om du bara vill toocopy en viss VHD i en behållare med flera filer kan du också ange hello filnamn hello /Pattern växeln.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-183">If you only want toocopy a specific VHD in a container with multiple files, you can also specify hello file name using hello /Pattern switch.</span></span> <span data-ttu-id="1b8c5-184">I det här exemplet endast hello-fil som heter **myFileName.vhd** ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-184">In this example, only hello file named **myFileName.vhd** will be copied.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


<span data-ttu-id="1b8c5-185">När den är klar, visas ett meddelande som ser ut ungefär så:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-185">When it is finished, you will get a message that looks something like:</span></span>

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

### <a name="troubleshooting"></a><span data-ttu-id="1b8c5-186">Felsökning</span><span class="sxs-lookup"><span data-stu-id="1b8c5-186">Troubleshooting</span></span>
* <span data-ttu-id="1b8c5-187">När du använder AZCopy, om du ser hello felet ”servern inte kunde tooauthenticate hello begäran”, kontrollera hello värdet för hello Authorization-huvud är formaterad korrekt inklusive hello signatur.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-187">When you use AZCopy, if you see hello error "Server failed tooauthenticate hello request", make sure hello value of hello Authorization header is formed correctly including hello signature.</span></span> <span data-ttu-id="1b8c5-188">Om du använder nyckel 2 eller hello sekundära lagringsplatsen nyckeln kan du prova med hello primära eller 1 lagringsutrymme nyckel.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-188">If you are using Key 2 or hello secondary storage key, try using hello primary or 1st storage key.</span></span>

## <a name="create-hello-new-vm"></a><span data-ttu-id="1b8c5-189">Skapa hello ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1b8c5-189">Create hello new VM</span></span> 

<span data-ttu-id="1b8c5-190">Du behöver toocreate nätverk och andra toobe för VM-resurser som används av hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-190">You need toocreate networking and other VM resources toobe used by hello new VM.</span></span>

### <a name="create-hello-subnet-and-vnet"></a><span data-ttu-id="1b8c5-191">Skapa hello undernät och virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="1b8c5-191">Create hello subNet and vNet</span></span>

<span data-ttu-id="1b8c5-192">Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-192">Create hello vNet and subNet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="1b8c5-193">Skapa hello undernät.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-193">Create hello subNet.</span></span> <span data-ttu-id="1b8c5-194">Det här exemplet skapar ett undernät med namnet **mySubNet**, i resursgrupp hello **myResourceGroup**, och anger hello undernätsprefixet för IP-adress för**10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-194">This example creates a subnet named **mySubNet**, in hello resource group **myResourceGroup**, and sets hello subnet address prefix too**10.0.0.0/24**.</span></span>
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="1b8c5-195">Skapa hello vNet.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-195">Create hello vNet.</span></span> <span data-ttu-id="1b8c5-196">Det här exemplet anger hello virtuella nätverket namnet toobe **myVnetName**, hello plats för**västra USA**, och hello-adressprefix för hello virtuellt nätverk för**10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-196">This example sets hello virtual network name toobe **myVnetName**, hello location too**West US**, and hello address prefix for hello virtual network too**10.0.0.0/16**.</span></span> 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="1b8c5-197">Skapa en offentlig IP-adress och ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="1b8c5-197">Create a public IP address and NIC</span></span>
<span data-ttu-id="1b8c5-198">tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-198">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="1b8c5-199">Skapa hello offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-199">Create hello public IP.</span></span> <span data-ttu-id="1b8c5-200">I det här exemplet hello offentliga IP-adressnamn har angetts för**myIP**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-200">In this example, hello public IP address name is set too**myIP**.</span></span>
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="1b8c5-201">Skapa hello NIC.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-201">Create hello NIC.</span></span> <span data-ttu-id="1b8c5-202">I det här exemplet hello NIC namn har angetts för**myNicName**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-202">In this example, hello NIC name is set too**myNicName**.</span></span>
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="1b8c5-203">Skapa hello nätverkssäkerhetsgruppen och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="1b8c5-203">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="1b8c5-204">toobe kan toolog i tooyour VM som använder RDP måste toohave en säkerhetsregel som tillåter RDP-åtkomst på port 3389.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-204">toobe able toolog in tooyour VM using RDP, you need toohave an security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="1b8c5-205">Eftersom hello VHD för hello ny virtuell dator har skapats från ett befintligt specialiserat kan VM efter hello VM skapas du använda ett befintligt konto från hello virtuella källdatorn som hade behörighet toolog med RDP.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-205">Because hello VHD for hello new VM was created from an existing specialized VM, after hello VM is created you can use an existing account from hello source virtual machine that had permission toolog on using RDP.</span></span>
<span data-ttu-id="1b8c5-206">Det här exemplet anger hello NSG namn för**myNsg** och hello RDP Regelnamn för**myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-206">This example sets hello NSG name too**myNsg** and hello RDP rule name too**myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="1b8c5-207">Mer information om slutpunkter och NSG-regler finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-207">For more information about endpoints and NSG rules, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="set-hello-vm-name-and-size"></a><span data-ttu-id="1b8c5-208">Ange namn på virtuell hello och storlek</span><span class="sxs-lookup"><span data-stu-id="1b8c5-208">Set hello VM name and size</span></span>

<span data-ttu-id="1b8c5-209">Det här exemplet anger hello namn på virtuell dator för ”myVM” och hello VM storleken för ”Standard_A2”.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-209">This example sets hello VM name too"myVM" and hello VM size too"Standard_A2".</span></span>
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a><span data-ttu-id="1b8c5-210">Lägg till hello NIC</span><span class="sxs-lookup"><span data-stu-id="1b8c5-210">Add hello NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-hello-os-disk"></a><span data-ttu-id="1b8c5-211">Konfigurera hello OS-disk</span><span class="sxs-lookup"><span data-stu-id="1b8c5-211">Configure hello OS disk</span></span>

1. <span data-ttu-id="1b8c5-212">Ange hello URI för hello VHD som du laddat upp eller kopieras.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-212">Set hello URI for hello VHD that you uploaded or copied.</span></span> <span data-ttu-id="1b8c5-213">I det här exemplet hello VHD-fil med namnet **myOsDisk.vhd** sparas i ett lagringskonto med namnet **Mittlagringskonto** i en behållare med namnet **Minbehållare**.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-213">In this example, hello VHD file named **myOsDisk.vhd** is kept in a storage account named **myStorageAccount** in a container named **myContainer**.</span></span>

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. <span data-ttu-id="1b8c5-214">Lägga till hello OS-disk.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-214">Add hello OS disk.</span></span> <span data-ttu-id="1b8c5-215">I det här exemplet är hello termen ”osDisk” appened toohello VM namn toocreate hello OS-disknamnet när hello OS-disken skapas.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-215">In this example, when hello OS disk is created, hello term "osDisk" is appened toohello VM name toocreate hello OS disk name.</span></span> <span data-ttu-id="1b8c5-216">Det här exemplet anger också att den här Windows-baserade virtuella Hårddisken ska vara anslutna toohello VM som hello OS-disk.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-216">This example also specifies that this Windows-based VHD should be attached toohello VM as hello OS disk.</span></span>
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

<span data-ttu-id="1b8c5-217">Valfritt: Om du har datadiskar som behöver toobe kopplade toohello VM, Lägg till hello datadiskar med hjälp av hello URL: er för data virtuella hårddiskar och hello lämpliga logiska enhetsnummer (Lun).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-217">Optional: If you have data disks that need toobe attached toohello VM, add hello data disks by using hello URLs of data VHDs and hello appropriate Logical Unit Number (Lun).</span></span>

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

<span data-ttu-id="1b8c5-218">När du använder ett lagringskonto, hello data och operativsystemet disk webbadresser ut ungefär så här: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-218">When using a storage account, hello data and operating system disk URLs look something like this: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span></span> <span data-ttu-id="1b8c5-219">Du hittar det på hello portal genom att bläddra toohello mål lagringsbehållaren, klicka på hello operativsystem eller data VHD som har kopierats och sedan kopiera hello innehållet i hello-URL.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-219">You can find this on hello portal by browsing toohello target storage container, clicking hello operating system or data VHD that was copied, and then copying hello contents of hello URL.</span></span>


### <a name="complete-hello-vm"></a><span data-ttu-id="1b8c5-220">Slutföra hello VM</span><span class="sxs-lookup"><span data-stu-id="1b8c5-220">Complete hello VM</span></span> 

<span data-ttu-id="1b8c5-221">Skapa hello VM som använder hello-konfigurationer som vi just har skapat.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-221">Create hello VM using hello configurations that we just created.</span></span>

```powershell
#Create hello new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

<span data-ttu-id="1b8c5-222">Om det här kommandot lyckades visas utdata som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-222">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="1b8c5-223">Kontrollera att hello VM skapades</span><span class="sxs-lookup"><span data-stu-id="1b8c5-223">Verify that hello VM was created</span></span>
<span data-ttu-id="1b8c5-224">Du bör se hello nyskapad VM antingen i hello [Azure-portalen](https://portal.azure.com)under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell hello kommandon:</span><span class="sxs-lookup"><span data-stu-id="1b8c5-224">You should see hello newly created VM either in hello [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="1b8c5-225">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b8c5-225">Next steps</span></span>
<span data-ttu-id="1b8c5-226">toosign i tooyour nya virtuella datorn, bläddra toohello VM i hello [portal](https://portal.azure.com), klickar du på **Anslut**, och öppna hello Remote Desktop RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-226">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="1b8c5-227">Använd hello autentiseringsuppgifter för den ursprungliga virtuella toosign i tooyour ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1b8c5-227">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="1b8c5-228">Mer information finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="1b8c5-228">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md).</span></span>

