---
title: "Skapa din första virtuella datorn i ett labb i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur du skapar din första virtuella datorn i ett labb i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: tarcher
ms.openlocfilehash: aa6b60b799e1e98815cf288d5612f98cd77cc00e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="5c29b-103">Skapa din första virtuella datorn i ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5c29b-103">Create your first VM in a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="5c29b-104">När du först komma åt DevTest Labs och vill skapa din första virtuella datorn, kommer troligen gör du det med hjälp av en förinstallerade [grundläggande marketplace-avbildning](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="5c29b-104">When you initially access DevTest Labs and want to create your first VM, you will likely do so using a pre-loaded [base marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="5c29b-105">Vid ett senare tillfälle du kommer även att kunna välja en [anpassad avbildning och en formel](devtest-lab-add-vm.md) när du skapar flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5c29b-105">Later on, you'll also be able to choose from a [custom image and a formula](devtest-lab-add-vm.md) when creating more VMs.</span></span> 

<span data-ttu-id="5c29b-106">Den här självstudiekursen vägleder dig genom att använda Azure portal för att lägga till den första virtuella datorn i ett labb i DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="5c29b-106">This tutorial walks you through using the Azure portal to add your first VM to a lab in DevTest Labs.</span></span>

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="5c29b-107">Steg för att lägga till den första virtuella datorn i ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5c29b-107">Steps to add your first VM to a lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="5c29b-108">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="5c29b-108">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="5c29b-109">Välj **fler tjänster**, och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="5c29b-109">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
1. <span data-ttu-id="5c29b-110">Välj labbet där du vill skapa den virtuella datorn från listan över övningar.</span><span class="sxs-lookup"><span data-stu-id="5c29b-110">From the list of labs, select the lab in which you want to create the VM.</span></span>  
1. <span data-ttu-id="5c29b-111">På testmiljön **översikt** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5c29b-111">On the lab's **Overview** blade, select **+ Add**.</span></span>  

    ![VM-knappen Lägg till](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="5c29b-113">På den **väljer du en bas** bladet Välj en marketplace-avbildning för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5c29b-113">On the **Choose a base** blade, select a marketplace image for the VM.</span></span>
1. <span data-ttu-id="5c29b-114">På den **virtuella** bladet anger du ett namn för den nya virtuella datorn i den **virtuellt datornamn** textruta.</span><span class="sxs-lookup"><span data-stu-id="5c29b-114">On the **Virtual machine** blade, enter a name for the new virtual machine in the **Virtual machine name** text box.</span></span>

    ![Blad för labbet VM](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. <span data-ttu-id="5c29b-116">Ange en **användarnamn** som har beviljats administratörsbehörighet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5c29b-116">Enter a **User Name** that is granted administrator privileges on the virtual machine.</span></span>  
1. <span data-ttu-id="5c29b-117">Ange ett lösenord i fältet med etiketten **skriver ett värde**.</span><span class="sxs-lookup"><span data-stu-id="5c29b-117">Enter a password in the text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="5c29b-118">Den **virtuella disktyp** avgör vilken lagringstyp som disk är tillåten för virtuella datorer i labbet.</span><span class="sxs-lookup"><span data-stu-id="5c29b-118">The **Virtual machine disk type** determines which storage disk type is allowed for the virtual machines in the lab.</span></span>
1. <span data-ttu-id="5c29b-119">Välj **storlek för virtuell dator** och välj en av de fördefinierade objekt som anger processorkärnor, ram-storlek och storleken på hårddisken på den virtuella datorn för att skapa.</span><span class="sxs-lookup"><span data-stu-id="5c29b-119">Select **Virtual machine size** and select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span>
1. <span data-ttu-id="5c29b-120">Välj **artefakter** och - från listan över artefakter - Välj och konfigurera artefakter som du vill lägga till basavbildningen.</span><span class="sxs-lookup"><span data-stu-id="5c29b-120">Select **Artifacts** and - from the list of artifacts - select and configure the artifacts that you want to add to the base image.</span></span>
    <span data-ttu-id="5c29b-121">**Obs:** om du inte har arbetat med DevTest Labs eller konfigurera artefakter, referera till den [lägga till en befintlig artefakt till en virtuell dator](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) avsnittet och sedan återvända hit när du är klar.</span><span class="sxs-lookup"><span data-stu-id="5c29b-121">**Note:** If you're new to DevTest Labs or configuring artifacts, refer to the [Add an existing artifact to a VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="5c29b-122">Välj **skapa** att lägga till den angivna virtuella datorn i labbet.</span><span class="sxs-lookup"><span data-stu-id="5c29b-122">Select **Create** to add the specified VM to the lab.</span></span>

   <span data-ttu-id="5c29b-123">Blad för labbet visar status för den virtuella datorn skapas - först som **skapa**, sedan som **kör** när den virtuella datorn har startats.</span><span class="sxs-lookup"><span data-stu-id="5c29b-123">The lab blade displays the status of the VM's creation - first as **Creating**, then as **Running** after the VM has been started.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c29b-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c29b-124">Next steps</span></span>
* <span data-ttu-id="5c29b-125">När den virtuella datorn har skapats kan du ansluta till den virtuella datorn genom att välja **Anslut** på bladet för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5c29b-125">Once the VM has been created, you can connect to the VM by selecting **Connect** on the VM's blade.</span></span>
* <span data-ttu-id="5c29b-126">Checka ut [lägga till en virtuell dator i ett labb](devtest-lab-add-vm.md) fullständig information om hur du lägger till följande virtuella datorer i labbet.</span><span class="sxs-lookup"><span data-stu-id="5c29b-126">Check out [Add a VM to a lab](devtest-lab-add-vm.md) for more complete info about adding subsequent VMs in your lab.</span></span>
* <span data-ttu-id="5c29b-127">Utforska den [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span><span class="sxs-lookup"><span data-stu-id="5c29b-127">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span></span>
