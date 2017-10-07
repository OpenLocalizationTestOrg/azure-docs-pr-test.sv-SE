---
title: aaaAttach en ohanterad data disk tooa Windows VM - Azure | Microsoft Docs
description: Hur hello tooattach ny eller befintlig ohanterade data disk tooa Windows VM i hello Azure portal med Resource Manager-distributionsmodellen.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3790fc59-7264-41df-b7a3-8d1226799885
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: 923a6663974143bf359970f9b0eb0d36cabcba9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-an-unmanaged-data-disk-tooa-windows-vm-in-hello-azure-portal"></a><span data-ttu-id="3d1f5-103">Hur tooattach en ohanterad data disk tooa Windows VM i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3d1f5-103">How tooattach an unmanaged data disk tooa Windows VM in hello Azure portal</span></span>

<span data-ttu-id="3d1f5-104">Den här artikeln lär du dig hur tooattach både nya och befintliga ohanterad diskar tooWindows virtuella datorer via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-104">This article shows you how tooattach both new and existing unmanaged disks tooWindows virtual machines through hello Azure portal.</span></span> <span data-ttu-id="3d1f5-105">Du kan också [ansluta en datadisk med hjälp av PowerShell](./attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3d1f5-105">You can also [attach a data disk using PowerShell](./attach-disk-ps.md).</span></span> <span data-ttu-id="3d1f5-106">Innan du gör detta kan du granska dessa tips:</span><span class="sxs-lookup"><span data-stu-id="3d1f5-106">Before you do this, review these tips:</span></span>

* <span data-ttu-id="3d1f5-107">hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-107">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="3d1f5-108">Mer information finns i [storlekar för virtuella datorer](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="3d1f5-108">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="3d1f5-109">toouse Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-109">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="3d1f5-110">Du kan använda diskar från Premium- och Standard storage-konton med dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="3d1f5-111">Premium-lagring är tillgänglig i vissa regioner.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="3d1f5-112">Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d1f5-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="3d1f5-113">För en ny disk, behöver du inte toocreate den första eftersom Azure skapar den när du ansluter den.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-113">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>


<span data-ttu-id="3d1f5-114">Du kan också [ansluta en datadisk med hjälp av Powershell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3d1f5-114">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>


## <a name="find-hello-virtual-machine"></a><span data-ttu-id="3d1f5-115">Hitta hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3d1f5-115">Find hello virtual machine</span></span>
1. <span data-ttu-id="3d1f5-116">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3d1f5-116">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="3d1f5-117">Hello-menyn hello vänster **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-117">In hello menu on hello left, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="3d1f5-118">Välj hello virtuella hello-listan.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-118">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="3d1f5-119">I bladet för virtuella datorer av hello klickar du på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-119">In hello Virtual machines blade, click **Disks**.</span></span>
   
<span data-ttu-id="3d1f5-120">Fortsätt genom att följa anvisningarna för att bifoga antingen en [ny disk](#option-1-attach-a-new-disk) eller en [befintlig disk](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="3d1f5-120">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="3d1f5-121">Alternativ 1: Anslut och initiera en ny disk</span><span class="sxs-lookup"><span data-stu-id="3d1f5-121">Option 1: Attach and initialize a new disk</span></span>
1. <span data-ttu-id="3d1f5-122">På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-122">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="3d1f5-123">I hello **bifoga hanterade diskar** bladet, ange ett namn för hello disk i **namn** och välj sedan **ny (tom disk)** i **typ av datakälla**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-123">In hello **Attach managed disk** blade, type a name for hello disk in **Name** and then select **New (empty disk)** in **Source type**.</span></span>
3. <span data-ttu-id="3d1f5-124">Under **lagringsbehållaren**, klicka på hello **Bläddra** knappen och bläddra toohello storage-konto och en behållare där du som hello nya VHD-toobe lagras och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-124">Under **Storage container**, click hello **Browse** button and browse toohello storage account and container where you would like hello new VHD toobe stored and then click **Select**.</span></span> 
  
   ![Granska diskinställningarna för](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. <span data-ttu-id="3d1f5-126">När du är klar med hello inställningar för hello datadisk klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-126">When you are done with hello settings for hello data disk, click **OK**.</span></span>
4. <span data-ttu-id="3d1f5-127">Tillbaka i hello **diskar** bladet, klickar du på **spara** tooadd hello disk toohello VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-127">Back in hello **Disks** blade, click **Save** tooadd hello disk toohello VM configuration.</span></span>


### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="3d1f5-128">Initiera en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="3d1f5-128">Initialize a new data disk</span></span>

1. <span data-ttu-id="3d1f5-129">Ansluta toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-129">Connect toohello virtual machine.</span></span> <span data-ttu-id="3d1f5-130">Instruktioner finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d1f5-130">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
1. <span data-ttu-id="3d1f5-131">Klicka på hello **starta** menyn i hello VM och skriva **diskmgmt.msc** och träffar **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-131">Click hello **Start** menu inside hello VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="3d1f5-132">Detta startar hello snapin-modulen Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-132">This starts hello Disk Management snap-in.</span></span>
2. <span data-ttu-id="3d1f5-133">Diskhantering märker att du har en ny, oinitierad disk och hello initiera Disk fönster visas.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-133">Disk Management recognizes that you have a new, uninitialized disk and hello Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="3d1f5-134">Kontrollera att hello ny disk är markerad och klicka på **OK** tooinitialize den.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-134">Make sure hello new disk is selected and click **OK** tooinitialize it.</span></span>
4. <span data-ttu-id="3d1f5-135">hello nya disken visas nu som **lediga**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-135">hello new disk now appears as **unallocated**.</span></span> <span data-ttu-id="3d1f5-136">Högerklicka på hello disk och välj **ny enkel volym**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-136">Right-click anywhere on hello disk and select **New simple volume**.</span></span> <span data-ttu-id="3d1f5-137">Hej **guiden Ny enkel volym** startar.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-137">hello **New Simple Volume Wizard** starts.</span></span>
5. <span data-ttu-id="3d1f5-138">Gå igenom guiden hello hålla alla standardvärden för hello när du är klar väljer **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-138">Go through hello wizard, keeping all of hello defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="3d1f5-139">Stäng Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-139">Close Disk Management.</span></span>
7. <span data-ttu-id="3d1f5-140">Du får ett popup-fönster som du behöver tooformat hello nya disken innan du kan använda den.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-140">You get a pop-up that you need tooformat hello new disk before you can use it.</span></span> <span data-ttu-id="3d1f5-141">Klicka på **Formatera disk**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-141">Click **Format disk**.</span></span>
8. <span data-ttu-id="3d1f5-142">I hello **Format ny disk** dialogrutan, kontrollera hello inställningar och klickar sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-142">In hello **Format new disk** dialog, check hello settings and then click **Start**.</span></span>
9. <span data-ttu-id="3d1f5-143">Du får en varning om att hello diskar formateras alla hello data, klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-143">You get a warning that formatting hello disks erases all of hello data, click **OK**.</span></span>
10. <span data-ttu-id="3d1f5-144">När hello format är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-144">When hello format is complete, click **OK**.</span></span>


## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="3d1f5-145">Alternativ 2: Bifoga en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="3d1f5-145">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="3d1f5-146">På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-146">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="3d1f5-147">På hello **bifoga ohanterade disk** bladet i **typ av datakälla** Välj **befintlig blob**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-147">On hello **Attach unmanaged disk** blade, in **Source type** select **Existing blob**.</span></span>

    ![Granska diskinställningarna för](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. <span data-ttu-id="3d1f5-149">Klicka på **Bläddra** toonavigate toohello storage-konto och behållare där hello befintlig virtuell Hårddisk finns.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-149">Click **Browse** toonavigate toohello storage account and container where hello existing VHD is located.</span></span> <span data-ttu-id="3d1f5-150">Klicka på hello VHD och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-150">Click and hello VHD and then click **Select**.</span></span>
4. <span data-ttu-id="3d1f5-151">Klicka på **OK** i hello **bifoga ohanterade disk** bladet.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-151">Click **OK** in hello **Attach unmanaged disk** blade.</span></span>
5. <span data-ttu-id="3d1f5-152">I hello **diskar** bladet, klickar du på **spara** tooadd hello diskkonfigurationen toohello för hello VM.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-152">In hello **Disks** blade, click **Save** tooadd hello disk toohello configuration for hello VM.</span></span>
   


## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="3d1f5-153">Använd Rensa med standardlagring</span><span class="sxs-lookup"><span data-stu-id="3d1f5-153">Use TRIM with standard storage</span></span>

<span data-ttu-id="3d1f5-154">Om du använder standardlagring (HDD), bör du aktivera TRIMNING.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-154">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="3d1f5-155">TRIMNING ignorerar oanvända block på disken hello så att endast debiteras du för lagring som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-155">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="3d1f5-156">Om du skapar stora filer och ta bort dem kan detta spara på kostnader.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-156">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="3d1f5-157">Du kan köra kommandot toocheck hello TRIM inställningen.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-157">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="3d1f5-158">Öppna en kommandotolk på din Windows-VM och skriv:</span><span class="sxs-lookup"><span data-stu-id="3d1f5-158">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="3d1f5-159">Om hello kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-159">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="3d1f5-160">Om den returnerar 1, kör du följande kommando tooenable TRIMNING hello:</span><span class="sxs-lookup"><span data-stu-id="3d1f5-160">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="3d1f5-161">När data har tagits bort från disken, kan du kontrollera hello TRIM operations tömning korrekt genom att köra defragmenterar med TRIMNING:</span><span class="sxs-lookup"><span data-stu-id="3d1f5-161">After deleting data from your disk, you can ensure hello TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="3d1f5-162">Du kan också kontrollera hello hela volymen trimmas genom att formatera hello volym.</span><span class="sxs-lookup"><span data-stu-id="3d1f5-162">You can also ensure hello entire volume is trimmed by formatting hello volume.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3d1f5-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d1f5-163">Next steps</span></span>
<span data-ttu-id="3d1f5-164">Om du program behöver toouse hello D: enhet toostore data, kan du [ändra hello enhetsbeteckningen för hello Windows diskutrymme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d1f5-164">If you application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

