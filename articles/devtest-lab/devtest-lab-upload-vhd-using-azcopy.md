---
title: "Överföra VHD-filen till Azure DevTest Labs med hjälp av AzCopy | Microsoft Docs"
description: "Överföra VHD-filen till labblagringskontot med hjälp av AzCopy"
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
ms.openlocfilehash: a4f43354740d9f17570932b0b9c753f46d67dc33
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a><span data-ttu-id="c18cf-103">Överföra VHD-filen till labblagringskontot med hjälp av AzCopy</span><span class="sxs-lookup"><span data-stu-id="c18cf-103">Upload VHD file to lab's storage account using AzCopy</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="c18cf-104">I Azure DevTest Labs kan VHD-filer användas för att skapa anpassade avbildningar som används för att etablera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c18cf-104">In Azure DevTest Labs, VHD files can be used to create custom images, which are used to provision virtual machines.</span></span> <span data-ttu-id="c18cf-105">Följande steg beskriver hur du använder kommandoradsverktyget azcopy för att ladda upp en VHD-fil till ett labb storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c18cf-105">The following steps walk you through using the AzCopy command-line utility to upload a VHD file to a lab's storage account.</span></span> <span data-ttu-id="c18cf-106">När du har överfört VHD-filen i [nästa steg avsnittet](#next-steps) innehåller vissa artiklar som illustrerar hur du skapar en anpassad avbildning från den överförda VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="c18cf-106">Once you've uploaded your VHD file, the [Next steps section](#next-steps) lists some articles that illustrate how to create a custom image from the uploaded VHD file.</span></span> <span data-ttu-id="c18cf-107">Mer information om diskar och virtuella hårddiskar i Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="c18cf-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

> [!NOTE] 
>  
> <span data-ttu-id="c18cf-108">AzCopy är ett kommandoradsverktyg som endast för Windows.</span><span class="sxs-lookup"><span data-stu-id="c18cf-108">AzCopy is a Windows-only command-line utility.</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="c18cf-109">Stegvisa instruktioner</span><span class="sxs-lookup"><span data-stu-id="c18cf-109">Step-by-step instructions</span></span>

<span data-ttu-id="c18cf-110">I följande steg beskriver hur du överför en VHD-fil till Azure DevTest Labs med [AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="c18cf-110">The following steps walk you through uploading a VHD file to Azure DevTest Labs using [AzCopy](http://aka.ms/downloadazcopy).</span></span> 

1. <span data-ttu-id="c18cf-111">Hämta namnet på den testmiljön storage-konto med Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="c18cf-111">Get the name of the lab's storage account using the Azure portal:</span></span>

1. <span data-ttu-id="c18cf-112">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="c18cf-112">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="c18cf-113">Välj **Fler tjänster** och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="c18cf-113">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="c18cf-114">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="c18cf-114">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="c18cf-115">På den testmiljön bladet välj **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="c18cf-115">On the lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="c18cf-116">Om labbet **Configuration** bladet väljer **anpassade avbildningar (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="c18cf-116">On the lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="c18cf-117">På den **anpassade avbildningar** bladet välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c18cf-117">On the **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="c18cf-118">På den **anpassad bild** bladet väljer **VHD**.</span><span class="sxs-lookup"><span data-stu-id="c18cf-118">On the **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="c18cf-119">På den **VHD** bladet väljer **överföra en virtuell Hårddisk med hjälp av PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="c18cf-119">On the **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Överför VHD med PowerShell](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. <span data-ttu-id="c18cf-121">Den **ladda upp en bild med PowerShell** bladet visar ett anrop till den **Add-AzureVhd** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c18cf-121">The **Upload an image using PowerShell** blade displays a call to the **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="c18cf-122">Den första parametern (*mål*) innehåller URI för en blob-behållare (*överför*) i följande format:</span><span class="sxs-lookup"><span data-stu-id="c18cf-122">The first parameter (*Destination*) contains the URI for a blob container (*uploads*) in the following format:</span></span>

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. <span data-ttu-id="c18cf-123">Anteckna den fullständiga URI eftersom den används i senare steg.</span><span class="sxs-lookup"><span data-stu-id="c18cf-123">Make note of the full URI as it is used in later steps.</span></span>

1. <span data-ttu-id="c18cf-124">Överför VHD-filen med hjälp av AzCopy:</span><span class="sxs-lookup"><span data-stu-id="c18cf-124">Upload the VHD file using AzCopy:</span></span>
 
1. <span data-ttu-id="c18cf-125">[Hämta och installera den senaste versionen av AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="c18cf-125">[Download and install the latest version of AzCopy](http://aka.ms/downloadazcopy).</span></span>

1. <span data-ttu-id="c18cf-126">Öppna ett kommandofönster och gå till installationskatalogen för AzCopy.</span><span class="sxs-lookup"><span data-stu-id="c18cf-126">Open a command window and navigate to the AzCopy installation directory.</span></span> <span data-ttu-id="c18cf-127">Alternativt kan du lägga till installationsplatsen AzCopy systemsökvägen.</span><span class="sxs-lookup"><span data-stu-id="c18cf-127">Optionally, you can add the AzCopy installation location to your system path.</span></span> <span data-ttu-id="c18cf-128">Som standard installeras AzCopy till följande katalog:</span><span class="sxs-lookup"><span data-stu-id="c18cf-128">By default, AzCopy is installed to the following directory:</span></span>

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. <span data-ttu-id="c18cf-129">Med den kontot nyckel och blob lagringsbehållaren URI, kör följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="c18cf-129">Using the storage account key and blob container URI, run the following command at the command prompt.</span></span> <span data-ttu-id="c18cf-130">Den *vhdFileName* värdet måste vara inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="c18cf-130">The *vhdFileName* value needs to be in quotes.</span></span> <span data-ttu-id="c18cf-131">Processen att överföra VHD-filen kan vara tidskrävande beroende på storleken på VHD-filen och anslutningshastigheten.</span><span class="sxs-lookup"><span data-stu-id="c18cf-131">The process of uploading a VHD file can be lengthy depending on the size of the VHD file and your connection speed.</span></span>   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a><span data-ttu-id="c18cf-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c18cf-132">Next steps</span></span>

- [<span data-ttu-id="c18cf-133">Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="c18cf-133">Create a custom image in Azure DevTest Labs from a VHD file using the Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="c18cf-134">Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="c18cf-134">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)