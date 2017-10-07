---
title: "Se hello D: enhet på en virtuell dator en datadisk | Microsoft Docs"
description: "Beskriver hur toochange enhetsbeteckningar för en virtuell Windows-dator så att du kan använda hello D: enhet som en dataenhet."
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
ms.openlocfilehash: 43939da1a890ac4049266487951e3889aa0ed9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-d-drive-as-a-data-drive-on-a-windows-vm"></a><span data-ttu-id="238fe-103">Använda hello D: enheten som en dataenhet på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="238fe-103">Use hello D: drive as a data drive on a Windows VM</span></span>
<span data-ttu-id="238fe-104">Om ditt program måste toouse hello D enhet toostore data, följer du dessa anvisningar toouse en annan enhetsbeteckning för hello diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="238fe-104">If your application needs toouse hello D drive toostore data, follow these instructions toouse a different drive letter for hello temporary disk.</span></span> <span data-ttu-id="238fe-105">Använd aldrig hello tillfälliga toostore diskdata måste tookeep.</span><span class="sxs-lookup"><span data-stu-id="238fe-105">Never use hello temporary disk toostore data that you need tookeep.</span></span>

<span data-ttu-id="238fe-106">Om du ändrar storlek eller **stoppa (Deallocate)** en virtuell dator, detta kan utlösa placeringen av hello virtuella tooa nya hypervisor.</span><span class="sxs-lookup"><span data-stu-id="238fe-106">If you resize or **Stop (Deallocate)** a virtual machine, this may trigger placement of hello virtual machine tooa new hypervisor.</span></span> <span data-ttu-id="238fe-107">En planerad eller oplanerad underhållshändelse kan också utlösa den här placering.</span><span class="sxs-lookup"><span data-stu-id="238fe-107">A planned or unplanned maintenance event may also trigger this placement.</span></span> <span data-ttu-id="238fe-108">I det här scenariot hello diskutrymme inte omtilldelas toohello första tillgängliga enhetsbeteckning.</span><span class="sxs-lookup"><span data-stu-id="238fe-108">In this scenario, hello temporary disk will be reassigned toohello first available drive letter.</span></span> <span data-ttu-id="238fe-109">Om du har ett program som kräver mer specifikt hello D: enhet behöver du toofollow dessa steg tootemporarily flytta hello pagefile.sys ansluta en ny datadisk och tilldela den hello bokstaven D och flytta hello pagefile.sys tillbaka toohello tillfälliga enheten.</span><span class="sxs-lookup"><span data-stu-id="238fe-109">If you have an application that specifically requires hello D: drive, you need toofollow these steps tootemporarily move hello pagefile.sys, attach a new data disk and assign it hello letter D and then move hello pagefile.sys back toohello temporary drive.</span></span> <span data-ttu-id="238fe-110">När du är klar, träder Azure inte tillbaka hello D: om hello VM flyttas tooa olika hypervisor.</span><span class="sxs-lookup"><span data-stu-id="238fe-110">Once complete, Azure will not take back hello D: if hello VM moves tooa different hypervisor.</span></span>

<span data-ttu-id="238fe-111">Mer information om hur Azure använder hello diskutrymme finns [förstå hello tillfälliga enheten på virtuella datorer i Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="238fe-111">For more information about how Azure uses hello temporary disk, see [Understanding hello temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>

## <a name="attach-hello-data-disk"></a><span data-ttu-id="238fe-112">Ansluta hello datadisk</span><span class="sxs-lookup"><span data-stu-id="238fe-112">Attach hello data disk</span></span>
<span data-ttu-id="238fe-113">Du måste först tooattach hello data disk toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="238fe-113">First, you'll need tooattach hello data disk toohello virtual machine.</span></span> <span data-ttu-id="238fe-114">toodo detta med hjälp av hello portal, se [hur tooattach hanterade data disken i hello Azure-portalen](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="238fe-114">toodo this using hello portal, see [How tooattach a managed data disk in hello Azure portal](attach-managed-disk-portal.md).</span></span>

## <a name="temporarily-move-pagefilesys-tooc-drive"></a><span data-ttu-id="238fe-115">Tillfälligt flytta pagefile.sys tooC enhet</span><span class="sxs-lookup"><span data-stu-id="238fe-115">Temporarily move pagefile.sys tooC drive</span></span>
1. <span data-ttu-id="238fe-116">Ansluta toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="238fe-116">Connect toohello virtual machine.</span></span> 
2. <span data-ttu-id="238fe-117">Högerklicka på hello **starta** -menyn och välj **System**.</span><span class="sxs-lookup"><span data-stu-id="238fe-117">Right-click hello **Start** menu and select **System**.</span></span>
3. <span data-ttu-id="238fe-118">Välj hello vänstra menyn **avancerade systeminställningar**.</span><span class="sxs-lookup"><span data-stu-id="238fe-118">In hello left-hand menu, select **Advanced system settings**.</span></span>
4. <span data-ttu-id="238fe-119">I hello **prestanda** väljer **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="238fe-119">In hello **Performance** section, select **Settings**.</span></span>
5. <span data-ttu-id="238fe-120">Välj hello **Avancerat** fliken.</span><span class="sxs-lookup"><span data-stu-id="238fe-120">Select hello **Advanced** tab.</span></span>
6. <span data-ttu-id="238fe-121">I hello **virtuellt minne** väljer **ändra**.</span><span class="sxs-lookup"><span data-stu-id="238fe-121">In hello **Virtual memory** section, select **Change**.</span></span>
7. <span data-ttu-id="238fe-122">Välj hello **C** enhet och klicka sedan på **storleken** och klicka sedan på **ange**.</span><span class="sxs-lookup"><span data-stu-id="238fe-122">Select hello **C** drive and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="238fe-123">Välj hello **D** enhet och klicka sedan på **Ingen växlingsfil** och klicka sedan på **ange**.</span><span class="sxs-lookup"><span data-stu-id="238fe-123">Select hello **D** drive and then click **No paging file** and then click **Set**.</span></span>
9. <span data-ttu-id="238fe-124">Klicka på Använd.</span><span class="sxs-lookup"><span data-stu-id="238fe-124">Click Apply.</span></span> <span data-ttu-id="238fe-125">Du får en varning om att hello-datorn måste startas om för att hello ändringar tootake påverkar toobe.</span><span class="sxs-lookup"><span data-stu-id="238fe-125">You will get a warning that hello computer needs toobe restarted for hello changes tootake affect.</span></span>
10. <span data-ttu-id="238fe-126">Starta om hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="238fe-126">Restart hello virtual machine.</span></span>

## <a name="change-hello-drive-letters"></a><span data-ttu-id="238fe-127">Ändra hello enhetsbeteckningar</span><span class="sxs-lookup"><span data-stu-id="238fe-127">Change hello drive letters</span></span>
1. <span data-ttu-id="238fe-128">En gång Hej VM omstarter, logga in igen toohello VM.</span><span class="sxs-lookup"><span data-stu-id="238fe-128">Once hello VM restarts, log back on toohello VM.</span></span>
2. <span data-ttu-id="238fe-129">Klicka på hello **starta** -menyn och skriv **diskmgmt.msc** och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="238fe-129">Click hello **Start** menu and type **diskmgmt.msc** and hit Enter.</span></span> <span data-ttu-id="238fe-130">Diskhantering startar.</span><span class="sxs-lookup"><span data-stu-id="238fe-130">Disk Management will start.</span></span>
3. <span data-ttu-id="238fe-131">Högerklicka på **D**hello tillfälliga lagringsenhet och välj **ändra enhetsbeteckning och sökvägar**.</span><span class="sxs-lookup"><span data-stu-id="238fe-131">Right-click on **D**, hello Temporary Storage drive, and select **Change Drive Letter and Paths**.</span></span>
4. <span data-ttu-id="238fe-132">Under enhetsbeteckning, väljer en ny enhet som **T** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="238fe-132">Under Drive letter, select a new drive such as **T** and then click **OK**.</span></span> 
5. <span data-ttu-id="238fe-133">Högerklicka på hello datadisk och välj **ändra enhetsbeteckning och sökvägar**.</span><span class="sxs-lookup"><span data-stu-id="238fe-133">Right-click on hello data disk, and select **Change Drive Letter and Paths**.</span></span>
6. <span data-ttu-id="238fe-134">Välj enhet under enhetsbeteckning **D** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="238fe-134">Under Drive letter, select drive **D** and then click **OK**.</span></span> 

## <a name="move-pagefilesys-back-toohello-temporary-storage-drive"></a><span data-ttu-id="238fe-135">Flytta pagefile.sys tillbaka toohello tillfälliga lagringsenhet</span><span class="sxs-lookup"><span data-stu-id="238fe-135">Move pagefile.sys back toohello temporary storage drive</span></span>
1. <span data-ttu-id="238fe-136">Högerklicka på hello **starta** -menyn och välj **System**</span><span class="sxs-lookup"><span data-stu-id="238fe-136">Right-click hello **Start** menu and select **System**</span></span>
2. <span data-ttu-id="238fe-137">Välj hello vänstra menyn **avancerade systeminställningar**.</span><span class="sxs-lookup"><span data-stu-id="238fe-137">In hello left-hand menu, select **Advanced system settings**.</span></span>
3. <span data-ttu-id="238fe-138">I hello **prestanda** väljer **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="238fe-138">In hello **Performance** section, select **Settings**.</span></span>
4. <span data-ttu-id="238fe-139">Välj hello **Avancerat** fliken.</span><span class="sxs-lookup"><span data-stu-id="238fe-139">Select hello **Advanced** tab.</span></span>
5. <span data-ttu-id="238fe-140">I hello **virtuellt minne** väljer **ändra**.</span><span class="sxs-lookup"><span data-stu-id="238fe-140">In hello **Virtual memory** section, select **Change**.</span></span>
6. <span data-ttu-id="238fe-141">Välj hello OS enhet **C** och på **Ingen växlingsfil** och klicka sedan på **ange**.</span><span class="sxs-lookup"><span data-stu-id="238fe-141">Select hello OS drive **C** and click **No paging file** and then click **Set**.</span></span>
7. <span data-ttu-id="238fe-142">Välj hello tillfällig lagringsenhet **T** och klicka sedan på **storleken** och klicka sedan på **ange**.</span><span class="sxs-lookup"><span data-stu-id="238fe-142">Select hello temporary storage drive **T** and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="238fe-143">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="238fe-143">Click **Apply**.</span></span> <span data-ttu-id="238fe-144">Du får en varning om att hello-datorn måste startas om för att hello ändringar tootake påverkar toobe.</span><span class="sxs-lookup"><span data-stu-id="238fe-144">You will get a warning that hello computer needs toobe restarted for hello changes tootake affect.</span></span>
9. <span data-ttu-id="238fe-145">Starta om hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="238fe-145">Restart hello virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="238fe-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="238fe-146">Next steps</span></span>
* <span data-ttu-id="238fe-147">Du kan öka hello lagring tillgänglig tooyour virtuella datorn med [koppla en ytterligare datadisk](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="238fe-147">You can increase hello storage available tooyour virtual machine by [attaching a additional data disk](attach-managed-disk-portal.md).</span></span>

