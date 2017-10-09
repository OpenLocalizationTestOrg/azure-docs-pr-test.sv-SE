---
title: "aaaMount Azure file storage från en Windows Azure-dator | Microsoft Docs"
description: "Lagra filen i hello moln med Azure file storage och montera din molnbaserade filresurs från en Azure virtuell dator (VM)."
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 965f1c1b3f0d07fec6d86f9312a05e02e8ce7fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a><span data-ttu-id="f69a8-103">Använda Azure-filresurser med virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="f69a8-103">Use Azure file shares with Windows VMs</span></span> 

<span data-ttu-id="f69a8-104">Du kan använda Azure-filresurser som ett sätt toostore och filerna från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f69a8-104">You can use Azure file shares as a way toostore and access files from your VM.</span></span> <span data-ttu-id="f69a8-105">Du kan till exempel lagra ett skript eller en programkonfigurationsfil som du vill att alla virtuella datorer tooshare.</span><span class="sxs-lookup"><span data-stu-id="f69a8-105">For example, you can store a script or an application configuration file that you want all your VMs tooshare.</span></span> <span data-ttu-id="f69a8-106">I det här avsnittet visar vi du hur toocreate och montera en Azure-filresurs och hur tooupload och hämta filer.</span><span class="sxs-lookup"><span data-stu-id="f69a8-106">In this topic, we show you how toocreate and mount an Azure file share, and how tooupload and download files.</span></span>

## <a name="connect-tooa-file-share-from-a-vm"></a><span data-ttu-id="f69a8-107">Ansluta tooa filresurs från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f69a8-107">Connect tooa file share from a VM</span></span>

<span data-ttu-id="f69a8-108">Det här avsnittet förutsätter att du redan har en filresurs som du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="f69a8-108">This section assumes you already have a file share that you want tooconnect to.</span></span> <span data-ttu-id="f69a8-109">Om du behöver toocreate en finns [skapa en filresurs](#create-a-file-share) senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f69a8-109">If you need toocreate one, see [Create a file share](#create-a-file-share) later in this topic.</span></span>

1. <span data-ttu-id="f69a8-110">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f69a8-110">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f69a8-111">På hello vänstra menyn **lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-111">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="f69a8-112">Välj ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f69a8-112">Choose your storage account.</span></span>
4. <span data-ttu-id="f69a8-113">I hello **översikt** sidan under **Services**väljer **filer**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-113">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="f69a8-114">Välj en filresurs.</span><span class="sxs-lookup"><span data-stu-id="f69a8-114">Select a file share.</span></span>
6. <span data-ttu-id="f69a8-115">Klicka på **Anslut** tooopen en sida som visar hello kommandoradssyntaxen för montering hello filresursen från Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="f69a8-115">Click **Connect** tooopen a page that shows hello command-line syntax for mounting hello file share from Windows or Linux.</span></span>
7. <span data-ttu-id="f69a8-116">Markera hello syntaxen för hello kommandot och klistra in den i anteckningar eller någon annanstans där du enkelt kan komma åt den.</span><span class="sxs-lookup"><span data-stu-id="f69a8-116">Highlight hello syntax of hello command and paste it into Notepad or someplace else where you can easily access it.</span></span> 
8. <span data-ttu-id="f69a8-117">Redigera hello syntax tooremove hello inledande ** > ** och Ersätt *[enhetsbeteckning]* med hello enhetsbeteckning (till exempel **Y:**) där du vill ha toomount hello filresurs.</span><span class="sxs-lookup"><span data-stu-id="f69a8-117">Edit hello syntax tooremove hello leading **> ** and replace *[drive letter]* with hello drive letter (for example, **Y:**) where you would like toomount hello file share.</span></span>
8. <span data-ttu-id="f69a8-118">Anslut tooyour VM och öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="f69a8-118">Connect tooyour VM and open a command prompt.</span></span>
9. <span data-ttu-id="f69a8-119">Klistra in hello redigeras anslutning syntax och tryck **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-119">Paste in hello edited connection syntax and hit **Enter**.</span></span>
10. <span data-ttu-id="f69a8-120">När hello anslutningen har skapats kan du få hello-meddelande **hello kommandot har slutförts.**</span><span class="sxs-lookup"><span data-stu-id="f69a8-120">When hello connection has been created, you get hello message **hello command completed successfully.**</span></span>
11. <span data-ttu-id="f69a8-121">Kontrollera hello anslutningen genom att skriva in hello enhet bokstav tooswitch toothat enhet och skriv sedan **dir** toosee hello innehållet i hello filresurs.</span><span class="sxs-lookup"><span data-stu-id="f69a8-121">Check hello connection by typing in hello drive letter tooswitch toothat drive and then type **dir** toosee hello contents of hello file share.</span></span>



## <a name="create-a-file-share"></a><span data-ttu-id="f69a8-122">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="f69a8-122">Create a file share</span></span> 
1. <span data-ttu-id="f69a8-123">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f69a8-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f69a8-124">På hello vänstra menyn **lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-124">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="f69a8-125">Välj ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f69a8-125">Choose your storage account.</span></span>
4. <span data-ttu-id="f69a8-126">I hello **översikt** sidan under **Services**väljer **filer**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-126">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="f69a8-127">På hello File Service klickar du på **+ filresurs** toocreate din första filresurs. \\</span><span class="sxs-lookup"><span data-stu-id="f69a8-127">In hello File Service page, click **+ File share** toocreate your first file share.\\</span></span>
6. <span data-ttu-id="f69a8-128">Fyll i hello filresursnamn.</span><span class="sxs-lookup"><span data-stu-id="f69a8-128">Fill in hello file share name.</span></span> <span data-ttu-id="f69a8-129">Filresursnamn kan använda gemena bokstäver, siffror och bindestreck som enda.</span><span class="sxs-lookup"><span data-stu-id="f69a8-129">File share names can use lowercase letters, numbers and single hyphens.</span></span> <span data-ttu-id="f69a8-130">hello namn kan inte börja med ett bindestreck och du kan inte använda flera bindestreck efter varandra.</span><span class="sxs-lookup"><span data-stu-id="f69a8-130">hello name cannot start with a hyphen and you can't use multiple, consecutive hyphens.</span></span> 
7. <span data-ttu-id="f69a8-131">Fyll i en gräns för hur stor hello-fil kan vara upp too5120 GB.</span><span class="sxs-lookup"><span data-stu-id="f69a8-131">Fill in a limit on how large hello file can be, up too5120 GB.</span></span>
8. <span data-ttu-id="f69a8-132">Klicka på **OK** toodeploy hello filresurs.</span><span class="sxs-lookup"><span data-stu-id="f69a8-132">Click **OK** toodeploy hello file share.</span></span>
   
## <a name="upload-files"></a><span data-ttu-id="f69a8-133">Överföra filer</span><span class="sxs-lookup"><span data-stu-id="f69a8-133">Upload files</span></span>
1. <span data-ttu-id="f69a8-134">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f69a8-134">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f69a8-135">På hello vänstra menyn **lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-135">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="f69a8-136">Välj ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f69a8-136">Choose your storage account.</span></span>
4. <span data-ttu-id="f69a8-137">I hello **översikt** sidan under **Services**väljer **filer**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-137">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="f69a8-138">Välj en filresurs.</span><span class="sxs-lookup"><span data-stu-id="f69a8-138">Select a file share.</span></span>
6. <span data-ttu-id="f69a8-139">Klicka på **överför** tooopen hello **ladda upp filer** sidan.</span><span class="sxs-lookup"><span data-stu-id="f69a8-139">Click **Upload** tooopen hello **Upload files** page.</span></span>
7. <span data-ttu-id="f69a8-140">Klicka på hello mappen ikonen toobrowse det lokala filsystemet för en fil tooupload.</span><span class="sxs-lookup"><span data-stu-id="f69a8-140">Click on hello folder icon toobrowse your local file system for a file tooupload.</span></span>   
8. <span data-ttu-id="f69a8-141">Klicka på **överför** tooupload hello filen toohello filresurs.</span><span class="sxs-lookup"><span data-stu-id="f69a8-141">Click **Upload** tooupload hello file toohello file share.</span></span>

## <a name="download-files"></a><span data-ttu-id="f69a8-142">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="f69a8-142">Download files</span></span>
1. <span data-ttu-id="f69a8-143">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f69a8-143">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f69a8-144">På hello vänstra menyn **lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-144">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="f69a8-145">Välj ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f69a8-145">Choose your storage account.</span></span>
4. <span data-ttu-id="f69a8-146">I hello **översikt** sidan under **Services**väljer **filer**.</span><span class="sxs-lookup"><span data-stu-id="f69a8-146">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="f69a8-147">Välj en filresurs.</span><span class="sxs-lookup"><span data-stu-id="f69a8-147">Select a file share.</span></span>
6. <span data-ttu-id="f69a8-148">Högerklicka på hello-filen och välja **hämta** toodownload den tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="f69a8-148">Right-click on hello file and choose **Download** toodownload it tooyour local machine.</span></span>
   

## <a name="next-steps"></a><span data-ttu-id="f69a8-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f69a8-149">Next steps</span></span>

<span data-ttu-id="f69a8-150">Du kan också skapa och hantera filresurser med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f69a8-150">You can also create and manage file shares using PowerShell.</span></span> <span data-ttu-id="f69a8-151">Mer information finns i [Kom igång med Azure File storage i Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="f69a8-151">For more information, see [Get started with Azure File storage on Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span></span>
