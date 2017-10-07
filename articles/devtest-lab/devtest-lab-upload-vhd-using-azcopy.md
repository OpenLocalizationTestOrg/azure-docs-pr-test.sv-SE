---
title: "aaaUpload VHD-filen med hjälp av AzCopy för tooAzure DevTest Labs | Microsoft Docs"
description: "Överföra VHD-filen toolab lagringskonto med hjälp av AzCopy"
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
ms.openlocfilehash: 14f9e933b0bd27451f6bcb94841ecc381213e578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-azcopy"></a><span data-ttu-id="2fab0-103">Överföra VHD-filen toolab lagringskonto med hjälp av AzCopy</span><span class="sxs-lookup"><span data-stu-id="2fab0-103">Upload VHD file toolab's storage account using AzCopy</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="2fab0-104">VHD-filer kan vara används toocreate anpassade avbildningar, som används tooprovision virtuella datorer i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="2fab0-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="2fab0-105">hello följande steg får du hjälp med hello AzCopy kommandoradsverktyget tooupload en VHD-filen tooa's labblagringskontot.</span><span class="sxs-lookup"><span data-stu-id="2fab0-105">hello following steps walk you through using hello AzCopy command-line utility tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="2fab0-106">När du har överfört VHD-filen hello [nästa steg avsnittet](#next-steps) innehåller vissa artiklar som illustrerar hur toocreate en anpassad avbildning från hello överföra VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="2fab0-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="2fab0-107">Mer information om diskar och virtuella hårddiskar i Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="2fab0-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

> [!NOTE] 
>  
> <span data-ttu-id="2fab0-108">AzCopy är ett kommandoradsverktyg som endast för Windows.</span><span class="sxs-lookup"><span data-stu-id="2fab0-108">AzCopy is a Windows-only command-line utility.</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="2fab0-109">Stegvisa instruktioner</span><span class="sxs-lookup"><span data-stu-id="2fab0-109">Step-by-step instructions</span></span>

<span data-ttu-id="2fab0-110">Hej följande steg gå du hjälp med att ladda upp en VHD-filen tooAzure DevTest Labs med [AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="2fab0-110">hello following steps walk you through uploading a VHD file tooAzure DevTest Labs using [AzCopy](http://aka.ms/downloadazcopy).</span></span> 

1. <span data-ttu-id="2fab0-111">Hämta hello namnet på hello lab storage-konto med hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="2fab0-111">Get hello name of hello lab's storage account using hello Azure portal:</span></span>

1. <span data-ttu-id="2fab0-112">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="2fab0-112">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="2fab0-113">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="2fab0-113">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="2fab0-114">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="2fab0-114">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="2fab0-115">På bladet hello lab väljer **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="2fab0-115">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="2fab0-116">På hello lab **Configuration** bladet väljer **anpassade avbildningar (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="2fab0-116">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="2fab0-117">På hello **anpassade avbildningar** bladet välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2fab0-117">On hello **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="2fab0-118">På hello **anpassad bild** bladet väljer **VHD**.</span><span class="sxs-lookup"><span data-stu-id="2fab0-118">On hello **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="2fab0-119">På hello **VHD** bladet väljer **överföra en virtuell Hårddisk med hjälp av PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="2fab0-119">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Överför VHD med PowerShell](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. <span data-ttu-id="2fab0-121">Hej **ladda upp en bild med PowerShell** bladet visar ett anrop toohello **Add-AzureVhd** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="2fab0-121">hello **Upload an image using PowerShell** blade displays a call toohello **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="2fab0-122">Hej första parametern (*mål*) innehåller hello URI för en blob-behållare (*överför*) i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="2fab0-122">hello first parameter (*Destination*) contains hello URI for a blob container (*uploads*) in hello following format:</span></span>

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. <span data-ttu-id="2fab0-123">Anteckna hello fullständiga URI eftersom den används i senare steg.</span><span class="sxs-lookup"><span data-stu-id="2fab0-123">Make note of hello full URI as it is used in later steps.</span></span>

1. <span data-ttu-id="2fab0-124">Överför hello VHD-fil med AzCopy:</span><span class="sxs-lookup"><span data-stu-id="2fab0-124">Upload hello VHD file using AzCopy:</span></span>
 
1. <span data-ttu-id="2fab0-125">[Hämta och installera hello senaste versionen av AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="2fab0-125">[Download and install hello latest version of AzCopy](http://aka.ms/downloadazcopy).</span></span>

1. <span data-ttu-id="2fab0-126">Öppna ett kommandofönster och gå toohello installationskatalogen för AzCopy.</span><span class="sxs-lookup"><span data-stu-id="2fab0-126">Open a command window and navigate toohello AzCopy installation directory.</span></span> <span data-ttu-id="2fab0-127">Du kan också kan du lägga till hello AzCopy tooyour systemsökvägen installationsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2fab0-127">Optionally, you can add hello AzCopy installation location tooyour system path.</span></span> <span data-ttu-id="2fab0-128">AzCopy är som standard installerade toohello följande katalog:</span><span class="sxs-lookup"><span data-stu-id="2fab0-128">By default, AzCopy is installed toohello following directory:</span></span>

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. <span data-ttu-id="2fab0-129">Med hjälp av hello konto nyckel och blob lagringsbehållaren URI, kör följande kommando vid kommandotolken hello hello.</span><span class="sxs-lookup"><span data-stu-id="2fab0-129">Using hello storage account key and blob container URI, run hello following command at hello command prompt.</span></span> <span data-ttu-id="2fab0-130">Hej *vhdFileName* värdet måste toobe inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="2fab0-130">hello *vhdFileName* value needs toobe in quotes.</span></span> <span data-ttu-id="2fab0-131">hello processen överför en VHD-fil kan vara tidskrävande beroende på hello storleken på hello VHD-filen och anslutningshastigheten.</span><span class="sxs-lookup"><span data-stu-id="2fab0-131">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span>   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a><span data-ttu-id="2fab0-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2fab0-132">Next steps</span></span>

- [<span data-ttu-id="2fab0-133">Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2fab0-133">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="2fab0-134">Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="2fab0-134">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)