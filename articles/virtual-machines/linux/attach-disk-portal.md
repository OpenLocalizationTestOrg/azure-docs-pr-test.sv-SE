---
title: aaaAttach tooa en data-disk Linux VM | Microsoft Docs
description: Hur hello tooattach nya eller befintliga data disk tooa Linux VM i hello Azure portal med Resource Manager-distributionsmodellen.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: cynthn
ms.openlocfilehash: 069c3c6f5e71a8c495342e6d0c6f3d92c4ed5053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-vm-in-hello-azure-portal"></a><span data-ttu-id="e522b-103">Hur tooattach data disk tooa Linux VM i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e522b-103">How tooattach a data disk tooa Linux VM in hello Azure portal</span></span>
<span data-ttu-id="e522b-104">Den här artikeln visar hur tooattach både nya och befintliga diskar tooa virtuell Linux-dator via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e522b-104">This article shows you how tooattach both new and existing disks tooa Linux virtual machine through hello Azure portal.</span></span> <span data-ttu-id="e522b-105">Du kan också [bifoga en data disk tooa Windows VM i hello Azure-portalen](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e522b-105">You can also [attach a data disk tooa Windows VM in hello Azure portal](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="e522b-106">Du kan välja toouse antingen Azure hanterade diskar eller är ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="e522b-106">You can choose toouse either Azure Managed Disks or unmanaged disks.</span></span> <span data-ttu-id="e522b-107">Hanterade diskar som hanteras av hello Azure-plattformen och kräver inte någon förberedelse eller plats toostore dem.</span><span class="sxs-lookup"><span data-stu-id="e522b-107">Managed disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="e522b-108">Ohanterad diskar krävs ett lagringskonto och har några [kvoter och gränser som gäller](../../azure-subscription-service-limits.md#storage-limits).</span><span class="sxs-lookup"><span data-stu-id="e522b-108">Unmanaged disks require a storage account and have some [quotas and limits that apply](../../azure-subscription-service-limits.md#storage-limits).</span></span> <span data-ttu-id="e522b-109">Mer information om Azure Managed Disks finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e522b-109">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="e522b-110">Granska de här tipsen innan du kopplar diskar tooyour VM:</span><span class="sxs-lookup"><span data-stu-id="e522b-110">Before you attach disks tooyour VM, review these tips:</span></span>

* <span data-ttu-id="e522b-111">hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga.</span><span class="sxs-lookup"><span data-stu-id="e522b-111">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="e522b-112">Mer information finns i [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e522b-112">For details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="e522b-113">toouse Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien.</span><span class="sxs-lookup"><span data-stu-id="e522b-113">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="e522b-114">Du kan använda Premium- och Standard diskar med dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e522b-114">You can use both Premium and Standard disks with these virtual machines.</span></span> <span data-ttu-id="e522b-115">Premium-lagring är tillgänglig i vissa regioner.</span><span class="sxs-lookup"><span data-stu-id="e522b-115">Premium storage is available in certain regions.</span></span> <span data-ttu-id="e522b-116">Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e522b-116">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="e522b-117">Diskar anslutna toovirtual datorer är faktiskt VHD-filer som är lagrade i Azure.</span><span class="sxs-lookup"><span data-stu-id="e522b-117">Disks attached toovirtual machines are actually .vhd files stored in Azure.</span></span> <span data-ttu-id="e522b-118">Mer information finns i [om diskar och virtuella hårddiskar för virtuella datorer](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e522b-118">For details, see [About disks and VHDs for virtual machines](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="find-hello-virtual-machine"></a><span data-ttu-id="e522b-119">Hitta hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e522b-119">Find hello virtual machine</span></span>
1. <span data-ttu-id="e522b-120">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e522b-120">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e522b-121">Hej hubbmenyn, klicka på **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="e522b-121">On hello Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="e522b-122">Välj hello virtuella hello-listan.</span><span class="sxs-lookup"><span data-stu-id="e522b-122">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="e522b-123">toohello virtuella datorer bladet i **Essentials**, klickar du på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="e522b-123">toohello Virtual machines blade, in **Essentials**, click **Disks**.</span></span>
   
    ![Öppna diskinställningarna för](./media/attach-disk-portal/find-disk-settings.png)

<span data-ttu-id="e522b-125">Fortsätt genom att följa anvisningarna för att bifoga antingen en [hanterade diskar](#use-azure-managed-disks) eller [ohanterade disk](#use-unmanaged-disks).</span><span class="sxs-lookup"><span data-stu-id="e522b-125">Continue by following instructions for attaching either a [managed disk](#use-azure-managed-disks) or [unmanaged disk](#use-unmanaged-disks).</span></span>

## <a name="use-azure-managed-disks"></a><span data-ttu-id="e522b-126">Använda Azure hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="e522b-126">Use Azure Managed Disks</span></span>

### <a name="attach-a-new-disk"></a><span data-ttu-id="e522b-127">Koppla en ny disk</span><span class="sxs-lookup"><span data-stu-id="e522b-127">Attach a new disk</span></span>

1. <span data-ttu-id="e522b-128">På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="e522b-128">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="e522b-129">Klicka på hello nedrullningsbara menyn för **namn** och välj **skapa disk**:</span><span class="sxs-lookup"><span data-stu-id="e522b-129">Click hello drop-down menu for **Name** and select **Create disk**:</span></span>

    ![Skapa Azure hanterade diskar](./media/attach-disk-portal/create-new-md.png)

3. <span data-ttu-id="e522b-131">Ange ett namn för din hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="e522b-131">Enter a name for your managed disk.</span></span> <span data-ttu-id="e522b-132">Granska hello standardinställningarna, uppdatera efter behov och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e522b-132">Review hello default settings, update as necessary, and then click **Create**.</span></span>
   
   ![Granska diskinställningarna för](./media/attach-disk-portal/create-new-md-settings.png)

4. <span data-ttu-id="e522b-134">Klicka på **spara** toocreate hello hanterade disken och uppdatera hello VM-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="e522b-134">Click **Save** toocreate hello managed disk and update hello VM configuration:</span></span>

   ![Spara nya Azure hanterade disken](./media/attach-disk-portal/confirm-create-new-md.png)

5. <span data-ttu-id="e522b-136">När Azure skapar hello disk och bifogas toohello virtuella datorn, hello nya disken visas i hello virtuella datorns Diskinställningar under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="e522b-136">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span> <span data-ttu-id="e522b-137">Eftersom hanterade diskar är en översta resurs, visas hello disken i hello rot hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="e522b-137">As managed disks are a top-level resource, hello disk appears at hello root of hello resource group:</span></span>

   ![Azure-hanterade disken i resursgruppen](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a><span data-ttu-id="e522b-139">Ansluta en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="e522b-139">Attach an existing disk</span></span>
1. <span data-ttu-id="e522b-140">På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="e522b-140">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="e522b-141">Klicka på hello nedrullningsbara menyn för **namn** tooview en lista över befintliga hanterade diskar tillgänglig tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e522b-141">Click hello drop-down menu for **Name** tooview a list of existing managed disks accessible tooyour Azure subscription.</span></span> <span data-ttu-id="e522b-142">Välj hello hanterade disken tooattach:</span><span class="sxs-lookup"><span data-stu-id="e522b-142">Select hello managed disk tooattach:</span></span>

   ![Bifoga den befintliga Azure hanterade disken](./media/attach-disk-portal/select-existing-md.png)

3. <span data-ttu-id="e522b-144">Klicka på **spara** tooattach hello befintliga hanterade disken och uppdatera hello VM-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="e522b-144">Click **Save** tooattach hello existing managed disk and update hello VM configuration:</span></span>
   
   ![Spara Azure hanteras Disk uppdateringar](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. <span data-ttu-id="e522b-146">När Azure bifogar hello disk toohello virtuell dator, visas den i hello virtuella datorns Diskinställningar under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="e522b-146">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-unmanaged-disks"></a><span data-ttu-id="e522b-147">Använda ohanterade diskar</span><span class="sxs-lookup"><span data-stu-id="e522b-147">Use unmanaged disks</span></span>

### <a name="attach-a-new-disk"></a><span data-ttu-id="e522b-148">Koppla en ny disk</span><span class="sxs-lookup"><span data-stu-id="e522b-148">Attach a new disk</span></span>

1. <span data-ttu-id="e522b-149">På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="e522b-149">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="e522b-150">Granska hello standardinställningarna, uppdatera efter behov och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e522b-150">Review hello default settings, update as necessary, and then click **OK**.</span></span>
   
   ![Granska diskinställningarna för](./media/attach-disk-portal/attach-new.png)
3. <span data-ttu-id="e522b-152">När Azure skapar hello disk och bifogas toohello virtuella datorn, hello nya disken visas i hello virtuella datorns Diskinställningar under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="e522b-152">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="attach-an-existing-disk"></a><span data-ttu-id="e522b-153">Ansluta en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="e522b-153">Attach an existing disk</span></span>
1. <span data-ttu-id="e522b-154">På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="e522b-154">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="e522b-155">Under **bifoga den befintliga disken**, klickar du på **VHD-filen**.</span><span class="sxs-lookup"><span data-stu-id="e522b-155">Under **Attach existing disk**, click **VHD File**.</span></span>
   
   ![Bifoga den befintliga disken](./media/attach-disk-portal/attach-existing.png)
3. <span data-ttu-id="e522b-157">Under **lagringskonton**, väljer hello-konto och behållare som innehåller hello VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="e522b-157">Under **Storage accounts**, select hello account and container that holds hello .vhd file.</span></span>
   
   ![Hitta VHD-plats](./media/attach-disk-portal/find-storage-container.png)
4. <span data-ttu-id="e522b-159">Välj hello VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="e522b-159">Select hello .vhd file.</span></span>
5. <span data-ttu-id="e522b-160">Under **bifoga den befintliga disken**, hello-filen markerade anges under **VHD-filen**.</span><span class="sxs-lookup"><span data-stu-id="e522b-160">Under **Attach existing disk**, hello file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="e522b-161">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e522b-161">Click **OK**.</span></span>
6. <span data-ttu-id="e522b-162">När Azure bifogar hello disk toohello virtuell dator, visas den i hello virtuella datorns Diskinställningar under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="e522b-162">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e522b-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e522b-163">Next steps</span></span>
<span data-ttu-id="e522b-164">När hello disken läggs tooprepare måste den för användning.</span><span class="sxs-lookup"><span data-stu-id="e522b-164">After hello disk is added, you need tooprepare it for use.</span></span> <span data-ttu-id="e522b-165">Mer information finns i [så här: initiera en ny datadisk i Linux](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="e522b-165">For more information, see [How to: Initialize a new data disk in Linux](add-disk.md).</span></span>
