---
title: aaaUpload eller kopiera en anpassad Linux VM med Azure CLI 2.0 | Microsoft Docs
description: "Ladda upp eller kopiera en anpassad virtuell dator med hjälp av hello Resource Manager-modellen och hello Azure CLI 2.0"
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
ms.openlocfilehash: 79af897120a6ba7f4a427ba6c7d0c31b7b870bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a><span data-ttu-id="e5c33-103">Skapa en Linux VM från anpassade disken med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e5c33-103">Create a Linux VM from custom disk with hello Azure CLI 2.0</span></span>

<!-- rename toocreate-vm-specialized -->

<span data-ttu-id="e5c33-104">Den här artikeln beskrivs hur du tooupload en anpassad virtuell hårddisk (VHD) eller kopiera en en befintlig virtuell Hårddisk i Azure och skapa nya virtuella Linux-datorer (VM) från hello anpassade disken.</span><span class="sxs-lookup"><span data-stu-id="e5c33-104">This article shows you how tooupload a customized virtual hard disk (VHD) or copy a an existing VHD in Azure and create new Linux virtual machines (VMs) from hello custom disk.</span></span> <span data-ttu-id="e5c33-105">Du kan installera och konfigurera en Linux distro tooyour krav och sedan använda den virtuella Hårddisken tooquickly skapa en ny Azure virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5c33-105">You can install and configure a Linux distro tooyour requirements and then use that VHD tooquickly create a new Azure virtual machine.</span></span>

<span data-ttu-id="e5c33-106">Om du vill toocreate flera virtuella datorer från anpassade disken, bör du skapa en avbildning från VM- eller VHD.</span><span class="sxs-lookup"><span data-stu-id="e5c33-106">If you want toocreate multiple VMs from your customized disk, you should create an image from your VM or VHD.</span></span> <span data-ttu-id="e5c33-107">Mer information finns i [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av hello CLI](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="e5c33-107">For more information, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md).</span></span>

<span data-ttu-id="e5c33-108">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="e5c33-108">You have two options:</span></span>
* [<span data-ttu-id="e5c33-109">Överföra en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="e5c33-109">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="e5c33-110">Kopiera en befintlig virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="e5c33-110">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a><span data-ttu-id="e5c33-111">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="e5c33-111">Quick commands</span></span>

<span data-ttu-id="e5c33-112">När du skapar en ny virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create) från en anpassad eller särskilda disk du **bifoga** hello disk (--bifoga-os-disk) istället för att ange en anpassad eller marketplace-avbildning (--bilden).</span><span class="sxs-lookup"><span data-stu-id="e5c33-112">When creating a new VM using [az vm create](/cli/azure/vm#create) from a customized or specialized disk you **attach** hello disk (--attach-os-disk) instead of specifying a custom or marketplace image (--image).</span></span> <span data-ttu-id="e5c33-113">hello följande exempel skapas en virtuell dator med namnet *myVM* med hello hanterade disk med namnet *myManagedDisk* skapas från den anpassade virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="e5c33-113">hello following example creates a VM named *myVM* using hello managed disk named *myManagedDisk* created from your customized VHD:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a><span data-ttu-id="e5c33-114">Krav</span><span class="sxs-lookup"><span data-stu-id="e5c33-114">Requirements</span></span>
<span data-ttu-id="e5c33-115">toocomplete hello följande steg, behöver du:</span><span class="sxs-lookup"><span data-stu-id="e5c33-115">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="e5c33-116">En virtuell Linux-dator som har förberetts för användning i Azure.</span><span class="sxs-lookup"><span data-stu-id="e5c33-116">A Linux virtual machine that has been prepared for use in Azure.</span></span> <span data-ttu-id="e5c33-117">Hej [Förbered hello VM](#prepare-the-vm) i den här artikeln beskriver hur toofind distro specifik information om hur du installerar hello Azure Linux-agenten (waagent) som krävs för hello VM toowork korrekt i Azure och du toobe kan tooconnect tooit via SSH.</span><span class="sxs-lookup"><span data-stu-id="e5c33-117">hello [Prepare hello VM](#prepare-the-vm) section of this article covers how toofind distro specific information on installing hello Azure Linux Agent (waagent) which is required for hello VM toowork properly in Azure and for you toobe able tooconnect tooit using SSH.</span></span>
* <span data-ttu-id="e5c33-118">hello VHD-filen från en befintlig [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuell disk i hello VHD-format.</span><span class="sxs-lookup"><span data-stu-id="e5c33-118">hello VHD file from an existing [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="e5c33-119">Flera verktyg finns toocreate en virtuell dator och virtuell Hårddisk:</span><span class="sxs-lookup"><span data-stu-id="e5c33-119">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="e5c33-120">Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), sköter toouse VHD som bildformat.</span><span class="sxs-lookup"><span data-stu-id="e5c33-120">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="e5c33-121">Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med **qemu img konvertera**.</span><span class="sxs-lookup"><span data-stu-id="e5c33-121">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using **qemu-img convert**.</span></span>
  * <span data-ttu-id="e5c33-122">Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5c33-122">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="e5c33-123">hello senare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="e5c33-123">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="e5c33-124">När du skapar en virtuell dator kan du ange VHD som hello-format.</span><span class="sxs-lookup"><span data-stu-id="e5c33-124">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="e5c33-125">Om det behövs kan du konvertera VHDX diskar tooVHD med [qemu img konvertera](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e5c33-125">If needed, you can convert VHDX disks tooVHD using [qemu-img convert](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="e5c33-126">Dessutom stöder Azure inte uppladdning dynamiska virtuella hårddiskar, så du måste tooconvert sådana diskar toostatic virtuella hårddiskar innan du laddar upp.</span><span class="sxs-lookup"><span data-stu-id="e5c33-126">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="e5c33-127">Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamiska diskar under hello processen överför tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e5c33-127">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 


* <span data-ttu-id="e5c33-128">Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="e5c33-128">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="e5c33-129">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="e5c33-129">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="e5c33-130">Exempel parameternamn ingår *myResourceGroup*, *mittlagringskonto*, och *mydisks*.</span><span class="sxs-lookup"><span data-stu-id="e5c33-130">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *mydisks*.</span></span>

<span data-ttu-id="e5c33-131"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="e5c33-131"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-vm"></a><span data-ttu-id="e5c33-132">Förbereda hello VM</span><span class="sxs-lookup"><span data-stu-id="e5c33-132">Prepare hello VM</span></span>

<span data-ttu-id="e5c33-133">Azure stöder olika Linux-distributioner (se [godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="e5c33-133">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="e5c33-134">hello följande artiklar när du går igenom hur tooprepare hello olika Linux-distributioner som stöds i Azure:</span><span class="sxs-lookup"><span data-stu-id="e5c33-134">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* [<span data-ttu-id="e5c33-135">CentOS-baserade distributioner</span><span class="sxs-lookup"><span data-stu-id="e5c33-135">CentOS-based Distributions</span></span>](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e5c33-136">Debian Linux</span><span class="sxs-lookup"><span data-stu-id="e5c33-136">Debian Linux</span></span>](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e5c33-137">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="e5c33-137">Oracle Linux</span></span>](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e5c33-138">Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="e5c33-138">Red Hat Enterprise Linux</span></span>](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e5c33-139">SLES & openSUSE</span><span class="sxs-lookup"><span data-stu-id="e5c33-139">SLES & openSUSE</span></span>](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e5c33-140">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e5c33-140">Ubuntu</span></span>](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e5c33-141">Andra - icke-godkända distributioner</span><span class="sxs-lookup"><span data-stu-id="e5c33-141">Other - Non-Endorsed Distributions</span></span>](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

<span data-ttu-id="e5c33-142">Se även hello [Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer allmänna tips om hur du förbereder Linux avbildningar för Azure.</span><span class="sxs-lookup"><span data-stu-id="e5c33-142">Also see hello [Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="e5c33-143">Hej [Azure-plattformen SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gäller tooVMs som kör Linux bara när en hello-godkända distributioner används med hello konfigurationsinformation som anges under stöds versioner i [Linux på Azure-godkända Distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e5c33-143">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="option-1-upload-a-vhd"></a><span data-ttu-id="e5c33-144">Alternativ 1: Ladda upp en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="e5c33-144">Option 1: Upload a VHD</span></span>

<span data-ttu-id="e5c33-145">Du kan överföra en anpassad virtuell Hårddisk som du har körs på en lokal dator eller som du exporterade från ett annat moln.</span><span class="sxs-lookup"><span data-stu-id="e5c33-145">You can upload a customized VHD that you have running on a local machine or that you exported from another cloud.</span></span> <span data-ttu-id="e5c33-146">toouse hello VHD toocreate en ny Azure VM, behöver du tooupload hello VHD tooa storage-konto och skapa en hanterad disk från hello VHD.</span><span class="sxs-lookup"><span data-stu-id="e5c33-146">toouse hello VHD toocreate a new Azure VM, you need tooupload hello VHD tooa storage account and create a managed disk from hello VHD.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="e5c33-147">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e5c33-147">Create a resource group</span></span>

<span data-ttu-id="e5c33-148">Innan du laddar upp din anpassade disk och skapar virtuella datorer, måste du först toocreate en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="e5c33-148">Before uploading your custom disk and creating VMs, you first need toocreate a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="e5c33-149">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats: [översikt över Azure hanterade diskar](../windows/managed-disks-overview.md)</span><span class="sxs-lookup"><span data-stu-id="e5c33-149">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location: [Azure Managed Disks overview](../windows/managed-disks-overview.md)</span></span>
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a><span data-ttu-id="e5c33-150">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e5c33-150">Create a storage account</span></span>

<span data-ttu-id="e5c33-151">Skapa ett lagringskonto för anpassade disk- och virtuella datorer med [az storage-konto skapar](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="e5c33-151">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> 

<span data-ttu-id="e5c33-152">hello följande exempel skapas ett lagringskonto med namnet *mittlagringskonto* i hello resursgruppen skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="e5c33-152">hello following example creates a storage account named *mystorageaccount* in hello resource group previously created:</span></span>

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a><span data-ttu-id="e5c33-153">Lista nycklar för lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e5c33-153">List storage account keys</span></span>
<span data-ttu-id="e5c33-154">Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure.</span><span class="sxs-lookup"><span data-stu-id="e5c33-154">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="e5c33-155">Dessa snabbtangenter används vid autentisering toohello storage-konto, som utför skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e5c33-155">These access keys are used when authenticating toohello storage account, like carrying out write operations.</span></span> <span data-ttu-id="e5c33-156">Läs mer om [hantera åtkomst till toostorage här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="e5c33-156">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="e5c33-157">Du visar hello snabbtangenter med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="e5c33-157">You view hello access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="e5c33-158">Visa hello snabbtangenter för hello storage-konto som du skapade:</span><span class="sxs-lookup"><span data-stu-id="e5c33-158">View hello access keys for hello storage account you created:</span></span>

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

<span data-ttu-id="e5c33-159">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="e5c33-159">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="e5c33-160">Anteckna **key1** som du vill använda den toointeract med ditt lagringskonto i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e5c33-160">Make a note of **key1** as you will use it toointeract with your storage account in hello next steps.</span></span>

### <a name="create-a-storage-container"></a><span data-ttu-id="e5c33-161">Skapa en lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="e5c33-161">Create a storage container</span></span>
<span data-ttu-id="e5c33-162">Samma sätt som du skapar olika kataloger toologically ordna det lokala filsystemet i hello skapar behållare i en storage-konto tooorganize diskarna.</span><span class="sxs-lookup"><span data-stu-id="e5c33-162">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your disks.</span></span> <span data-ttu-id="e5c33-163">Ett lagringskonto kan innehålla valfritt antal behållare.</span><span class="sxs-lookup"><span data-stu-id="e5c33-163">A storage account can contain any number of containers.</span></span> <span data-ttu-id="e5c33-164">Skapa en behållare med [az lagringsbehållaren skapa](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="e5c33-164">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="e5c33-165">hello följande exempel skapas en behållare med namnet *mydisks*:</span><span class="sxs-lookup"><span data-stu-id="e5c33-165">hello following example creates a container named *mydisks*:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-hello-vhd"></a><span data-ttu-id="e5c33-166">Överför hello VHD</span><span class="sxs-lookup"><span data-stu-id="e5c33-166">Upload hello VHD</span></span>
<span data-ttu-id="e5c33-167">Ladda upp din anpassade disk med [az storage blob överför](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="e5c33-167">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="e5c33-168">Du överför och lagra anpassade disken som en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="e5c33-168">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="e5c33-169">Ange din åtkomstnyckel, hello-behållare som du skapade i föregående steg i hello och hello sökvägen toohello anpassade disk på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="e5c33-169">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
<span data-ttu-id="e5c33-170">Överför hello VHD kan ta en stund.</span><span class="sxs-lookup"><span data-stu-id="e5c33-170">Uploading hello VHD may take a while.</span></span>

### <a name="create-a-managed-disk"></a><span data-ttu-id="e5c33-171">Skapa en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="e5c33-171">Create a managed disk</span></span>


<span data-ttu-id="e5c33-172">Skapa en hanterad disk från hello VHD använder [az disk skapa](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="e5c33-172">Create a managed disk from hello VHD using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="e5c33-173">hello följande exempel skapas en hanterad disk med namnet *myManagedDisk* från hello VHD du överförde tooyour med namnet lagringskonto och en behållare:</span><span class="sxs-lookup"><span data-stu-id="e5c33-173">hello following example creates a managed disk named *myManagedDisk* from hello VHD you uploaded tooyour named storage account and container:</span></span>

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a><span data-ttu-id="e5c33-174">Alternativ 2: Kopiera en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5c33-174">Option 2: Copy an existing VM</span></span>

<span data-ttu-id="e5c33-175">Du kan också skapa hello anpassade VM i Azure och sedan kopiera hello OS-disk och koppla den tooa nya VM toocreate en annan kopia.</span><span class="sxs-lookup"><span data-stu-id="e5c33-175">You can also create hello customized VM in Azure and then copy hello OS disk and attach it tooa new VM toocreate another copy.</span></span> <span data-ttu-id="e5c33-176">Det här är bra för testning, men om du vill toouse en befintlig virtuell Azure-dator som hello modell för flera nya virtuella datorer, verkligen bör du skapa en **bild** i stället.</span><span class="sxs-lookup"><span data-stu-id="e5c33-176">This is fine for testing, but if you want toouse an existing Azure VM as hello model for multiple new VMs, you really should create an **image** instead.</span></span> <span data-ttu-id="e5c33-177">Mer information om hur du skapar en avbildning från en befintlig Azure-VM finns [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av hello CLI](tutorial-custom-images.md)</span><span class="sxs-lookup"><span data-stu-id="e5c33-177">For more information about creating an image from an existing Azure VM, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md)</span></span>

### <a name="create-a-snapshot"></a><span data-ttu-id="e5c33-178">Skapa en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="e5c33-178">Create a snapshot</span></span>

<span data-ttu-id="e5c33-179">Det här exemplet skapas en ögonblicksbild av en virtuell dator med namnet *myVM* i resursgruppen *myResourceGroup* och skapar en ögonblicksbild med namnet *osDiskSnapshot*.</span><span class="sxs-lookup"><span data-stu-id="e5c33-179">This example creates a snapshot of a VM named *myVM* in resource group *myResourceGroup* and creates a snapshot named *osDiskSnapshot*.</span></span>

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-hello-managed-disk"></a><span data-ttu-id="e5c33-180">Skapa hello hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="e5c33-180">Create hello managed disk</span></span>

<span data-ttu-id="e5c33-181">Skapa en ny hanterade disk från hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="e5c33-181">Create a new managed disk from hello snapshot.</span></span>

<span data-ttu-id="e5c33-182">Hämta hello-ID för hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="e5c33-182">Get hello ID of hello snapshot.</span></span> <span data-ttu-id="e5c33-183">I det här exemplet hello ögonblicksbild heter *osDiskSnapshot* och finns i hello *myResourceGroup* resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e5c33-183">In this example, hello snapshot is named *osDiskSnapshot* and it is in hello *myResourceGroup* resource group.</span></span>

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

<span data-ttu-id="e5c33-184">Skapa hello hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="e5c33-184">Create hello managed disk.</span></span> <span data-ttu-id="e5c33-185">I det här exemplet skapar vi en hanterad disk med namnet *myManagedDisk* från våra ögonblicksbild som är 128 GB i storlek i standardlagring.</span><span class="sxs-lookup"><span data-stu-id="e5c33-185">In this example, we will create a managed disk named *myManagedDisk* from our snapshot, that is 128GB in size in standard storage.</span></span>

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-hello-vm"></a><span data-ttu-id="e5c33-186">Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="e5c33-186">Create hello VM</span></span>

<span data-ttu-id="e5c33-187">Nu ska du skapa den virtuella datorn med [az vm skapa](/cli/azure/vm#create) och bifoga (--bifoga-os-disk) hello hanteras disk som hello OS-disk.</span><span class="sxs-lookup"><span data-stu-id="e5c33-187">Now, create your VM with [az vm create](/cli/azure/vm#create) and attach (--attach-os-disk) hello managed disk as hello OS disk.</span></span> <span data-ttu-id="e5c33-188">hello följande exempel skapas en virtuell dator med namnet *myNewVM* med hello hanterade disken som skapas från den överförda virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="e5c33-188">hello following example creates a VM named *myNewVM* using hello managed disk created from your uploaded VHD:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

<span data-ttu-id="e5c33-189">Du bör vara kan tooSSH till hello VM med hello referenser från Virtuella hello källdatorn.</span><span class="sxs-lookup"><span data-stu-id="e5c33-189">You should be able tooSSH into hello VM using hello credentials from hello source VM.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e5c33-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5c33-190">Next steps</span></span>
<span data-ttu-id="e5c33-191">När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e5c33-191">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="e5c33-192">Du kan också för[lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e5c33-192">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="e5c33-193">Om du har program som körs på din virtuella dator som du behöver tooaccess för[öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e5c33-193">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

