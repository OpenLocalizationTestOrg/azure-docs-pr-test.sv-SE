---
title: Ladda upp en anpassad avbildning Linux med Azure CLI 1.0 | Microsoft Docs
description: "Skapa och ladda upp en virtuell hårddisk (VHD) till Azure med en anpassad avbildning för Linux med hjälp av Resource Manager-distributionsmodellen och Azure CLI 1.0."
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
ms.openlocfilehash: ca4c6cb9296028275b2b032af0c94baabeec1223
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-the-azure-cli-10"></a><span data-ttu-id="19728-103">Ladda upp och skapa en Linux VM från anpassad avbildning med hjälp av Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="19728-103">Upload and create a Linux VM from custom disk image by using the Azure CLI 1.0</span></span>
<span data-ttu-id="19728-104">Den här artikeln visar hur du överför en virtuell hårddisk (VHD) till Azure med hjälp av Resource Manager-distributionsmodellen och skapar virtuella Linux-datorer från den här anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="19728-104">This article shows you how to upload a virtual hard disk (VHD) to Azure using the Resource Manager deployment model and create Linux VMs from this custom image.</span></span> <span data-ttu-id="19728-105">Den här funktionen kan du installera och konfigurera en Linux-distro enligt dina behov och sedan använda den virtuella Hårddisken för att snabbt skapa virtuella Azure-datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="19728-105">This functionality allows you to install and configure a Linux distro to your requirements and then use that VHD to quickly create Azure virtual machines (VMs).</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="19728-106">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="19728-106">CLI versions to complete the task</span></span>
<span data-ttu-id="19728-107">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="19728-107">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="19728-108">[Azure CLI 1.0](#quick-commands) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="19728-108">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="19728-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="19728-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="19728-110">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="19728-110">Quick commands</span></span>
<span data-ttu-id="19728-111">Om du behöver utföra aktiviteten följande avsnittet beskriver grundläggande kommandon för att överföra en virtuell dator till Azure.</span><span class="sxs-lookup"><span data-stu-id="19728-111">If you need to quickly accomplish the task, the following section details the base commands to upload a VM to Azure.</span></span> <span data-ttu-id="19728-112">Mer detaljerad information och kontext för varje steg finns resten av dokumentet [startar här](#requirements).</span><span class="sxs-lookup"><span data-stu-id="19728-112">More detailed information and context for each step can be found the rest of the document, [starting here](#requirements).</span></span>

<span data-ttu-id="19728-113">Se till att du har [Azure CLI 1.0](../../cli-install-nodejs.md) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="19728-113">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="19728-114">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="19728-114">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="19728-115">Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `myimages`.</span><span class="sxs-lookup"><span data-stu-id="19728-115">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="19728-116">Först skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="19728-116">First, create a resource group.</span></span> <span data-ttu-id="19728-117">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `WestUs` plats:</span><span class="sxs-lookup"><span data-stu-id="19728-117">The following example creates a resource group named `myResourceGroup` in the `WestUs` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

<span data-ttu-id="19728-118">Skapa ett lagringskonto för att lagra dina virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="19728-118">Create a storage account to hold your virtual disks.</span></span> <span data-ttu-id="19728-119">I följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="19728-119">The following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

<span data-ttu-id="19728-120">Lista över åtkomstnycklarna för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="19728-120">List the access keys for your storage account.</span></span> <span data-ttu-id="19728-121">Anteckna `key1`:</span><span class="sxs-lookup"><span data-stu-id="19728-121">Make a note of `key1`:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="19728-122">Skapa en behållare i ditt lagringskonto med hjälp av lagringsnyckeln som du har köpt.</span><span class="sxs-lookup"><span data-stu-id="19728-122">Create a container within your storage account using the storage key you obtained.</span></span> <span data-ttu-id="19728-123">I följande exempel skapas en behållare med namnet `myimages` med lagring nyckelvärdet från `key1`:</span><span class="sxs-lookup"><span data-stu-id="19728-123">The following example creates a container named `myimages` using the storage key value from `key1`:</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

<span data-ttu-id="19728-124">Slutligen överför den virtuella Hårddisken till den behållare som du skapade.</span><span class="sxs-lookup"><span data-stu-id="19728-124">Finally, upload your VHD to the container you created.</span></span> <span data-ttu-id="19728-125">Ange den lokala sökvägen till den virtuella Hårddisken under `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="19728-125">Specify the local path to your VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

<span data-ttu-id="19728-126">Nu kan du skapa en virtuell dator från den överförda virtuella hårddisken [med hjälp av en Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="19728-126">You can now create a VM from your uploaded virtual disk [using a Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span></span> <span data-ttu-id="19728-127">Du kan också använda CLI genom att ange URI: N till hårddisken (`--image-urn`).</span><span class="sxs-lookup"><span data-stu-id="19728-127">You can also use the CLI by specifying the URI to your disk (`--image-urn`).</span></span> <span data-ttu-id="19728-128">I följande exempel skapas en virtuell dator med namnet `myVM` med hjälp av den virtuella disken överfört tidigare:</span><span class="sxs-lookup"><span data-stu-id="19728-128">The following example creates a VM named `myVM` using the virtual disk previously uploaded:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

<span data-ttu-id="19728-129">Mål-lagringskontot måste vara samma som om du har överfört din virtuell disk till.</span><span class="sxs-lookup"><span data-stu-id="19728-129">The destination storage account has to be the same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="19728-130">Du måste också ange eller svar efterfrågar alla ytterligare parametrar som krävs av den `azure vm create` kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="19728-130">You also need to specify, or answer prompts for, all the additional parameters required by the `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="19728-131">Du kan läsa mer om den [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="19728-131">You can read more about the [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="19728-132">Krav</span><span class="sxs-lookup"><span data-stu-id="19728-132">Requirements</span></span>
<span data-ttu-id="19728-133">Du behöver följande för att slutföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="19728-133">To complete the following steps, you need:</span></span>

* <span data-ttu-id="19728-134">**Linux-operativsystem i en VHD-fil** -installera en [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) till en virtuell disk i VHD-format.</span><span class="sxs-lookup"><span data-stu-id="19728-134">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) to a virtual disk in the VHD format.</span></span> <span data-ttu-id="19728-135">Det finns flera verktyg för att skapa en virtuell dator och virtuell Hårddisk:</span><span class="sxs-lookup"><span data-stu-id="19728-135">Multiple tools exist to create a VM and VHD:</span></span>
  * <span data-ttu-id="19728-136">Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), och ser till att använda VHD som bildformat.</span><span class="sxs-lookup"><span data-stu-id="19728-136">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care to use VHD as your image format.</span></span> <span data-ttu-id="19728-137">Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="19728-137">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="19728-138">Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="19728-138">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="19728-139">Nyare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="19728-139">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="19728-140">När du skapar en virtuell dator kan du ange VHD som formatet.</span><span class="sxs-lookup"><span data-stu-id="19728-140">When you create a VM, specify VHD as the format.</span></span> <span data-ttu-id="19728-141">Om det behövs, du kan konvertera VHDX-diskar på VHD med [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="19728-141">If needed, you can convert VHDX disks to VHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or the [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="19728-142">Dessutom stöder inte Azure överför dynamiska virtuella hårddiskar, så du måste konvertera dessa diskar till statiska virtuella hårddiskar innan du laddar upp.</span><span class="sxs-lookup"><span data-stu-id="19728-142">Further, Azure does not support uploading dynamic VHDs, so you need to convert such disks to static VHDs before uploading.</span></span> <span data-ttu-id="19728-143">Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) att konvertera dynamiska diskar under processen med att ladda upp till Azure.</span><span class="sxs-lookup"><span data-stu-id="19728-143">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) to convert dynamic disks during the process of uploading to Azure.</span></span>
> 
> 

* <span data-ttu-id="19728-144">Virtuella datorer skapas från den anpassade avbildningen måste finnas i samma lagringskonto som själva bilden</span><span class="sxs-lookup"><span data-stu-id="19728-144">VMs created from your custom image must reside in the same storage account as the image itself</span></span>
  * <span data-ttu-id="19728-145">Skapa ett lagringskonto och en behållare för både dina anpassade avbildningen och skapade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="19728-145">Create a storage account and container to hold both your custom image and created VMs</span></span>
  * <span data-ttu-id="19728-146">När du har skapat din virtuella dator, kan du ta bort bilden</span><span class="sxs-lookup"><span data-stu-id="19728-146">After you have created all your VMs, you can safely delete your image</span></span>

<span data-ttu-id="19728-147">Se till att du har [Azure CLI 1.0](../../cli-install-nodejs.md) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="19728-147">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="19728-148">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="19728-148">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="19728-149">Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `myimages`.</span><span class="sxs-lookup"><span data-stu-id="19728-149">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="19728-150"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="19728-150"><a id="prepimage"> </a></span></span>

## <a name="prepare-the-image-to-be-uploaded"></a><span data-ttu-id="19728-151">Förbereda avbildningen som skulle överföras</span><span class="sxs-lookup"><span data-stu-id="19728-151">Prepare the image to be uploaded</span></span>
<span data-ttu-id="19728-152">Azure stöder olika Linux-distributioner (se [godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="19728-152">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="19728-153">I följande artiklar när du går igenom hur du förbereder de olika Linux-distributioner som stöds i Azure:</span><span class="sxs-lookup"><span data-stu-id="19728-153">The following articles guide you through how to prepare the various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="19728-154">**[CentOS-baserade distributioner](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="19728-154">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="19728-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="19728-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="19728-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="19728-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="19728-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="19728-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="19728-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="19728-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="19728-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="19728-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="19728-160">**[Andra - icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="19728-160">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="19728-161">Se även den  **[Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes)**  mer allmänna tips om hur du förbereder Linux avbildningar för Azure.</span><span class="sxs-lookup"><span data-stu-id="19728-161">Also see the **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="19728-162">Den [Azure-plattformen SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gäller för virtuella datorer som kör Linux endast när en av de påtecknade distributioner används med konfigurationsinformation som anges under stöds versioner i [Linux på Azure-Endorsed distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19728-162">The [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies to VMs running Linux only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="create-a-resource-group"></a><span data-ttu-id="19728-163">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="19728-163">Create a resource group</span></span>
<span data-ttu-id="19728-164">Resursgrupper samordnar logiskt alla Azure-resurser som stöder virtuella datorer, till exempel virtuella nätverk och lagring.</span><span class="sxs-lookup"><span data-stu-id="19728-164">Resource groups logically bring together all the Azure resources to support your virtual machines, such as the virtual networking and storage.</span></span> <span data-ttu-id="19728-165">Läs mer om [Azure resursgrupper här](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="19728-165">Read more about [Azure resource groups here](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="19728-166">Innan du överför din anpassade diskavbildning och skapa virtuella datorer, måste du först skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="19728-166">Before uploading your custom disk image and creating VMs, you first need to create a resource group.</span></span> 

<span data-ttu-id="19728-167">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `WestUS` plats:</span><span class="sxs-lookup"><span data-stu-id="19728-167">The following example creates a resource group named `myResourceGroup` in the `WestUS` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="19728-168">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="19728-168">Create a storage account</span></span>
<span data-ttu-id="19728-169">Virtuella datorer lagras som sidblobbar i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="19728-169">VMs are stored as page blobs within a storage account.</span></span> <span data-ttu-id="19728-170">Läs mer om [Azure-blobblagring här](../../storage/common/storage-introduction.md#blob-storage).</span><span class="sxs-lookup"><span data-stu-id="19728-170">Read more about [Azure blob storage here](../../storage/common/storage-introduction.md#blob-storage).</span></span> <span data-ttu-id="19728-171">Du skapar ett lagringskonto för dina anpassade diskavbildning och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="19728-171">You create a storage account for your custom disk image and VMs.</span></span> <span data-ttu-id="19728-172">Virtuella datorer som du skapar från din anpassade diskavbildning måste vara i samma lagringskonto som den bilden.</span><span class="sxs-lookup"><span data-stu-id="19728-172">Any VMs that you create from your custom disk image need to be in the same storage account as that image.</span></span>

<span data-ttu-id="19728-173">I följande exempel skapas ett lagringskonto med namnet `mystorageaccount` i resursgruppen som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="19728-173">The following example creates a storage account named `mystorageaccount` in the resource group previously created:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="19728-174">Lista nycklar för lagringskonto</span><span class="sxs-lookup"><span data-stu-id="19728-174">List storage account keys</span></span>
<span data-ttu-id="19728-175">Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure.</span><span class="sxs-lookup"><span data-stu-id="19728-175">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="19728-176">Åtkomstnycklarna används vid autentisering till lagringskontot, såsom för att utföra skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="19728-176">These access keys are used when authenticating to the storage account, such as to carry out write operations.</span></span> <span data-ttu-id="19728-177">Läs mer om [hantera åtkomst till lagring här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="19728-177">Read more about [managing access to storage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="19728-178">Du kan visa snabbtangenter med den `azure storage account keys list` kommando.</span><span class="sxs-lookup"><span data-stu-id="19728-178">You can view access keys with the `azure storage account keys list` command.</span></span>

<span data-ttu-id="19728-179">Visa åtkomstnycklar för lagringskontot som du skapade:</span><span class="sxs-lookup"><span data-stu-id="19728-179">View the access keys for the storage account you created:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="19728-180">Utdatan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="19728-180">The output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
<span data-ttu-id="19728-181">Anteckna `key1` eftersom du ska använda för att interagera med ditt lagringskonto i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="19728-181">Make a note of `key1` as you will use it to interact with your storage account in the next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="19728-182">Skapa en lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="19728-182">Create a storage container</span></span>
<span data-ttu-id="19728-183">På samma sätt som du skapar olika kataloger för att organisera logiskt det lokala filsystemet, kan du skapa behållare i ett lagringskonto för att organisera dina virtuella diskar och bilder.</span><span class="sxs-lookup"><span data-stu-id="19728-183">In the same way that you create different directories to logically organize your local file system, you create containers within a storage account to organize your virtual disks and images.</span></span> <span data-ttu-id="19728-184">Ett lagringskonto kan innehålla valfritt antal behållare.</span><span class="sxs-lookup"><span data-stu-id="19728-184">A storage account can contain any number of containers.</span></span> 

<span data-ttu-id="19728-185">I följande exempel skapas en behållare med namnet `myimages`, ange snabbtangent hämtades i föregående steg (`key1`):</span><span class="sxs-lookup"><span data-stu-id="19728-185">The following example creates a container named `myimages`, specifying the access key obtained in the previous step (`key1`):</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a><span data-ttu-id="19728-186">Överför VHD</span><span class="sxs-lookup"><span data-stu-id="19728-186">Upload VHD</span></span>
<span data-ttu-id="19728-187">Nu kan du faktiskt överföra din egen avbildning.</span><span class="sxs-lookup"><span data-stu-id="19728-187">Now you can actually upload your custom disk image.</span></span> <span data-ttu-id="19728-188">Som med alla virtuella diskar som används av virtuella datorer kan du ladda upp och lagra din egen avbildning som en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="19728-188">As with all virtual disks used by VMs, you upload and store your custom disk image as a page blob.</span></span>

<span data-ttu-id="19728-189">Ange din snabbtangent, den behållare som du skapade i föregående steg och sökvägen till den anpassade diskavbildning på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="19728-189">Specify your access key, the container you created in the previous step, and then the path to the custom disk image on your local computer:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a><span data-ttu-id="19728-190">Skapa virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="19728-190">Create VM from custom image</span></span>
<span data-ttu-id="19728-191">När du skapar virtuella datorer från din anpassade diskavbildning ange URI till disk image.</span><span class="sxs-lookup"><span data-stu-id="19728-191">When you create VMs from your custom disk image, specify the URI to the disk image.</span></span> <span data-ttu-id="19728-192">Se till att målet lagring kontot matchar där din anpassade diskavbildning lagras.</span><span class="sxs-lookup"><span data-stu-id="19728-192">Ensure that the destination storage account matches where your custom disk image is stored.</span></span> <span data-ttu-id="19728-193">Du kan skapa den virtuella datorn med hjälp av Azure CLI eller Resource Manager JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="19728-193">You can create your VM using the Azure CLI or Resource Manager JSON template.</span></span>

### <a name="create-a-vm-using-the-azure-cli"></a><span data-ttu-id="19728-194">Skapa en virtuell dator med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="19728-194">Create a VM using the Azure CLI</span></span>
<span data-ttu-id="19728-195">Du anger den `--image-urn` parameter med det `azure vm create` kommando för att peka på din egen avbildning.</span><span class="sxs-lookup"><span data-stu-id="19728-195">You specify the `--image-urn` parameter with the `azure vm create` command to point to your custom disk image.</span></span> <span data-ttu-id="19728-196">Se till att `--storage-account-name` matchar det lagringskonto där din anpassade diskavbildning lagras.</span><span class="sxs-lookup"><span data-stu-id="19728-196">Ensure that `--storage-account-name` matches the storage account where your custom disk image is stored.</span></span> <span data-ttu-id="19728-197">Du behöver inte använda samma behållare som anpassade disk image för att lagra dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="19728-197">You do not have to use the same container as the custom disk image to store your VMs.</span></span> <span data-ttu-id="19728-198">Se till att skapa några ytterligare behållare på samma sätt som de föregående stegen innan du laddar upp din anpassade diskavbildningar.</span><span class="sxs-lookup"><span data-stu-id="19728-198">Make sure to create any additional containers in the same way as the earlier steps before uploading your custom disk images.</span></span>

<span data-ttu-id="19728-199">I följande exempel skapas en virtuell dator med namnet `myVM` från din egen avbildning:</span><span class="sxs-lookup"><span data-stu-id="19728-199">The following example creates a VM named `myVM` from your custom disk image:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

<span data-ttu-id="19728-200">Du måste ange eller svar efterfrågar alla ytterligare parametrar som krävs av den `azure vm create` kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="19728-200">You still need to specify, or answer prompts for, all the additional parameters required by the `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="19728-201">Läs mer om den [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="19728-201">Read more about the [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

### <a name="create-a-vm-using-a-json-template"></a><span data-ttu-id="19728-202">Skapa en virtuell dator med en JSON-mall</span><span class="sxs-lookup"><span data-stu-id="19728-202">Create a VM using a JSON template</span></span>
<span data-ttu-id="19728-203">Azure Resource Manager-mallar är JavaScript Object Notation (JSON) filer som definierar den miljö du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="19728-203">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define the environment you wish to build.</span></span> <span data-ttu-id="19728-204">Mallarna är uppdelade att olika resursproviders, till exempel beräkning eller nätverket.</span><span class="sxs-lookup"><span data-stu-id="19728-204">The templates are broken down in to different resource providers such as compute or network.</span></span> <span data-ttu-id="19728-205">Du kan använda befintliga mallar eller skriva egna.</span><span class="sxs-lookup"><span data-stu-id="19728-205">You can use existing templates or write your own.</span></span> <span data-ttu-id="19728-206">Läs mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="19728-206">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="19728-207">I den `Microsoft.Compute/virtualMachines` providern av mallen som du har en `storageProfile` nod som innehåller konfigurationsinformation för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="19728-207">Within the `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains the configuration details for your VM.</span></span> <span data-ttu-id="19728-208">Så här redigerar du två huvudsakliga parametrarna är den `image` och `vhd` URI: er som pekar på din egen avbildning och din nya VM virtuell disk.</span><span class="sxs-lookup"><span data-stu-id="19728-208">The two main parameters to edit are the `image` and `vhd` URIs that point to your custom disk image and your new VM's virtual disk.</span></span> <span data-ttu-id="19728-209">Nedan visas ett exempel på JSON för att använda en anpassad avbildning:</span><span class="sxs-lookup"><span data-stu-id="19728-209">The following shows an example of the JSON for using a custom disk image:</span></span>

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

<span data-ttu-id="19728-210">Du kan använda [denna befintlig mall för att skapa en virtuell dator från en anpassad avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) eller läser du om [skapa egna mallar för Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="19728-210">You can use [this existing template to create a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="19728-211">När du har en mall kan du skapa dina virtuella datorer med hjälp av den `azure group deployment create` kommando.</span><span class="sxs-lookup"><span data-stu-id="19728-211">Once you have a template configured, you create your VMs using the `azure group deployment create` command.</span></span> <span data-ttu-id="19728-212">Ange URI för JSON-mall med det `--template-uri` parameter:</span><span class="sxs-lookup"><span data-stu-id="19728-212">Specify the URI of your JSON template with the `--template-uri` parameter:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="19728-213">Om du har en JSON-fil som lagras lokalt på datorn, kan du använda den `--template-file` parameter i stället:</span><span class="sxs-lookup"><span data-stu-id="19728-213">If you have a JSON file stored locally on your computer, you can use the `--template-file` parameter instead:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="19728-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="19728-214">Next steps</span></span>
<span data-ttu-id="19728-215">När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="19728-215">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="19728-216">Du kan också [lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) till din nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="19728-216">You may also want to [add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to your new VMs.</span></span> <span data-ttu-id="19728-217">Om du har program som körs på din virtuella dator som du behöver åtkomst till [öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19728-217">If you have applications running on your VMs that you need to access, be sure to [open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

