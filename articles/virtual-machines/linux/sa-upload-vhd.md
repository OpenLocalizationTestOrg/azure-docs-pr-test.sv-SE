---
title: aaaUpload en anpassad Linux-disk med Azure CLI 2.0 | Microsoft Docs
description: "Skapa och ladda upp en virtuell hårddisk (VHD) tooAzure med hello Resource Manager-modellen och hello Azure CLI 2.0"
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
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: cf8556ab4dfe6c4e5eff4e99fe1ddc44393f774c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a><span data-ttu-id="49631-103">Ladda upp och skapa en Linux VM från anpassade disken med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="49631-103">Upload and create a Linux VM from custom disk with hello Azure CLI 2.0</span></span>
<span data-ttu-id="49631-104">Den här artikeln visar hur tooupload en virtuell hårddisk (VHD) tooan Azure storage-konto med hello Azure CLI 2.0 och skapa virtuella Linux-datorer från den här anpassade disken.</span><span class="sxs-lookup"><span data-stu-id="49631-104">This article shows you how tooupload a virtual hard disk (VHD) tooan Azure storage account with hello Azure CLI 2.0 and create Linux VMs from this custom disk.</span></span> <span data-ttu-id="49631-105">Du kan också utföra dessa steg med hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49631-105">You can also perform these steps with hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="49631-106">Den här funktionen kan du tooinstall och konfigurera en Linux distro tooyour krav och sedan använda den virtuella Hårddisken tooquickly skapa virtuella Azure-datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="49631-106">This functionality allows you tooinstall and configure a Linux distro tooyour requirements and then use that VHD tooquickly create Azure virtual machines (VMs).</span></span>

<span data-ttu-id="49631-107">Det här avsnittet använder storage-konton för hello sista virtuella hårddiskar, men du kan också göra följande med hjälp av [hanterade diskar](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="49631-107">This topic uses storage accounts for hello final VHDs, but you can also do these steps using [managed disks](upload-vhd.md).</span></span> 

## <a name="quick-commands"></a><span data-ttu-id="49631-108">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="49631-108">Quick commands</span></span>
<span data-ttu-id="49631-109">Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello bas kommandon tooupload en VHD-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="49631-109">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VHD tooAzure.</span></span> <span data-ttu-id="49631-110">Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#requirements).</span><span class="sxs-lookup"><span data-stu-id="49631-110">More detailed information and context for each step can be found hello rest of hello document, [starting here](#requirements).</span></span>

<span data-ttu-id="49631-111">Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="49631-111">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="49631-112">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="49631-112">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="49631-113">Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="49631-113">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="49631-114">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="49631-114">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="49631-115">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `WestUs` plats:</span><span class="sxs-lookup"><span data-stu-id="49631-115">hello following example creates a resource group named `myResourceGroup` in hello `WestUs` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="49631-116">Skapa en storage-konto toohold din virtuella diskar med [az storage-konto skapar](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="49631-116">Create a storage account toohold your virtual disks with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="49631-117">hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="49631-117">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

<span data-ttu-id="49631-118">Visa en lista över hello åtkomstnycklarna för ditt lagringskonto med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="49631-118">List hello access keys for your storage account with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="49631-119">Anteckna `key1`:</span><span class="sxs-lookup"><span data-stu-id="49631-119">Make a note of `key1`:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="49631-120">Skapa en behållare i ditt lagringskonto med hjälp av hello lagringsnyckel du fick med [az lagringsbehållaren skapa](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="49631-120">Create a container within your storage account using hello storage key you obtained with [az storage container create](/cli/azure/storage/container#create).</span></span> <span data-ttu-id="49631-121">hello följande exempel skapas en behållare med namnet `mydisks` med hello lagring nyckelvärde från `key1`:</span><span class="sxs-lookup"><span data-stu-id="49631-121">hello following example creates a container named `mydisks` using hello storage key value from `key1`:</span></span>

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

<span data-ttu-id="49631-122">Slutligen överför din VHD toohello-behållare som du skapat med [az storage blob överför](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="49631-122">Finally, upload your VHD toohello container you created with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="49631-123">Ange hello lokal sökväg tooyour VHD under `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="49631-123">Specify hello local path tooyour VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

<span data-ttu-id="49631-124">Ange hello URI tooyour disk (`--image`) med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="49631-124">Specify hello URI tooyour disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="49631-125">hello följande exempel skapas en virtuell dator med namnet `myVM` med hello virtuella disken överfört tidigare:</span><span class="sxs-lookup"><span data-stu-id="49631-125">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="49631-126">Hej destinationslagringskontot har toobe hello samma som om du har överfört din virtuell disk till.</span><span class="sxs-lookup"><span data-stu-id="49631-126">hello destination storage account has toobe hello same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="49631-127">Du också behöva toospecify eller besvara efterfrågas, alla hello ytterligare parametrar som krävs av hello **az vm skapa** kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="49631-127">You also need toospecify, or answer prompts for, all hello additional parameters required by hello **az vm create** command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="49631-128">Du kan läsa mer om hello [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="49631-128">You can read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="49631-129">Krav</span><span class="sxs-lookup"><span data-stu-id="49631-129">Requirements</span></span>
<span data-ttu-id="49631-130">toocomplete hello följande steg, behöver du:</span><span class="sxs-lookup"><span data-stu-id="49631-130">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="49631-131">**Linux-operativsystem i en VHD-fil** -installera en [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuell disk i hello VHD format.</span><span class="sxs-lookup"><span data-stu-id="49631-131">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="49631-132">Flera verktyg finns toocreate en virtuell dator och virtuell Hårddisk:</span><span class="sxs-lookup"><span data-stu-id="49631-132">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="49631-133">Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), sköter toouse VHD som bildformat.</span><span class="sxs-lookup"><span data-stu-id="49631-133">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="49631-134">Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="49631-134">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="49631-135">Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="49631-135">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="49631-136">hello senare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="49631-136">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="49631-137">När du skapar en virtuell dator kan du ange VHD som hello-format.</span><span class="sxs-lookup"><span data-stu-id="49631-137">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="49631-138">Om det behövs kan du konvertera VHDX diskar tooVHD med [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="49631-138">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="49631-139">Dessutom stöder Azure inte uppladdning dynamiska virtuella hårddiskar, så du måste tooconvert sådana diskar toostatic virtuella hårddiskar innan du laddar upp.</span><span class="sxs-lookup"><span data-stu-id="49631-139">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="49631-140">Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamiska diskar under hello processen överför tooAzure.</span><span class="sxs-lookup"><span data-stu-id="49631-140">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 

* <span data-ttu-id="49631-141">Virtuella datorer skapas från anpassade disken måste finnas i hello samma lagringskonto som själva hello disken</span><span class="sxs-lookup"><span data-stu-id="49631-141">VMs created from your custom disk must reside in hello same storage account as hello disk itself</span></span>
  * <span data-ttu-id="49631-142">Skapa en storage-konto och en behållare toohold både dina anpassade disk och skapade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="49631-142">Create a storage account and container toohold both your custom disk and created VMs</span></span>
  * <span data-ttu-id="49631-143">När du har skapat din virtuella dator, kan du ta bort disken</span><span class="sxs-lookup"><span data-stu-id="49631-143">After you have created all your VMs, you can safely delete your disk</span></span>

<span data-ttu-id="49631-144">Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="49631-144">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="49631-145">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="49631-145">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="49631-146">Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="49631-146">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="49631-147"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="49631-147"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-disk-toobe-uploaded"></a><span data-ttu-id="49631-148">Förbereda hello disk toobe har överförts</span><span class="sxs-lookup"><span data-stu-id="49631-148">Prepare hello disk toobe uploaded</span></span>
<span data-ttu-id="49631-149">Azure stöder olika Linux-distributioner (se [godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="49631-149">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="49631-150">hello följande artiklar när du går igenom hur tooprepare hello olika Linux-distributioner som stöds i Azure:</span><span class="sxs-lookup"><span data-stu-id="49631-150">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="49631-151">**[CentOS-baserade distributioner](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="49631-151">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="49631-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="49631-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="49631-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="49631-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="49631-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="49631-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="49631-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="49631-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="49631-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="49631-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="49631-157">**[Andra - icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="49631-157">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="49631-158">Se även hello  **[Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes)**  mer allmänna tips om hur du förbereder Linux avbildningar för Azure.</span><span class="sxs-lookup"><span data-stu-id="49631-158">Also see hello **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="49631-159">Hej [Azure-plattformen SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gäller tooVMs som kör Linux bara när en hello-godkända distributioner används med hello konfigurationsinformation som anges under stöds versioner i [Linux på Azure-godkända Distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49631-159">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="create-a-resource-group"></a><span data-ttu-id="49631-160">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="49631-160">Create a resource group</span></span>
<span data-ttu-id="49631-161">Resursgrupper sätta logiskt ihop alla hello Azure-resurser toosupport virtuella datorer, till exempel hello virtuella nätverk och lagring.</span><span class="sxs-lookup"><span data-stu-id="49631-161">Resource groups logically bring together all hello Azure resources toosupport your virtual machines, such as hello virtual networking and storage.</span></span> <span data-ttu-id="49631-162">Resursgrupper för mer information, se [resursgrupper översikt](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="49631-162">For more information resource groups, see [resource groups overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="49631-163">Innan du laddar upp din anpassade disk och skapar virtuella datorer, måste du först toocreate en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="49631-163">Before uploading your custom disk and creating VMs, you first need toocreate a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="49631-164">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westus` plats:</span><span class="sxs-lookup"><span data-stu-id="49631-164">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a><span data-ttu-id="49631-165">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="49631-165">Create a storage account</span></span>

<span data-ttu-id="49631-166">Skapa ett lagringskonto för anpassade disk- och virtuella datorer med [az storage-konto skapar](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="49631-166">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="49631-167">Virtuella datorer med ohanterad diskar som du skapar från din anpassade disken måste toobe i hello samma lagringskonto som disken.</span><span class="sxs-lookup"><span data-stu-id="49631-167">Any VMs with unmanaged disks that you create from your custom disk need toobe in hello same storage account as that disk.</span></span> 

<span data-ttu-id="49631-168">hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount` i hello resursgruppen skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="49631-168">hello following example creates a storage account named `mystorageaccount` in hello resource group previously created:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="49631-169">Lista nycklar för lagringskonto</span><span class="sxs-lookup"><span data-stu-id="49631-169">List storage account keys</span></span>
<span data-ttu-id="49631-170">Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure.</span><span class="sxs-lookup"><span data-stu-id="49631-170">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="49631-171">Dessa snabbtangenter används vid autentisering toohello storage-konto, till exempel toocarry ut skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="49631-171">These access keys are used when authenticating toohello storage account, such as toocarry out write operations.</span></span> <span data-ttu-id="49631-172">Läs mer om [hantera åtkomst till toostorage här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="49631-172">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="49631-173">Du visar hello snabbtangenter med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="49631-173">You view hello access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="49631-174">Visa hello snabbtangenter för hello storage-konto som du skapade:</span><span class="sxs-lookup"><span data-stu-id="49631-174">View hello access keys for hello storage account you created:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="49631-175">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="49631-175">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="49631-176">Anteckna `key1` som du vill använda den toointeract med ditt lagringskonto i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="49631-176">Make a note of `key1` as you will use it toointeract with your storage account in hello next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="49631-177">Skapa en lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="49631-177">Create a storage container</span></span>
<span data-ttu-id="49631-178">Samma sätt som du skapar olika kataloger toologically ordna det lokala filsystemet i hello skapar behållare i en storage-konto tooorganize diskarna.</span><span class="sxs-lookup"><span data-stu-id="49631-178">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your disks.</span></span> <span data-ttu-id="49631-179">Ett lagringskonto kan innehålla valfritt antal behållare.</span><span class="sxs-lookup"><span data-stu-id="49631-179">A storage account can contain any number of containers.</span></span> <span data-ttu-id="49631-180">Skapa en behållare med [az lagringsbehållaren skapa](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="49631-180">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="49631-181">hello följande exempel skapas en behållare med namnet `mydisks`:</span><span class="sxs-lookup"><span data-stu-id="49631-181">hello following example creates a container named `mydisks`:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a><span data-ttu-id="49631-182">Överför VHD</span><span class="sxs-lookup"><span data-stu-id="49631-182">Upload VHD</span></span>
<span data-ttu-id="49631-183">Ladda upp din anpassade disk med [az storage blob överför](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="49631-183">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="49631-184">Du överför och lagra anpassade disken som en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="49631-184">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="49631-185">Ange din åtkomstnyckel, hello-behållare som du skapade i föregående steg i hello och hello sökvägen toohello anpassade disk på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="49631-185">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-hello-vm"></a><span data-ttu-id="49631-186">Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="49631-186">Create hello VM</span></span>
<span data-ttu-id="49631-187">toocreate en virtuell dator med ohanterad diskar, ange hello URI tooyour disk (`--image`) med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="49631-187">toocreate a VM with unmanaged disks, specify hello URI tooyour disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="49631-188">hello följande exempel skapas en virtuell dator med namnet `myVM` med hello virtuella disken överfört tidigare:</span><span class="sxs-lookup"><span data-stu-id="49631-188">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

<span data-ttu-id="49631-189">Du anger hello `--image` parameter med [az vm skapa](/cli/azure/vm#create) toopoint tooyour anpassade disk.</span><span class="sxs-lookup"><span data-stu-id="49631-189">You specify hello `--image` parameter with [az vm create](/cli/azure/vm#create) toopoint tooyour custom disk.</span></span> <span data-ttu-id="49631-190">Se till att `--storage-account` matchar hello lagringskonto där anpassade disken ska lagras.</span><span class="sxs-lookup"><span data-stu-id="49631-190">Ensure that `--storage-account` matches hello storage account where your custom disk is stored.</span></span> <span data-ttu-id="49631-191">Du har inte toouse hello samma behållare som hello anpassade disk toostore dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="49631-191">You do not have toouse hello same container as hello custom disk toostore your VMs.</span></span> <span data-ttu-id="49631-192">Se till att toocreate eventuella ytterligare behållare i hello samma sätt som hello tidigare steg innan du laddar upp din anpassade disk.</span><span class="sxs-lookup"><span data-stu-id="49631-192">Make sure toocreate any additional containers in hello same way as hello earlier steps before uploading your custom disk.</span></span>

<span data-ttu-id="49631-193">hello följande exempel skapas en virtuell dator med namnet `myVM` från anpassade disken:</span><span class="sxs-lookup"><span data-stu-id="49631-193">hello following example creates a VM named `myVM` from your custom disk:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="49631-194">Du fortfarande behöver toospecify eller besvara efterfrågas, alla hello ytterligare parametrar som krävs av hello **az vm skapa** kommandot, till exempel användarnamn och SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="49631-194">You still need toospecify, or answer prompts for, all hello additional parameters required by hello **az vm create** command such as username and SSH keys.</span></span>


## <a name="resource-manager-template"></a><span data-ttu-id="49631-195">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="49631-195">Resource Manager template</span></span>
<span data-ttu-id="49631-196">Azure Resource Manager-mallar är JavaScript Object Notation (JSON) filer som definierar hello-miljö som du vill toobuild.</span><span class="sxs-lookup"><span data-stu-id="49631-196">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define hello environment you wish toobuild.</span></span> <span data-ttu-id="49631-197">hello mallar är uppdelade i toodifferent resursproviders, till exempel beräkning eller nätverket.</span><span class="sxs-lookup"><span data-stu-id="49631-197">hello templates are broken down in toodifferent resource providers such as compute or network.</span></span> <span data-ttu-id="49631-198">Du kan använda befintliga mallar eller skriva egna.</span><span class="sxs-lookup"><span data-stu-id="49631-198">You can use existing templates or write your own.</span></span> <span data-ttu-id="49631-199">Läs mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="49631-199">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="49631-200">Inom hello `Microsoft.Compute/virtualMachines` providern av mallen som du har en `storageProfile` nod som innehåller hello konfigurationsinformation för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="49631-200">Within hello `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains hello configuration details for your VM.</span></span> <span data-ttu-id="49631-201">hello två huvudsakliga parametrar tooedit är hello `image` och `vhd` URI: er som pekar tooyour anpassade disk och den nya Virtuella datorns virtuella disk.</span><span class="sxs-lookup"><span data-stu-id="49631-201">hello two main parameters tooedit are hello `image` and `vhd` URIs that point tooyour custom disk and your new VM's virtual disk.</span></span> <span data-ttu-id="49631-202">hello nedan visar ett exempel på hello JSON för att använda en anpassad disk:</span><span class="sxs-lookup"><span data-stu-id="49631-202">hello following shows an example of hello JSON for using a custom disk:</span></span>

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

<span data-ttu-id="49631-203">Du kan använda [detta befintliga mallen toocreate en virtuell dator från en anpassad avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) eller läser du om [skapa egna mallar för Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="49631-203">You can use [this existing template toocreate a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="49631-204">När du har en mall kan du använda [az distribution skapa](/cli/azure/group/deployment#create) toocreate dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="49631-204">Once you have a template configured, use [az group deployment create](/cli/azure/group/deployment#create) toocreate your VMs.</span></span> <span data-ttu-id="49631-205">Ange hello URI för JSON-mall med hello `--template-uri` parameter:</span><span class="sxs-lookup"><span data-stu-id="49631-205">Specify hello URI of your JSON template with hello `--template-uri` parameter:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="49631-206">Om du har en JSON-fil som lagras lokalt på datorn, kan du använda hello `--template-file` parameter i stället:</span><span class="sxs-lookup"><span data-stu-id="49631-206">If you have a JSON file stored locally on your computer, you can use hello `--template-file` parameter instead:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="49631-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49631-207">Next steps</span></span>
<span data-ttu-id="49631-208">När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="49631-208">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="49631-209">Du kan också för[lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="49631-209">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="49631-210">Om du har program som körs på din virtuella dator som du behöver tooaccess för[öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49631-210">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

