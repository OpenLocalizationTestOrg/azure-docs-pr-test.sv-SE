---
title: "aaaUpload VHD-filen tooAzure DevTest Labs med hjälp av PowerShell | Microsoft Docs"
description: "Överföra VHD-filen toolab storage-konto med hjälp av PowerShell"
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
ms.openlocfilehash: 9c3ee96e212457b0ef8203714b419350cb97f895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-powershell"></a><span data-ttu-id="6a143-103">Överföra VHD-filen toolab storage-konto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a143-103">Upload VHD file toolab's storage account using PowerShell</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="6a143-104">VHD-filer kan vara används toocreate anpassade avbildningar, som används tooprovision virtuella datorer i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="6a143-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="6a143-105">hello följande steg beskriver hur du använder PowerShell tooupload en VHD-filen tooa's labblagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6a143-105">hello following steps walk you through using PowerShell tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="6a143-106">När du har överfört VHD-filen hello [nästa steg avsnittet](#next-steps) innehåller vissa artiklar som illustrerar hur toocreate en anpassad avbildning från hello överföra VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="6a143-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="6a143-107">Mer information om diskar och virtuella hårddiskar i Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="6a143-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="6a143-108">Stegvisa instruktioner</span><span class="sxs-lookup"><span data-stu-id="6a143-108">Step-by-step instructions</span></span>

<span data-ttu-id="6a143-109">hello följande steg gå du hjälp med att ladda upp en VHD-filen tooAzure DevTest Labs med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6a143-109">hello following steps walk you through uploading a VHD file tooAzure DevTest Labs using PowerShell.</span></span> 

1. <span data-ttu-id="6a143-110">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="6a143-110">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="6a143-111">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="6a143-111">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="6a143-112">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="6a143-112">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="6a143-113">På bladet hello lab väljer **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="6a143-113">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="6a143-114">På hello lab **Configuration** bladet väljer **anpassade avbildningar (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="6a143-114">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="6a143-115">På hello **anpassade avbildningar** bladet välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6a143-115">On hello **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="6a143-116">På hello **anpassad bild** bladet väljer **VHD**.</span><span class="sxs-lookup"><span data-stu-id="6a143-116">On hello **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="6a143-117">På hello **VHD** bladet väljer **överföra en virtuell Hårddisk med hjälp av PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6a143-117">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Överför VHD med PowerShell](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. <span data-ttu-id="6a143-119">På hello **ladda upp en bild med PowerShell** bladet, kopiera hello genereras PowerShell-skript tooa textredigerare.</span><span class="sxs-lookup"><span data-stu-id="6a143-119">On hello **Upload an image using PowerShell** blade, copy hello generated PowerShell script tooa text editor.</span></span>

1. <span data-ttu-id="6a143-120">Ändra hello **LocalFilePath** parametern för hello **Add-AzureVhd** cmdlet toopoint toohello platsen för hello VHD-filen som du vill tooupload.</span><span class="sxs-lookup"><span data-stu-id="6a143-120">Modify hello **LocalFilePath** parameter of hello **Add-AzureVhd** cmdlet toopoint toohello location of hello VHD file you want tooupload.</span></span>

1. <span data-ttu-id="6a143-121">I PowerShell-Kommandotolken kör hello **Add-AzureVhd** cmdlet (med hello ändrade **LocalFilePath** parametern).</span><span class="sxs-lookup"><span data-stu-id="6a143-121">At a PowerShell prompt, run hello **Add-AzureVhd** cmdlet (with hello modified **LocalFilePath** parameter).</span></span>

> [!WARNING] 
> 
> <span data-ttu-id="6a143-122">hello processen överför en VHD-fil kan vara tidskrävande beroende på hello storleken på hello VHD-filen och anslutningshastigheten.</span><span class="sxs-lookup"><span data-stu-id="6a143-122">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a143-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a143-123">Next steps</span></span>

- [<span data-ttu-id="6a143-124">Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6a143-124">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="6a143-125">Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a143-125">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
