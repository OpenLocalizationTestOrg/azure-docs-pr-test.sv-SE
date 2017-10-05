---
title: "Lägg till en claimable virtuell dator i ett labb i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur du lägger till en claimable virtuell dator i ett labb i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: 98950d72e90b0e178bae2fffa7644fd824a25eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="4a42f-103">Lägg till en claimable virtuell dator i ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="4a42f-103">Add a claimable VM to a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="4a42f-104">Du lägger till en claimable virtuell dator i ett labb på liknande sätt att hur du [lägga till en standard VM](devtest-lab-add-vm.md) – från en *grundläggande* som är antingen en [anpassad avbildning](devtest-lab-create-template.md), [formeln](devtest-lab-manage-formulas.md) , eller [Marketplace-avbildning](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="4a42f-104">You add a claimable VM to a lab in a similar manner to how you [add a standard VM](devtest-lab-add-vm.md) – from a *base* that is either a [custom image](devtest-lab-create-template.md), [formula](devtest-lab-manage-formulas.md), or [Marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="4a42f-105">Den här självstudiekursen vägleder dig genom att använda Azure portal för att lägga till en claimable virtuell dator i ett labb i DevTest Labs och visas hur en användare följer för att kräva att den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4a42f-105">This tutorial walks you through using the Azure portal to add a claimable VM to a lab in DevTest Labs, and shows the process a user follows to claim the VM.</span></span>

> [!NOTE]
> <span data-ttu-id="4a42f-106">Om du distribuerar lab virtuella datorer via [Azure Resource Manager-mallar](devtest-lab-create-environment-from-arm.md), du kan skapa claimable virtuella datorer genom att ange den **allowClaim** egenskap till true i egenskapsavsnittet.</span><span class="sxs-lookup"><span data-stu-id="4a42f-106">If you deploy lab VMs through [Azure Resource Manager templates](devtest-lab-create-environment-from-arm.md), you can create claimable VMs by setting the **allowClaim** property to true in the properties section.</span></span>
>
>

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="4a42f-107">Steg för att lägga till en claimable virtuell dator i ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="4a42f-107">Steps to add a claimable VM to a lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="4a42f-108">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="4a42f-108">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="4a42f-109">Välj **fler tjänster**, och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="4a42f-109">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
1. <span data-ttu-id="4a42f-110">Välj från listan över labs labbet där du vill skapa claimable VM.</span><span class="sxs-lookup"><span data-stu-id="4a42f-110">From the list of labs, select the lab in which you want to create the claimable VM.</span></span>  
1. <span data-ttu-id="4a42f-111">På testmiljön **översikt** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4a42f-111">On the lab's **Overview** blade, select **+ Add**.</span></span>  

    ![VM-knappen Lägg till](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="4a42f-113">På den **väljer du en bas** bladet Välj en bas för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4a42f-113">On the **Choose a base** blade, select a base for the VM.</span></span>
1. <span data-ttu-id="4a42f-114">På den **virtuella** bladet anger du ett namn för den nya virtuella datorn i den **virtuellt datornamn** textruta.</span><span class="sxs-lookup"><span data-stu-id="4a42f-114">On the **Virtual machine** blade, enter a name for the new virtual machine in the **Virtual machine name** text box.</span></span>

    ![Blad för labbet VM](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. <span data-ttu-id="4a42f-116">Ange en **användarnamn** som har beviljats administratörsbehörighet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4a42f-116">Enter a **User Name** that is granted administrator privileges on the virtual machine.</span></span>  
1. <span data-ttu-id="4a42f-117">Om du vill använda ett lösenord som lagras i din [hemliga store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store)väljer **använda en sparad hemlighet**, och ange ett nyckelvärde som motsvarar ditt hemliga (lösenord).</span><span class="sxs-lookup"><span data-stu-id="4a42f-117">If you want to use a password stored in your [secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), select **Use a saved secret**, and specify a key value that corresponds to your secret (password).</span></span> <span data-ttu-id="4a42f-118">Annars kan ange ett lösenord i fältet med etiketten **skriver ett värde**.</span><span class="sxs-lookup"><span data-stu-id="4a42f-118">Otherwise, enter a password in the text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="4a42f-119">Den **virtuella disktyp** avgör vilken lagringstyp som disk är tillåten för virtuella datorer i labbet.</span><span class="sxs-lookup"><span data-stu-id="4a42f-119">The **Virtual machine disk type** determines which storage disk type is allowed for the virtual machines in the lab.</span></span>
1. <span data-ttu-id="4a42f-120">Välj **storlek för virtuell dator** och välj en av de fördefinierade objekt som anger processorkärnor, ram-storlek och storleken på hårddisken på den virtuella datorn för att skapa.</span><span class="sxs-lookup"><span data-stu-id="4a42f-120">Select **Virtual machine size** and select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span>
1. <span data-ttu-id="4a42f-121">Välj **artefakter** och listan över artefakter och välj och konfigurera artefakter som du vill lägga till basavbildningen.</span><span class="sxs-lookup"><span data-stu-id="4a42f-121">Select **Artifacts** and from the list of artifacts, select and configure the artifacts that you want to add to the base image.</span></span> <span data-ttu-id="4a42f-122">Om du inte har arbetat med DevTest Labs eller konfigurera artefakter, referera till den [lägga till en befintlig artefakt till en virtuell dator](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) avsnittet och sedan återvända hit när du är klar.</span><span class="sxs-lookup"><span data-stu-id="4a42f-122">If you're new to DevTest Labs or configuring artifacts, refer to the [Add an existing artifact to a VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="4a42f-123">Välj **avancerade inställningar** att konfigurera Virtuellt Nätverksalternativ och upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="4a42f-123">Select **Advanced settings** to configure the VM's network options and expiration options.</span></span> <span data-ttu-id="4a42f-124">Under **anspråk alternativ**, Välj **Ja** så att datorn claimable.</span><span class="sxs-lookup"><span data-stu-id="4a42f-124">Under **Claim options**, choose **Yes** to make the machine claimable.</span></span>

  ![Välja att göra den virtuella datorn claimable.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. <span data-ttu-id="4a42f-126">Om du vill visa eller kopiera Azure Resource Manager-mallen finns i [spara Azure Resource Manager-mall](devtest-lab-add-vm.md#save-azure-resource-manager-template) avsnittet och återvända hit när du är klar.</span><span class="sxs-lookup"><span data-stu-id="4a42f-126">If you want to view or copy the Azure Resource Manager template, refer to the [Save Azure Resource Manager template](devtest-lab-add-vm.md#save-azure-resource-manager-template) section, and return here when finished.</span></span>
1. <span data-ttu-id="4a42f-127">Välj **skapa** att lägga till den angivna virtuella datorn i labbet.</span><span class="sxs-lookup"><span data-stu-id="4a42f-127">Select **Create** to add the specified VM to the lab.</span></span>
1. <span data-ttu-id="4a42f-128">Blad för labbet visar status för den virtuella datorn skapas - först som **skapa**, sedan som **kör** när den virtuella datorn har startats.</span><span class="sxs-lookup"><span data-stu-id="4a42f-128">The lab blade displays the status of the VM's creation - first as **Creating**, then as **Running** after the VM has been started.</span></span>


## <a name="using-a-claimable-vm"></a><span data-ttu-id="4a42f-129">Med hjälp av en claimable VM</span><span class="sxs-lookup"><span data-stu-id="4a42f-129">Using a claimable VM</span></span>

<span data-ttu-id="4a42f-130">En användare kan göra anspråk på någon virtuell dator i listan över ”Claimable virtuella datorer” genom att göra något av följande:</span><span class="sxs-lookup"><span data-stu-id="4a42f-130">A user can claim any VM from the list of "Claimable virtual machines" by doing one of these steps:</span></span>

* <span data-ttu-id="4a42f-131">I listan över ”Claimable virtuella datorer” längst ned på bladet för den testmiljön översikt, högerklicka på någon av de virtuella datorerna i listan och väljer **anspråk datorn**.</span><span class="sxs-lookup"><span data-stu-id="4a42f-131">From the list of "Claimable virtual machines" at the bottom of the lab's Overview blade, right-click on one of the VMs in the list and choose **Claim machine**.</span></span>

 ![Begäran om en specifik claimable VM.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* <span data-ttu-id="4a42f-133">Längst upp i den **översikt** bladet välj **anspråk någon**.</span><span class="sxs-lookup"><span data-stu-id="4a42f-133">At the top of the **Overview** blade, choose **Claim any**.</span></span> <span data-ttu-id="4a42f-134">En slumpmässig virtuell dator är tilldelad från listan över claimable virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4a42f-134">A random virtual machine is assigned from the list of claimable VMs.</span></span>

 ![Begära alla claimable VM.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


<span data-ttu-id="4a42f-136">När en användare anspråk som en virtuell dator, flyttas till sin lista över ”mina virtuella datorer” och är inte längre claimable av någon annan användare.</span><span class="sxs-lookup"><span data-stu-id="4a42f-136">After a user claims a VM, it is moved up into their list of "My virtual machines" and is no longer claimable by any other user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a42f-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a42f-137">Next steps</span></span>
* <span data-ttu-id="4a42f-138">När den virtuella datorn har skapats kan du ansluta till den virtuella datorn genom att välja **Anslut** på bladet för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4a42f-138">Once the VM has been created, you can connect to the VM by selecting **Connect** on the VM's blade.</span></span>
* <span data-ttu-id="4a42f-139">Utforska den [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span><span class="sxs-lookup"><span data-stu-id="4a42f-139">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span></span>
