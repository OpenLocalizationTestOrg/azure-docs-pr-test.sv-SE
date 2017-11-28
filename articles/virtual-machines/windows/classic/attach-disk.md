---
title: Ansluta en disk till en klassisk Azure-VM | Microsoft Docs
description: Ansluta en datadisk till en Windows-dator som skapats med den klassiska distributionsmodellen och initiera den.
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
ms.openlocfilehash: 087d5cda354f6e1780bddd3725859444177abd16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a><span data-ttu-id="58f4d-103">Koppla en datadisk till en virtuell Windows-dator skapad med den klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="58f4d-103">Attach a data disk to a Windows virtual machine created with the classic deployment model</span></span>
<!--
Refernce article:
    If you want to use the new portal, see [How to attach a data disk to a Windows VM in the Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

<span data-ttu-id="58f4d-104">Den här artikeln visar hur du ansluter nya och befintliga diskar som skapats med den klassiska distributionsmodellen till en Windows-dator med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="58f4d-104">This article shows you how to attach new and existing disks created with the Classic deployment model to a Windows virtual machine using the Azure portal.</span></span>

<span data-ttu-id="58f4d-105">Du kan också [ansluta en datadisk till en Linux-VM i Azure portal](../../linux/attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="58f4d-105">You can also [attach a data disk to a Linux VM in the Azure portal](../../linux/attach-disk-portal.md).</span></span>

<span data-ttu-id="58f4d-106">Innan du ansluter en disk, granska de här tipsen:</span><span class="sxs-lookup"><span data-stu-id="58f4d-106">Before you attach a disk, review these tips:</span></span>

* <span data-ttu-id="58f4d-107">Storleken på den virtuella datorn styr hur många datadiskar som du kan bifoga.</span><span class="sxs-lookup"><span data-stu-id="58f4d-107">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="58f4d-108">Mer information finns i [storlekar för virtuella datorer](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58f4d-108">For details, see [Sizes for virtual machines](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="58f4d-109">Om du vill använda Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien.</span><span class="sxs-lookup"><span data-stu-id="58f4d-109">To use Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="58f4d-110">Du kan använda diskar från Premium- och Standard storage-konton med dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="58f4d-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="58f4d-111">Premium-lagring är tillgänglig i vissa regioner.</span><span class="sxs-lookup"><span data-stu-id="58f4d-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="58f4d-112">Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58f4d-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="58f4d-113">För en ny disk behöver du inte skapa först eftersom Azure skapar den när du ansluter den.</span><span class="sxs-lookup"><span data-stu-id="58f4d-113">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="58f4d-114">Du kan också [ansluta en datadisk med hjälp av Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="58f4d-114">You can also [attach a data disk using Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58f4d-115">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="58f4d-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>

## <a name="find-the-virtual-machine"></a><span data-ttu-id="58f4d-116">Hitta den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="58f4d-116">Find the virtual machine</span></span>
1. <span data-ttu-id="58f4d-117">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="58f4d-117">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="58f4d-118">Välj den virtuella datorn från resursen som visas på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="58f4d-118">Select the virtual machine from the resource listed on the dashboard.</span></span>
3. <span data-ttu-id="58f4d-119">I det vänstra fönstret under **inställningar**, klickar du på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-119">In the left pane under **Settings**, click **Disks**.</span></span>

    ![Öppna diskinställningarna för](./media/attach-disk/virtualmachinedisks.png)

<span data-ttu-id="58f4d-121">Fortsätt genom att följa anvisningarna för att bifoga antingen en [ny disk](#option-1-attach-a-new-disk) eller en [befintlig disk](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="58f4d-121">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="58f4d-122">Alternativ 1: Anslut och initiera en ny disk</span><span class="sxs-lookup"><span data-stu-id="58f4d-122">Option 1: Attach and initialize a new disk</span></span>

1. <span data-ttu-id="58f4d-123">På den **diskar** bladet, klickar du på **bifoga nya**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-123">On the **Disks** blade, click **Attach new**.</span></span>
2. <span data-ttu-id="58f4d-124">Granska standardinställningarna, uppdatera efter behov och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-124">Review the default settings, update as necessary, and then click **OK**.</span></span>

   ![Granska diskinställningarna för](./media/attach-disk/attach-new.png)

3. <span data-ttu-id="58f4d-126">När Azure skapar disken och kopplar den till den virtuella datorn, den nya disken visas i inställningarna för den virtuella disken under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-126">After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="58f4d-127">Initiera en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="58f4d-127">Initialize a new data disk</span></span>

1. <span data-ttu-id="58f4d-128">Ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="58f4d-128">Connect to the virtual machine.</span></span> <span data-ttu-id="58f4d-129">Instruktioner finns i [ansluta och logga in på en virtuell Azure-dator kör Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58f4d-129">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="58f4d-130">När du loggar in på den virtuella datorn öppnar **Serverhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-130">After you log on to the virtual machine, open **Server Manager**.</span></span> <span data-ttu-id="58f4d-131">I den vänstra rutan, Välj **fil- och lagringstjänster**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-131">In the left pane, select **File and Storage Services**.</span></span>

    ![Öppna Serverhanteraren](../media/attach-disk-portal/fileandstorageservices.png)

3. <span data-ttu-id="58f4d-133">Välj **diskar**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-133">Select **Disks**.</span></span>
4. <span data-ttu-id="58f4d-134">Den **diskar** innehåller diskarna.</span><span class="sxs-lookup"><span data-stu-id="58f4d-134">The **Disks** section lists the disks.</span></span> <span data-ttu-id="58f4d-135">En virtuell dator har mest ofta disk 0, disk 1 och disk 2.</span><span class="sxs-lookup"><span data-stu-id="58f4d-135">Most often, a virtual machine has disk 0, disk 1, and disk 2.</span></span> <span data-ttu-id="58f4d-136">0 är operativsystemdisk, disk 1 är en tillfällig disk och 2 är datadisk som nyligen kopplade till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="58f4d-136">Disk 0 is the operating system disk, disk 1 is the temporary disk, and disk 2 is the data disk newly attached to the virtual machine.</span></span> <span data-ttu-id="58f4d-137">Datadisken visar partitionen som **okänd**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-137">The data disk lists the Partition as **Unknown**.</span></span>

 <span data-ttu-id="58f4d-138">Högerklicka på disken och markera **initiera**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-138">Right-click the disk and select **Initialize**.</span></span>

5. <span data-ttu-id="58f4d-139">Du meddelas att alla data raderas när disken initieras.</span><span class="sxs-lookup"><span data-stu-id="58f4d-139">You're notified that all data will be erased when the disk is initialized.</span></span> <span data-ttu-id="58f4d-140">Klicka på **Ja** att bekräfta varningen och initiera disken.</span><span class="sxs-lookup"><span data-stu-id="58f4d-140">Click **Yes** to acknowledge the warning and initialize the disk.</span></span> <span data-ttu-id="58f4d-141">När installationen är klar visas partitionen som **GPT**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-141">Once complete, the partition will be listed as **GPT**.</span></span> <span data-ttu-id="58f4d-142">Högerklicka på disken igen och välj **ny volym**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-142">Right-click the disk again and select **New Volume**.</span></span>

6. <span data-ttu-id="58f4d-143">Slutför guiden med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="58f4d-143">Complete the wizard using the default values.</span></span> <span data-ttu-id="58f4d-144">När guiden är klar i **volymer** avsnitt visas den nya volymen.</span><span class="sxs-lookup"><span data-stu-id="58f4d-144">When the wizard is done, the **Volumes** section lists the new volume.</span></span> <span data-ttu-id="58f4d-145">Disken är nu online och redo att lagra data.</span><span class="sxs-lookup"><span data-stu-id="58f4d-145">The disk is now online and ready to store data.</span></span>

    ![Volymen har startats](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="58f4d-147">Alternativ 2: Bifoga en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="58f4d-147">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="58f4d-148">På den **diskar** bladet, klickar du på **bifoga befintliga**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-148">On the **Disks** blade, click **Attach existing**.</span></span>
2. <span data-ttu-id="58f4d-149">Under **bifoga den befintliga disken**, klickar du på **plats**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-149">Under **Attach existing disk**, click **Location**.</span></span>

   ![Bifoga den befintliga disken](./media/attach-disk/attachexistingdisksettings.png)
3. <span data-ttu-id="58f4d-151">Under **lagringskonton**, Välj det konto och behållare som innehåller VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="58f4d-151">Under **Storage accounts**, select the account and container that holds the .vhd file.</span></span>

   ![Hitta VHD-plats](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. <span data-ttu-id="58f4d-153">Välj VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="58f4d-153">Select the .vhd file.</span></span>
5. <span data-ttu-id="58f4d-154">Under **bifoga den befintliga disken**, den markerade filen finns under **VHD-filen**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-154">Under **Attach existing disk**, the file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="58f4d-155">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-155">Click **OK**.</span></span>
6. <span data-ttu-id="58f4d-156">När Azure bifogar disken till den virtuella datorn, visas den i inställningarna för den virtuella disken under **Datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="58f4d-156">After Azure attaches the disk to the virtual machine, it's listed in the virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="58f4d-157">Använd Rensa med standardlagring</span><span class="sxs-lookup"><span data-stu-id="58f4d-157">Use TRIM with standard storage</span></span>

<span data-ttu-id="58f4d-158">Om du använder standardlagring (HDD), bör du aktivera TRIMNING.</span><span class="sxs-lookup"><span data-stu-id="58f4d-158">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="58f4d-159">TRIMNING ignorerar oanvända block på disken så att endast debiteras du för lagring som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="58f4d-159">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="58f4d-160">Med TRIMNING spara kostnader, inklusive oanvända block som uppstår vid borttagning av stora filer.</span><span class="sxs-lookup"><span data-stu-id="58f4d-160">Using TRIM can save costs, including unused blocks that result from deleting large files.</span></span>

<span data-ttu-id="58f4d-161">Du kan köra det här kommandot för att kontrollera TRIM inställningen.</span><span class="sxs-lookup"><span data-stu-id="58f4d-161">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="58f4d-162">Öppna en kommandotolk på din Windows-VM och skriv:</span><span class="sxs-lookup"><span data-stu-id="58f4d-162">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="58f4d-163">Om kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="58f4d-163">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="58f4d-164">Om den returnerar 1, kör du följande kommando för att aktivera TRIMNING:</span><span class="sxs-lookup"><span data-stu-id="58f4d-164">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a><span data-ttu-id="58f4d-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58f4d-165">Next steps</span></span>
<span data-ttu-id="58f4d-166">Om ditt program behöver använda D: enhet för att lagra data, kan du [ändra enhetsbeteckningen för den tillfälliga disken i Windows](../../virtual-machines-windows-change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="58f4d-166">If your application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](../../virtual-machines-windows-change-drive-letter.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58f4d-167">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="58f4d-167">Additional resources</span></span>
[<span data-ttu-id="58f4d-168">Om diskar och virtuella hårddiskar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="58f4d-168">About disks and VHDs for virtual machines</span></span>](../../virtual-machines-linux-about-disks-vhds.md)
