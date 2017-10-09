---
title: "aaaDownload en Windows-VHD från Azure | Microsoft Docs"
description: "Hämta en Windows-VHD med hello Azure-portalen."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: d0ca8842db98f22751f01648c0ba4e5cde090043
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-windows-vhd-from-azure"></a><span data-ttu-id="ca102-103">Hämta Windows VHD från Azure</span><span class="sxs-lookup"><span data-stu-id="ca102-103">Download a Windows VHD from Azure</span></span>

<span data-ttu-id="ca102-104">I den här artikeln får du lära dig hur toodownload en [Windows virtuell hårddisk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) filen från Azure med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ca102-104">In this article, you learn how toodownload a [Windows virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) file from Azure using hello Azure portal.</span></span> 

<span data-ttu-id="ca102-105">Virtuella datorer (VM) i Azure används [diskar](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) som plats-toostore ett operativsystem, program och data.</span><span class="sxs-lookup"><span data-stu-id="ca102-105">Virtual machines (VMs) in Azure use [disks](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="ca102-106">Alla virtuella Azure-datorer har minst två diskar – en disk i Windows-operativsystem och en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="ca102-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="ca102-107">hello operativsystemdisk har skapats ursprungligen från en avbildning och både hello operativsystemdisken och hello avbildningen är virtuella hårddiskar som lagras i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ca102-107">hello operating system disk is initially created from an image, and both hello operating system disk and hello image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="ca102-108">Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="ca102-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

## <a name="stop-hello-vm"></a><span data-ttu-id="ca102-109">Stoppa hello VM</span><span class="sxs-lookup"><span data-stu-id="ca102-109">Stop hello VM</span></span>

<span data-ttu-id="ca102-110">En virtuell Hårddisk kan inte hämtas från Azure om den är ansluten tooa använder VM.</span><span class="sxs-lookup"><span data-stu-id="ca102-110">A VHD can’t be downloaded from Azure if it's attached tooa running VM.</span></span> <span data-ttu-id="ca102-111">Du måste toostop hello VM toodownload en virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="ca102-111">You need toostop hello VM toodownload a VHD.</span></span> <span data-ttu-id="ca102-112">Om du vill toouse en virtuell Hårddisk som en [bild](tutorial-custom-images.md) toocreate andra virtuella datorer med nya diskar kan du använda [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello operativsystem finns i hello-filen och sedan stoppa hello VM.</span><span class="sxs-lookup"><span data-stu-id="ca102-112">If you want toouse a VHD as an [image](tutorial-custom-images.md) toocreate other VMs with new disks, you use [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello operating system contained in hello file and then stop hello VM.</span></span> <span data-ttu-id="ca102-113">toouse hello VHD som en disk för en ny instans av en befintlig virtuell dator eller en datadisk du bara behöver toostop och frigöra hello VM.</span><span class="sxs-lookup"><span data-stu-id="ca102-113">toouse hello VHD as a disk for a new instance of an existing VM or data disk, you only need toostop and deallocate hello VM.</span></span>

<span data-ttu-id="ca102-114">toouse Hej VHD som en bild toocreate andra virtuella datorer, gör följande:</span><span class="sxs-lookup"><span data-stu-id="ca102-114">toouse hello VHD as an image toocreate other VMs, complete these steps:</span></span>

1.  <span data-ttu-id="ca102-115">Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ca102-115">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="ca102-116">[Ansluta toohello VM](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ca102-116">[Connect toohello VM](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
3.  <span data-ttu-id="ca102-117">Öppna hello Kommandotolken som administratör på hello VM.</span><span class="sxs-lookup"><span data-stu-id="ca102-117">On hello VM, open hello Command Prompt window as an administrator.</span></span>
4.  <span data-ttu-id="ca102-118">Ändra hello katalogen för*%windir%\system32\sysprep* och köra sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="ca102-118">Change hello directory too*%windir%\system32\sysprep* and run sysprep.exe.</span></span>
5.  <span data-ttu-id="ca102-119">Hello Systemförberedelseverktyget i dialogrutan Välj **ange System Out of Box Experience (OOBE)**, och se till att **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="ca102-119">In hello System Preparation Tool dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that **Generalize** is selected.</span></span>
6.  <span data-ttu-id="ca102-120">Välj i avstängningsalternativ, **avstängning**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca102-120">In Shutdown Options, select **Shutdown**, and then click **OK**.</span></span> 

<span data-ttu-id="ca102-121">toouse hello VHD som en disk för en ny instans av en befintlig virtuell dator eller en datadisk, utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca102-121">toouse hello VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="ca102-122">Hej hubbmenyn i hello Azure-portalen, klicka på **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="ca102-122">On hello Hub menu in hello Azure portal, click **Virtual Machines**.</span></span>
2.  <span data-ttu-id="ca102-123">Välj hello VM hello-listan.</span><span class="sxs-lookup"><span data-stu-id="ca102-123">Select hello VM from hello list.</span></span>
3.  <span data-ttu-id="ca102-124">Klicka på hello bladet för hello VM **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="ca102-124">On hello blade for hello VM, click **Stop**.</span></span>

    ![Stoppa VM](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="ca102-126">Generera en SAS-URL</span><span class="sxs-lookup"><span data-stu-id="ca102-126">Generate SAS URL</span></span>

<span data-ttu-id="ca102-127">toodownload hello VHD-filen, behöver du toogenerate en [signatur för delad åtkomst (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span><span class="sxs-lookup"><span data-stu-id="ca102-127">toodownload hello VHD file, you need toogenerate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="ca102-128">När hello URL: en genereras tilldelas en förfallotid toohello URL.</span><span class="sxs-lookup"><span data-stu-id="ca102-128">When hello URL is generated, an expiration time is assigned toohello URL.</span></span>

1.  <span data-ttu-id="ca102-129">På menyn hello av hello-bladet för hello VM **diskar**.</span><span class="sxs-lookup"><span data-stu-id="ca102-129">On hello menu of hello blade for hello VM, click **Disks**.</span></span>
2.  <span data-ttu-id="ca102-130">Välj hello operativsystemdisken för hello VM och klicka sedan på **exportera**.</span><span class="sxs-lookup"><span data-stu-id="ca102-130">Select hello operating system disk for hello VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="ca102-131">Ange hello förfallotid för hello-URL för*36000*.</span><span class="sxs-lookup"><span data-stu-id="ca102-131">Set hello expiration time of hello URL too*36000*.</span></span>
4.  <span data-ttu-id="ca102-132">Klicka på **generera URL**.</span><span class="sxs-lookup"><span data-stu-id="ca102-132">Click **Generate URL**.</span></span>

    ![Generera en URL](./media/download-vhd/export-generate.png)

> [!NOTE]
> <span data-ttu-id="ca102-134">hello förfallotid ökas från hello standard tooprovide tillräckligt med tid för toodownload hello stor VHD-filen för ett Windows Server-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="ca102-134">hello expiration time is increased from hello default tooprovide enough time toodownload hello large VHD file for a Windows Server operating system.</span></span> <span data-ttu-id="ca102-135">Du kan förvänta dig en VHD-fil som innehåller hello Windows Server operativsystem tootake toodownload för flera timmar beroende på anslutningen.</span><span class="sxs-lookup"><span data-stu-id="ca102-135">You can expect a VHD file that contains hello Windows Server operating system tootake several hours toodownload depending on your connection.</span></span> <span data-ttu-id="ca102-136">Om du hämtar en virtuell Hårddisk för en datadisk räcker hello standardtid.</span><span class="sxs-lookup"><span data-stu-id="ca102-136">If you are downloading a VHD for a data disk, hello default time is sufficient.</span></span> 
> 
> 

## <a name="download-vhd"></a><span data-ttu-id="ca102-137">Hämta VHD</span><span class="sxs-lookup"><span data-stu-id="ca102-137">Download VHD</span></span>

1.  <span data-ttu-id="ca102-138">Klicka på Hämta hello VHD-filen under hello-URL som har genererats.</span><span class="sxs-lookup"><span data-stu-id="ca102-138">Under hello URL that was generated, click Download hello VHD file.</span></span>

    ![Hämta VHD](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="ca102-140">Du kan behöva tooclick **spara** i hello webbläsare toostart hello hämtning.</span><span class="sxs-lookup"><span data-stu-id="ca102-140">You may need tooclick **Save** in hello browser toostart hello download.</span></span> <span data-ttu-id="ca102-141">hello standardnamnet för hello VHD-filen är *abcd*.</span><span class="sxs-lookup"><span data-stu-id="ca102-141">hello default name for hello VHD file is *abcd*.</span></span>

    ![Klicka på Spara i hello webbläsare](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="ca102-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca102-143">Next steps</span></span>

- <span data-ttu-id="ca102-144">Lär dig hur för[ladda upp en VHD-filen tooAzure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ca102-144">Learn how too[upload a VHD file tooAzure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
- <span data-ttu-id="ca102-145">[Skapa hanterade diskar från ohanterade diskar i ett lagringskonto](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ca102-145">[Create managed disks from unmanaged disks in a storage account](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
- <span data-ttu-id="ca102-146">[Hantera Azure-diskar med PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ca102-146">[Manage Azure disks with PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

