---
title: "aaaUpload VHD-filen med hjälp av Microsoft Azure Lagringsutforskaren för tooAzure DevTest Labs | Microsoft Docs"
description: "Överföra VHD-filen toolab storage-konto med Microsoft Azure Lagringsutforskaren"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 686691e3676cea4b2d7cd8bf045bc43a792c667e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-microsoft-azure-storage-explorer"></a><span data-ttu-id="75e68-103">Överföra VHD-filen toolab storage-konto med Microsoft Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="75e68-103">Upload VHD file toolab's storage account using Microsoft Azure Storage Explorer</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="75e68-104">VHD-filer kan vara används toocreate anpassade avbildningar, som används tooprovision virtuella datorer i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="75e68-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="75e68-105">Den här artikeln beskrivs hur toouse [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooa labblagringskontot tooupload en VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="75e68-105">This article illustrates how toouse [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="75e68-106">När du har överfört VHD-filen hello [nästa steg avsnittet](#next-steps) innehåller vissa artiklar som illustrerar hur toocreate en anpassad avbildning från hello överföra VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="75e68-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="75e68-107">Mer information om diskar och virtuella hårddiskar i Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="75e68-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="75e68-108">Stegvisa instruktioner</span><span class="sxs-lookup"><span data-stu-id="75e68-108">Step-by-step instructions</span></span>

<span data-ttu-id="75e68-109">Hej följande steg gå dig genom att ladda upp en VHD-filen tooDevTest övningar med [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="75e68-109">hello following steps walk you through uploading a VHD file tooDevTest Labs using [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>

1. <span data-ttu-id="75e68-110">[Hämta och installera hello senaste versionen av hello Microsoft Azure Lagringsutforskaren](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="75e68-110">[Download and install hello latest version of hello Microsoft Azure Storage Explorer](http://www.storageexplorer.com).</span></span>

1. <span data-ttu-id="75e68-111">Hämta hello namnet på hello lab storage-konto med hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="75e68-111">Get hello name of hello lab's storage account using hello Azure portal:</span></span>

    1. <span data-ttu-id="75e68-112">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="75e68-112">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
    
    1. <span data-ttu-id="75e68-113">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="75e68-113">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>
    
    1. <span data-ttu-id="75e68-114">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="75e68-114">From hello list of labs, select hello desired lab.</span></span>  
    
    1. <span data-ttu-id="75e68-115">På bladet hello lab väljer **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="75e68-115">On hello lab's blade, select **Configuration**.</span></span> 
    
    1. <span data-ttu-id="75e68-116">På hello lab **Configuration** bladet väljer **anpassade avbildningar (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="75e68-116">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>
    
    1. <span data-ttu-id="75e68-117">På hello **anpassade avbildningar** bladet välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="75e68-117">On hello **Custom images** blade, Select **+Add**.</span></span> 
    
    1. <span data-ttu-id="75e68-118">På hello **anpassad bild** bladet väljer **VHD**.</span><span class="sxs-lookup"><span data-stu-id="75e68-118">On hello **Custom image** blade, select **VHD**.</span></span>
    
    1. <span data-ttu-id="75e68-119">På hello **VHD** bladet väljer **överföra en virtuell Hårddisk med hjälp av PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="75e68-119">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>
    
        ![Överför VHD med PowerShell][0]
    
    1. <span data-ttu-id="75e68-121">Hej **ladda upp en bild med PowerShell** bladet visar ett anrop toohello **Add-AzureVhd** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75e68-121">hello **Upload an image using PowerShell** blade displays a call toohello **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="75e68-122">Hej första parametern (*mål*) innehåller hello lagringskontonamnet för hello labbet i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="75e68-122">hello first parameter (*Destination*) contains hello storage account name for hello lab in hello following format:</span></span>
    
        <span data-ttu-id="75e68-123">https://<STORAGE-ACCOUNT-Name>.BLOB.Core.Windows.NET/uploads/...</span><span class="sxs-lookup"><span data-stu-id="75e68-123">https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...</span></span> 

    1. <span data-ttu-id="75e68-124">Anteckna hello lagringskontonamnet eftersom den används i senare steg.</span><span class="sxs-lookup"><span data-stu-id="75e68-124">Make note of hello storage account name as it is used in later steps.</span></span>
    
1. <span data-ttu-id="75e68-125">Ansluta tooan konto för Azure-prenumeration med Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="75e68-125">Connect tooan Azure subscription account using Storage Explorer.</span></span>

    > [!TIP] 
    > 
    > <span data-ttu-id="75e68-126">Lagringsutforskaren stöder flera anslutningsalternativ.</span><span class="sxs-lookup"><span data-stu-id="75e68-126">Storage Explorer supports several connection options.</span></span> <span data-ttu-id="75e68-127">Det här avsnittet visar anslutning tooa storage-konto som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="75e68-127">This section illustrates connecting tooa storage account associated with your Azure subscription.</span></span> <span data-ttu-id="75e68-128">toosee hello andra anslutningsalternativ som stöds av Lagringsutforskaren finns i artikeln toohello [komma igång med Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="75e68-128">toosee hello other connection options supported by Storage Explorer, refer toohello article, [Getting started with Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>
 
    1. <span data-ttu-id="75e68-129">Öppna Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="75e68-129">Open Storage Explorer.</span></span>
    
    1. <span data-ttu-id="75e68-130">I Lagringsutforskaren, Välj **Azure kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="75e68-130">In Storage Explorer, select **Azure Account settings**.</span></span> 
    
        ![Inställningar för Azure-konto][1]
    
    1. <span data-ttu-id="75e68-132">hello vänster visar hello Microsoft-konton som du har loggat in till.</span><span class="sxs-lookup"><span data-stu-id="75e68-132">hello left pane displays hello Microsoft accounts you've logged in to.</span></span> <span data-ttu-id="75e68-133">tooconnect tooanother kontot som väljer **Lägg till ett konto**, och följ hello dialogrutor toosign in med ett Microsoft-konto som är associerad med minst en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="75e68-133">tooconnect tooanother account, select **Add an account**, and follow hello dialogs toosign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>
    
        ![Lägga till ett konto][2]
    
    1. <span data-ttu-id="75e68-135">När du loggar in med ett Microsoft-konto, fylls hello vänster med hello Azure-prenumerationer som är associerade med det kontot.</span><span class="sxs-lookup"><span data-stu-id="75e68-135">Once you successfully sign in with a Microsoft account, hello left pane populates with hello Azure subscriptions associated with that account.</span></span> <span data-ttu-id="75e68-136">Välj hello Azure-prenumerationer som du vill toowork och välj sedan **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="75e68-136">Select hello Azure subscriptions with which you want toowork, and then select **Apply**.</span></span> <span data-ttu-id="75e68-137">(Att välja **alla prenumerationer** växlar hello val av alla eller inga av hello som anges Azure-prenumerationer.)</span><span class="sxs-lookup"><span data-stu-id="75e68-137">(Selecting **All subscriptions** toggles hello selection of all or none of hello listed Azure subscriptions.)</span></span>
    
        ![Välja Azure-prenumerationer][3]
    
    1. <span data-ttu-id="75e68-139">hello vänster visar hello storage-konton som är associerade med hello valda Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="75e68-139">hello left pane displays hello storage accounts associated with hello selected Azure subscriptions.</span></span>
    
        ![Valda Azure-prenumerationer][4]

1. <span data-ttu-id="75e68-141">Leta upp hello labblagringskontot:</span><span class="sxs-lookup"><span data-stu-id="75e68-141">Locate hello lab's storage account:</span></span>

    1. <span data-ttu-id="75e68-142">Leta upp hello Lagringsutforskaren vänster och expanderar hello noden för hello Azure-prenumeration som äger hello labbet.</span><span class="sxs-lookup"><span data-stu-id="75e68-142">In hello Storage Explorer left pane, locate, and expand hello node for hello Azure subscription that owns hello lab.</span></span>
    
    1. <span data-ttu-id="75e68-143">Expandera under hello prenumeration nod **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="75e68-143">Under hello subscription's node, expand **Storage Accounts**.</span></span>

    1. <span data-ttu-id="75e68-144">Expandera hello lab konto nod tooreveal lagringsnoder för **Blobbbehållare**, **filresurser**, **köer**, och **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="75e68-144">Expand hello lab's storage account node tooreveal nodes for **Blob Containers**, **File Shares**, **Queues**, and **Tables**.</span></span>
    
    1. <span data-ttu-id="75e68-145">Expandera hello **Blobbbehållare** nod.</span><span class="sxs-lookup"><span data-stu-id="75e68-145">Expand hello **Blob Containers** node.</span></span>
    
    1. <span data-ttu-id="75e68-146">Välj hello överföringar blob-behållaren toodisplay innehållet i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="75e68-146">Select hello uploads blob container toodisplay its contents in hello right pane.</span></span>
        
        ![Ladda upp katalogen][5]

1. <span data-ttu-id="75e68-148">Överför hello VHD-fil med Lagringsutforskaren:</span><span class="sxs-lookup"><span data-stu-id="75e68-148">Upload hello VHD file using Storage Explorer:</span></span>

    1. <span data-ttu-id="75e68-149">I hello Lagringsutforskaren högra rutan, bör du se en lista över hello blobbar i hello **överför** hello labblagringskontot blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="75e68-149">In hello Storage Explorer right pane, you should see a listing of hello blobs in hello **uploads** blob container of hello lab's storage account.</span></span> <span data-ttu-id="75e68-150">Välj hello blob editor verktygsfältet **överför**</span><span class="sxs-lookup"><span data-stu-id="75e68-150">On hello blob editor toolbar, select **Upload**</span></span> 
        
        ![Överför knappen][6]
    
    1. <span data-ttu-id="75e68-152">Från hello **överför** nedrullningsbara menyn och väljer **Överför filer...** .</span><span class="sxs-lookup"><span data-stu-id="75e68-152">From hello **Upload** drop-down menu, select **Upload files...**.</span></span>
    
    1. <span data-ttu-id="75e68-153">På hello **ladda upp filer** dialogrutan, Välj hello tre punkter.</span><span class="sxs-lookup"><span data-stu-id="75e68-153">On hello **Upload files** dialog, select hello ellipsis.</span></span>
        
        ![Välj fil][8]  

    1. <span data-ttu-id="75e68-155">På hello **Välj filer tooupload** dialogrutan Bläddra toohello önskad VHD-filen markerar du den och välj sedan **öppna**.</span><span class="sxs-lookup"><span data-stu-id="75e68-155">On hello **Select files tooupload** dialog, browse toohello desired VHD file, select it, and then select **Open**.</span></span>
    
    1. <span data-ttu-id="75e68-156">När returnerade toohello **ladda upp filer** dialogrutan Ändra **Blob typen** för**Sidblob**.</span><span class="sxs-lookup"><span data-stu-id="75e68-156">When returned toohello **Upload files** dialog, change **Blob type** too**Page Blob**.</span></span>
    
    1. <span data-ttu-id="75e68-157">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="75e68-157">Select **Upload**.</span></span>

        ![Välj fil][9]  
    
    1. <span data-ttu-id="75e68-159">hello Lagringsutforskaren **aktivitetsloggen** visar hello download status (tillsammans med länkar toocancel hello överför).</span><span class="sxs-lookup"><span data-stu-id="75e68-159">hello Storage Explorer **Activity Log** pane shows hello download status (along with links toocancel hello upload).</span></span> <span data-ttu-id="75e68-160">hello processen överför en VHD-fil kan vara tidskrävande beroende på hello storleken på hello VHD-filen och anslutningshastigheten.</span><span class="sxs-lookup"><span data-stu-id="75e68-160">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span> 

        ![Överför filstatus][10]  

## <a name="next-steps"></a><span data-ttu-id="75e68-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75e68-162">Next steps</span></span>

- [<span data-ttu-id="75e68-163">Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="75e68-163">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="75e68-164">Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="75e68-164">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
