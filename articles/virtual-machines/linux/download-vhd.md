---
title: "aaaDownload en Linux VHD från Azure | Microsoft Docs"
description: "Hämta en Linux VHD med hello Azure CLI och hello Azure-portalen."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: 7e08e985a64a6be581b8f5eedcce60fbd314eaf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-linux-vhd-from-azure"></a><span data-ttu-id="e0f38-103">Hämta en Linux-VHD från Azure</span><span class="sxs-lookup"><span data-stu-id="e0f38-103">Download a Linux VHD from Azure</span></span>

<span data-ttu-id="e0f38-104">I den här artikeln får du lära dig hur toodownload en [Linux virtuell hårddisk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) filen från Azure med hjälp av hello Azure CLI och Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0f38-104">In this article, you learn how toodownload a [Linux virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) file from Azure using hello Azure CLI and Azure portal.</span></span> 

<span data-ttu-id="e0f38-105">Virtuella datorer (VM) i Azure används [diskar](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som plats-toostore ett operativsystem, program och data.</span><span class="sxs-lookup"><span data-stu-id="e0f38-105">Virtual machines (VMs) in Azure use [disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="e0f38-106">Alla virtuella Azure-datorer har minst två diskar – en disk i Windows-operativsystem och en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="e0f38-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="e0f38-107">hello operativsystemdisk har skapats ursprungligen från en avbildning och både hello operativsystemdisken och hello avbildningen är virtuella hårddiskar som lagras i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e0f38-107">hello operating system disk is initially created from an image, and both hello operating system disk and hello image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="e0f38-108">Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="e0f38-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

<span data-ttu-id="e0f38-109">Om du inte redan har gjort det installerar [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="e0f38-109">If you haven't already done so, install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>

## <a name="stop-hello-vm"></a><span data-ttu-id="e0f38-110">Stoppa hello VM</span><span class="sxs-lookup"><span data-stu-id="e0f38-110">Stop hello VM</span></span>

<span data-ttu-id="e0f38-111">En virtuell Hårddisk kan inte hämtas från Azure om den är ansluten tooa använder VM.</span><span class="sxs-lookup"><span data-stu-id="e0f38-111">A VHD can’t be downloaded from Azure if it's attached tooa running VM.</span></span> <span data-ttu-id="e0f38-112">Du måste toostop hello VM toodownload en virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="e0f38-112">You need toostop hello VM toodownload a VHD.</span></span> <span data-ttu-id="e0f38-113">Om du vill toouse en virtuell Hårddisk som en [bild](tutorial-custom-images.md) toocreate andra virtuella datorer med nya diskar du behöver toodeprovision och generalisera hello operativsystem finns i hello filen och stoppa hello VM.</span><span class="sxs-lookup"><span data-stu-id="e0f38-113">If you want toouse a VHD as an [image](tutorial-custom-images.md) toocreate other VMs with new disks, you need toodeprovision and generalize hello operating system contained in hello file and stop hello VM.</span></span> <span data-ttu-id="e0f38-114">toouse hello VHD som en disk för en ny instans av en befintlig virtuell dator eller en datadisk du bara behöver toostop och frigöra hello VM.</span><span class="sxs-lookup"><span data-stu-id="e0f38-114">toouse hello VHD as a disk for a new instance of an existing VM or data disk, you only need toostop and deallocate hello VM.</span></span>

<span data-ttu-id="e0f38-115">toouse Hej VHD som en bild toocreate andra virtuella datorer, gör följande:</span><span class="sxs-lookup"><span data-stu-id="e0f38-115">toouse hello VHD as an image toocreate other VMs, complete these steps:</span></span>

1. <span data-ttu-id="e0f38-116">Använda SSH, hello kontonamn och hello offentliga IP-adress hello VM tooconnect tooit och ta bort etableringen av den.</span><span class="sxs-lookup"><span data-stu-id="e0f38-116">Use SSH, hello account name, and hello public IP address of hello VM tooconnect tooit and deprovision it.</span></span> <span data-ttu-id="e0f38-117">hello + användaren parametern tar också bort hello senaste etablerade användarkonto.</span><span class="sxs-lookup"><span data-stu-id="e0f38-117">hello +user parameter also removes hello last provisioned user account.</span></span> <span data-ttu-id="e0f38-118">Om du gräddning kontoautentiseringsuppgifter toohello VM, lämnar ut detta + user-parameter.</span><span class="sxs-lookup"><span data-stu-id="e0f38-118">If you are baking account credentials in toohello VM, leave out this +user parameter.</span></span> <span data-ttu-id="e0f38-119">hello följande exempel tar bort hello senaste etablerade användarkonto:</span><span class="sxs-lookup"><span data-stu-id="e0f38-119">hello following example removes hello last provisioned user account:</span></span>

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. <span data-ttu-id="e0f38-120">Logga in tooyour Azure-konto med [az inloggningen](https://docs.microsoft.com/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="e0f38-120">Sign in tooyour Azure account with [az login](https://docs.microsoft.com/cli/azure/#login).</span></span>
3. <span data-ttu-id="e0f38-121">Stoppa och frigöra hello VM.</span><span class="sxs-lookup"><span data-stu-id="e0f38-121">Stop and deallocate hello VM.</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="e0f38-122">Generalisera hello VM.</span><span class="sxs-lookup"><span data-stu-id="e0f38-122">Generalize hello VM.</span></span> 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

<span data-ttu-id="e0f38-123">toouse hello VHD som en disk för en ny instans av en befintlig virtuell dator eller en datadisk, utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0f38-123">toouse hello VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="e0f38-124">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e0f38-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="e0f38-125">Hej hubbmenyn, klicka på **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="e0f38-125">On hello Hub menu, click **Virtual Machines**.</span></span>
3.  <span data-ttu-id="e0f38-126">Välj hello VM hello-listan.</span><span class="sxs-lookup"><span data-stu-id="e0f38-126">Select hello VM from hello list.</span></span>
4.  <span data-ttu-id="e0f38-127">Klicka på hello bladet för hello VM **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="e0f38-127">On hello blade for hello VM, click **Stop**.</span></span>

    ![Stoppa VM](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="e0f38-129">Generera en SAS-URL</span><span class="sxs-lookup"><span data-stu-id="e0f38-129">Generate SAS URL</span></span>

<span data-ttu-id="e0f38-130">toodownload hello VHD-filen, behöver du toogenerate en [signatur för delad åtkomst (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span><span class="sxs-lookup"><span data-stu-id="e0f38-130">toodownload hello VHD file, you need toogenerate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="e0f38-131">När hello URL: en genereras tilldelas en förfallotid toohello URL.</span><span class="sxs-lookup"><span data-stu-id="e0f38-131">When hello URL is generated, an expiration time is assigned toohello URL.</span></span>

1.  <span data-ttu-id="e0f38-132">På menyn hello av hello-bladet för hello VM **diskar**.</span><span class="sxs-lookup"><span data-stu-id="e0f38-132">On hello menu of hello blade for hello VM, click **Disks**.</span></span>
2.  <span data-ttu-id="e0f38-133">Välj hello operativsystemdisken för hello VM och klicka sedan på **exportera**.</span><span class="sxs-lookup"><span data-stu-id="e0f38-133">Select hello operating system disk for hello VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="e0f38-134">Klicka på **generera URL**.</span><span class="sxs-lookup"><span data-stu-id="e0f38-134">Click **Generate URL**.</span></span>

    ![Generera en URL](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a><span data-ttu-id="e0f38-136">Hämta VHD</span><span class="sxs-lookup"><span data-stu-id="e0f38-136">Download VHD</span></span>

1.  <span data-ttu-id="e0f38-137">Klicka på Hämta hello VHD-filen under hello-URL som har genererats.</span><span class="sxs-lookup"><span data-stu-id="e0f38-137">Under hello URL that was generated, click Download hello VHD file.</span></span>

    ![Hämta VHD](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="e0f38-139">Du kan behöva tooclick **spara** i hello webbläsare toostart hello hämtning.</span><span class="sxs-lookup"><span data-stu-id="e0f38-139">You may need tooclick **Save** in hello browser toostart hello download.</span></span> <span data-ttu-id="e0f38-140">hello standardnamnet för hello VHD-filen är *abcd*.</span><span class="sxs-lookup"><span data-stu-id="e0f38-140">hello default name for hello VHD file is *abcd*.</span></span>

    ![Klicka på Spara i hello webbläsare](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="e0f38-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0f38-142">Next steps</span></span>

- <span data-ttu-id="e0f38-143">Lär dig hur för[överför och skapa en Linux VM från anpassade disken med hello Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0f38-143">Learn how too[upload and create a Linux VM from custom disk with hello Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
- <span data-ttu-id="e0f38-144">[Hantera Azure-diskarna hello Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0f38-144">[Manage Azure disks hello Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

