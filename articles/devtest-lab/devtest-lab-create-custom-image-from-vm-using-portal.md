---
title: "Skapa en anpassad avbildning i Azure DevTest Labs från en virtuell dator | Microsoft Docs"
description: "Lär dig hur du skapar en anpassad avbildning i Azure DevTest Labs från en allokerad virtuell dator med hjälp av Azure portal"
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
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vm"></a><span data-ttu-id="12c38-103">Skapa en anpassad avbildning från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="12c38-103">Create a custom image from a VM</span></span>

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="12c38-104">Stegvisa instruktioner</span><span class="sxs-lookup"><span data-stu-id="12c38-104">Step-by-step instructions</span></span>

<span data-ttu-id="12c38-105">Du kan skapa en anpassad avbildning från en allokerad virtuell dator och därefter använda den anpassade bilden för att skapa identiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="12c38-105">You can create a custom image from a provisioned VM, and afterwards use that custom image to create identical VMs.</span></span> <span data-ttu-id="12c38-106">Följande steg visar hur du skapar en anpassad avbildning från en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="12c38-106">The following steps illustrate how to create a custom image from a VM:</span></span>

1. <span data-ttu-id="12c38-107">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="12c38-107">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="12c38-108">Välj **Fler tjänster** och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="12c38-108">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="12c38-109">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="12c38-109">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="12c38-110">På den testmiljön bladet välj **Mina virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="12c38-110">On the lab's blade, select **My virtual machines**.</span></span>
 
1. <span data-ttu-id="12c38-111">På den **Mina virtuella datorer** bladet Välj den virtuella datorn från vilken du vill skapa den anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="12c38-111">On the **My virtual machines** blade, select the VM from which you want to create the custom image.</span></span>

1. <span data-ttu-id="12c38-112">På bladet för den virtuella datorn, väljer **Skapa anpassad avbildning (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="12c38-112">On the VM's blade, select **Create custom image (VHD)**.</span></span>

    ![Skapa anpassad avbildning menyobjekt](./media/devtest-lab-create-template/create-custom-image.png)

1. <span data-ttu-id="12c38-114">På den **skapa avbildningen** bladet, ange ett namn och beskrivning för den anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="12c38-114">On the **Create image** blade, enter a name and description for your custom image.</span></span> <span data-ttu-id="12c38-115">Den här informationen visas i listan över databaser när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="12c38-115">This information is displayed in the list of bases when you create a VM.</span></span>

    ![Skapa anpassad avbildning bladet](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. <span data-ttu-id="12c38-117">Välj om sysprep körs på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="12c38-117">Select whether sysprep was run on the VM.</span></span> <span data-ttu-id="12c38-118">Om inte kört sysprep på den virtuella datorn, kan du ange om du vill att sysprep körs när en virtuell dator skapas från den här anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="12c38-118">If the sysprep was not run on the VM, specify whether you want sysprep run when a VM is created from this custom image.</span></span>

1. <span data-ttu-id="12c38-119">Välj **OK** när du är klar för att skapa den anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="12c38-119">Select **OK** when finished to create the custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="12c38-120">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="12c38-120">Related blog posts</span></span>

- [<span data-ttu-id="12c38-121">Anpassade avbildningar eller formler?</span><span class="sxs-lookup"><span data-stu-id="12c38-121">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="12c38-122">Kopiera anpassade avbildningar mellan Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="12c38-122">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="12c38-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12c38-123">Next steps</span></span>

- [<span data-ttu-id="12c38-124">Lägga till en virtuell dator i labbet</span><span class="sxs-lookup"><span data-stu-id="12c38-124">Add a VM to your lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
