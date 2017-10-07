---
title: aaaUpload en generalize VHD toocreate flera virtuella datorer i Azure | Microsoft Docs
description: "Överför en generaliserad virtuell Hårddisk tooan Azure storage-konto toocreate en virtuell Windows-dator toouse med hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: cynthn
ms.openlocfilehash: aa1af2a0acf81685e62853de71afa51e819cb696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-tooazure-toocreate-a-new-vm"></a><span data-ttu-id="32caf-103">Överför en generaliserad virtuell Hårddisk tooAzure toocreate en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="32caf-103">Upload a generalized VHD tooAzure toocreate a new VM</span></span>

<span data-ttu-id="32caf-104">Det här avsnittet beskriver ladda upp en generaliserad ohanterade disk tooa storage-konto och sedan skapa en ny virtuell dator med hello upp disken.</span><span class="sxs-lookup"><span data-stu-id="32caf-104">This topic covers uploading a generalized unmanaged disk tooa storage account and then creating a new VM using hello uploaded disk.</span></span> <span data-ttu-id="32caf-105">En generaliserad virtuell hårddiskavbildning har haft all personlig information bort med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="32caf-105">A generalized VHD image has had all of your personal account information removed using Sysprep.</span></span> 

<span data-ttu-id="32caf-106">Om du vill att toocreate en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto finns [skapa en virtuell dator från en särskild virtuell Hårddisk](sa-create-vm-specialized.md).</span><span class="sxs-lookup"><span data-stu-id="32caf-106">If you want toocreate a VM from a specialized VHD in a storage account, see [Create a VM from a specialized VHD](sa-create-vm-specialized.md).</span></span>

<span data-ttu-id="32caf-107">Det här avsnittet beskriver med lagringskonton, men vi rekommenderar att kunder flytta toousing hanteras diskarna i stället.</span><span class="sxs-lookup"><span data-stu-id="32caf-107">This topic covers using storage accounts, but we recommend customers move toousing Managed Disks instead.</span></span> <span data-ttu-id="32caf-108">För en fullständig genomgång av hur tooprepare, ladda upp och skapa en ny virtuell dator med hjälp av hanterade diskar, se [skapa en ny virtuell dator från en generaliserad virtuell Hårddisk har överförts tooAzure med hjälp av hanterade diskar](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="32caf-108">For a complete walk-through of how tooprepare, upload and create a new VM using managed disks, see [Create a new VM from a generalized VHD uploaded tooAzure using Managed Disks](upload-generalized-managed.md).</span></span>



## <a name="prepare-hello-vm"></a><span data-ttu-id="32caf-109">Förbereda hello VM</span><span class="sxs-lookup"><span data-stu-id="32caf-109">Prepare hello VM</span></span>

<span data-ttu-id="32caf-110">En generaliserad virtuell Hårddisk har haft all personlig information bort med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="32caf-110">A generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="32caf-111">Om du avser toouse hello VHD som en bild toocreate nya virtuella datorer från bör du:</span><span class="sxs-lookup"><span data-stu-id="32caf-111">If you intend toouse hello VHD as an image toocreate new VMs from, you should:</span></span>
  
  * <span data-ttu-id="32caf-112">[Förbereda en Windows-VHD tooupload tooAzure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="32caf-112">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md).</span></span> 
  * <span data-ttu-id="32caf-113">Generalisera hello virtuell dator med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="32caf-113">Generalize hello virtual machine using Sysprep</span></span>

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a><span data-ttu-id="32caf-114">Generalisera Windows-dator med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="32caf-114">Generalize a Windows virtual machine using Sysprep</span></span>
<span data-ttu-id="32caf-115">Det här avsnittet beskrivs hur du toogeneralize din Windows-dator för användning som en bild.</span><span class="sxs-lookup"><span data-stu-id="32caf-115">This section shows you how toogeneralize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="32caf-116">Sysprep tar bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild.</span><span class="sxs-lookup"><span data-stu-id="32caf-116">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="32caf-117">Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="32caf-117">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="32caf-118">Kontrollera att hello serverroller som körs på datorn hello stöds av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="32caf-118">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="32caf-119">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="32caf-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32caf-120">Om du kör Sysprep innan du laddar upp din VHD tooAzure för hello första gången, kontrollera att du har [förberett din virtuella dator](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du kör Sysprep.</span><span class="sxs-lookup"><span data-stu-id="32caf-120">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="32caf-121">Logga in toohello Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="32caf-121">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="32caf-122">Öppna hello Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="32caf-122">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="32caf-123">Ändra hello katalogen för**%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="32caf-123">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="32caf-124">I hello **systemförberedelseverktyget** dialogrutan **ange System Out of Box Experience (OOBE)**, och se till att hello **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="32caf-124">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="32caf-125">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="32caf-125">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="32caf-126">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="32caf-126">Click **OK**.</span></span>
   
    ![Starta Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="32caf-128">När Sysprep är klar stänger hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="32caf-128">When Sysprep completes, it shuts down hello virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="32caf-129">Starta inte om hello VM tills du är klar uppladdning hello VHD tooAzure eller skapa en avbildning från hello VM.</span><span class="sxs-lookup"><span data-stu-id="32caf-129">Do not restart hello VM until you are done uploading hello VHD tooAzure or creating an image from hello VM.</span></span> <span data-ttu-id="32caf-130">Kör Sysprep toogeneralize om hello VM av misstag hämtar startas om den igen.</span><span class="sxs-lookup"><span data-stu-id="32caf-130">If hello VM accidentally gets restarted, run Sysprep toogeneralize it again.</span></span>
> 
> 


## <a name="upload-hello-vhd"></a><span data-ttu-id="32caf-131">Överför hello VHD</span><span class="sxs-lookup"><span data-stu-id="32caf-131">Upload hello VHD</span></span>

<span data-ttu-id="32caf-132">Överför hello VHD tooan Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="32caf-132">Upload hello VHD tooan Azure storage account.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="32caf-133">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="32caf-133">Log in tooAzure</span></span>
<span data-ttu-id="32caf-134">Om du inte redan har PowerShell version 1.4 eller senare installerat, läsa [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="32caf-134">If you don't already have PowerShell version 1.4 or above installed, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="32caf-135">Öppna Azure PowerShell och logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="32caf-135">Open Azure PowerShell and sign in tooyour Azure account.</span></span> <span data-ttu-id="32caf-136">Ett popup-fönster som öppnas för du tooenter dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="32caf-136">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="32caf-137">Hämta hello prenumerations-ID: N för din tillgängliga prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="32caf-137">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="32caf-138">Ange hello korrekt prenumeration med hjälp av hello prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="32caf-138">Set hello correct subscription using hello subscription ID.</span></span> <span data-ttu-id="32caf-139">Ersätt `<subscriptionID>` med hello-ID för hello rätt prenumeration.</span><span class="sxs-lookup"><span data-stu-id="32caf-139">Replace `<subscriptionID>` with hello ID of hello correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-hello-storage-account"></a><span data-ttu-id="32caf-140">Hämta hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="32caf-140">Get hello storage account</span></span>
<span data-ttu-id="32caf-141">Du behöver ett lagringskonto i Azure toostore hello upp VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="32caf-141">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="32caf-142">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="32caf-142">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="32caf-143">tooshow hello tillgängliga storage-konton, skriver du:</span><span class="sxs-lookup"><span data-stu-id="32caf-143">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="32caf-144">Om du vill toouse ett befintligt lagringskonto fortsätta toohello [överför hello VM-avbildning](#upload-the-vm-vhd-to-your-storage-account) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="32caf-144">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="32caf-145">Följ dessa steg om du behöver toocreate ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="32caf-145">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="32caf-146">Du måste hello namnet på hello resursgruppen där hello storage-konto ska skapas.</span><span class="sxs-lookup"><span data-stu-id="32caf-146">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="32caf-147">toofind ut hello resursgrupper i din prenumeration, typ:</span><span class="sxs-lookup"><span data-stu-id="32caf-147">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="32caf-148">en resursgrupp med namnet toocreate **myResourceGroup** i hello **västra USA** region, typ:</span><span class="sxs-lookup"><span data-stu-id="32caf-148">toocreate a resource group named **myResourceGroup** in hello **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="32caf-149">Skapa ett lagringskonto med namnet **mittlagringskonto** i den här resursgruppen genom att använda hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="32caf-149">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-hello-upload"></a><span data-ttu-id="32caf-150">Starta överföring av hello</span><span class="sxs-lookup"><span data-stu-id="32caf-150">Start hello upload</span></span> 

<span data-ttu-id="32caf-151">Använd hello [Lägg till AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello avbildningen tooa behållare på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="32caf-151">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello image tooa container in your storage account.</span></span> <span data-ttu-id="32caf-152">Det här exemplet överföringar hello filen **myVHD.vhd** från `"C:\Users\Public\Documents\Virtual hard disks\"` tooa lagringskontonamnet **mittlagringskonto** i hello **myResourceGroup** resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="32caf-152">This example uploads hello file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named **mystorageaccount** in hello **myResourceGroup** resource group.</span></span> <span data-ttu-id="32caf-153">hello-fil placeras i hello behållare med namnet **minbehållare** och hello nytt filnamn blir **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="32caf-153">hello file will be placed into hello container named **mycontainer** and hello new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="32caf-154">Om det lyckas, får du ett svar som ser ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="32caf-154">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="32caf-155">Det här kommandot kan ta en stund beroende på nätverksanslutningen och hello storleken på VHD-filen, toocomplete.</span><span class="sxs-lookup"><span data-stu-id="32caf-155">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete.</span></span>


## <a name="create-a-new-vm"></a><span data-ttu-id="32caf-156">Skapa en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="32caf-156">Create a new VM</span></span> 

<span data-ttu-id="32caf-157">Du kan nu använda hello överföra VHD toocreate en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="32caf-157">You can now use hello uploaded VHD toocreate a new VM.</span></span> 

### <a name="set-hello-uri-of-hello-vhd"></a><span data-ttu-id="32caf-158">Ange hello hello VHD-URI</span><span class="sxs-lookup"><span data-stu-id="32caf-158">Set hello URI of hello VHD</span></span>

<span data-ttu-id="32caf-159">hello URI för hello VHD toouse har hello format: https://**mittlagringskonto**.blob.core.windows.net/**minbehållare**/**MyVhdName**VHD.</span><span class="sxs-lookup"><span data-stu-id="32caf-159">hello URI for hello VHD toouse is in hello format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="32caf-160">I det här exemplet hello VHD med namnet **myVHD** är i hello lagringskonto **mittlagringskonto** i hello behållaren **minbehållare**.</span><span class="sxs-lookup"><span data-stu-id="32caf-160">In this example hello VHD named **myVHD** is in hello storage account **mystorageaccount** in hello container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="32caf-161">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="32caf-161">Create a virtual network</span></span>
<span data-ttu-id="32caf-162">Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32caf-162">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="32caf-163">Skapa hello undernät.</span><span class="sxs-lookup"><span data-stu-id="32caf-163">Create hello subnet.</span></span> <span data-ttu-id="32caf-164">hello följande exempel skapar ett undernät med namnet **mySubnet** i hello resursgruppen **myResourceGroup** med hello adressprefixet **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="32caf-164">hello following sample creates a subnet named **mySubnet** in hello resource group **myResourceGroup** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="32caf-165">Skapa hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="32caf-165">Create hello virtual network.</span></span> <span data-ttu-id="32caf-166">hello följande exempel skapar ett virtuellt nätverk med namnet **myVnet** i hello **västra USA** plats med hello adressprefixet **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="32caf-166">hello following sample creates a virtual network named **myVnet** in hello **West US** location with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="32caf-167">Skapa en offentlig IP-adress och gränssnitt</span><span class="sxs-lookup"><span data-stu-id="32caf-167">Create a public IP address and network interface</span></span>
<span data-ttu-id="32caf-168">tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="32caf-168">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="32caf-169">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="32caf-169">Create a public IP address.</span></span> <span data-ttu-id="32caf-170">Det här exemplet skapas en offentlig IP-adress med namnet **myPip**.</span><span class="sxs-lookup"><span data-stu-id="32caf-170">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="32caf-171">Skapa hello NIC.</span><span class="sxs-lookup"><span data-stu-id="32caf-171">Create hello NIC.</span></span> <span data-ttu-id="32caf-172">Det här exemplet skapar ett nätverkskort med namnet **myNic**.</span><span class="sxs-lookup"><span data-stu-id="32caf-172">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="32caf-173">Skapa hello nätverkssäkerhetsgruppen och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="32caf-173">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="32caf-174">toobe kan toolog i tooyour VM som använder RDP måste toohave en säkerhetsregel som tillåter RDP-åtkomst på port 3389.</span><span class="sxs-lookup"><span data-stu-id="32caf-174">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="32caf-175">Det här exemplet skapas en NSG som heter **myNsg** som innehåller en regel med namnet **myRdpRule** som tillåter RDP-trafik via port 3389.</span><span class="sxs-lookup"><span data-stu-id="32caf-175">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="32caf-176">Mer information om NSG: er finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="32caf-176">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="32caf-177">Skapa en variabel för hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="32caf-177">Create a variable for hello virtual network</span></span>
<span data-ttu-id="32caf-178">Skapa en variabel för hello slutförts virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="32caf-178">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a><span data-ttu-id="32caf-179">Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="32caf-179">Create hello VM</span></span>
<span data-ttu-id="32caf-180">hello visar följande PowerShell-skript hur tooset hello konfigurationer av virtuella datorer och Använd hello överförs VM-avbildning som hello källa för hello ny installation.</span><span class="sxs-lookup"><span data-stu-id="32caf-180">hello following PowerShell script shows how tooset up hello virtual machine configurations and use hello uploaded VM image as hello source for hello new installation.</span></span>



```powershell
# Enter a new user name and password toouse as hello local administrator account 
    # for remotely accessing hello VM.
    $cred = Get-Credential

    # Name of hello storage account where hello VHD is located. This example sets hello 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of hello virtual machine. This example sets hello VM name as "myVM".
    $vmName = "myVM"

    # Size of hello virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See hello VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for hello VM. This examples sets hello computer name as "myComputer".
    $computerName = "myComputer"

    # Name of hello disk that holds hello OS. This example sets hello 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets hello SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get hello storage account where hello uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set hello VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set hello Windows operating system configuration and add hello NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create hello OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure hello OS disk toobe created from hello existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create hello new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="32caf-181">Kontrollera att hello VM skapades</span><span class="sxs-lookup"><span data-stu-id="32caf-181">Verify that hello VM was created</span></span>
<span data-ttu-id="32caf-182">När du är klar bör du se hello nyskapad VM i hello [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande hello PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="32caf-182">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="32caf-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32caf-183">Next steps</span></span>
<span data-ttu-id="32caf-184">din nya virtuella datorn med Azure PowerShell finns i toomanage [hantera virtuella datorer med Azure Resource Manager och PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="32caf-184">toomanage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


