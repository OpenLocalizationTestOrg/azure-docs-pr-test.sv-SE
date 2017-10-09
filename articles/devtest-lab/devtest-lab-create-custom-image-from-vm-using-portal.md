---
title: "aaaCreate en anpassad avbildning i Azure DevTest Labs från en virtuell dator | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avbildning i Azure DevTest Labs från ett etablerade VM använder hello Azure-portalen"
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
ms.openlocfilehash: 7dccb79d3db4aae676c7bd2f6b800301210491e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vm"></a><span data-ttu-id="53379-103">Skapa en anpassad avbildning från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="53379-103">Create a custom image from a VM</span></span>

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="53379-104">Stegvisa instruktioner</span><span class="sxs-lookup"><span data-stu-id="53379-104">Step-by-step instructions</span></span>

<span data-ttu-id="53379-105">Du kan skapa en anpassad avbildning från en allokerad virtuell dator och därefter använda den anpassade bilden toocreate identiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="53379-105">You can create a custom image from a provisioned VM, and afterwards use that custom image toocreate identical VMs.</span></span> <span data-ttu-id="53379-106">hello följande steg visar hur toocreate en anpassad avbildning från en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="53379-106">hello following steps illustrate how toocreate a custom image from a VM:</span></span>

1. <span data-ttu-id="53379-107">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="53379-107">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="53379-108">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="53379-108">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="53379-109">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="53379-109">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="53379-110">På bladet hello lab väljer **Mina virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="53379-110">On hello lab's blade, select **My virtual machines**.</span></span>
 
1. <span data-ttu-id="53379-111">På hello **Mina virtuella datorer** bladet välj hello VM som du vill toocreate hello anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="53379-111">On hello **My virtual machines** blade, select hello VM from which you want toocreate hello custom image.</span></span>

1. <span data-ttu-id="53379-112">På bladet hello VM väljer **Skapa anpassad avbildning (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="53379-112">On hello VM's blade, select **Create custom image (VHD)**.</span></span>

    ![Skapa anpassad avbildning menyobjekt](./media/devtest-lab-create-template/create-custom-image.png)

1. <span data-ttu-id="53379-114">På hello **skapa avbildningen** bladet, ange ett namn och beskrivning för den anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="53379-114">On hello **Create image** blade, enter a name and description for your custom image.</span></span> <span data-ttu-id="53379-115">Den här informationen visas i hello lista över databaser när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="53379-115">This information is displayed in hello list of bases when you create a VM.</span></span>

    ![Skapa anpassad avbildning bladet](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. <span data-ttu-id="53379-117">Välj om sysprep kördes på hello VM.</span><span class="sxs-lookup"><span data-stu-id="53379-117">Select whether sysprep was run on hello VM.</span></span> <span data-ttu-id="53379-118">Om hello sysprep inte kördes på hello VM, kan du ange om du vill att sysprep körs när en virtuell dator skapas från den här anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="53379-118">If hello sysprep was not run on hello VM, specify whether you want sysprep run when a VM is created from this custom image.</span></span>

1. <span data-ttu-id="53379-119">Välj **OK** när klar toocreate hello anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="53379-119">Select **OK** when finished toocreate hello custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="53379-120">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="53379-120">Related blog posts</span></span>

- [<span data-ttu-id="53379-121">Anpassade avbildningar eller formler?</span><span class="sxs-lookup"><span data-stu-id="53379-121">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="53379-122">Kopiera anpassade avbildningar mellan Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="53379-122">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="53379-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53379-123">Next steps</span></span>

- [<span data-ttu-id="53379-124">Lägga till ett VM tooyour labb</span><span class="sxs-lookup"><span data-stu-id="53379-124">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
