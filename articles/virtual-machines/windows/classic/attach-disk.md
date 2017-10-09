---
title: aaaAttach en disk tooa klassiska Azure-VM | Microsoft Docs
description: Koppla en data disk tooa Windows virtuell dator som skapats med hello klassiska distributionsmodellen och initiera den.
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: bfe1b0fa066277d28d3862a18da4b1023cb4452d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="attach-a-data-disk-tooa-windows-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="05a71-103">Koppla en data disk tooa Windows virtuell dator som skapats med hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="05a71-103">Attach a data disk tooa Windows virtual machine created with hello classic deployment model</span></span>
<!--
Refernce article:
    If you want toouse hello new portal, see [How tooattach a data disk tooa Windows VM in hello Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

<span data-ttu-id="05a71-104">Den här artikeln visar hur tooattach nya och befintliga diskar skapas med hello klassisk distribution modellen tooa Windows virtuell dator med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="05a71-104">This article shows you how tooattach new and existing disks created with hello Classic deployment model tooa Windows virtual machine using hello Azure portal.</span></span>

<span data-ttu-id="05a71-105">Du kan också [bifoga en data disk tooa Linux VM i hello Azure-portalen](../../linux/attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="05a71-105">You can also [attach a data disk tooa Linux VM in hello Azure portal](../../linux/attach-disk-portal.md).</span></span>

<span data-ttu-id="05a71-106">Innan du ansluter en disk, granska de här tipsen:</span><span class="sxs-lookup"><span data-stu-id="05a71-106">Before you attach a disk, review these tips:</span></span>

* <span data-ttu-id="05a71-107">hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga.</span><span class="sxs-lookup"><span data-stu-id="05a71-107">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="05a71-108">Mer information finns i [storlekar för virtuella datorer](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="05a71-108">For details, see [Sizes for virtual machines](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="05a71-109">toouse Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien.</span><span class="sxs-lookup"><span data-stu-id="05a71-109">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="05a71-110">Du kan använda diskar från Premium- och Standard storage-konton med dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="05a71-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="05a71-111">Premium-lagring är tillgänglig i vissa regioner.</span><span class="sxs-lookup"><span data-stu-id="05a71-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="05a71-112">Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="05a71-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="05a71-113">För en ny disk, behöver du inte toocreate den första eftersom Azure skapar den när du ansluter den.</span><span class="sxs-lookup"><span data-stu-id="05a71-113">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="05a71-114">Du kan också [ansluta en datadisk med hjälp av Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="05a71-114">You can also [attach a data disk using Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05a71-115">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="05a71-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>

## <a name="find-hello-virtual-machine"></a><span data-ttu-id="05a71-116">Hitta hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="05a71-116">Find hello virtual machine</span></span>
1. <span data-ttu-id="05a71-117">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="05a71-117">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="05a71-118">Välj hello virtuell dator från hello resursen som visas på instrumentpanelen för hello.</span><span class="sxs-lookup"><span data-stu-id="05a71-118">Select hello virtual machine from hello resource listed on hello dashboard.</span></span>
3. <span data-ttu-id="05a71-119">I hello vänstra fönstret under **inställningar**, klickar du på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="05a71-119">In hello left pane under **Settings**, click **Disks**.</span></span>

    ![Öppna diskinställningarna för](./media/attach-disk/virtualmachinedisks.png)

<span data-ttu-id="05a71-121">Fortsätt genom att följa anvisningarna för att bifoga antingen en [ny disk](#option-1-attach-a-new-disk) eller en [befintlig disk](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="05a71-121">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="05a71-122">Alternativ 1: Anslut och initiera en ny disk</span><span class="sxs-lookup"><span data-stu-id="05a71-122">Option 1: Attach and initialize a new disk</span></span>

1. <span data-ttu-id="05a71-123">På hello **diskar** bladet, klickar du på **bifoga nya**.</span><span class="sxs-lookup"><span data-stu-id="05a71-123">On hello **Disks** blade, click **Attach new**.</span></span>
2. <span data-ttu-id="05a71-124">Granska hello standardinställningarna, uppdatera efter behov och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="05a71-124">Review hello default settings, update as necessary, and then click **OK**.</span></span>

   ![Granska diskinställningarna för](./media/attach-disk/attach-new.png)

3. <span data-ttu-id="05a71-126">När Azure skapar hello disk och bifogas toohello virtuella datorn, hello nya disken visas i hello virtuella datorns Diskinställningar under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="05a71-126">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="05a71-127">Initiera en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="05a71-127">Initialize a new data disk</span></span>

1. <span data-ttu-id="05a71-128">Ansluta toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="05a71-128">Connect toohello virtual machine.</span></span> <span data-ttu-id="05a71-129">Instruktioner finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="05a71-129">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="05a71-130">När du loggar in toohello virtuella datorn, öppna **Serverhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="05a71-130">After you log on toohello virtual machine, open **Server Manager**.</span></span> <span data-ttu-id="05a71-131">I hello vänster och välj **fil- och lagringstjänster**.</span><span class="sxs-lookup"><span data-stu-id="05a71-131">In hello left pane, select **File and Storage Services**.</span></span>

    ![Öppna Serverhanteraren](../media/attach-disk-portal/fileandstorageservices.png)

3. <span data-ttu-id="05a71-133">Välj **diskar**.</span><span class="sxs-lookup"><span data-stu-id="05a71-133">Select **Disks**.</span></span>
4. <span data-ttu-id="05a71-134">Hej **diskar** avsnitt visar hello diskar.</span><span class="sxs-lookup"><span data-stu-id="05a71-134">hello **Disks** section lists hello disks.</span></span> <span data-ttu-id="05a71-135">En virtuell dator har mest ofta disk 0, disk 1 och disk 2.</span><span class="sxs-lookup"><span data-stu-id="05a71-135">Most often, a virtual machine has disk 0, disk 1, and disk 2.</span></span> <span data-ttu-id="05a71-136">0 är hello operativsystemdisk, 1 är hello tillfälliga disk och disk 2 är hello datadisk nyligen ansluten toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="05a71-136">Disk 0 is hello operating system disk, disk 1 is hello temporary disk, and disk 2 is hello data disk newly attached toohello virtual machine.</span></span> <span data-ttu-id="05a71-137">hello data disk visar hello Partition som **okänd**.</span><span class="sxs-lookup"><span data-stu-id="05a71-137">hello data disk lists hello Partition as **Unknown**.</span></span>

 <span data-ttu-id="05a71-138">Högerklicka på hello disken och välj **initiera**.</span><span class="sxs-lookup"><span data-stu-id="05a71-138">Right-click hello disk and select **Initialize**.</span></span>

5. <span data-ttu-id="05a71-139">Du meddelas att alla data raderas när hello disken initieras.</span><span class="sxs-lookup"><span data-stu-id="05a71-139">You're notified that all data will be erased when hello disk is initialized.</span></span> <span data-ttu-id="05a71-140">Klicka på **Ja** tooacknowledge hello varning och initiera hello disk.</span><span class="sxs-lookup"><span data-stu-id="05a71-140">Click **Yes** tooacknowledge hello warning and initialize hello disk.</span></span> <span data-ttu-id="05a71-141">När installationen är klar visas hello partition som **GPT**.</span><span class="sxs-lookup"><span data-stu-id="05a71-141">Once complete, hello partition will be listed as **GPT**.</span></span> <span data-ttu-id="05a71-142">Högerklicka på hello disken igen och välj **ny volym**.</span><span class="sxs-lookup"><span data-stu-id="05a71-142">Right-click hello disk again and select **New Volume**.</span></span>

6. <span data-ttu-id="05a71-143">Slutför guiden för hello med hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="05a71-143">Complete hello wizard using hello default values.</span></span> <span data-ttu-id="05a71-144">När hello guiden är klar hello **volymer** avsnittet visar hello ny volym.</span><span class="sxs-lookup"><span data-stu-id="05a71-144">When hello wizard is done, hello **Volumes** section lists hello new volume.</span></span> <span data-ttu-id="05a71-145">hello disken är nu online och klara toostore data.</span><span class="sxs-lookup"><span data-stu-id="05a71-145">hello disk is now online and ready toostore data.</span></span>

    ![Volymen har startats](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="05a71-147">Alternativ 2: Bifoga en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="05a71-147">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="05a71-148">På hello **diskar** bladet, klickar du på **bifoga befintliga**.</span><span class="sxs-lookup"><span data-stu-id="05a71-148">On hello **Disks** blade, click **Attach existing**.</span></span>
2. <span data-ttu-id="05a71-149">Under **bifoga den befintliga disken**, klickar du på **plats**.</span><span class="sxs-lookup"><span data-stu-id="05a71-149">Under **Attach existing disk**, click **Location**.</span></span>

   ![Bifoga den befintliga disken](./media/attach-disk/attachexistingdisksettings.png)
3. <span data-ttu-id="05a71-151">Under **lagringskonton**, väljer hello-konto och behållare som innehåller hello VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="05a71-151">Under **Storage accounts**, select hello account and container that holds hello .vhd file.</span></span>

   ![Hitta VHD-plats](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. <span data-ttu-id="05a71-153">Välj hello VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="05a71-153">Select hello .vhd file.</span></span>
5. <span data-ttu-id="05a71-154">Under **bifoga den befintliga disken**, hello-filen markerade anges under **VHD-filen**.</span><span class="sxs-lookup"><span data-stu-id="05a71-154">Under **Attach existing disk**, hello file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="05a71-155">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="05a71-155">Click **OK**.</span></span>
6. <span data-ttu-id="05a71-156">När Azure bifogar hello disk toohello virtuell dator, visas den i hello virtuella datorns Diskinställningar under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="05a71-156">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="05a71-157">Använd Rensa med standardlagring</span><span class="sxs-lookup"><span data-stu-id="05a71-157">Use TRIM with standard storage</span></span>

<span data-ttu-id="05a71-158">Om du använder standardlagring (HDD), bör du aktivera TRIMNING.</span><span class="sxs-lookup"><span data-stu-id="05a71-158">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="05a71-159">TRIMNING ignorerar oanvända block på disken hello så att endast debiteras du för lagring som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="05a71-159">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="05a71-160">Med TRIMNING spara kostnader, inklusive oanvända block som uppstår vid borttagning av stora filer.</span><span class="sxs-lookup"><span data-stu-id="05a71-160">Using TRIM can save costs, including unused blocks that result from deleting large files.</span></span>

<span data-ttu-id="05a71-161">Du kan köra kommandot toocheck hello TRIM inställningen.</span><span class="sxs-lookup"><span data-stu-id="05a71-161">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="05a71-162">Öppna en kommandotolk på din Windows-VM och skriv:</span><span class="sxs-lookup"><span data-stu-id="05a71-162">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="05a71-163">Om hello kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="05a71-163">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="05a71-164">Om den returnerar 1, kör du följande kommando tooenable TRIMNING hello:</span><span class="sxs-lookup"><span data-stu-id="05a71-164">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a><span data-ttu-id="05a71-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05a71-165">Next steps</span></span>
<span data-ttu-id="05a71-166">Om ditt program behöver toouse hello D: enhet toostore data, kan du [ändra hello enhetsbeteckningen för hello Windows diskutrymme](../../virtual-machines-windows-change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="05a71-166">If your application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](../../virtual-machines-windows-change-drive-letter.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="05a71-167">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="05a71-167">Additional resources</span></span>
[<span data-ttu-id="05a71-168">Om diskar och virtuella hårddiskar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="05a71-168">About disks and VHDs for virtual machines</span></span>](../../virtual-machines-linux-about-disks-vhds.md)
