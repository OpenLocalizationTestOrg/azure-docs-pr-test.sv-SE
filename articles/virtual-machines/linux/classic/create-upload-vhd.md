---
title: aaaCreate och ladda upp en Linux VHD-tooAzure | Microsoft Docs
description: "Skapa och ladda upp en Azure virtuell hårddisk (VHD) som innehåller hello Linux-operativsystem med hjälp av hello klassiska distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: iainfou
ms.openlocfilehash: 77b01316386c4a6eb68c129fa68d42f0a8996edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-hello-linux-operating-system"></a><span data-ttu-id="8b802-103">Skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem</span><span class="sxs-lookup"><span data-stu-id="8b802-103">Creating and Uploading a Virtual Hard Disk that Contains hello Linux Operating System</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="8b802-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="8b802-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="8b802-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8b802-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="8b802-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="8b802-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="8b802-107">Du kan också [ladda upp en anpassad avbildning med Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8b802-107">You can also [upload a custom disk image using Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="8b802-108">Den här artikeln visar hur toocreate och ladda upp en virtuell hårddisk (VHD) så att du kan använda den som din egen avbildning toocreate virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="8b802-108">This article shows you how toocreate and upload a virtual hard disk (VHD) so you can use it as your own image toocreate virtual machines in Azure.</span></span> <span data-ttu-id="8b802-109">Lär dig hur tooprepare hello-operativsystemet så att du kan använda den toocreate flera virtuella datorer baserat på avbildningen.</span><span class="sxs-lookup"><span data-stu-id="8b802-109">Learn how tooprepare hello operating system so you can use it toocreate multiple virtual machines based on that image.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="8b802-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8b802-110">Prerequisites</span></span>
<span data-ttu-id="8b802-111">Den här artikeln förutsätter att du har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8b802-111">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="8b802-112">**Linux-operativsystem i en VHD-fil** -du har installerat en [Azure-godkända Linux-distribution](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuell disk i hello VHD-format.</span><span class="sxs-lookup"><span data-stu-id="8b802-112">**Linux operating system installed in a .vhd file** - You have installed an [Azure-endorsed Linux distribution](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="8b802-113">Flera verktyg finns toocreate en virtuell dator och virtuell Hårddisk:</span><span class="sxs-lookup"><span data-stu-id="8b802-113">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="8b802-114">Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), sköter toouse VHD som bildformat.</span><span class="sxs-lookup"><span data-stu-id="8b802-114">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="8b802-115">Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="8b802-115">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="8b802-116">Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b802-116">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="8b802-117">hello senare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="8b802-117">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="8b802-118">När du skapar en virtuell dator kan du ange VHD som hello-format.</span><span class="sxs-lookup"><span data-stu-id="8b802-118">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="8b802-119">Om det behövs kan du konvertera VHDX diskar tooVHD med [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8b802-119">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="8b802-120">Dessutom stöder Azure inte uppladdning dynamiska virtuella hårddiskar, så du måste tooconvert sådana diskar toostatic virtuella hårddiskar innan du laddar upp.</span><span class="sxs-lookup"><span data-stu-id="8b802-120">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="8b802-121">Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamiska diskar under hello processen överför tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8b802-121">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>

* <span data-ttu-id="8b802-122">**Azure-kommandoradsgränssnittet** -installera hello senaste [Azure-kommandoradsgränssnittet](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello VHD.</span><span class="sxs-lookup"><span data-stu-id="8b802-122">**Azure Command-line Interface** - Install hello latest [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello VHD.</span></span>

<span data-ttu-id="8b802-123"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="8b802-123"><a id="prepimage"> </a></span></span>

## <a name="step-1-prepare-hello-image-toobe-uploaded"></a><span data-ttu-id="8b802-124">Steg 1: Förbered hello avbildningen toobe har överförts</span><span class="sxs-lookup"><span data-stu-id="8b802-124">Step 1: Prepare hello image toobe uploaded</span></span>
<span data-ttu-id="8b802-125">Azure stöder olika Linux-distributioner (se [godkända distributioner](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="8b802-125">Azure supports various Linux distributions (see [Endorsed Distributions](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="8b802-126">hello följande artiklar leder dig igenom hur tooprepare hello olika Linux-distributioner som stöds på Azure.</span><span class="sxs-lookup"><span data-stu-id="8b802-126">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure.</span></span> <span data-ttu-id="8b802-127">När du har slutfört hello stegen i följande guider hello du återvända hit när du har en VHD-fil som är redo tooupload tooAzure:</span><span class="sxs-lookup"><span data-stu-id="8b802-127">After you complete hello steps in hello following guides, come back here once you have a VHD file that is ready tooupload tooAzure:</span></span>

* <span data-ttu-id="8b802-128">**[CentOS-baserade distributioner](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="8b802-128">**[CentOS-based Distributions](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="8b802-129">**[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="8b802-129">**[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="8b802-130">**[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="8b802-130">**[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="8b802-131">**[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="8b802-131">**[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="8b802-132">**[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="8b802-132">**[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="8b802-133">**[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="8b802-133">**[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="8b802-134">**[Andra - icke-godkända distributioner](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="8b802-134">**[Other - Non-Endorsed Distributions](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

> [!NOTE]
> <span data-ttu-id="8b802-135">hello Azure-plattformen SLA gäller toovirtual datorer som kör Linux OS endast när en hello-godkända distributioner används med hello konfigurationsinformation som anges under stöds versioner hello [Linux på Azure-Endorsed distributioner ](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8b802-135">hello Azure platform SLA applies toovirtual machines running hello Linux OS only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="8b802-136">Alla Linux-distributioner i hello Azure-avbildning galleriet är påtecknade distributioner med hello nödvändig konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8b802-136">All Linux distributions in hello Azure image gallery are endorsed distributions with hello required configuration.</span></span>
> 
> 

<span data-ttu-id="8b802-137">Se även hello  **[Linux installationsinformation](../create-upload-generic.md#general-linux-installation-notes)**  mer allmänna tips om hur du förbereder Linux avbildningar för Azure.</span><span class="sxs-lookup"><span data-stu-id="8b802-137">Also see hello **[Linux Installation Notes](../create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

<span data-ttu-id="8b802-138"><a id="connect"> </a></span><span class="sxs-lookup"><span data-stu-id="8b802-138"><a id="connect"> </a></span></span>

## <a name="step-2-prepare-hello-connection-tooazure"></a><span data-ttu-id="8b802-139">Steg 2: Förbered hello anslutning tooAzure</span><span class="sxs-lookup"><span data-stu-id="8b802-139">Step 2: Prepare hello connection tooAzure</span></span>
<span data-ttu-id="8b802-140">Kontrollera att du använder hello Azure CLI i hello klassiska distributionsmodellen (`azure config mode asm`), logga sedan in tooyour konto:</span><span class="sxs-lookup"><span data-stu-id="8b802-140">Make sure you are using hello Azure CLI in hello classic deployment model (`azure config mode asm`), then log in tooyour account:</span></span>

```azurecli
azure login
```


<span data-ttu-id="8b802-141"><a id="upload"> </a></span><span class="sxs-lookup"><span data-stu-id="8b802-141"><a id="upload"> </a></span></span>

## <a name="step-3-upload-hello-image-tooazure"></a><span data-ttu-id="8b802-142">Steg 3: Överför hello bilden tooAzure</span><span class="sxs-lookup"><span data-stu-id="8b802-142">Step 3: Upload hello image tooAzure</span></span>
<span data-ttu-id="8b802-143">Du behöver en storage-konto tooupload VHD-filen till.</span><span class="sxs-lookup"><span data-stu-id="8b802-143">You need a storage account tooupload your VHD file to.</span></span> <span data-ttu-id="8b802-144">Du kan antingen välja ett befintligt lagringskonto eller [skapa en ny](../../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="8b802-144">You can either pick an existing storage account or [create a new one](../../../storage/common/storage-create-storage-account.md).</span></span>

<span data-ttu-id="8b802-145">Använd hello Azure CLI tooupload hello avbildningen med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8b802-145">Use hello Azure CLI tooupload hello image by using hello following command:</span></span>

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

<span data-ttu-id="8b802-146">I hello förra exemplet:</span><span class="sxs-lookup"><span data-stu-id="8b802-146">In hello previous example:</span></span>

* <span data-ttu-id="8b802-147">**BlobStorageURL** är hello URL för hello storage-konto som du planerar toouse</span><span class="sxs-lookup"><span data-stu-id="8b802-147">**BlobStorageURL** is hello URL for hello storage account you plan toouse</span></span>
* <span data-ttu-id="8b802-148">**YourImagesFolder** är hello behållare i blobblagring dit toostore bilderna</span><span class="sxs-lookup"><span data-stu-id="8b802-148">**YourImagesFolder** is hello container within blob storage where you want toostore your images</span></span>
* <span data-ttu-id="8b802-149">**VHDName** är hello etiketten som visas i portalen tooidentify hello virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="8b802-149">**VHDName** is hello label that appears in portal tooidentify hello virtual hard disk.</span></span>
* <span data-ttu-id="8b802-150">**PathToVHDFile** hello sökvägen och namnet på hello VHD-filen på din dator.</span><span class="sxs-lookup"><span data-stu-id="8b802-150">**PathToVHDFile** is hello full path and name of hello .vhd file on your machine.</span></span>

<span data-ttu-id="8b802-151">hello följande kommando visar en komplett exempel:</span><span class="sxs-lookup"><span data-stu-id="8b802-151">hello following command shows a complete example:</span></span>

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-hello-image"></a><span data-ttu-id="8b802-152">Steg 4: Skapa en virtuell dator från hello avbildning</span><span class="sxs-lookup"><span data-stu-id="8b802-152">Step 4: Create a VM from hello image</span></span>
<span data-ttu-id="8b802-153">Du skapar en virtuell dator med hjälp av `azure vm create` i hello samma sätt som vanliga virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8b802-153">You create a VM using `azure vm create` in hello same way as a regular VM.</span></span> <span data-ttu-id="8b802-154">Ange hello namn du gav avbildningen i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="8b802-154">Specify hello name you gave your image in hello previous step.</span></span> <span data-ttu-id="8b802-155">I följande exempel hello, vi använda hello **myImage** avbildningsnamn i hello föregående steg:</span><span class="sxs-lookup"><span data-stu-id="8b802-155">In hello following example, we use hello **myImage** image name given in hello previous step:</span></span>

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

<span data-ttu-id="8b802-156">toocreate egna virtuella datorer, ange egna användarnamn + lösenord, plats, DNS-namn och namn.</span><span class="sxs-lookup"><span data-stu-id="8b802-156">toocreate your own VMs, provide your own username + password, location, DNS name, and image name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b802-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b802-157">Next steps</span></span>
<span data-ttu-id="8b802-158">Mer information finns i [Azure CLI-referens för hello Azure klassiska distributionsmodellen](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="8b802-158">For more information, see [Azure CLI reference for hello Azure classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

[Step 1: Prepare hello image toobe uploaded]:#prepimage
[Step 2: Prepare hello connection tooAzure]:#connect
[Step 3: Upload hello image tooAzure]:#upload
