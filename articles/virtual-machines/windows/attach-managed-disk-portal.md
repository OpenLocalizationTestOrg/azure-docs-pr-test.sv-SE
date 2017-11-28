---
title: Koppla en datadisk hanterade till en Windows-VM - Azure | Microsoft Docs
description: "Så här kopplar du nya hanterade datadisk för en virtuell Windows-dator i Azure-portalen med hjälp av Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: f0cf88a06c5470ef173b22e7213419a6c8760723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-a-managed-data-disk-to-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="96388-103">Så här kopplar du en datadisk hanterade till en virtuell Windows-dator i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="96388-103">How to attach a managed data disk to a Windows VM in the Azure portal</span></span>

<span data-ttu-id="96388-104">Den här artikeln visar hur du kopplar en ny datadisk hanterade till Windows-datorer via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="96388-104">This article shows you how to attach a new managed data disk to Windows virtual machines through the Azure portal.</span></span> <span data-ttu-id="96388-105">Innan du gör detta kan du granska dessa tips:</span><span class="sxs-lookup"><span data-stu-id="96388-105">Before you do this, review these tips:</span></span>

* <span data-ttu-id="96388-106">Storleken på den virtuella datorn styr hur många datadiskar som du kan bifoga.</span><span class="sxs-lookup"><span data-stu-id="96388-106">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="96388-107">Mer information finns i [storlekar för virtuella datorer](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="96388-107">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="96388-108">För en ny disk behöver du inte skapa först eftersom Azure skapar den när du ansluter den.</span><span class="sxs-lookup"><span data-stu-id="96388-108">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="96388-109">Du kan också [ansluta en datadisk med hjälp av Powershell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="96388-109">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>



## <a name="add-a-data-disk"></a><span data-ttu-id="96388-110">Lägg till en datadisk</span><span class="sxs-lookup"><span data-stu-id="96388-110">Add a data disk</span></span>
1. <span data-ttu-id="96388-111">Klicka på menyn till vänster **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="96388-111">In the menu on the left, click **Virtual Machines**.</span></span>
2. <span data-ttu-id="96388-112">Välj den virtuella datorn i listan.</span><span class="sxs-lookup"><span data-stu-id="96388-112">Select the virtual machine from the list.</span></span>
3. <span data-ttu-id="96388-113">Klicka på bladet för virtuella datorer **diskar**.</span><span class="sxs-lookup"><span data-stu-id="96388-113">On the virtual machine blade, click **Disks**.</span></span>
   4. <span data-ttu-id="96388-114">På den **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="96388-114">On the **Disks** blade, click **+ Add data disk**.</span></span>
5. <span data-ttu-id="96388-115">Välj i listrutan för den nya disken **skapa tom**.</span><span class="sxs-lookup"><span data-stu-id="96388-115">In the drop-down for the new disk, select **Create empty**.</span></span>
6. <span data-ttu-id="96388-116">I den **skapa hanterade diskar** bladet, Skriv ett namn för disken och justera de andra inställningarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="96388-116">In the **Create managed disk** blade, type in a name for the disk and adjust the other settings as necessary.</span></span> <span data-ttu-id="96388-117">När du är klar klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="96388-117">When you are done, click **Create**.</span></span>
7. <span data-ttu-id="96388-118">I den **diskar** bladet Klicka på Spara för att spara den nya diskkonfigurationen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="96388-118">In the **Disks** blade, click save to save the new disk configuration for the VM.</span></span>
6. <span data-ttu-id="96388-119">När Azure skapar disken och kopplar den till den virtuella datorn, den nya disken visas i inställningarna för den virtuella disken under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="96388-119">After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="initialize-a-new-data-disk"></a><span data-ttu-id="96388-120">Initiera en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="96388-120">Initialize a new data disk</span></span>

1. <span data-ttu-id="96388-121">Ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="96388-121">Connect to the VM.</span></span>
1. <span data-ttu-id="96388-122">Klicka på start-menyn i den virtuella datorn och skriva **diskmgmt.msc** och träffar **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="96388-122">Click the start menu inside the VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="96388-123">Detta startar snapin-modulen Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="96388-123">This will start the Disk Management snap-in.</span></span>
2. <span data-ttu-id="96388-124">Diskhantering identifierar att du har en ny, ej initierad disk och initiera Disk-fönstret visas.</span><span class="sxs-lookup"><span data-stu-id="96388-124">Disk Management will recognize that you have a new, un-initialized disk and the Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="96388-125">Kontrollera att den nya disken är markerad och klicka på **OK** att initiera den.</span><span class="sxs-lookup"><span data-stu-id="96388-125">Make sure the new disk is selected and click **OK** to initialize it.</span></span>
4. <span data-ttu-id="96388-126">Den nya disken visas nu som **lediga**.</span><span class="sxs-lookup"><span data-stu-id="96388-126">The new disk will now appear as **unallocated**.</span></span> <span data-ttu-id="96388-127">Högerklicka på disken och välj **ny enkel volym**.</span><span class="sxs-lookup"><span data-stu-id="96388-127">Right-click anywhere on the disk and select **New simple volume**.</span></span> <span data-ttu-id="96388-128">Den **guiden Ny enkel volym** startar.</span><span class="sxs-lookup"><span data-stu-id="96388-128">The **New Simple Volume Wizard** will start.</span></span>
5. <span data-ttu-id="96388-129">Gå igenom guiden och hålla alla standardinställningar, när du är klar väljer **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="96388-129">Go through the wizard, keeping all of the defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="96388-130">Stäng Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="96388-130">Close Disk Management.</span></span>
7. <span data-ttu-id="96388-131">Du får ett popup-fönster som du vill formatera den nya disken innan du kan använda den.</span><span class="sxs-lookup"><span data-stu-id="96388-131">You will get a pop-up that you need to format the new disk before you can use it.</span></span> <span data-ttu-id="96388-132">Klicka på **Formatera disk**.</span><span class="sxs-lookup"><span data-stu-id="96388-132">Click **Format disk**.</span></span>
8. <span data-ttu-id="96388-133">I den **Format ny disk** dialogrutan Kontrollera inställningarna och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="96388-133">In the **Format new disk** dialog, check the settings and then click **Start**.</span></span>
9. <span data-ttu-id="96388-134">Du får en varning om att formatera diskarna kommer att radera alla data, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="96388-134">You will get a warning that formatting the disks will erase all of the data, click **OK**.</span></span>
10. <span data-ttu-id="96388-135">När formatet är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="96388-135">When the format is complete, click **OK**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="96388-136">Använd Rensa med standardlagring</span><span class="sxs-lookup"><span data-stu-id="96388-136">Use TRIM with standard storage</span></span>

<span data-ttu-id="96388-137">Om du använder standardlagring (HDD), bör du aktivera TRIMNING.</span><span class="sxs-lookup"><span data-stu-id="96388-137">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="96388-138">TRIMNING ignorerar oanvända block på disken så att endast debiteras du för lagring som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="96388-138">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="96388-139">Om du skapar stora filer och ta bort dem kan detta spara på kostnader.</span><span class="sxs-lookup"><span data-stu-id="96388-139">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="96388-140">Du kan köra det här kommandot för att kontrollera TRIM inställningen.</span><span class="sxs-lookup"><span data-stu-id="96388-140">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="96388-141">Öppna en kommandotolk på din Windows-VM och skriv:</span><span class="sxs-lookup"><span data-stu-id="96388-141">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="96388-142">Om kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="96388-142">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="96388-143">Om den returnerar 1, kör du följande kommando för att aktivera TRIMNING:</span><span class="sxs-lookup"><span data-stu-id="96388-143">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="96388-144">När du tar bort data från disken, kan du se till åtgärderna TRIM tömning korrekt genom att köra defragmenterar med TRIMNING:</span><span class="sxs-lookup"><span data-stu-id="96388-144">After deleting data from your disk, you can ensure the TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="96388-145">Du kan också kontrollera hela volymen trimmas genom att formatera volymen.</span><span class="sxs-lookup"><span data-stu-id="96388-145">You can also ensure the entire volume is trimmed by formatting the volume.</span></span>

## <a name="next-steps"></a><span data-ttu-id="96388-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96388-146">Next steps</span></span>
<span data-ttu-id="96388-147">Om du program behöver använda D: enhet för att lagra data, kan du [ändra enhetsbeteckningen för den tillfälliga disken i Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="96388-147">If you application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
