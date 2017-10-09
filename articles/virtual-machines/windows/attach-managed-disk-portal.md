---
title: aaaAttach en hanterad data disk tooa Windows VM - Azure | Microsoft Docs
description: Hur tooattach nya hanterade data disk tooa hello Windows VM i hello Azure portal med Resource Manager-distributionsmodellen.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: bacc0589ad2d93e4d3d055c8f837f8db27291ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-managed-data-disk-tooa-windows-vm-in-hello-azure-portal"></a><span data-ttu-id="9f43d-103">Hur tooattach hanterade data disk tooa Windows VM i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9f43d-103">How tooattach a managed data disk tooa Windows VM in hello Azure portal</span></span>

<span data-ttu-id="9f43d-104">Den här artikeln visar hur tooattach en nya hanterade data disk tooWindows virtuella datorer via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9f43d-104">This article shows you how tooattach a new managed data disk tooWindows virtual machines through hello Azure portal.</span></span> <span data-ttu-id="9f43d-105">Innan du gör detta kan du granska dessa tips:</span><span class="sxs-lookup"><span data-stu-id="9f43d-105">Before you do this, review these tips:</span></span>

* <span data-ttu-id="9f43d-106">hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga.</span><span class="sxs-lookup"><span data-stu-id="9f43d-106">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="9f43d-107">Mer information finns i [storlekar för virtuella datorer](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="9f43d-107">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="9f43d-108">För en ny disk, behöver du inte toocreate den första eftersom Azure skapar den när du ansluter den.</span><span class="sxs-lookup"><span data-stu-id="9f43d-108">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="9f43d-109">Du kan också [ansluta en datadisk med hjälp av Powershell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="9f43d-109">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>



## <a name="add-a-data-disk"></a><span data-ttu-id="9f43d-110">Lägg till en datadisk</span><span class="sxs-lookup"><span data-stu-id="9f43d-110">Add a data disk</span></span>
1. <span data-ttu-id="9f43d-111">Hello-menyn hello vänster **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-111">In hello menu on hello left, click **Virtual Machines**.</span></span>
2. <span data-ttu-id="9f43d-112">Välj hello virtuella hello-listan.</span><span class="sxs-lookup"><span data-stu-id="9f43d-112">Select hello virtual machine from hello list.</span></span>
3. <span data-ttu-id="9f43d-113">På hello virtuella bladet, klickar du på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-113">On hello virtual machine blade, click **Disks**.</span></span>
   4. <span data-ttu-id="9f43d-114">På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-114">On hello **Disks** blade, click **+ Add data disk**.</span></span>
5. <span data-ttu-id="9f43d-115">I hello kombinationsrutan hello ny disk, väljer **skapa tom**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-115">In hello drop-down for hello new disk, select **Create empty**.</span></span>
6. <span data-ttu-id="9f43d-116">I hello **skapa hanterade diskar** bladet, Skriv ett namn för hello disken och justera hello övriga inställningar efter behov.</span><span class="sxs-lookup"><span data-stu-id="9f43d-116">In hello **Create managed disk** blade, type in a name for hello disk and adjust hello other settings as necessary.</span></span> <span data-ttu-id="9f43d-117">När du är klar klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-117">When you are done, click **Create**.</span></span>
7. <span data-ttu-id="9f43d-118">I hello **diskar** blad, klicka på Spara toosave hello nya diskkonfigurationen för hello VM.</span><span class="sxs-lookup"><span data-stu-id="9f43d-118">In hello **Disks** blade, click save toosave hello new disk configuration for hello VM.</span></span>
6. <span data-ttu-id="9f43d-119">När Azure skapar hello disk och bifogas toohello virtuella datorn, hello nya disken visas i hello virtuella datorns Diskinställningar under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-119">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="initialize-a-new-data-disk"></a><span data-ttu-id="9f43d-120">Initiera en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="9f43d-120">Initialize a new data disk</span></span>

1. <span data-ttu-id="9f43d-121">Ansluta toohello VM.</span><span class="sxs-lookup"><span data-stu-id="9f43d-121">Connect toohello VM.</span></span>
1. <span data-ttu-id="9f43d-122">Klicka på hello start-menyn i hello VM och ange **diskmgmt.msc** och träffar **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-122">Click hello start menu inside hello VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="9f43d-123">Detta startar hello snapin-modulen Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="9f43d-123">This will start hello Disk Management snap-in.</span></span>
2. <span data-ttu-id="9f43d-124">Diskhantering identifierar att du har en ny, ej initierad disk och hello initiera Disk fönster visas.</span><span class="sxs-lookup"><span data-stu-id="9f43d-124">Disk Management will recognize that you have a new, un-initialized disk and hello Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="9f43d-125">Kontrollera att hello ny disk är markerad och klicka på **OK** tooinitialize den.</span><span class="sxs-lookup"><span data-stu-id="9f43d-125">Make sure hello new disk is selected and click **OK** tooinitialize it.</span></span>
4. <span data-ttu-id="9f43d-126">hello nya disken visas nu som **lediga**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-126">hello new disk will now appear as **unallocated**.</span></span> <span data-ttu-id="9f43d-127">Högerklicka på hello disk och välj **ny enkel volym**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-127">Right-click anywhere on hello disk and select **New simple volume**.</span></span> <span data-ttu-id="9f43d-128">Hej **guiden Ny enkel volym** startar.</span><span class="sxs-lookup"><span data-stu-id="9f43d-128">hello **New Simple Volume Wizard** will start.</span></span>
5. <span data-ttu-id="9f43d-129">Gå igenom guiden hello hålla alla standardvärden för hello när du är klar väljer **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-129">Go through hello wizard, keeping all of hello defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="9f43d-130">Stäng Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="9f43d-130">Close Disk Management.</span></span>
7. <span data-ttu-id="9f43d-131">Du får ett popup-fönster som du behöver tooformat hello nya disken innan du kan använda den.</span><span class="sxs-lookup"><span data-stu-id="9f43d-131">You will get a pop-up that you need tooformat hello new disk before you can use it.</span></span> <span data-ttu-id="9f43d-132">Klicka på **Formatera disk**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-132">Click **Format disk**.</span></span>
8. <span data-ttu-id="9f43d-133">I hello **Format ny disk** dialogrutan, kontrollera hello inställningar och klickar sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-133">In hello **Format new disk** dialog, check hello settings and then click **Start**.</span></span>
9. <span data-ttu-id="9f43d-134">Du får en varning om att formatering hello diskar raderas alla hello data, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-134">You will get a warning that formatting hello disks will erase all of hello data, click **OK**.</span></span>
10. <span data-ttu-id="9f43d-135">När hello format är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f43d-135">When hello format is complete, click **OK**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="9f43d-136">Använd Rensa med standardlagring</span><span class="sxs-lookup"><span data-stu-id="9f43d-136">Use TRIM with standard storage</span></span>

<span data-ttu-id="9f43d-137">Om du använder standardlagring (HDD), bör du aktivera TRIMNING.</span><span class="sxs-lookup"><span data-stu-id="9f43d-137">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="9f43d-138">TRIMNING ignorerar oanvända block på disken hello så att endast debiteras du för lagring som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="9f43d-138">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="9f43d-139">Om du skapar stora filer och ta bort dem kan detta spara på kostnader.</span><span class="sxs-lookup"><span data-stu-id="9f43d-139">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="9f43d-140">Du kan köra kommandot toocheck hello TRIM inställningen.</span><span class="sxs-lookup"><span data-stu-id="9f43d-140">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="9f43d-141">Öppna en kommandotolk på din Windows-VM och skriv:</span><span class="sxs-lookup"><span data-stu-id="9f43d-141">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="9f43d-142">Om hello kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="9f43d-142">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="9f43d-143">Om den returnerar 1, kör du följande kommando tooenable TRIMNING hello:</span><span class="sxs-lookup"><span data-stu-id="9f43d-143">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="9f43d-144">När data har tagits bort från disken, kan du kontrollera hello TRIM operations tömning korrekt genom att köra defragmenterar med TRIMNING:</span><span class="sxs-lookup"><span data-stu-id="9f43d-144">After deleting data from your disk, you can ensure hello TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="9f43d-145">Du kan också kontrollera hello hela volymen trimmas genom att formatera hello volym.</span><span class="sxs-lookup"><span data-stu-id="9f43d-145">You can also ensure hello entire volume is trimmed by formatting hello volume.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f43d-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9f43d-146">Next steps</span></span>
<span data-ttu-id="9f43d-147">Om du program behöver toouse hello D: enhet toostore data, kan du [ändra hello enhetsbeteckningen för hello Windows diskutrymme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9f43d-147">If you application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
