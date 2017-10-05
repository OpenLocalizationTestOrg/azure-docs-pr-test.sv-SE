---
title: "Överför generalize VHD att skapa flera virtuella datorer i Azure | Microsoft Docs"
description: "Överför en generaliserad virtuell Hårddisk till en Azure storage-konto för att skapa en virtuell Windows-dator för användning med Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: e6fc49855b449a7723a7f8a0c1c41516b3a44ee5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="upload-a-generalized-vhd-to-azure-to-create-a-new-vm"></a><span data-ttu-id="135a4-103">Ladda upp en generaliserad virtuell Hårddisk till Azure för att skapa en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="135a4-103">Upload a generalized VHD to Azure to create a new VM</span></span>

<span data-ttu-id="135a4-104">Det här avsnittet beskriver ladda upp en generaliserad ohanterade disk till ett lagringskonto och sedan skapa en ny virtuell dator med den överförda disken.</span><span class="sxs-lookup"><span data-stu-id="135a4-104">This topic covers uploading a generalized unmanaged disk to a storage account and then creating a new VM using the uploaded disk.</span></span> <span data-ttu-id="135a4-105">En generaliserad virtuell hårddiskavbildning har haft all personlig information bort med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="135a4-105">A generalized VHD image has had all of your personal account information removed using Sysprep.</span></span> 

<span data-ttu-id="135a4-106">Om du vill skapa en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto finns [skapa en virtuell dator från en särskild virtuell Hårddisk](sa-create-vm-specialized.md).</span><span class="sxs-lookup"><span data-stu-id="135a4-106">If you want to create a VM from a specialized VHD in a storage account, see [Create a VM from a specialized VHD](sa-create-vm-specialized.md).</span></span>

<span data-ttu-id="135a4-107">Det här avsnittet beskriver med lagringskonton, men vi rekommenderar kunder flytta till med hjälp av hanterade diskar i stället.</span><span class="sxs-lookup"><span data-stu-id="135a4-107">This topic covers using storage accounts, but we recommend customers move to using Managed Disks instead.</span></span> <span data-ttu-id="135a4-108">En fullständig genomgång av hur du förbereder, ladda upp och skapa en ny virtuell dator med hjälp av hanterade diskar finns [skapa en ny virtuell dator från en generaliserad virtuell Hårddisk som har överförts till Azure med hjälp av hanterade diskar](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="135a4-108">For a complete walk-through of how to prepare, upload and create a new VM using managed disks, see [Create a new VM from a generalized VHD uploaded to Azure using Managed Disks](upload-generalized-managed.md).</span></span>



## <a name="prepare-the-vm"></a><span data-ttu-id="135a4-109">Förbereda den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="135a4-109">Prepare the VM</span></span>

<span data-ttu-id="135a4-110">En generaliserad virtuell Hårddisk har haft all personlig information bort med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="135a4-110">A generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="135a4-111">Om du tänker använda den virtuella Hårddisken som en bild för att skapa nya virtuella datorer från bör du:</span><span class="sxs-lookup"><span data-stu-id="135a4-111">If you intend to use the VHD as an image to create new VMs from, you should:</span></span>
  
  * <span data-ttu-id="135a4-112">[Förbereda en Windows-VHD att överföra till Azure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="135a4-112">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md).</span></span> 
  * <span data-ttu-id="135a4-113">Generalisera den virtuella datorn med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="135a4-113">Generalize the virtual machine using Sysprep</span></span>

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a><span data-ttu-id="135a4-114">Generalisera Windows-dator med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="135a4-114">Generalize a Windows virtual machine using Sysprep</span></span>
<span data-ttu-id="135a4-115">Det här avsnittet visar hur du generalisera din Windows-dator för användning som en bild.</span><span class="sxs-lookup"><span data-stu-id="135a4-115">This section shows you how to generalize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="135a4-116">Sysprep tar bort alla dina personlig information, bland annat och förbereder datorn som ska användas som en bild.</span><span class="sxs-lookup"><span data-stu-id="135a4-116">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="135a4-117">Mer information om Sysprep finns [så att använda Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="135a4-117">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="135a4-118">Se till att serverroller som körs på datorn som stöds av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="135a4-118">Make sure the server roles running on the machine are supported by Sysprep.</span></span> <span data-ttu-id="135a4-119">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="135a4-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="135a4-120">Om du kör Sysprep innan du laddar upp den virtuella Hårddisken till Azure för första gången, kontrollera att du har [förberett din virtuella dator](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du kör Sysprep.</span><span class="sxs-lookup"><span data-stu-id="135a4-120">If you are running Sysprep before uploading your VHD to Azure for the first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="135a4-121">Logga in på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="135a4-121">Sign in to the Windows virtual machine.</span></span>
2. <span data-ttu-id="135a4-122">Öppna Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="135a4-122">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="135a4-123">Ändra katalogen till **%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="135a4-123">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="135a4-124">I den **systemförberedelseverktyget** dialogrutan **ange System Out of Box Experience (OOBE)**, och se till att den **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="135a4-124">In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="135a4-125">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="135a4-125">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="135a4-126">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="135a4-126">Click **OK**.</span></span>
   
    ![Starta Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="135a4-128">När Sysprep har slutförts stängs den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="135a4-128">When Sysprep completes, it shuts down the virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="135a4-129">Starta inte om den virtuella datorn förrän du är klar överföra den virtuella Hårddisken till Azure eller skapa en avbildning från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="135a4-129">Do not restart the VM until you are done uploading the VHD to Azure or creating an image from the VM.</span></span> <span data-ttu-id="135a4-130">Kör Sysprep för att generalisera den igen om den virtuella datorn av misstag hämtar startas om.</span><span class="sxs-lookup"><span data-stu-id="135a4-130">If the VM accidentally gets restarted, run Sysprep to generalize it again.</span></span>
> 
> 


## <a name="upload-the-vhd"></a><span data-ttu-id="135a4-131">Överför den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="135a4-131">Upload the VHD</span></span>

<span data-ttu-id="135a4-132">Överför den virtuella Hårddisken till ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="135a4-132">Upload the VHD to an Azure storage account.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="135a4-133">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="135a4-133">Log in to Azure</span></span>
<span data-ttu-id="135a4-134">Om du inte redan har PowerShell version 1.4 eller senare installerat, läsa [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="135a4-134">If you don't already have PowerShell version 1.4 or above installed, read [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="135a4-135">Öppna Azure PowerShell och logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="135a4-135">Open Azure PowerShell and sign in to your Azure account.</span></span> <span data-ttu-id="135a4-136">Ett popup-fönster öppnas där du kan ange dina autentiseringsuppgifter för Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="135a4-136">A pop-up window opens for you to enter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="135a4-137">Hämta ID: N för prenumeration för din tillgängliga prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="135a4-137">Get the subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="135a4-138">Ange rätt prenumerationen med prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="135a4-138">Set the correct subscription using the subscription ID.</span></span> <span data-ttu-id="135a4-139">Ersätt `<subscriptionID>` med ID korrekt prenumeration.</span><span class="sxs-lookup"><span data-stu-id="135a4-139">Replace `<subscriptionID>` with the ID of the correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-the-storage-account"></a><span data-ttu-id="135a4-140">Hämta storage-konto</span><span class="sxs-lookup"><span data-stu-id="135a4-140">Get the storage account</span></span>
<span data-ttu-id="135a4-141">Du behöver ett lagringskonto i Azure för att lagra överförda VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="135a4-141">You need a storage account in Azure to store the uploaded VM image.</span></span> <span data-ttu-id="135a4-142">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="135a4-142">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="135a4-143">Om du vill visa tillgängliga storage-konton, skriver du:</span><span class="sxs-lookup"><span data-stu-id="135a4-143">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="135a4-144">Om du vill använda ett befintligt lagringskonto fortsätter du till den [överför den Virtuella datoravbildningen](#upload-the-vm-vhd-to-your-storage-account) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="135a4-144">If you want to use an existing storage account, proceed to the [Upload the VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="135a4-145">Följ dessa steg om du behöver skapa ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="135a4-145">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="135a4-146">Du måste namnet på resursgruppen där lagringskontot ska skapas.</span><span class="sxs-lookup"><span data-stu-id="135a4-146">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="135a4-147">Om du vill ta reda på alla resursgrupper i din prenumeration, skriver du:</span><span class="sxs-lookup"><span data-stu-id="135a4-147">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="135a4-148">Så här skapar du en resursgrupp med namnet **myResourceGroup** i den **västra USA** region, typ:</span><span class="sxs-lookup"><span data-stu-id="135a4-148">To create a resource group named **myResourceGroup** in the **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="135a4-149">Skapa ett lagringskonto med namnet **mittlagringskonto** i den här resursgruppen med hjälp av den [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="135a4-149">Create a storage account named **mystorageaccount** in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-the-upload"></a><span data-ttu-id="135a4-150">Starta överföringen</span><span class="sxs-lookup"><span data-stu-id="135a4-150">Start the upload</span></span> 

<span data-ttu-id="135a4-151">Använd den [Lägg till AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) för att ladda upp avbildningen till en behållare i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="135a4-151">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the image to a container in your storage account.</span></span> <span data-ttu-id="135a4-152">Det här exemplet överför filen **myVHD.vhd** från `"C:\Users\Public\Documents\Virtual hard disks\"` till ett lagringskonto med namnet **mittlagringskonto** i den **myResourceGroup** resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="135a4-152">This example uploads the file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named **mystorageaccount** in the **myResourceGroup** resource group.</span></span> <span data-ttu-id="135a4-153">Filen placeras i behållare med namnet **minbehållare** och det nya namnet kommer att **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="135a4-153">The file will be placed into the container named **mycontainer** and the new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="135a4-154">Om det lyckas, kan du få ett svar som liknar denna:</span><span class="sxs-lookup"><span data-stu-id="135a4-154">If successful, you get a response that looks similar to this:</span></span>

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

<span data-ttu-id="135a4-155">Det här kommandot kan ta en stund att slutföra beroende på nätverksanslutningen och storleken på VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="135a4-155">Depending on your network connection and the size of your VHD file, this command may take a while to complete.</span></span>


## <a name="create-a-new-vm"></a><span data-ttu-id="135a4-156">Skapa en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="135a4-156">Create a new VM</span></span> 

<span data-ttu-id="135a4-157">Du kan nu använda den överförda virtuella Hårddisken för att skapa en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="135a4-157">You can now use the uploaded VHD to create a new VM.</span></span> 

### <a name="set-the-uri-of-the-vhd"></a><span data-ttu-id="135a4-158">Ange URI för den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="135a4-158">Set the URI of the VHD</span></span>

<span data-ttu-id="135a4-159">URI för den virtuella Hårddisken ska använda är i formatet: https://**mittlagringskonto**.blob.core.windows.net/**minbehållare**/**MyVhdName**VHD.</span><span class="sxs-lookup"><span data-stu-id="135a4-159">The URI for the VHD to use is in the format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="135a4-160">I det här exemplet den virtuella Hårddisken med namnet **myVHD** finns i lagringskontot **mittlagringskonto** i behållaren **minbehållare**.</span><span class="sxs-lookup"><span data-stu-id="135a4-160">In this example the VHD named **myVHD** is in the storage account **mystorageaccount** in the container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="135a4-161">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="135a4-161">Create a virtual network</span></span>
<span data-ttu-id="135a4-162">Skapa vNet och undernät för den [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="135a4-162">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="135a4-163">Skapa undernätet.</span><span class="sxs-lookup"><span data-stu-id="135a4-163">Create the subnet.</span></span> <span data-ttu-id="135a4-164">Följande exempel skapar ett undernät med namnet **mySubnet** i resursgruppen **myResourceGroup** med adressprefixet för **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="135a4-164">The following sample creates a subnet named **mySubnet** in the resource group **myResourceGroup** with the address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="135a4-165">Skapa det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="135a4-165">Create the virtual network.</span></span> <span data-ttu-id="135a4-166">Följande exempel skapar ett virtuellt nätverk med namnet **myVnet** i den **västra USA** plats med adressprefixet för **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="135a4-166">The following sample creates a virtual network named **myVnet** in the **West US** location with the address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="135a4-167">Skapa en offentlig IP-adress och gränssnitt</span><span class="sxs-lookup"><span data-stu-id="135a4-167">Create a public IP address and network interface</span></span>
<span data-ttu-id="135a4-168">För att upprätta kommunikation med den virtuella datorn i det virtuella nätverket behöver du en [offentlig IP-adress](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="135a4-168">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="135a4-169">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="135a4-169">Create a public IP address.</span></span> <span data-ttu-id="135a4-170">Det här exemplet skapas en offentlig IP-adress med namnet **myPip**.</span><span class="sxs-lookup"><span data-stu-id="135a4-170">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="135a4-171">Skapa nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="135a4-171">Create the NIC.</span></span> <span data-ttu-id="135a4-172">Det här exemplet skapar ett nätverkskort med namnet **myNic**.</span><span class="sxs-lookup"><span data-stu-id="135a4-172">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="135a4-173">Skapa säkerhetsgrupp för nätverk och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="135a4-173">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="135a4-174">Du måste ha en säkerhetsregel som tillåter RDP-åtkomst på port 3389 för att kunna logga in på den virtuella datorn med RDP.</span><span class="sxs-lookup"><span data-stu-id="135a4-174">To be able to log in to your VM using RDP, you need to have a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="135a4-175">Det här exemplet skapas en NSG som heter **myNsg** som innehåller en regel med namnet **myRdpRule** som tillåter RDP-trafik via port 3389.</span><span class="sxs-lookup"><span data-stu-id="135a4-175">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="135a4-176">Mer information om NSG: er finns [öppna portar till en virtuell dator i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="135a4-176">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="135a4-177">Skapa en variabel för det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="135a4-177">Create a variable for the virtual network</span></span>
<span data-ttu-id="135a4-178">Skapa en variabel för det virtuella nätverket som slutförda.</span><span class="sxs-lookup"><span data-stu-id="135a4-178">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a><span data-ttu-id="135a4-179">Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="135a4-179">Create the VM</span></span>
<span data-ttu-id="135a4-180">Följande PowerShell-skript visar hur du ställer in konfigurationer av virtuella datorer och använder överförda VM-avbildning som källa för den nya installationen.</span><span class="sxs-lookup"><span data-stu-id="135a4-180">The following PowerShell script shows how to set up the virtual machine configurations and use the uploaded VM image as the source for the new installation.</span></span>



```powershell
# Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential

    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"

    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"

    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="135a4-181">Kontrollera att den virtuella datorn har skapats</span><span class="sxs-lookup"><span data-stu-id="135a4-181">Verify that the VM was created</span></span>
<span data-ttu-id="135a4-182">När du är klar bör du se den nya virtuella datorn i den [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="135a4-182">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="135a4-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="135a4-183">Next steps</span></span>
<span data-ttu-id="135a4-184">För att hantera din nya virtuella datorn med Azure PowerShell, se [hantera virtuella datorer med Azure Resource Manager och PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="135a4-184">To manage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


