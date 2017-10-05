---
title: "Hämta en avbildning av en Windows Azure-dator | Microsoft Docs"
description: Avbilda en virtuell Windows-dator skapad med den klassiska distributionsmodellen.
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
ms.openlocfilehash: 6032263848c469ce2f416306e5c91c29f4cb30e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a><span data-ttu-id="be714-103">Avbilda en virtuell Windows-dator skapad med den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="be714-103">Capture an image of an Azure Windows virtual machine created with the classic deployment model.</span></span>
> [!IMPORTANT]
> <span data-ttu-id="be714-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="be714-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="be714-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="be714-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="be714-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="be714-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="be714-107">Resource Manager modellinformation finns i [samla in en hanterad avbildning av en generaliserad virtuell dator i Azure](../capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="be714-107">For Resource Manager model information, see [Capture a managed image of a generalized VM in Azure](../capture-image-resource.md).</span></span>

<span data-ttu-id="be714-108">Den här artikeln visar hur du avbildar en Azure-dator som kör Windows, så du kan använda den som en bild för att skapa andra virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="be714-108">This article shows you how to capture an Azure virtual machine running Windows so you can use it as an image to create other virtual machines.</span></span> <span data-ttu-id="be714-109">Den här avbildningen innehåller operativsystemdisken och eventuella hårddiskar som är kopplade till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="be714-109">This image includes the operating system disk and any data disks that are attached to the virtual machine.</span></span> <span data-ttu-id="be714-110">Det finns inget nätverkskonfigurationer, så du behöver konfigurera nätverkskonfigurationer när du skapar de andra virtuella datorer som använder avbildningen.</span><span class="sxs-lookup"><span data-stu-id="be714-110">It doesn't include networking configurations, so you'll need to set up network configurations when you create the other virtual machines that use the image.</span></span>

<span data-ttu-id="be714-111">Azure lagrar bilden under **VM-avbildningar (klassisk)**, **Compute** tjänst som visas när du visar alla Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="be714-111">Azure stores the image under **VM images (classic)**, a **Compute** service that is listed when you view all the Azure services.</span></span> <span data-ttu-id="be714-112">Det här är samma plats där alla bilder som du har överfört lagras.</span><span class="sxs-lookup"><span data-stu-id="be714-112">This is the same place where any images you've uploaded are stored.</span></span> <span data-ttu-id="be714-113">Mer information om avbildningar finns [om avbildningar för virtuella datorer](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be714-113">For details about images, see [About images for virtual machines](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="be714-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="be714-114">Before you begin</span></span>
<span data-ttu-id="be714-115">Dessa instruktioner förutsätter att du redan skapat en virtuell Azure-dator och konfigurerat operativsystemet, inklusive bifoga datadiskar.</span><span class="sxs-lookup"><span data-stu-id="be714-115">These steps assume that you've already created an Azure virtual machine and configured the operating system, including attaching any data disks.</span></span> <span data-ttu-id="be714-116">Om du inte gjort det ännu, finns i följande artiklar för information om hur du skapar och förbereder den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="be714-116">If you haven't done this yet, see the following articles for information on creating and preparing the virtual machine:</span></span>

* [<span data-ttu-id="be714-117">Skapa en virtuell dator från en avbildning</span><span class="sxs-lookup"><span data-stu-id="be714-117">Create a virtual machine from an image</span></span>](createportal.md)
* [<span data-ttu-id="be714-118">Så här kopplar du en datadisk till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="be714-118">How to attach a data disk to a virtual machine</span></span>](attach-disk.md)
* <span data-ttu-id="be714-119">Se till att serverroller som stöds med Sysprep.</span><span class="sxs-lookup"><span data-stu-id="be714-119">Make sure the server roles are supported with Sysprep.</span></span> <span data-ttu-id="be714-120">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="be714-120">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

> [!WARNING]
> <span data-ttu-id="be714-121">Den här processen tar bort den ursprungliga virtuella datorn när den är klar.</span><span class="sxs-lookup"><span data-stu-id="be714-121">This process deletes the original virtual machine after it's captured.</span></span>
>
>

<span data-ttu-id="be714-122">Innan du gör en avbildning av en virtuell Azure-dator, rekommenderas det att säkerhetskopiera den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="be714-122">Prior to capturing an image of an Azure virtual machine, it is recommended the target virtual machine be backed up.</span></span> <span data-ttu-id="be714-123">Virtuella Azure-datorer kan säkerhetskopieras med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="be714-123">Azure virtual machines can be backed up using Azure Backup.</span></span> <span data-ttu-id="be714-124">Mer information finns i [Säkerhetskopiera virtuella Azure-datorer](../../../backup/backup-azure-vms.md).</span><span class="sxs-lookup"><span data-stu-id="be714-124">For details, see [Back up Azure virtual machines](../../../backup/backup-azure-vms.md).</span></span> <span data-ttu-id="be714-125">Det finns andra lösningar från certifierade partner.</span><span class="sxs-lookup"><span data-stu-id="be714-125">Other solutions are available from certified partners.</span></span> <span data-ttu-id="be714-126">Om du vill ta reda på vad som finns för närvarande kan du söka på Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="be714-126">To find out what’s currently available, search the Azure Marketplace.</span></span>

## <a name="capture-the-virtual-machine"></a><span data-ttu-id="be714-127">Avbilda den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="be714-127">Capture the virtual machine</span></span>
1. <span data-ttu-id="be714-128">I den [Azure-portalen](http://portal.azure.com), **Anslut** till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="be714-128">In the [Azure portal](http://portal.azure.com), **Connect** to the virtual machine.</span></span> <span data-ttu-id="be714-129">Instruktioner finns i [så att logga in på en virtuell dator som kör Windows Server][How to sign in to a virtual machine running Windows Server].</span><span class="sxs-lookup"><span data-stu-id="be714-129">For instructions, see [How to sign in to a virtual machine running Windows Server][How to sign in to a virtual machine running Windows Server].</span></span>
2. <span data-ttu-id="be714-130">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="be714-130">Open a Command Prompt window as an administrator.</span></span>
3. <span data-ttu-id="be714-131">Ändra katalogen till `%windir%\system32\sysprep`, och sedan köra sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="be714-131">Change the directory to `%windir%\system32\sysprep`, and then run sysprep.exe.</span></span>
4. <span data-ttu-id="be714-132">Den **systemförberedelseverktyget** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="be714-132">The **System Preparation Tool** dialog box appears.</span></span> <span data-ttu-id="be714-133">Gör följande:</span><span class="sxs-lookup"><span data-stu-id="be714-133">Do the following:</span></span>

   * <span data-ttu-id="be714-134">I **Rensa systemåtgärd**väljer **ange System Out of Box Experience (OOBE)** och se till att **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="be714-134">In **System Cleanup Action**, select **Enter System Out-of-Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span> <span data-ttu-id="be714-135">Mer information om att använda Sysprep finns [så att använda Sysprep: en introduktion][How to Use Sysprep: An Introduction].</span><span class="sxs-lookup"><span data-stu-id="be714-135">For more information about using Sysprep, see [How to Use Sysprep: An Introduction][How to Use Sysprep: An Introduction].</span></span>
   * <span data-ttu-id="be714-136">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="be714-136">In **Shutdown Options**, select **Shutdown**.</span></span>
   * <span data-ttu-id="be714-137">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="be714-137">Click **OK**.</span></span>

   ![Kör Sysprep](./media/capture-image/SysprepGeneral.png)
5. <span data-ttu-id="be714-139">Sysprep stängs den virtuella datorn, som ändrar status för den virtuella datorn i Azure portal och **stoppad**.</span><span class="sxs-lookup"><span data-stu-id="be714-139">Sysprep shuts down the virtual machine, which changes the status of the virtual machine in the Azure portal to **Stopped**.</span></span>
6. <span data-ttu-id="be714-140">I Azure-portalen klickar du på **virtuella datorer (klassisk)** och välj den virtuella dator som du vill samla in.</span><span class="sxs-lookup"><span data-stu-id="be714-140">In the Azure portal, click **Virtual Machines (classic)** and select the virtual machine you want to capture.</span></span> <span data-ttu-id="be714-141">Den **VM-avbildningar (klassisk)** grupp visas under **Compute** när du visar **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="be714-141">The **VM images (classic)** group is listed under **Compute** when you view **More services**.</span></span>

7. <span data-ttu-id="be714-142">Klicka på kommandofältet **avbilda**.</span><span class="sxs-lookup"><span data-stu-id="be714-142">On the command bar, click **Capture**.</span></span>

   ![Avbilda virtuell dator](./media/capture-image/CaptureVM.png)

   <span data-ttu-id="be714-144">Den **avbilda den virtuella datorn** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="be714-144">The **Capture the Virtual Machine** dialog box appears.</span></span>

8. <span data-ttu-id="be714-145">I **avbildningsnamn**, Skriv ett namn för den nya avbildningen.</span><span class="sxs-lookup"><span data-stu-id="be714-145">In **Image name**, type a name for the new image.</span></span> <span data-ttu-id="be714-146">I **avbildningsetiketten**, ange en etikett för den nya avbildningen.</span><span class="sxs-lookup"><span data-stu-id="be714-146">In **Image label**, type a label for the new image.</span></span>

9. <span data-ttu-id="be714-147">Klicka på **jag har kört Sysprep på den virtuella datorn**.</span><span class="sxs-lookup"><span data-stu-id="be714-147">Click **I've run Sysprep on the virtual machine**.</span></span> <span data-ttu-id="be714-148">Den här kryssrutan refererar till åtgärder med Sysprep i steg 3-5.</span><span class="sxs-lookup"><span data-stu-id="be714-148">This checkbox refers to the actions with Sysprep in steps 3-5.</span></span> <span data-ttu-id="be714-149">En avbildning _måste_ vara generaliserad med Sysprep innan du lägger till en Windows Server-avbildning till en uppsättning anpassade avbildningar.</span><span class="sxs-lookup"><span data-stu-id="be714-149">An image _must_ be generalized by running Sysprep before you add a Windows Server image to your set of custom images.</span></span>

10. <span data-ttu-id="be714-150">När avbildningen har slutförts, den nya avbildningen blir tillgängligt i den **Marketplace**i den **Compute**, **VM-avbildningar (klassisk)** behållare.</span><span class="sxs-lookup"><span data-stu-id="be714-150">Once the capture completes, the new image becomes available in the **Marketplace**, in the **Compute**, **VM images (classic)** container.</span></span>

    ![Avbildningen lyckades](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="be714-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be714-152">Next steps</span></span>
<span data-ttu-id="be714-153">Bilden är redo att användas för att skapa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="be714-153">The image is ready to be used to create virtual machines.</span></span> <span data-ttu-id="be714-154">Om du vill göra det, ska du skapa en virtuell dator genom att välja den **fler tjänster** menyalternativet längst ned i tjänster menyn och sedan **VM-avbildningar (klassisk)** i den **Compute** grupp.</span><span class="sxs-lookup"><span data-stu-id="be714-154">To do this, you'll create a virtual machine by selecting the **More services** menu item at the bottom of the services menu, then **VM images (classic)** in the **Compute** group.</span></span> <span data-ttu-id="be714-155">Instruktioner finns i [skapa en virtuell dator från en avbildning](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="be714-155">For instructions, see [Create a virtual machine from an image](createportal.md).</span></span>

[How to sign in to a virtual machine running Windows Server]:connect-logon.md
[How to Use Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
