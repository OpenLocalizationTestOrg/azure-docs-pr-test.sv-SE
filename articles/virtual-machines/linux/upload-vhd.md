---
title: Ladda upp eller kopiera en anpassad Linux VM med Azure CLI 2.0 | Microsoft Docs
description: Ladda upp eller kopiera en anpassad virtuell dator med Resource Manager-distributionsmodellen och Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/06/2017
ms.author: cynthn
ms.openlocfilehash: 7c297725c26ea6c44403a10ecdcc3542f89f10b4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a><span data-ttu-id="b7c47-103">Skapa en Linux VM från anpassade disken med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b7c47-103">Create a Linux VM from custom disk with the Azure CLI 2.0</span></span>

<!-- rename to create-vm-specialized -->

<span data-ttu-id="b7c47-104">Den här artikeln visar hur du överför en anpassad virtuell hårddisk (VHD) eller kopiera en en befintlig virtuell Hårddisk i Azure och skapa nya virtuella Linux-datorer (VM) från den anpassa disken.</span><span class="sxs-lookup"><span data-stu-id="b7c47-104">This article shows you how to upload a customized virtual hard disk (VHD) or copy a an existing VHD in Azure and create new Linux virtual machines (VMs) from the custom disk.</span></span> <span data-ttu-id="b7c47-105">Du kan installera och konfigurera en Linux-distro enligt dina behov och sedan använda den virtuella Hårddisken för att snabbt skapa en ny Azure virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b7c47-105">You can install and configure a Linux distro to your requirements and then use that VHD to quickly create a new Azure virtual machine.</span></span>

<span data-ttu-id="b7c47-106">Om du vill skapa flera virtuella datorer från din anpassade disken bör du skapa en avbildning från virtuell dator eller virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b7c47-106">If you want to create multiple VMs from your customized disk, you should create an image from your VM or VHD.</span></span> <span data-ttu-id="b7c47-107">Mer information finns i [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av CLI](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="b7c47-107">For more information, see [Create a custom image of an Azure VM using the CLI](tutorial-custom-images.md).</span></span>

<span data-ttu-id="b7c47-108">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="b7c47-108">You have two options:</span></span>
* [<span data-ttu-id="b7c47-109">Överföra en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="b7c47-109">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="b7c47-110">Kopiera en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="b7c47-110">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a><span data-ttu-id="b7c47-111">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="b7c47-111">Quick commands</span></span>

<span data-ttu-id="b7c47-112">När du skapar en ny virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create) från en anpassad eller särskilda disk du **bifoga** disken (--bifoga-os-disk) istället för att ange en anpassad eller marketplace-avbildning (--bilden).</span><span class="sxs-lookup"><span data-stu-id="b7c47-112">When creating a new VM using [az vm create](/cli/azure/vm#create) from a customized or specialized disk you **attach** the disk (--attach-os-disk) instead of specifying a custom or marketplace image (--image).</span></span> <span data-ttu-id="b7c47-113">I följande exempel skapas en virtuell dator med namnet *myVM* med den hantera disken med namnet *myManagedDisk* skapas från den anpassade virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="b7c47-113">The following example creates a VM named *myVM* using the managed disk named *myManagedDisk* created from your customized VHD:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a><span data-ttu-id="b7c47-114">Krav</span><span class="sxs-lookup"><span data-stu-id="b7c47-114">Requirements</span></span>
<span data-ttu-id="b7c47-115">Du behöver följande för att slutföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7c47-115">To complete the following steps, you need:</span></span>

* <span data-ttu-id="b7c47-116">En virtuell Linux-dator som har förberetts för användning i Azure.</span><span class="sxs-lookup"><span data-stu-id="b7c47-116">A Linux virtual machine that has been prepared for use in Azure.</span></span> <span data-ttu-id="b7c47-117">Den [förbereda den virtuella datorn](#prepare-the-vm) i den här artikeln beskriver hur du hittar distro specifik information om hur du installerar Azure Linux-agenten (waagent) som krävs för den virtuella datorn ska fungera korrekt i Azure och du ska kunna ansluta till den via SSH.</span><span class="sxs-lookup"><span data-stu-id="b7c47-117">The [Prepare the VM](#prepare-the-vm) section of this article covers how to find distro specific information on installing the Azure Linux Agent (waagent) which is required for the VM to work properly in Azure and for you to be able to connect to it using SSH.</span></span>
* <span data-ttu-id="b7c47-118">VHD-filen från en befintlig [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) till en virtuell disk i VHD-format.</span><span class="sxs-lookup"><span data-stu-id="b7c47-118">The VHD file from an existing [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) to a virtual disk in the VHD format.</span></span> <span data-ttu-id="b7c47-119">Det finns flera verktyg för att skapa en virtuell dator och virtuell Hårddisk:</span><span class="sxs-lookup"><span data-stu-id="b7c47-119">Multiple tools exist to create a VM and VHD:</span></span>
  * <span data-ttu-id="b7c47-120">Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), och ser till att använda VHD som bildformat.</span><span class="sxs-lookup"><span data-stu-id="b7c47-120">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care to use VHD as your image format.</span></span> <span data-ttu-id="b7c47-121">Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med **qemu img konvertera**.</span><span class="sxs-lookup"><span data-stu-id="b7c47-121">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using **qemu-img convert**.</span></span>
  * <span data-ttu-id="b7c47-122">Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="b7c47-122">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="b7c47-123">Nyare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="b7c47-123">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="b7c47-124">När du skapar en virtuell dator kan du ange VHD som formatet.</span><span class="sxs-lookup"><span data-stu-id="b7c47-124">When you create a VM, specify VHD as the format.</span></span> <span data-ttu-id="b7c47-125">Om det behövs, du kan konvertera VHDX-diskar på VHD med [qemu img konvertera](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b7c47-125">If needed, you can convert VHDX disks to VHD using [qemu-img convert](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or the [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="b7c47-126">Dessutom stöder inte Azure överför dynamiska virtuella hårddiskar, så du måste konvertera dessa diskar till statiska virtuella hårddiskar innan du laddar upp.</span><span class="sxs-lookup"><span data-stu-id="b7c47-126">Further, Azure does not support uploading dynamic VHDs, so you need to convert such disks to static VHDs before uploading.</span></span> <span data-ttu-id="b7c47-127">Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) att konvertera dynamiska diskar under processen med att ladda upp till Azure.</span><span class="sxs-lookup"><span data-stu-id="b7c47-127">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) to convert dynamic disks during the process of uploading to Azure.</span></span>
> 
> 


* <span data-ttu-id="b7c47-128">Se till att du har senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="b7c47-128">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="b7c47-129">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="b7c47-129">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="b7c47-130">Exempel parameternamn ingår *myResourceGroup*, *mittlagringskonto*, och *mydisks*.</span><span class="sxs-lookup"><span data-stu-id="b7c47-130">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *mydisks*.</span></span>

<span data-ttu-id="b7c47-131"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="b7c47-131"><a id="prepimage"> </a></span></span>

## <a name="prepare-the-vm"></a><span data-ttu-id="b7c47-132">Förbereda den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b7c47-132">Prepare the VM</span></span>

<span data-ttu-id="b7c47-133">Azure stöder olika Linux-distributioner (se [godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="b7c47-133">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="b7c47-134">I följande artiklar när du går igenom hur du förbereder de olika Linux-distributioner som stöds i Azure:</span><span class="sxs-lookup"><span data-stu-id="b7c47-134">The following articles guide you through how to prepare the various Linux distributions that are supported on Azure:</span></span>

* [<span data-ttu-id="b7c47-135">CentOS-baserade distributioner</span><span class="sxs-lookup"><span data-stu-id="b7c47-135">CentOS-based Distributions</span></span>](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="b7c47-136">Debian Linux</span><span class="sxs-lookup"><span data-stu-id="b7c47-136">Debian Linux</span></span>](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="b7c47-137">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="b7c47-137">Oracle Linux</span></span>](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="b7c47-138">Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="b7c47-138">Red Hat Enterprise Linux</span></span>](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="b7c47-139">SLES & openSUSE</span><span class="sxs-lookup"><span data-stu-id="b7c47-139">SLES & openSUSE</span></span>](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="b7c47-140">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="b7c47-140">Ubuntu</span></span>](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="b7c47-141">Andra - icke-godkända distributioner</span><span class="sxs-lookup"><span data-stu-id="b7c47-141">Other - Non-Endorsed Distributions</span></span>](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

<span data-ttu-id="b7c47-142">Se även den [Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer allmänna tips om hur du förbereder Linux avbildningar för Azure.</span><span class="sxs-lookup"><span data-stu-id="b7c47-142">Also see the [Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="b7c47-143">Den [Azure-plattformen SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gäller för virtuella datorer som kör Linux endast när en av de påtecknade distributioner används med konfigurationsinformation som anges under stöds versioner i [Linux på Azure-Endorsed distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7c47-143">The [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies to VMs running Linux only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="option-1-upload-a-vhd"></a><span data-ttu-id="b7c47-144">Alternativ 1: Ladda upp en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="b7c47-144">Option 1: Upload a VHD</span></span>

<span data-ttu-id="b7c47-145">Du kan överföra en anpassad virtuell Hårddisk som du har körs på en lokal dator eller som du exporterade från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="b7c47-145">You can upload a customized VHD that you have running on a local machine or that you exported from another cloud.</span></span> <span data-ttu-id="b7c47-146">Om du vill använda den virtuella Hårddisken för att skapa en ny Azure VM, måste du överföra den virtuella Hårddisken till ett lagringskonto och skapa en hanterade diskar från den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="b7c47-146">To use the VHD to create a new Azure VM, you need to upload the VHD to a storage account and create a managed disk from the VHD.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="b7c47-147">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b7c47-147">Create a resource group</span></span>

<span data-ttu-id="b7c47-148">Innan du laddar upp din anpassade disk och skapar virtuella datorer, måste du först skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b7c47-148">Before uploading your custom disk and creating VMs, you first need to create a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="b7c47-149">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* plats: [översikt över Azure hanterade diskar](../windows/managed-disks-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b7c47-149">The following example creates a resource group named *myResourceGroup* in the *eastus* location: [Azure Managed Disks overview](../windows/managed-disks-overview.md)</span></span>
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a><span data-ttu-id="b7c47-150">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="b7c47-150">Create a storage account</span></span>

<span data-ttu-id="b7c47-151">Skapa ett lagringskonto för anpassade disk- och virtuella datorer med [az storage-konto skapar](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="b7c47-151">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> 

<span data-ttu-id="b7c47-152">I följande exempel skapas ett lagringskonto med namnet *mittlagringskonto* i resursgruppen som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="b7c47-152">The following example creates a storage account named *mystorageaccount* in the resource group previously created:</span></span>

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a><span data-ttu-id="b7c47-153">Lista nycklar för lagringskonto</span><span class="sxs-lookup"><span data-stu-id="b7c47-153">List storage account keys</span></span>
<span data-ttu-id="b7c47-154">Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure.</span><span class="sxs-lookup"><span data-stu-id="b7c47-154">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="b7c47-155">Dessa snabbtangenter används vid autentisering till storage-konto som utför skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="b7c47-155">These access keys are used when authenticating to the storage account, like carrying out write operations.</span></span> <span data-ttu-id="b7c47-156">Läs mer om [hantera åtkomst till lagring här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="b7c47-156">Read more about [managing access to storage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="b7c47-157">Du kan visa snabbtangenterna med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="b7c47-157">You view the access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="b7c47-158">Visa åtkomstnycklar för lagringskontot som du skapade:</span><span class="sxs-lookup"><span data-stu-id="b7c47-158">View the access keys for the storage account you created:</span></span>

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

<span data-ttu-id="b7c47-159">Utdatan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="b7c47-159">The output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="b7c47-160">Anteckna **key1** eftersom du ska använda för att interagera med ditt lagringskonto i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="b7c47-160">Make a note of **key1** as you will use it to interact with your storage account in the next steps.</span></span>

### <a name="create-a-storage-container"></a><span data-ttu-id="b7c47-161">Skapa en lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="b7c47-161">Create a storage container</span></span>
<span data-ttu-id="b7c47-162">På samma sätt som du skapar olika kataloger för att organisera logiskt det lokala filsystemet, kan du skapa behållare i ett lagringskonto för att organisera dina diskar.</span><span class="sxs-lookup"><span data-stu-id="b7c47-162">In the same way that you create different directories to logically organize your local file system, you create containers within a storage account to organize your disks.</span></span> <span data-ttu-id="b7c47-163">Ett lagringskonto kan innehålla valfritt antal behållare.</span><span class="sxs-lookup"><span data-stu-id="b7c47-163">A storage account can contain any number of containers.</span></span> <span data-ttu-id="b7c47-164">Skapa en behållare med [az lagringsbehållaren skapa](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="b7c47-164">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="b7c47-165">I följande exempel skapas en behållare med namnet *mydisks*:</span><span class="sxs-lookup"><span data-stu-id="b7c47-165">The following example creates a container named *mydisks*:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a><span data-ttu-id="b7c47-166">Överför den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="b7c47-166">Upload the VHD</span></span>
<span data-ttu-id="b7c47-167">Ladda upp din anpassade disk med [az storage blob överför](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="b7c47-167">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="b7c47-168">Du överför och lagra anpassade disken som en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="b7c47-168">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="b7c47-169">Ange din snabbtangent, den behållare som du skapade i föregående steg och sökvägen till den anpassa disken på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="b7c47-169">Specify your access key, the container you created in the previous step, and then the path to the custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
<span data-ttu-id="b7c47-170">Överför den virtuella Hårddisken kan ta en stund.</span><span class="sxs-lookup"><span data-stu-id="b7c47-170">Uploading the VHD may take a while.</span></span>

### <a name="create-a-managed-disk"></a><span data-ttu-id="b7c47-171">Skapa en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="b7c47-171">Create a managed disk</span></span>


<span data-ttu-id="b7c47-172">Skapa en hanterade diskar från den virtuella Hårddisken med hjälp av [az disk skapa](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="b7c47-172">Create a managed disk from the VHD using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="b7c47-173">I följande exempel skapas en hanterad disk med namnet *myManagedDisk* från den virtuella Hårddisken som du har överfört till namngivna storage-konto och behållare:</span><span class="sxs-lookup"><span data-stu-id="b7c47-173">The following example creates a managed disk named *myManagedDisk* from the VHD you uploaded to your named storage account and container:</span></span>

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a><span data-ttu-id="b7c47-174">Alternativ 2: Kopiera en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b7c47-174">Option 2: Copy an existing VM</span></span>

<span data-ttu-id="b7c47-175">Du kan också skapa anpassade VM i Azure och sedan kopiera OS-disk och koppla den till en ny virtuell dator att skapa en annan kopia.</span><span class="sxs-lookup"><span data-stu-id="b7c47-175">You can also create the customized VM in Azure and then copy the OS disk and attach it to a new VM to create another copy.</span></span> <span data-ttu-id="b7c47-176">Det här är bra för testning, men om du vill använda en befintlig virtuell Azure-dator som modell för flera nya virtuella datorer verkligen bör du skapa en **bild** i stället.</span><span class="sxs-lookup"><span data-stu-id="b7c47-176">This is fine for testing, but if you want to use an existing Azure VM as the model for multiple new VMs, you really should create an **image** instead.</span></span> <span data-ttu-id="b7c47-177">Mer information om hur du skapar en avbildning från en befintlig Azure-VM finns [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av CLI](tutorial-custom-images.md)</span><span class="sxs-lookup"><span data-stu-id="b7c47-177">For more information about creating an image from an existing Azure VM, see [Create a custom image of an Azure VM using the CLI](tutorial-custom-images.md)</span></span>

### <a name="create-a-snapshot"></a><span data-ttu-id="b7c47-178">Skapa en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="b7c47-178">Create a snapshot</span></span>

<span data-ttu-id="b7c47-179">Det här exemplet skapas en ögonblicksbild av en virtuell dator med namnet *myVM* i resursgruppen *myResourceGroup* och skapar en ögonblicksbild med namnet *osDiskSnapshot*.</span><span class="sxs-lookup"><span data-stu-id="b7c47-179">This example creates a snapshot of a VM named *myVM* in resource group *myResourceGroup* and creates a snapshot named *osDiskSnapshot*.</span></span>

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a><span data-ttu-id="b7c47-180">Skapa den hantera disken</span><span class="sxs-lookup"><span data-stu-id="b7c47-180">Create the managed disk</span></span>

<span data-ttu-id="b7c47-181">Skapa en ny hanterade disk från ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="b7c47-181">Create a new managed disk from the snapshot.</span></span>

<span data-ttu-id="b7c47-182">Hämta ID för ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="b7c47-182">Get the ID of the snapshot.</span></span> <span data-ttu-id="b7c47-183">I det här exemplet ögonblicksbilden heter *osDiskSnapshot* och finns i den *myResourceGroup* resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b7c47-183">In this example, the snapshot is named *osDiskSnapshot* and it is in the *myResourceGroup* resource group.</span></span>

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

<span data-ttu-id="b7c47-184">Skapa den hantera disken.</span><span class="sxs-lookup"><span data-stu-id="b7c47-184">Create the managed disk.</span></span> <span data-ttu-id="b7c47-185">I det här exemplet skapar vi en hanterad disk med namnet *myManagedDisk* från våra ögonblicksbild som är 128 GB i storlek i standardlagring.</span><span class="sxs-lookup"><span data-stu-id="b7c47-185">In this example, we will create a managed disk named *myManagedDisk* from our snapshot, that is 128GB in size in standard storage.</span></span>

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a><span data-ttu-id="b7c47-186">Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b7c47-186">Create the VM</span></span>

<span data-ttu-id="b7c47-187">Nu ska du skapa den virtuella datorn med [az vm skapa](/cli/azure/vm#create) och bifoga (--bifoga-os-disk) hanterade disken som OS-disk.</span><span class="sxs-lookup"><span data-stu-id="b7c47-187">Now, create your VM with [az vm create](/cli/azure/vm#create) and attach (--attach-os-disk) the managed disk as the OS disk.</span></span> <span data-ttu-id="b7c47-188">I följande exempel skapas en virtuell dator med namnet *myNewVM* med den hantera disken som skapas från den överförda virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="b7c47-188">The following example creates a VM named *myNewVM* using the managed disk created from your uploaded VHD:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

<span data-ttu-id="b7c47-189">Du bör kunna SSH till den virtuella datorn med autentiseringsuppgifter från den Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="b7c47-189">You should be able to SSH into the VM using the credentials from the source VM.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b7c47-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7c47-190">Next steps</span></span>
<span data-ttu-id="b7c47-191">När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7c47-191">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="b7c47-192">Du kan också [lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) till din nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b7c47-192">You may also want to [add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to your new VMs.</span></span> <span data-ttu-id="b7c47-193">Om du har program som körs på din virtuella dator som du behöver åtkomst till [öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7c47-193">If you have applications running on your VMs that you need to access, be sure to [open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

