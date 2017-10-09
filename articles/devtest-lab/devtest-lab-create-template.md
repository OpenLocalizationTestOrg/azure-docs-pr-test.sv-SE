---
title: "aaaCreate en anpassad avbildning Azure DevTest Labs från en VHD-fil | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avbildning i Azure DevTest Labs från en VHD-filen med hello Azure-portalen"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 80af8ea1cb72380f868df0a76c4a0dcd92e63cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="691cf-103">Skapa en anpassad avbildning från en VHD-fil</span><span class="sxs-lookup"><span data-stu-id="691cf-103">Create a custom image from a VHD file</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="691cf-104">Stegvisa instruktioner</span><span class="sxs-lookup"><span data-stu-id="691cf-104">Step-by-step instructions</span></span>

<span data-ttu-id="691cf-105">hello följande steg beskriver hur du skapar en anpassad avbildning från en VHD-fil med hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="691cf-105">hello following steps walk you through creating a custom image from a VHD file using hello Azure portal:</span></span>

1. <span data-ttu-id="691cf-106">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="691cf-106">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="691cf-107">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="691cf-107">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="691cf-108">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="691cf-108">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="691cf-109">På bladet hello lab väljer **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="691cf-109">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="691cf-110">På hello lab **Configuration** bladet väljer **anpassade avbildningar (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="691cf-110">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="691cf-111">På hello **anpassade avbildningar** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="691cf-111">On hello **Custom images** blade, select **+Add**.</span></span>

    ![Lägg till anpassad bild](./media/devtest-lab-create-template/add-custom-image.png)

1. <span data-ttu-id="691cf-113">Ange hello namn på hello anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="691cf-113">Enter hello name of hello custom image.</span></span> <span data-ttu-id="691cf-114">Det här namnet visas i hello lista över grundläggande bilder när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="691cf-114">This name is displayed in hello list of base images when creating a VM.</span></span>

1. <span data-ttu-id="691cf-115">Ange hello beskrivning av hello anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="691cf-115">Enter hello description of hello custom image.</span></span> <span data-ttu-id="691cf-116">Den här beskrivningen visas i hello lista över grundläggande bilder när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="691cf-116">This description is displayed in hello list of base images when creating a VM.</span></span>

1. <span data-ttu-id="691cf-117">Välj **VHD**.</span><span class="sxs-lookup"><span data-stu-id="691cf-117">Select **VHD**.</span></span>

1. <span data-ttu-id="691cf-118">Från hello **VHD** bladet, Välj hello önskad VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="691cf-118">From hello **VHD** blade, select hello desired VHD file.</span></span>

1. <span data-ttu-id="691cf-119">Välj **OK** tooclose hello **VHD** bladet.</span><span class="sxs-lookup"><span data-stu-id="691cf-119">Select **OK** tooclose hello **VHD** blade.</span></span>

1. <span data-ttu-id="691cf-120">Välj **Operativsystemskonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="691cf-120">Select **OS configuration**.</span></span>

1. <span data-ttu-id="691cf-121">På hello **Operativsystemskonfiguration** väljer du antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="691cf-121">On hello **OS configuration** tab, select either **Windows** or **Linux**.</span></span>

1. <span data-ttu-id="691cf-122">Om **Windows** har valts anger via hello kryssrutan om *Sysprep* har körts på hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="691cf-122">If **Windows** is selected, specify via hello checkbox whether *Sysprep* has been run on hello machine.</span></span> 

1. <span data-ttu-id="691cf-123">Välj **OK** tooclose hello **Operativsystemets konfiguration** bladet.</span><span class="sxs-lookup"><span data-stu-id="691cf-123">Select **OK** tooclose hello **OS configuration** blade.</span></span>

1. <span data-ttu-id="691cf-124">Välj **OK** toocreate hello anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="691cf-124">Select **OK** toocreate hello custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="691cf-125">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="691cf-125">Related blog posts</span></span>

- [<span data-ttu-id="691cf-126">Anpassade avbildningar eller formler?</span><span class="sxs-lookup"><span data-stu-id="691cf-126">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="691cf-127">Kopiera anpassade avbildningar mellan Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="691cf-127">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="691cf-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="691cf-128">Next steps</span></span>

- [<span data-ttu-id="691cf-129">Lägga till ett VM tooyour labb</span><span class="sxs-lookup"><span data-stu-id="691cf-129">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
