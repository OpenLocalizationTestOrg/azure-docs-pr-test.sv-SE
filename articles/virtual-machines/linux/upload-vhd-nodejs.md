---
title: aaaUpload en anpassad avbildning Linux med Azure CLI 1.0 | Microsoft Docs
description: "Skapa och ladda upp en virtuell hårddisk (VHD) tooAzure med en anpassad avbildning för Linux med hjälp av hello Resource Manager-modellen och hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
ms.openlocfilehash: 0bbd232debee1e632233d1c010e388dbc1548bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-hello-azure-cli-10"></a><span data-ttu-id="94ccf-103">Ladda upp och skapa en Linux VM av anpassade diskavbildning med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="94ccf-103">Upload and create a Linux VM from custom disk image by using hello Azure CLI 1.0</span></span>
<span data-ttu-id="94ccf-104">Den här artikeln visar hur tooupload som en virtuell hårddisk (VHD) tooAzure med hello Resource Manager-distributionsmodellen och skapar virtuella Linux-datorer från den här anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="94ccf-104">This article shows you how tooupload a virtual hard disk (VHD) tooAzure using hello Resource Manager deployment model and create Linux VMs from this custom image.</span></span> <span data-ttu-id="94ccf-105">Den här funktionen kan du tooinstall och konfigurera en Linux distro tooyour krav och sedan använda den virtuella Hårddisken tooquickly skapa virtuella Azure-datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="94ccf-105">This functionality allows you tooinstall and configure a Linux distro tooyour requirements and then use that VHD tooquickly create Azure virtual machines (VMs).</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="94ccf-106">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="94ccf-106">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="94ccf-107">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="94ccf-107">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="94ccf-108">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="94ccf-108">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="94ccf-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="94ccf-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="94ccf-110">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="94ccf-110">Quick commands</span></span>
<span data-ttu-id="94ccf-111">Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello bas kommandon tooupload tooAzure en VM.</span><span class="sxs-lookup"><span data-stu-id="94ccf-111">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VM tooAzure.</span></span> <span data-ttu-id="94ccf-112">Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#requirements).</span><span class="sxs-lookup"><span data-stu-id="94ccf-112">More detailed information and context for each step can be found hello rest of hello document, [starting here](#requirements).</span></span>

<span data-ttu-id="94ccf-113">Se till att du har [hello Azure CLI 1.0](../../cli-install-nodejs.md) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="94ccf-113">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="94ccf-114">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="94ccf-114">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="94ccf-115">Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `myimages`.</span><span class="sxs-lookup"><span data-stu-id="94ccf-115">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="94ccf-116">Först skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="94ccf-116">First, create a resource group.</span></span> <span data-ttu-id="94ccf-117">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `WestUs` plats:</span><span class="sxs-lookup"><span data-stu-id="94ccf-117">hello following example creates a resource group named `myResourceGroup` in hello `WestUs` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

<span data-ttu-id="94ccf-118">Skapa en storage-konto toohold din virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="94ccf-118">Create a storage account toohold your virtual disks.</span></span> <span data-ttu-id="94ccf-119">hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="94ccf-119">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

<span data-ttu-id="94ccf-120">Lista över hello åtkomstnycklarna för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="94ccf-120">List hello access keys for your storage account.</span></span> <span data-ttu-id="94ccf-121">Anteckna `key1`:</span><span class="sxs-lookup"><span data-stu-id="94ccf-121">Make a note of `key1`:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="94ccf-122">Skapa en behållare i ditt lagringskonto med hjälp av hello lagringsnyckel som du fick.</span><span class="sxs-lookup"><span data-stu-id="94ccf-122">Create a container within your storage account using hello storage key you obtained.</span></span> <span data-ttu-id="94ccf-123">hello följande exempel skapas en behållare med namnet `myimages` med hello lagring nyckelvärde från `key1`:</span><span class="sxs-lookup"><span data-stu-id="94ccf-123">hello following example creates a container named `myimages` using hello storage key value from `key1`:</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

<span data-ttu-id="94ccf-124">Slutligen överför din VHD toohello-behållare som du skapade.</span><span class="sxs-lookup"><span data-stu-id="94ccf-124">Finally, upload your VHD toohello container you created.</span></span> <span data-ttu-id="94ccf-125">Ange hello lokal sökväg tooyour VHD under `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="94ccf-125">Specify hello local path tooyour VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

<span data-ttu-id="94ccf-126">Nu kan du skapa en virtuell dator från den överförda virtuella hårddisken [med hjälp av en Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="94ccf-126">You can now create a VM from your uploaded virtual disk [using a Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span></span> <span data-ttu-id="94ccf-127">Du kan också använda hello CLI genom att ange hello URI tooyour disk (`--image-urn`).</span><span class="sxs-lookup"><span data-stu-id="94ccf-127">You can also use hello CLI by specifying hello URI tooyour disk (`--image-urn`).</span></span> <span data-ttu-id="94ccf-128">hello följande exempel skapas en virtuell dator med namnet `myVM` med hello virtuella disken överfört tidigare:</span><span class="sxs-lookup"><span data-stu-id="94ccf-128">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

<span data-ttu-id="94ccf-129">Hej destinationslagringskontot har toobe hello samma som om du har överfört din virtuell disk till.</span><span class="sxs-lookup"><span data-stu-id="94ccf-129">hello destination storage account has toobe hello same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="94ccf-130">Du också behöva toospecify eller besvara efterfrågas, alla hello ytterligare parametrar som krävs av hello `azure vm create` kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="94ccf-130">You also need toospecify, or answer prompts for, all hello additional parameters required by hello `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="94ccf-131">Du kan läsa mer om hello [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="94ccf-131">You can read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="94ccf-132">Krav</span><span class="sxs-lookup"><span data-stu-id="94ccf-132">Requirements</span></span>
<span data-ttu-id="94ccf-133">toocomplete hello följande steg, behöver du:</span><span class="sxs-lookup"><span data-stu-id="94ccf-133">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="94ccf-134">**Linux-operativsystem i en VHD-fil** -installera en [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuell disk i hello VHD format.</span><span class="sxs-lookup"><span data-stu-id="94ccf-134">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="94ccf-135">Flera verktyg finns toocreate en virtuell dator och virtuell Hårddisk:</span><span class="sxs-lookup"><span data-stu-id="94ccf-135">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="94ccf-136">Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), sköter toouse VHD som bildformat.</span><span class="sxs-lookup"><span data-stu-id="94ccf-136">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="94ccf-137">Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="94ccf-137">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="94ccf-138">Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="94ccf-138">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="94ccf-139">hello senare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="94ccf-139">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="94ccf-140">När du skapar en virtuell dator kan du ange VHD som hello-format.</span><span class="sxs-lookup"><span data-stu-id="94ccf-140">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="94ccf-141">Om det behövs kan du konvertera VHDX diskar tooVHD med [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="94ccf-141">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="94ccf-142">Dessutom stöder Azure inte uppladdning dynamiska virtuella hårddiskar, så du måste tooconvert sådana diskar toostatic virtuella hårddiskar innan du laddar upp.</span><span class="sxs-lookup"><span data-stu-id="94ccf-142">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="94ccf-143">Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamiska diskar under hello processen överför tooAzure.</span><span class="sxs-lookup"><span data-stu-id="94ccf-143">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 

* <span data-ttu-id="94ccf-144">Virtuella datorer skapas från den anpassade avbildningen måste finnas i hello samma lagringskonto som själva hello-bilden</span><span class="sxs-lookup"><span data-stu-id="94ccf-144">VMs created from your custom image must reside in hello same storage account as hello image itself</span></span>
  * <span data-ttu-id="94ccf-145">Skapa en storage-konto och en behållare toohold både dina anpassade avbildningen och skapade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="94ccf-145">Create a storage account and container toohold both your custom image and created VMs</span></span>
  * <span data-ttu-id="94ccf-146">När du har skapat din virtuella dator, kan du ta bort bilden</span><span class="sxs-lookup"><span data-stu-id="94ccf-146">After you have created all your VMs, you can safely delete your image</span></span>

<span data-ttu-id="94ccf-147">Se till att du har [hello Azure CLI 1.0](../../cli-install-nodejs.md) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="94ccf-147">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="94ccf-148">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="94ccf-148">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="94ccf-149">Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `myimages`.</span><span class="sxs-lookup"><span data-stu-id="94ccf-149">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="94ccf-150"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="94ccf-150"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-image-toobe-uploaded"></a><span data-ttu-id="94ccf-151">Förbereda hello avbildningen toobe har överförts</span><span class="sxs-lookup"><span data-stu-id="94ccf-151">Prepare hello image toobe uploaded</span></span>
<span data-ttu-id="94ccf-152">Azure stöder olika Linux-distributioner (se [godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="94ccf-152">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="94ccf-153">hello följande artiklar när du går igenom hur tooprepare hello olika Linux-distributioner som stöds i Azure:</span><span class="sxs-lookup"><span data-stu-id="94ccf-153">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="94ccf-154">**[CentOS-baserade distributioner](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="94ccf-154">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="94ccf-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="94ccf-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="94ccf-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="94ccf-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="94ccf-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="94ccf-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="94ccf-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="94ccf-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="94ccf-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="94ccf-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="94ccf-160">**[Andra - icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="94ccf-160">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="94ccf-161">Se även hello  **[Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes)**  mer allmänna tips om hur du förbereder Linux avbildningar för Azure.</span><span class="sxs-lookup"><span data-stu-id="94ccf-161">Also see hello **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="94ccf-162">Hej [Azure-plattformen SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gäller tooVMs som kör Linux bara när en hello-godkända distributioner används med hello konfigurationsinformation som anges under stöds versioner i [Linux på Azure-godkända Distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="94ccf-162">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="create-a-resource-group"></a><span data-ttu-id="94ccf-163">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="94ccf-163">Create a resource group</span></span>
<span data-ttu-id="94ccf-164">Resursgrupper sätta logiskt ihop alla hello Azure-resurser toosupport virtuella datorer, till exempel hello virtuella nätverk och lagring.</span><span class="sxs-lookup"><span data-stu-id="94ccf-164">Resource groups logically bring together all hello Azure resources toosupport your virtual machines, such as hello virtual networking and storage.</span></span> <span data-ttu-id="94ccf-165">Läs mer om [Azure resursgrupper här](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="94ccf-165">Read more about [Azure resource groups here](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="94ccf-166">Innan du överför din anpassade diskavbildning och skapa virtuella datorer, måste du först toocreate en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="94ccf-166">Before uploading your custom disk image and creating VMs, you first need toocreate a resource group.</span></span> 

<span data-ttu-id="94ccf-167">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `WestUS` plats:</span><span class="sxs-lookup"><span data-stu-id="94ccf-167">hello following example creates a resource group named `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="94ccf-168">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="94ccf-168">Create a storage account</span></span>
<span data-ttu-id="94ccf-169">Virtuella datorer lagras som sidblobbar i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="94ccf-169">VMs are stored as page blobs within a storage account.</span></span> <span data-ttu-id="94ccf-170">Läs mer om [Azure-blobblagring här](../../storage/common/storage-introduction.md#blob-storage).</span><span class="sxs-lookup"><span data-stu-id="94ccf-170">Read more about [Azure blob storage here](../../storage/common/storage-introduction.md#blob-storage).</span></span> <span data-ttu-id="94ccf-171">Du skapar ett lagringskonto för dina anpassade diskavbildning och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="94ccf-171">You create a storage account for your custom disk image and VMs.</span></span> <span data-ttu-id="94ccf-172">Virtuella datorer som du skapar anpassade disk image måste toobe i hello samma lagringskonto som den bilden.</span><span class="sxs-lookup"><span data-stu-id="94ccf-172">Any VMs that you create from your custom disk image need toobe in hello same storage account as that image.</span></span>

<span data-ttu-id="94ccf-173">hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount` i hello resursgruppen skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="94ccf-173">hello following example creates a storage account named `mystorageaccount` in hello resource group previously created:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="94ccf-174">Lista nycklar för lagringskonto</span><span class="sxs-lookup"><span data-stu-id="94ccf-174">List storage account keys</span></span>
<span data-ttu-id="94ccf-175">Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure.</span><span class="sxs-lookup"><span data-stu-id="94ccf-175">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="94ccf-176">Dessa snabbtangenter används vid autentisering toohello storage-konto, till exempel toocarry ut skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="94ccf-176">These access keys are used when authenticating toohello storage account, such as toocarry out write operations.</span></span> <span data-ttu-id="94ccf-177">Läs mer om [hantera åtkomst till toostorage här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="94ccf-177">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="94ccf-178">Du kan visa snabbtangenter med hello `azure storage account keys list` kommando.</span><span class="sxs-lookup"><span data-stu-id="94ccf-178">You can view access keys with hello `azure storage account keys list` command.</span></span>

<span data-ttu-id="94ccf-179">Visa hello snabbtangenter för hello storage-konto som du skapade:</span><span class="sxs-lookup"><span data-stu-id="94ccf-179">View hello access keys for hello storage account you created:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="94ccf-180">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="94ccf-180">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
<span data-ttu-id="94ccf-181">Anteckna `key1` som du vill använda den toointeract med ditt lagringskonto i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="94ccf-181">Make a note of `key1` as you will use it toointeract with your storage account in hello next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="94ccf-182">Skapa en lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="94ccf-182">Create a storage container</span></span>
<span data-ttu-id="94ccf-183">Samma sätt som du skapar olika kataloger toologically ordna det lokala filsystemet i hello skapar behållare i en storage-konto tooorganize dina virtuella diskar och bilder.</span><span class="sxs-lookup"><span data-stu-id="94ccf-183">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your virtual disks and images.</span></span> <span data-ttu-id="94ccf-184">Ett lagringskonto kan innehålla valfritt antal behållare.</span><span class="sxs-lookup"><span data-stu-id="94ccf-184">A storage account can contain any number of containers.</span></span> 

<span data-ttu-id="94ccf-185">hello följande exempel skapas en behållare med namnet `myimages`, att ange hello åtkomstnyckel hämtades i föregående steg i hello (`key1`):</span><span class="sxs-lookup"><span data-stu-id="94ccf-185">hello following example creates a container named `myimages`, specifying hello access key obtained in hello previous step (`key1`):</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a><span data-ttu-id="94ccf-186">Överför VHD</span><span class="sxs-lookup"><span data-stu-id="94ccf-186">Upload VHD</span></span>
<span data-ttu-id="94ccf-187">Nu kan du faktiskt överföra din egen avbildning.</span><span class="sxs-lookup"><span data-stu-id="94ccf-187">Now you can actually upload your custom disk image.</span></span> <span data-ttu-id="94ccf-188">Som med alla virtuella diskar som används av virtuella datorer kan du ladda upp och lagra din egen avbildning som en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="94ccf-188">As with all virtual disks used by VMs, you upload and store your custom disk image as a page blob.</span></span>

<span data-ttu-id="94ccf-189">Ange din åtkomstnyckel, hello-behållare som du skapade i föregående steg i hello och hello sökvägen toohello anpassade diskavbildning på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="94ccf-189">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk image on your local computer:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a><span data-ttu-id="94ccf-190">Skapa virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="94ccf-190">Create VM from custom image</span></span>
<span data-ttu-id="94ccf-191">När du skapar virtuella datorer från din anpassade diskavbildning ange hello URI toohello diskavbildning.</span><span class="sxs-lookup"><span data-stu-id="94ccf-191">When you create VMs from your custom disk image, specify hello URI toohello disk image.</span></span> <span data-ttu-id="94ccf-192">Se till att hello mål storage-konto matchar där din anpassade diskavbildning lagras.</span><span class="sxs-lookup"><span data-stu-id="94ccf-192">Ensure that hello destination storage account matches where your custom disk image is stored.</span></span> <span data-ttu-id="94ccf-193">Du kan skapa den virtuella datorn med hjälp av hello Azure CLI eller Resource Manager JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="94ccf-193">You can create your VM using hello Azure CLI or Resource Manager JSON template.</span></span>

### <a name="create-a-vm-using-hello-azure-cli"></a><span data-ttu-id="94ccf-194">Skapa en virtuell dator med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="94ccf-194">Create a VM using hello Azure CLI</span></span>
<span data-ttu-id="94ccf-195">Du anger hello `--image-urn` parameter med hello `azure vm create` kommandot toopoint tooyour anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="94ccf-195">You specify hello `--image-urn` parameter with hello `azure vm create` command toopoint tooyour custom disk image.</span></span> <span data-ttu-id="94ccf-196">Se till att `--storage-account-name` matchar hello lagringskonto där din anpassade diskavbildning ska lagras.</span><span class="sxs-lookup"><span data-stu-id="94ccf-196">Ensure that `--storage-account-name` matches hello storage account where your custom disk image is stored.</span></span> <span data-ttu-id="94ccf-197">Du har inte toouse hello samma behållare som hello anpassade disk image toostore dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="94ccf-197">You do not have toouse hello same container as hello custom disk image toostore your VMs.</span></span> <span data-ttu-id="94ccf-198">Se till att toocreate eventuella ytterligare behållare i hello samma sätt som hello tidigare steg innan du laddar upp din anpassade diskavbildningar.</span><span class="sxs-lookup"><span data-stu-id="94ccf-198">Make sure toocreate any additional containers in hello same way as hello earlier steps before uploading your custom disk images.</span></span>

<span data-ttu-id="94ccf-199">hello följande exempel skapas en virtuell dator med namnet `myVM` från din egen avbildning:</span><span class="sxs-lookup"><span data-stu-id="94ccf-199">hello following example creates a VM named `myVM` from your custom disk image:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

<span data-ttu-id="94ccf-200">Du fortfarande behöver toospecify eller besvara efterfrågas, alla hello ytterligare parametrar som krävs av hello `azure vm create` kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="94ccf-200">You still need toospecify, or answer prompts for, all hello additional parameters required by hello `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="94ccf-201">Läs mer om hello [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="94ccf-201">Read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

### <a name="create-a-vm-using-a-json-template"></a><span data-ttu-id="94ccf-202">Skapa en virtuell dator med en JSON-mall</span><span class="sxs-lookup"><span data-stu-id="94ccf-202">Create a VM using a JSON template</span></span>
<span data-ttu-id="94ccf-203">Azure Resource Manager-mallar är JavaScript Object Notation (JSON) filer som definierar hello-miljö som du vill toobuild.</span><span class="sxs-lookup"><span data-stu-id="94ccf-203">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define hello environment you wish toobuild.</span></span> <span data-ttu-id="94ccf-204">hello mallar är uppdelade i toodifferent resursproviders, till exempel beräkning eller nätverket.</span><span class="sxs-lookup"><span data-stu-id="94ccf-204">hello templates are broken down in toodifferent resource providers such as compute or network.</span></span> <span data-ttu-id="94ccf-205">Du kan använda befintliga mallar eller skriva egna.</span><span class="sxs-lookup"><span data-stu-id="94ccf-205">You can use existing templates or write your own.</span></span> <span data-ttu-id="94ccf-206">Läs mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="94ccf-206">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="94ccf-207">Inom hello `Microsoft.Compute/virtualMachines` providern av mallen som du har en `storageProfile` nod som innehåller hello konfigurationsinformation för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="94ccf-207">Within hello `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains hello configuration details for your VM.</span></span> <span data-ttu-id="94ccf-208">hello två huvudsakliga parametrar tooedit är hello `image` och `vhd` URI: er som tooyour anpassade diskavbildning och den nya Virtuella datorns virtuella disken.</span><span class="sxs-lookup"><span data-stu-id="94ccf-208">hello two main parameters tooedit are hello `image` and `vhd` URIs that point tooyour custom disk image and your new VM's virtual disk.</span></span> <span data-ttu-id="94ccf-209">hello nedan visar ett exempel på hello JSON för att använda en anpassad avbildning:</span><span class="sxs-lookup"><span data-stu-id="94ccf-209">hello following shows an example of hello JSON for using a custom disk image:</span></span>

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

<span data-ttu-id="94ccf-210">Du kan använda [detta befintliga mallen toocreate en virtuell dator från en anpassad avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) eller läser du om [skapa egna mallar för Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="94ccf-210">You can use [this existing template toocreate a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="94ccf-211">När du har en mall kan du skapa dina virtuella datorer med hjälp av hello `azure group deployment create` kommando.</span><span class="sxs-lookup"><span data-stu-id="94ccf-211">Once you have a template configured, you create your VMs using hello `azure group deployment create` command.</span></span> <span data-ttu-id="94ccf-212">Ange hello URI för JSON-mall med hello `--template-uri` parameter:</span><span class="sxs-lookup"><span data-stu-id="94ccf-212">Specify hello URI of your JSON template with hello `--template-uri` parameter:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="94ccf-213">Om du har en JSON-fil som lagras lokalt på datorn, kan du använda hello `--template-file` parameter i stället:</span><span class="sxs-lookup"><span data-stu-id="94ccf-213">If you have a JSON file stored locally on your computer, you can use hello `--template-file` parameter instead:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="94ccf-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94ccf-214">Next steps</span></span>
<span data-ttu-id="94ccf-215">När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="94ccf-215">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="94ccf-216">Du kan också för[lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="94ccf-216">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="94ccf-217">Om du har program som körs på din virtuella dator som du behöver tooaccess för[öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="94ccf-217">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

