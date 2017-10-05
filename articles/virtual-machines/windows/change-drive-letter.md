---
title: "Gör att enheten D: för en virtuell dator en datadisk | Microsoft Docs"
description: "Beskriver hur du ändrar enhetsbeteckningar för en virtuell Windows-dator så att du kan använda D: enheten som en dataenhet."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: cynthn
ms.openlocfilehash: 7667175c01be2421bfc3badd83b1d8aaeb29bfde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a><span data-ttu-id="05014-103">Använda D: enheten som en dataenhet på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="05014-103">Use the D: drive as a data drive on a Windows VM</span></span>
<span data-ttu-id="05014-104">Om ditt program behöver använda enhet D för att lagra data, följer du anvisningarna för att använda en annan enhetsbeteckning för den tillfälliga disken.</span><span class="sxs-lookup"><span data-stu-id="05014-104">If your application needs to use the D drive to store data, follow these instructions to use a different drive letter for the temporary disk.</span></span> <span data-ttu-id="05014-105">Använd aldrig tillfälliga disken för att lagra data som du behöver.</span><span class="sxs-lookup"><span data-stu-id="05014-105">Never use the temporary disk to store data that you need to keep.</span></span>

<span data-ttu-id="05014-106">Om du ändrar storlek eller **stoppa (Deallocate)** en virtuell dator, detta kan utlösa placeringen av den virtuella datorn till en ny hypervisor.</span><span class="sxs-lookup"><span data-stu-id="05014-106">If you resize or **Stop (Deallocate)** a virtual machine, this may trigger placement of the virtual machine to a new hypervisor.</span></span> <span data-ttu-id="05014-107">En planerad eller oplanerad underhållshändelse kan också utlösa den här placering.</span><span class="sxs-lookup"><span data-stu-id="05014-107">A planned or unplanned maintenance event may also trigger this placement.</span></span> <span data-ttu-id="05014-108">I det här scenariot tilldelas tillfälliga disken den första tillgängliga enhetsbeteckningen.</span><span class="sxs-lookup"><span data-stu-id="05014-108">In this scenario, the temporary disk will be reassigned to the first available drive letter.</span></span> <span data-ttu-id="05014-109">Om du har ett program som kräver mer specifikt D: enhet måste följa dessa steg för att tillfälligt flytta pagefile.sys, ansluta en ny datadisk och tilldela den bokstaven D och flytta pagefile.sys tillbaka till den tillfälliga enheten.</span><span class="sxs-lookup"><span data-stu-id="05014-109">If you have an application that specifically requires the D: drive, you need to follow these steps to temporarily move the pagefile.sys, attach a new data disk and assign it the letter D and then move the pagefile.sys back to the temporary drive.</span></span> <span data-ttu-id="05014-110">När du är klar, träder Azure inte tillbaka D: om den virtuella datorn flyttas till en annan hypervisor.</span><span class="sxs-lookup"><span data-stu-id="05014-110">Once complete, Azure will not take back the D: if the VM moves to a different hypervisor.</span></span>

<span data-ttu-id="05014-111">Mer information om hur Azure använder temporär disk finns [förstå den tillfälliga enheten på virtuella datorer i Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="05014-111">For more information about how Azure uses the temporary disk, see [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>

## <a name="attach-the-data-disk"></a><span data-ttu-id="05014-112">Bifoga datadisken</span><span class="sxs-lookup"><span data-stu-id="05014-112">Attach the data disk</span></span>
<span data-ttu-id="05014-113">Först måste du bifoga datadisken för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="05014-113">First, you'll need to attach the data disk to the virtual machine.</span></span> <span data-ttu-id="05014-114">Om du vill göra detta med hjälp av portalen finns [så här kopplar du en datadisk hanterade i Azure portal](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="05014-114">To do this using the portal, see [How to attach a managed data disk in the Azure portal](attach-managed-disk-portal.md).</span></span>

## <a name="temporarily-move-pagefilesys-to-c-drive"></a><span data-ttu-id="05014-115">Tillfälligt flytta pagefile.sys till C-enheten</span><span class="sxs-lookup"><span data-stu-id="05014-115">Temporarily move pagefile.sys to C drive</span></span>
1. <span data-ttu-id="05014-116">Ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="05014-116">Connect to the virtual machine.</span></span> 
2. <span data-ttu-id="05014-117">Högerklicka på den **starta** -menyn och välj **System**.</span><span class="sxs-lookup"><span data-stu-id="05014-117">Right-click the **Start** menu and select **System**.</span></span>
3. <span data-ttu-id="05014-118">Välj i den vänstra menyn **avancerade systeminställningar**.</span><span class="sxs-lookup"><span data-stu-id="05014-118">In the left-hand menu, select **Advanced system settings**.</span></span>
4. <span data-ttu-id="05014-119">I den **prestanda** väljer **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="05014-119">In the **Performance** section, select **Settings**.</span></span>
5. <span data-ttu-id="05014-120">Välj den **Avancerat** fliken.</span><span class="sxs-lookup"><span data-stu-id="05014-120">Select the **Advanced** tab.</span></span>
6. <span data-ttu-id="05014-121">I den **virtuellt minne** väljer **ändra**.</span><span class="sxs-lookup"><span data-stu-id="05014-121">In the **Virtual memory** section, select **Change**.</span></span>
7. <span data-ttu-id="05014-122">Välj den **C** enhet och klicka sedan på **storleken** och klicka sedan på **ange**.</span><span class="sxs-lookup"><span data-stu-id="05014-122">Select the **C** drive and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="05014-123">Välj den **D** enhet och klicka sedan på **Ingen växlingsfil** och klicka sedan på **ange**.</span><span class="sxs-lookup"><span data-stu-id="05014-123">Select the **D** drive and then click **No paging file** and then click **Set**.</span></span>
9. <span data-ttu-id="05014-124">Klicka på Använd.</span><span class="sxs-lookup"><span data-stu-id="05014-124">Click Apply.</span></span> <span data-ttu-id="05014-125">Du får en varning om att datorn måste startas om för att ändringarna ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="05014-125">You will get a warning that the computer needs to be restarted for the changes to take affect.</span></span>
10. <span data-ttu-id="05014-126">Starta om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="05014-126">Restart the virtual machine.</span></span>

## <a name="change-the-drive-letters"></a><span data-ttu-id="05014-127">Ändra enhetsbeteckningarna</span><span class="sxs-lookup"><span data-stu-id="05014-127">Change the drive letters</span></span>
1. <span data-ttu-id="05014-128">När den virtuella datorn har startats om loggar du in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="05014-128">Once the VM restarts, log back on to the VM.</span></span>
2. <span data-ttu-id="05014-129">Klicka på den **starta** -menyn och skriv **diskmgmt.msc** och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="05014-129">Click the **Start** menu and type **diskmgmt.msc** and hit Enter.</span></span> <span data-ttu-id="05014-130">Diskhantering startar.</span><span class="sxs-lookup"><span data-stu-id="05014-130">Disk Management will start.</span></span>
3. <span data-ttu-id="05014-131">Högerklicka på **D**, tillfällig lagringsenhet och välj **ändra enhetsbeteckning och sökvägar**.</span><span class="sxs-lookup"><span data-stu-id="05014-131">Right-click on **D**, the Temporary Storage drive, and select **Change Drive Letter and Paths**.</span></span>
4. <span data-ttu-id="05014-132">Under enhetsbeteckning, väljer en ny enhet som **T** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="05014-132">Under Drive letter, select a new drive such as **T** and then click **OK**.</span></span> 
5. <span data-ttu-id="05014-133">Högerklicka på datadisken och välj **ändra enhetsbeteckning och sökvägar**.</span><span class="sxs-lookup"><span data-stu-id="05014-133">Right-click on the data disk, and select **Change Drive Letter and Paths**.</span></span>
6. <span data-ttu-id="05014-134">Välj enhet under enhetsbeteckning **D** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="05014-134">Under Drive letter, select drive **D** and then click **OK**.</span></span> 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a><span data-ttu-id="05014-135">Flytta pagefile.sys tillbaka till tillfällig lagring-enhet</span><span class="sxs-lookup"><span data-stu-id="05014-135">Move pagefile.sys back to the temporary storage drive</span></span>
1. <span data-ttu-id="05014-136">Högerklicka på den **starta** -menyn och välj **System**</span><span class="sxs-lookup"><span data-stu-id="05014-136">Right-click the **Start** menu and select **System**</span></span>
2. <span data-ttu-id="05014-137">Välj i den vänstra menyn **avancerade systeminställningar**.</span><span class="sxs-lookup"><span data-stu-id="05014-137">In the left-hand menu, select **Advanced system settings**.</span></span>
3. <span data-ttu-id="05014-138">I den **prestanda** väljer **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="05014-138">In the **Performance** section, select **Settings**.</span></span>
4. <span data-ttu-id="05014-139">Välj den **Avancerat** fliken.</span><span class="sxs-lookup"><span data-stu-id="05014-139">Select the **Advanced** tab.</span></span>
5. <span data-ttu-id="05014-140">I den **virtuellt minne** väljer **ändra**.</span><span class="sxs-lookup"><span data-stu-id="05014-140">In the **Virtual memory** section, select **Change**.</span></span>
6. <span data-ttu-id="05014-141">Välj OS-enhet **C** och på **Ingen växlingsfil** och klicka sedan på **ange**.</span><span class="sxs-lookup"><span data-stu-id="05014-141">Select the OS drive **C** and click **No paging file** and then click **Set**.</span></span>
7. <span data-ttu-id="05014-142">Välj enhet för tillfällig lagring **T** och klicka sedan på **storleken** och klicka sedan på **ange**.</span><span class="sxs-lookup"><span data-stu-id="05014-142">Select the temporary storage drive **T** and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="05014-143">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="05014-143">Click **Apply**.</span></span> <span data-ttu-id="05014-144">Du får en varning om att datorn måste startas om för att ändringarna ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="05014-144">You will get a warning that the computer needs to be restarted for the changes to take affect.</span></span>
9. <span data-ttu-id="05014-145">Starta om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="05014-145">Restart the virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05014-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05014-146">Next steps</span></span>
* <span data-ttu-id="05014-147">Du kan öka tillgängligt lagringsutrymme till den virtuella datorn av [koppla en ytterligare datadisk](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="05014-147">You can increase the storage available to your virtual machine by [attaching a additional data disk](attach-managed-disk-portal.md).</span></span>

