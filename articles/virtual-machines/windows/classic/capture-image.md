---
title: aaaCapture en avbildning av en Windows Azure-dator | Microsoft Docs
description: "Hämta en avbildning av en virtuell dator för Windows Azure som skapats med hello klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: b9bbc437012aa44295f90941c9d72e39509df28f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="00656-103">Hämta en avbildning av en virtuell dator för Windows Azure som skapats med hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="00656-103">Capture an image of an Azure Windows virtual machine created with hello classic deployment model.</span></span>
> [!IMPORTANT]
> <span data-ttu-id="00656-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="00656-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="00656-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="00656-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="00656-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="00656-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="00656-107">Resource Manager modellinformation finns i [samla in en hanterad avbildning av en generaliserad virtuell dator i Azure](../capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="00656-107">For Resource Manager model information, see [Capture a managed image of a generalized VM in Azure](../capture-image-resource.md).</span></span>

<span data-ttu-id="00656-108">Den här artikeln beskrivs hur du toocapture en virtuell Azure-dator med Windows så att du kan använda den som en bild toocreate andra virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="00656-108">This article shows you how toocapture an Azure virtual machine running Windows so you can use it as an image toocreate other virtual machines.</span></span> <span data-ttu-id="00656-109">Den här avbildningen innehåller hello operativsystemdisken och datadiskar som är anslutna toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="00656-109">This image includes hello operating system disk and any data disks that are attached toohello virtual machine.</span></span> <span data-ttu-id="00656-110">Det finns inget nätverkskonfigurationer, så du behöver tooset upp nätverkskonfigurationer när du skapar hello andra virtuella datorer som använder hello bild.</span><span class="sxs-lookup"><span data-stu-id="00656-110">It doesn't include networking configurations, so you'll need tooset up network configurations when you create hello other virtual machines that use hello image.</span></span>

<span data-ttu-id="00656-111">Azure lagrar hello bilden under **VM-avbildningar (klassisk)**, **Compute** tjänst som visas när du visar alla hello Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="00656-111">Azure stores hello image under **VM images (classic)**, a **Compute** service that is listed when you view all hello Azure services.</span></span> <span data-ttu-id="00656-112">Detta är hello samma ställe där du har överfört bilder lagras.</span><span class="sxs-lookup"><span data-stu-id="00656-112">This is hello same place where any images you've uploaded are stored.</span></span> <span data-ttu-id="00656-113">Mer information om avbildningar finns [om avbildningar för virtuella datorer](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="00656-113">For details about images, see [About images for virtual machines](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="00656-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="00656-114">Before you begin</span></span>
<span data-ttu-id="00656-115">De här stegen förutsätter att du har redan skapat en virtuell Azure-dator och konfigurerat hello operativsystemet, inklusive bifoga datadiskar.</span><span class="sxs-lookup"><span data-stu-id="00656-115">These steps assume that you've already created an Azure virtual machine and configured hello operating system, including attaching any data disks.</span></span> <span data-ttu-id="00656-116">Om du inte gjort det ännu, se hello följande artiklar information för att skapa och förbereder hello virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="00656-116">If you haven't done this yet, see hello following articles for information on creating and preparing hello virtual machine:</span></span>

* [<span data-ttu-id="00656-117">Skapa en virtuell dator från en avbildning</span><span class="sxs-lookup"><span data-stu-id="00656-117">Create a virtual machine from an image</span></span>](createportal.md)
* [<span data-ttu-id="00656-118">Hur tooattach data disk tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="00656-118">How tooattach a data disk tooa virtual machine</span></span>](attach-disk.md)
* <span data-ttu-id="00656-119">Kontrollera att hello serverroller stöds med Sysprep.</span><span class="sxs-lookup"><span data-stu-id="00656-119">Make sure hello server roles are supported with Sysprep.</span></span> <span data-ttu-id="00656-120">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="00656-120">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

> [!WARNING]
> <span data-ttu-id="00656-121">Den här processen tar bort hello ursprungliga virtuella datorn när den är klar.</span><span class="sxs-lookup"><span data-stu-id="00656-121">This process deletes hello original virtual machine after it's captured.</span></span>
>
>

<span data-ttu-id="00656-122">Tidigare toocapturing en bild av en virtuell Azure-dator bör säkerhetskopieras hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="00656-122">Prior toocapturing an image of an Azure virtual machine, it is recommended hello target virtual machine be backed up.</span></span> <span data-ttu-id="00656-123">Virtuella Azure-datorer kan säkerhetskopieras med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="00656-123">Azure virtual machines can be backed up using Azure Backup.</span></span> <span data-ttu-id="00656-124">Mer information finns i [Säkerhetskopiera virtuella Azure-datorer](../../../backup/backup-azure-vms.md).</span><span class="sxs-lookup"><span data-stu-id="00656-124">For details, see [Back up Azure virtual machines](../../../backup/backup-azure-vms.md).</span></span> <span data-ttu-id="00656-125">Det finns andra lösningar från certifierade partner.</span><span class="sxs-lookup"><span data-stu-id="00656-125">Other solutions are available from certified partners.</span></span> <span data-ttu-id="00656-126">toofind reda på vad som är tillgänglig, Sök hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="00656-126">toofind out what’s currently available, search hello Azure Marketplace.</span></span>

## <a name="capture-hello-virtual-machine"></a><span data-ttu-id="00656-127">Avbilda hello virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="00656-127">Capture hello virtual machine</span></span>
1. <span data-ttu-id="00656-128">I hello [Azure-portalen](http://portal.azure.com), **Anslut** toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="00656-128">In hello [Azure portal](http://portal.azure.com), **Connect** toohello virtual machine.</span></span> <span data-ttu-id="00656-129">Instruktioner finns i [hur toosign i tooa virtuell dator som kör Windows Server][How toosign in tooa virtual machine running Windows Server].</span><span class="sxs-lookup"><span data-stu-id="00656-129">For instructions, see [How toosign in tooa virtual machine running Windows Server][How toosign in tooa virtual machine running Windows Server].</span></span>
2. <span data-ttu-id="00656-130">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="00656-130">Open a Command Prompt window as an administrator.</span></span>
3. <span data-ttu-id="00656-131">Ändra hello katalogen för`%windir%\system32\sysprep`, och sedan köra sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="00656-131">Change hello directory too`%windir%\system32\sysprep`, and then run sysprep.exe.</span></span>
4. <span data-ttu-id="00656-132">Hej **systemförberedelseverktyget** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="00656-132">hello **System Preparation Tool** dialog box appears.</span></span> <span data-ttu-id="00656-133">Hej du följande:</span><span class="sxs-lookup"><span data-stu-id="00656-133">Do hello following:</span></span>

   * <span data-ttu-id="00656-134">I **Rensa systemåtgärd**väljer **ange System Out of Box Experience (OOBE)** och se till att **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="00656-134">In **System Cleanup Action**, select **Enter System Out-of-Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span> <span data-ttu-id="00656-135">Mer information om att använda Sysprep finns [hur tooUse Sysprep: en introduktion][How tooUse Sysprep: An Introduction].</span><span class="sxs-lookup"><span data-stu-id="00656-135">For more information about using Sysprep, see [How tooUse Sysprep: An Introduction][How tooUse Sysprep: An Introduction].</span></span>
   * <span data-ttu-id="00656-136">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="00656-136">In **Shutdown Options**, select **Shutdown**.</span></span>
   * <span data-ttu-id="00656-137">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="00656-137">Click **OK**.</span></span>

   ![Kör Sysprep](./media/capture-image/SysprepGeneral.png)
5. <span data-ttu-id="00656-139">Sysprep stängs av hello virtuell dator som ändrar hello status för hello virtuell dator i hello Azure-portalen för**stoppad**.</span><span class="sxs-lookup"><span data-stu-id="00656-139">Sysprep shuts down hello virtual machine, which changes hello status of hello virtual machine in hello Azure portal too**Stopped**.</span></span>
6. <span data-ttu-id="00656-140">I hello Azure-portalen klickar du på **virtuella datorer (klassisk)** och välj hello virtuell dator som du vill toocapture.</span><span class="sxs-lookup"><span data-stu-id="00656-140">In hello Azure portal, click **Virtual Machines (classic)** and select hello virtual machine you want toocapture.</span></span> <span data-ttu-id="00656-141">Hej **VM-avbildningar (klassisk)** grupp visas under **Compute** när du visar **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="00656-141">hello **VM images (classic)** group is listed under **Compute** when you view **More services**.</span></span>

7. <span data-ttu-id="00656-142">Klicka på hello kommandofältet **avbilda**.</span><span class="sxs-lookup"><span data-stu-id="00656-142">On hello command bar, click **Capture**.</span></span>

   ![Avbilda virtuell dator](./media/capture-image/CaptureVM.png)

   <span data-ttu-id="00656-144">Hej **avbilda hello virtuella** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="00656-144">hello **Capture hello Virtual Machine** dialog box appears.</span></span>

8. <span data-ttu-id="00656-145">I **avbildningsnamn**, Skriv ett namn för hello ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="00656-145">In **Image name**, type a name for hello new image.</span></span> <span data-ttu-id="00656-146">I **avbildningsetiketten**, ange en etikett för hello ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="00656-146">In **Image label**, type a label for hello new image.</span></span>

9. <span data-ttu-id="00656-147">Klicka på **jag har kört Sysprep på den virtuella datorn hello**.</span><span class="sxs-lookup"><span data-stu-id="00656-147">Click **I've run Sysprep on hello virtual machine**.</span></span> <span data-ttu-id="00656-148">Den här kryssrutan refererar toohello åtgärder med Sysprep i steg 3-5.</span><span class="sxs-lookup"><span data-stu-id="00656-148">This checkbox refers toohello actions with Sysprep in steps 3-5.</span></span> <span data-ttu-id="00656-149">En avbildning _måste_ vara generaliserad med Sysprep innan du lägger till en Windows Server avbildningen tooyour uppsättning anpassade avbildningar.</span><span class="sxs-lookup"><span data-stu-id="00656-149">An image _must_ be generalized by running Sysprep before you add a Windows Server image tooyour set of custom images.</span></span>

10. <span data-ttu-id="00656-150">När hello avbildningen har slutförts hello ny avbildning blir tillgängligt i hello **Marketplace**, i hello **Compute**, **VM-avbildningar (klassisk)** behållare.</span><span class="sxs-lookup"><span data-stu-id="00656-150">Once hello capture completes, hello new image becomes available in hello **Marketplace**, in hello **Compute**, **VM images (classic)** container.</span></span>

    ![Avbildningen lyckades](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="00656-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00656-152">Next steps</span></span>
<span data-ttu-id="00656-153">hello bilden är klar toobe används toocreate virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="00656-153">hello image is ready toobe used toocreate virtual machines.</span></span> <span data-ttu-id="00656-154">toodo, ska du skapa en virtuell dator genom att välja hello **fler tjänster** menyalternativet längst hello hello services menyn och sedan **VM-avbildningar (klassisk)** i hello **Compute**grupp.</span><span class="sxs-lookup"><span data-stu-id="00656-154">toodo this, you'll create a virtual machine by selecting hello **More services** menu item at hello bottom of hello services menu, then **VM images (classic)** in hello **Compute** group.</span></span> <span data-ttu-id="00656-155">Instruktioner finns i [skapa en virtuell dator från en avbildning](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="00656-155">For instructions, see [Create a virtual machine from an image](createportal.md).</span></span>

[How toosign in tooa virtual machine running Windows Server]:connect-logon.md
[How tooUse Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[hello virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of hello virtual machine]: ./media/capture-image/CaptureVM.png
[Enter hello image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use hello captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
