---
title: "aaaCreate en Azure RemoteApp-avbildning baserat på en virtuell dator i Azure | Microsoft Docs"
description: "Lär dig hur toocreate en bild för Azure RemoteApp genom att börja med en virtuell Azure-dator."
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
ms.openlocfilehash: 2d432bcb15be68a2946d91b5f36f41d980726338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a><span data-ttu-id="e1bd6-103">Skapa en Azure RemoteApp-avbildning baserat på en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="e1bd6-103">Create a Azure RemoteApp image based on an Azure virtual machine</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e1bd6-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="e1bd6-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="e1bd6-106">Du kan skapa Azure RemoteApp-bilder (som håller hello-appar som du delar i samlingen) från en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-106">You can create Azure RemoteApp images (which hold hello apps you share in your collection) from an Azure virtual machine.</span></span> <span data-ttu-id="e1bd6-107">Du kan också välja toouse en avbildning av virtuell dator som vi har lagt till toohello Azure VM avbildningen galleriet som uppfyller alla krav för avbildningen av hello Azure RemoteApp - du kan använda den VM-avbildningen som en startpunkt för egna VM, om du vill.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-107">You could also choose toouse a virtual machine image we added toohello Azure VM image gallery that meets all hello Azure RemoteApp image requirements - you can use that VM image as a starting point for your own VM, if you want.</span></span> <span data-ttu-id="e1bd6-108">Titta bara för hello ”Windows Server värd för fjärrskrivbordssession” bilden hello-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-108">Just look for hello "Windows Server Remote Desktop Session Host" image in hello library.</span></span>

<span data-ttu-id="e1bd6-109">Det finns två steg toocreate en egen avbildning baserat på en Azure VM – skapa hello avbildning och överför den från hello Azure VM biblioteket tooAzure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-109">There are two steps toocreate your own image based on an Azure VM - create hello image and then upload it from hello Azure VM library tooAzure RemoteApp.</span></span>

## <a name="create-a-custom-image-based-on-an-azure-vm"></a><span data-ttu-id="e1bd6-110">Skapa en anpassad avbildning baserat på en Azure VM</span><span class="sxs-lookup"><span data-stu-id="e1bd6-110">Create a custom image based on an Azure VM</span></span>
<span data-ttu-id="e1bd6-111">Använd de här stegen toocreate en avbildning baserat på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-111">Use these steps toocreate an image based on an Azure VM.</span></span>

1. <span data-ttu-id="e1bd6-112">Skapa en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-112">Create an Azure virtual machine.</span></span> <span data-ttu-id="e1bd6-113">Du kan använda hello ”Windows Server värd för fjärrskrivbordssession” eller ”Windows Server Remote Desktop Session Host med Microsoft Office 365 ProPlus” hello-avbildning från hello Azure virtuella avbildningen gallery.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-113">You can use hello "Windows Server Remote Desktop Session Host" or hello "Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus" image from hello Azure virtual machine image gallery.</span></span> <span data-ttu-id="e1bd6-114">Den här avbildningen uppfyller alla hello Azure RemoteApp mallen avbildningen.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-114">This image meets all hello Azure RemoteApp template image requirements.</span></span>
   
    <span data-ttu-id="e1bd6-115">Mer information finns i [skapa en virtuell dator som kör Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e1bd6-115">For details, see [Create a VM running Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="e1bd6-116">Anslut toohello VM och installera och konfigurera hello appar som du vill tooshare via RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-116">Connect toohello VM and install and configure hello apps that you want tooshare through RemoteApp.</span></span> <span data-ttu-id="e1bd6-117">Se till att tooperform några ytterligare Windows-konfigurationer som krävs av dina appar.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-117">Make sure tooperform any additional Windows configurations required by your apps.</span></span>
   
    <span data-ttu-id="e1bd6-118">Mer information finns i [hur tooLog på tooa virtuell dator kör Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e1bd6-118">For details, see [How tooLog on tooa Virtual Machine Running Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
3. <span data-ttu-id="e1bd6-119">Om du använder en av hello Windows Server värd för fjärrskrivbordssession bilder, finns det en inkluderade verifieringsskriptet som ser till att den virtuella datorn uppfyller hello RemoteApp pre-reqs.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-119">If you are using one of hello Windows Server Remote Desktop Session Host images, there is an included validation script that will ensure your VM meets hello RemoteApp pre-reqs.</span></span> <span data-ttu-id="e1bd6-120">toorun skript, dubbelklicka på **ValidateRemoteAppImage** hello skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-120">toorun script, double-click **ValidateRemoteAppImage** on hello desktop.</span></span> <span data-ttu-id="e1bd6-121">Se till att alla fel som rapporteras av skriptet hello korrigeras innan du fortsätter toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-121">Ensure that all errors reported by hello script are fixed before proceeding toohello next step.</span></span>
4. <span data-ttu-id="e1bd6-122">SYSPREP generalize och hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-122">SYSPREP generalize and capture hello image.</span></span> <span data-ttu-id="e1bd6-123">Se [hur tooCapture en virtuell Windows-dator tooUse som en mall](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-123">See [How tooCapture a Windows Virtual Machine tooUse as a Template](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) for instructions.</span></span>

## <a name="import-hello-image-into-hello-azure-remoteapp-image-library"></a><span data-ttu-id="e1bd6-124">Importera hello bild till hello Azure RemoteApp bildbibliotek</span><span class="sxs-lookup"><span data-stu-id="e1bd6-124">Import hello image into hello Azure RemoteApp image library</span></span>
<span data-ttu-id="e1bd6-125">Använd de här stegen tooimport hello ny avbildning i Azure RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="e1bd6-125">Use these steps tooimport hello new image into Azure RemoteApp:</span></span>

1. <span data-ttu-id="e1bd6-126">I hello **Mallavbildningarna** fliken:</span><span class="sxs-lookup"><span data-stu-id="e1bd6-126">In hello **Template Images** tab:</span></span>
   
   * <span data-ttu-id="e1bd6-127">Om du har inga befintliga bilder, klickar du på **överföra eller importera en Mallavbildningen**.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-127">If you have no existing images, click **Upload or Import a Template Image**.</span></span>
   * <span data-ttu-id="e1bd6-128">Om du redan har minst en bild, klickar du på  **+**  tooadd en ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-128">If you have at least one image already, click **+** tooadd a new image.</span></span>
2. <span data-ttu-id="e1bd6-129">Välj **importera en bild från de virtuella datorerna** biblioteket och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-129">Select **Import an image from your Virtual Machines** library, and then click **Next**.</span></span>
3. <span data-ttu-id="e1bd6-130">Välj den anpassade avbildningen hello listan och bekräfta att du följt hello stegen när du skapade avbildningen på hello nästa sida.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-130">On hello next page, select your custom image from hello list and confirm that you followed hello steps listed when you created your image.</span></span> <span data-ttu-id="e1bd6-131">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-131">Click **Next**.</span></span>
4. <span data-ttu-id="e1bd6-132">Ange ett namn för hello ny RemoteApp-avbildning och välj hello plats och sedan på hello markering toostart hello importen.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-132">Enter a name for hello new RemoteApp image and pick hello location, then click hello checkmark toostart hello import process.</span></span>

> [!NOTE]
> <span data-ttu-id="e1bd6-133">Du kan importera bilder från Azure-plats som stöds av Azure Virtual Machines tooany Azure-plats som stöds av Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-133">You can import images from any Azure location supported by Azure Virtual Machines tooany Azure location supported by Azure RemoteApp.</span></span> <span data-ttu-id="e1bd6-134">Hello import kan ta upp too25 minuter beroende på hello platser.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-134">Depending on hello locations hello import can take up too25 minutes.</span></span>
> 
> 

<span data-ttu-id="e1bd6-135">Nu är du klar toocreate den nya samlingen antingen en [moln](remoteapp-create-cloud-deployment.md) samling eller [hybrid](remoteapp-create-hybrid-deployment.md), beroende på dina behov.</span><span class="sxs-lookup"><span data-stu-id="e1bd6-135">Now you are ready toocreate your new collection, either a [cloud](remoteapp-create-cloud-deployment.md) collection or [hybrid](remoteapp-create-hybrid-deployment.md), depending on your needs.</span></span>

