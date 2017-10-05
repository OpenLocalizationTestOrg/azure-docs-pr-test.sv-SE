---
title: "Skapa en Azure RemoteApp-avbildning baserat på en virtuell dator i Azure | Microsoft Docs"
description: "Lär dig mer om att skapa en avbildning för Azure RemoteApp genom att börja med en virtuell Azure-dator."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d41583ef-6cd8-4115-8dcb-b2cd5b3d301a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ee64b86835af8e6237cddcd8acc779fc6dac8214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a><span data-ttu-id="cbbd6-103">Skapa en Azure RemoteApp-avbildning baserat på en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="cbbd6-103">Create a Azure RemoteApp image based on an Azure virtual machine</span></span>
> [!IMPORTANT]
> <span data-ttu-id="cbbd6-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="cbbd6-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="cbbd6-106">Du kan skapa Azure RemoteApp-bilder (som rymmer de appar som du delar i samlingen) från en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-106">You can create Azure RemoteApp images (which hold the apps you share in your collection) from an Azure virtual machine.</span></span> <span data-ttu-id="cbbd6-107">Du kan också välja att använda en avbildning av virtuell dator som vi har lagt till i avbildningen Virtuella Azure-galleriet som uppfyller alla krav för Azure RemoteApp-avbildning – du kan använda den VM-avbildningen som en startpunkt för egna VM, om du vill.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-107">You could also choose to use a virtual machine image we added to the Azure VM image gallery that meets all the Azure RemoteApp image requirements - you can use that VM image as a starting point for your own VM, if you want.</span></span> <span data-ttu-id="cbbd6-108">Sök bara efter ”Windows Server värd för fjärrskrivbordssession” avbildning i biblioteket.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-108">Just look for the "Windows Server Remote Desktop Session Host" image in the library.</span></span>

<span data-ttu-id="cbbd6-109">Det finns två steg för att skapa en egen avbildning baserat på en Azure VM – skapa avbildningen och överföra den Virtuella Azure-biblioteket till Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-109">There are two steps to create your own image based on an Azure VM - create the image and then upload it from the Azure VM library to Azure RemoteApp.</span></span>

## <a name="create-a-custom-image-based-on-an-azure-vm"></a><span data-ttu-id="cbbd6-110">Skapa en anpassad avbildning baserat på en Azure VM</span><span class="sxs-lookup"><span data-stu-id="cbbd6-110">Create a custom image based on an Azure VM</span></span>
<span data-ttu-id="cbbd6-111">Följ dessa steg för att skapa en avbildning baserat på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-111">Use these steps to create an image based on an Azure VM.</span></span>

1. <span data-ttu-id="cbbd6-112">Skapa en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-112">Create an Azure virtual machine.</span></span> <span data-ttu-id="cbbd6-113">Du kan använda den ”Windows Server värd för fjärrskrivbordssession” eller ”Windows Server Remote Desktop Session Host med Microsoft Office 365 ProPlus” bilden från galleriet virtuella Azure-datorn för avbildning.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-113">You can use the "Windows Server Remote Desktop Session Host" or the "Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus" image from the Azure virtual machine image gallery.</span></span> <span data-ttu-id="cbbd6-114">Den här avbildningen uppfyller alla Azure RemoteApp mallen avbildningen.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-114">This image meets all the Azure RemoteApp template image requirements.</span></span>
   
    <span data-ttu-id="cbbd6-115">Mer information finns i [skapa en virtuell dator som kör Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbbd6-115">For details, see [Create a VM running Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="cbbd6-116">Ansluta till den virtuella datorn och installera och konfigurera de appar som du vill dela via RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-116">Connect to the VM and install and configure the apps that you want to share through RemoteApp.</span></span> <span data-ttu-id="cbbd6-117">Se till att utföra några ytterligare Windows-konfigurationer som krävs av dina appar.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-117">Make sure to perform any additional Windows configurations required by your apps.</span></span>
   
    <span data-ttu-id="cbbd6-118">Mer information finns i [så att logga in på en virtuell dator kör Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbbd6-118">For details, see [How to Log on to a Virtual Machine Running Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
3. <span data-ttu-id="cbbd6-119">Om du använder en av avbildningarna Windows Server värd för fjärrskrivbordssession, finns det en inkluderade verifieringsskriptet som ser till att den virtuella datorn uppfyller RemoteApp pre-reqs.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-119">If you are using one of the Windows Server Remote Desktop Session Host images, there is an included validation script that will ensure your VM meets the RemoteApp pre-reqs.</span></span> <span data-ttu-id="cbbd6-120">Kör skript genom att dubbelklicka på **ValidateRemoteAppImage** på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-120">To run script, double-click **ValidateRemoteAppImage** on the desktop.</span></span> <span data-ttu-id="cbbd6-121">Se till att alla fel som rapporteras av skriptet korrigeras innan du fortsätter till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-121">Ensure that all errors reported by the script are fixed before proceeding to the next step.</span></span>
4. <span data-ttu-id="cbbd6-122">SYSPREP generalize och spara avbildningen.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-122">SYSPREP generalize and capture the image.</span></span> <span data-ttu-id="cbbd6-123">Se [så här skapar du en virtuell dator i Windows till mall](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-123">See [How to Capture a Windows Virtual Machine to Use as a Template](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) for instructions.</span></span>

## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a><span data-ttu-id="cbbd6-124">Importera den till Azure RemoteApp-bildbibliotek</span><span class="sxs-lookup"><span data-stu-id="cbbd6-124">Import the image into the Azure RemoteApp image library</span></span>
<span data-ttu-id="cbbd6-125">Följ dessa steg för att importera den nya avbildningen till Azure RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="cbbd6-125">Use these steps to import the new image into Azure RemoteApp:</span></span>

1. <span data-ttu-id="cbbd6-126">I den **Mallavbildningarna** fliken:</span><span class="sxs-lookup"><span data-stu-id="cbbd6-126">In the **Template Images** tab:</span></span>
   
   * <span data-ttu-id="cbbd6-127">Om du har inga befintliga bilder, klickar du på **överföra eller importera en Mallavbildningen**.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-127">If you have no existing images, click **Upload or Import a Template Image**.</span></span>
   * <span data-ttu-id="cbbd6-128">Om du redan har minst en bild, klickar du på  **+**  att lägga till en ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-128">If you have at least one image already, click **+** to add a new image.</span></span>
2. <span data-ttu-id="cbbd6-129">Välj **importera en bild från de virtuella datorerna** biblioteket och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-129">Select **Import an image from your Virtual Machines** library, and then click **Next**.</span></span>
3. <span data-ttu-id="cbbd6-130">Välj den anpassade avbildningen i listan och bekräfta att du har följt stegen när du skapade avbildningen på nästa sida.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-130">On the next page, select your custom image from the list and confirm that you followed the steps listed when you created your image.</span></span> <span data-ttu-id="cbbd6-131">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-131">Click **Next**.</span></span>
4. <span data-ttu-id="cbbd6-132">Ange ett namn för den nya avbildningen för RemoteApp- och välj platsen, och klicka sedan på bockmarkeringen för att starta importen.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-132">Enter a name for the new RemoteApp image and pick the location, then click the checkmark to start the import process.</span></span>

> [!NOTE]
> <span data-ttu-id="cbbd6-133">Du kan importera bilder från Azure-plats som stöds av Azure virtuella datorer till Azure-plats som stöds av Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-133">You can import images from any Azure location supported by Azure Virtual Machines to any Azure location supported by Azure RemoteApp.</span></span> <span data-ttu-id="cbbd6-134">Importen kan ta upp till 25 minuter beroende på platserna.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-134">Depending on the locations the import can take up to 25 minutes.</span></span>
> 
> 

<span data-ttu-id="cbbd6-135">Nu är du redo att skapa den nya samlingen antingen en [moln](remoteapp-create-cloud-deployment.md) samling eller [hybrid](remoteapp-create-hybrid-deployment.md), beroende på dina behov.</span><span class="sxs-lookup"><span data-stu-id="cbbd6-135">Now you are ready to create your new collection, either a [cloud](remoteapp-create-cloud-deployment.md) collection or [hybrid](remoteapp-create-hybrid-deployment.md), depending on your needs.</span></span>

