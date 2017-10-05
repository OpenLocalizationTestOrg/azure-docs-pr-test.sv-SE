---
title: Ansluta en ohanterad datadisk till en Windows-VM - Azure | Microsoft Docs
description: "Så här kopplar du ny eller befintlig ohanterade datadisk till en virtuell Windows-dator i Azure-portalen med hjälp av Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: c0886302c82641f8cc1a00d3972870d58ba8afb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-an-unmanaged-data-disk-to-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="843d0-103">Hur du kopplar en ohanterad datadisk till en virtuell Windows-dator i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="843d0-103">How to attach an unmanaged data disk to a Windows VM in the Azure portal</span></span>

<span data-ttu-id="843d0-104">Den här artikeln visar hur du kopplar både nya och befintliga ohanterade diskar till Windows-datorer via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="843d0-104">This article shows you how to attach both new and existing unmanaged disks to Windows virtual machines through the Azure portal.</span></span> <span data-ttu-id="843d0-105">Du kan också [ansluta en datadisk med hjälp av PowerShell](./attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="843d0-105">You can also [attach a data disk using PowerShell](./attach-disk-ps.md).</span></span> <span data-ttu-id="843d0-106">Innan du gör detta kan du granska dessa tips:</span><span class="sxs-lookup"><span data-stu-id="843d0-106">Before you do this, review these tips:</span></span>

* <span data-ttu-id="843d0-107">Storleken på den virtuella datorn styr hur många datadiskar som du kan bifoga.</span><span class="sxs-lookup"><span data-stu-id="843d0-107">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="843d0-108">Mer information finns i [storlekar för virtuella datorer](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="843d0-108">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="843d0-109">Om du vill använda Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien.</span><span class="sxs-lookup"><span data-stu-id="843d0-109">To use Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="843d0-110">Du kan använda diskar från Premium- och Standard storage-konton med dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="843d0-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="843d0-111">Premium-lagring är tillgänglig i vissa regioner.</span><span class="sxs-lookup"><span data-stu-id="843d0-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="843d0-112">Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="843d0-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="843d0-113">För en ny disk behöver du inte skapa först eftersom Azure skapar den när du ansluter den.</span><span class="sxs-lookup"><span data-stu-id="843d0-113">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>


<span data-ttu-id="843d0-114">Du kan också [ansluta en datadisk med hjälp av Powershell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="843d0-114">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>


## <a name="find-the-virtual-machine"></a><span data-ttu-id="843d0-115">Hitta den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="843d0-115">Find the virtual machine</span></span>
1. <span data-ttu-id="843d0-116">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="843d0-116">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="843d0-117">Klicka på menyn till vänster **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="843d0-117">In the menu on the left, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="843d0-118">Välj den virtuella datorn i listan.</span><span class="sxs-lookup"><span data-stu-id="843d0-118">Select the virtual machine from the list.</span></span>
4. <span data-ttu-id="843d0-119">I bladet för virtuella datorer, klickar du på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="843d0-119">In the Virtual machines blade, click **Disks**.</span></span>
   
<span data-ttu-id="843d0-120">Fortsätt genom att följa anvisningarna för att bifoga antingen en [ny disk](#option-1-attach-a-new-disk) eller en [befintlig disk](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="843d0-120">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="843d0-121">Alternativ 1: Anslut och initiera en ny disk</span><span class="sxs-lookup"><span data-stu-id="843d0-121">Option 1: Attach and initialize a new disk</span></span>
1. <span data-ttu-id="843d0-122">På den **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="843d0-122">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="843d0-123">I den **bifoga hanterade diskar** bladet, ange ett namn för disken i **namn** och välj sedan **ny (tom disk)** i **typ av datakälla**.</span><span class="sxs-lookup"><span data-stu-id="843d0-123">In the **Attach managed disk** blade, type a name for the disk in **Name** and then select **New (empty disk)** in **Source type**.</span></span>
3. <span data-ttu-id="843d0-124">Under **lagringsbehållaren**, klicka på den **Bläddra** knappen och bläddra till lagringskontot och behållare där du vill att den nya virtuella Hårddisken lagras och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="843d0-124">Under **Storage container**, click the **Browse** button and browse to the storage account and container where you would like the new VHD to be stored and then click **Select**.</span></span> 
  
   ![Granska diskinställningarna för](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. <span data-ttu-id="843d0-126">När du är klar med inställningarna för datadisken klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="843d0-126">When you are done with the settings for the data disk, click **OK**.</span></span>
4. <span data-ttu-id="843d0-127">I den **diskar** bladet, klickar du på **spara** att lägga till disken i VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="843d0-127">Back in the **Disks** blade, click **Save** to add the disk to the VM configuration.</span></span>


### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="843d0-128">Initiera en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="843d0-128">Initialize a new data disk</span></span>

1. <span data-ttu-id="843d0-129">Ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="843d0-129">Connect to the virtual machine.</span></span> <span data-ttu-id="843d0-130">Instruktioner finns i [ansluta och logga in på en virtuell Azure-dator kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="843d0-130">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
1. <span data-ttu-id="843d0-131">Klicka på den **starta** menyn inuti den virtuella datorn och skriv **diskmgmt.msc** och träffar **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="843d0-131">Click the **Start** menu inside the VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="843d0-132">Detta startar snapin-modulen Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="843d0-132">This starts the Disk Management snap-in.</span></span>
2. <span data-ttu-id="843d0-133">Diskhantering märker att du har en ny, oinitierad disk och initiera Disk-fönstret visas.</span><span class="sxs-lookup"><span data-stu-id="843d0-133">Disk Management recognizes that you have a new, uninitialized disk and the Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="843d0-134">Kontrollera att den nya disken är markerad och klicka på **OK** att initiera den.</span><span class="sxs-lookup"><span data-stu-id="843d0-134">Make sure the new disk is selected and click **OK** to initialize it.</span></span>
4. <span data-ttu-id="843d0-135">Den nya disken visas nu som **lediga**.</span><span class="sxs-lookup"><span data-stu-id="843d0-135">The new disk now appears as **unallocated**.</span></span> <span data-ttu-id="843d0-136">Högerklicka på disken och välj **ny enkel volym**.</span><span class="sxs-lookup"><span data-stu-id="843d0-136">Right-click anywhere on the disk and select **New simple volume**.</span></span> <span data-ttu-id="843d0-137">Den **guiden Ny enkel volym** startar.</span><span class="sxs-lookup"><span data-stu-id="843d0-137">The **New Simple Volume Wizard** starts.</span></span>
5. <span data-ttu-id="843d0-138">Gå igenom guiden och hålla alla standardinställningar, när du är klar väljer **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="843d0-138">Go through the wizard, keeping all of the defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="843d0-139">Stäng Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="843d0-139">Close Disk Management.</span></span>
7. <span data-ttu-id="843d0-140">Du får ett popup-fönster som du vill formatera den nya disken innan du kan använda den.</span><span class="sxs-lookup"><span data-stu-id="843d0-140">You get a pop-up that you need to format the new disk before you can use it.</span></span> <span data-ttu-id="843d0-141">Klicka på **Formatera disk**.</span><span class="sxs-lookup"><span data-stu-id="843d0-141">Click **Format disk**.</span></span>
8. <span data-ttu-id="843d0-142">I den **Format ny disk** dialogrutan Kontrollera inställningarna och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="843d0-142">In the **Format new disk** dialog, check the settings and then click **Start**.</span></span>
9. <span data-ttu-id="843d0-143">Du får en varning om att diskarna formateras tas alla data, klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="843d0-143">You get a warning that formatting the disks erases all of the data, click **OK**.</span></span>
10. <span data-ttu-id="843d0-144">När formatet är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="843d0-144">When the format is complete, click **OK**.</span></span>


## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="843d0-145">Alternativ 2: Bifoga en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="843d0-145">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="843d0-146">På den **diskar** bladet, klickar du på **+ Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="843d0-146">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="843d0-147">På den **bifoga ohanterade disk** bladet i **typ av datakälla** Välj **befintlig blob**.</span><span class="sxs-lookup"><span data-stu-id="843d0-147">On the **Attach unmanaged disk** blade, in **Source type** select **Existing blob**.</span></span>

    ![Granska diskinställningarna för](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. <span data-ttu-id="843d0-149">Klicka på **Bläddra** att navigera till lagringskontot och där det finns befintliga VHD-behållare.</span><span class="sxs-lookup"><span data-stu-id="843d0-149">Click **Browse** to navigate to the storage account and container where the existing VHD is located.</span></span> <span data-ttu-id="843d0-150">Klicka på och VHD- och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="843d0-150">Click and the VHD and then click **Select**.</span></span>
4. <span data-ttu-id="843d0-151">Klicka på **OK** i den **bifoga ohanterade disk** bladet.</span><span class="sxs-lookup"><span data-stu-id="843d0-151">Click **OK** in the **Attach unmanaged disk** blade.</span></span>
5. <span data-ttu-id="843d0-152">I den **diskar** bladet, klickar du på **spara** att lägga till disken i konfigurationen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="843d0-152">In the **Disks** blade, click **Save** to add the disk to the configuration for the VM.</span></span>
   


## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="843d0-153">Använd Rensa med standardlagring</span><span class="sxs-lookup"><span data-stu-id="843d0-153">Use TRIM with standard storage</span></span>

<span data-ttu-id="843d0-154">Om du använder standardlagring (HDD), bör du aktivera TRIMNING.</span><span class="sxs-lookup"><span data-stu-id="843d0-154">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="843d0-155">TRIMNING ignorerar oanvända block på disken så att endast debiteras du för lagring som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="843d0-155">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="843d0-156">Om du skapar stora filer och ta bort dem kan detta spara på kostnader.</span><span class="sxs-lookup"><span data-stu-id="843d0-156">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="843d0-157">Du kan köra det här kommandot för att kontrollera TRIM inställningen.</span><span class="sxs-lookup"><span data-stu-id="843d0-157">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="843d0-158">Öppna en kommandotolk på din Windows-VM och skriv:</span><span class="sxs-lookup"><span data-stu-id="843d0-158">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="843d0-159">Om kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="843d0-159">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="843d0-160">Om den returnerar 1, kör du följande kommando för att aktivera TRIMNING:</span><span class="sxs-lookup"><span data-stu-id="843d0-160">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="843d0-161">När du tar bort data från disken, kan du se till åtgärderna TRIM tömning korrekt genom att köra defragmenterar med TRIMNING:</span><span class="sxs-lookup"><span data-stu-id="843d0-161">After deleting data from your disk, you can ensure the TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="843d0-162">Du kan också kontrollera hela volymen trimmas genom att formatera volymen.</span><span class="sxs-lookup"><span data-stu-id="843d0-162">You can also ensure the entire volume is trimmed by formatting the volume.</span></span>


## <a name="next-steps"></a><span data-ttu-id="843d0-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="843d0-163">Next steps</span></span>
<span data-ttu-id="843d0-164">Om du program behöver använda D: enhet för att lagra data, kan du [ändra enhetsbeteckningen för den tillfälliga disken i Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="843d0-164">If you application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

