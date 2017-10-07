---
title: aaaAdd ett claimable VM tooa labb i Azure DevTest Labs | Microsoft Docs
description: "Lär dig hur tooadd ett claimable virtuella tooa labb i Azure DevTest Labs"
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
ms.openlocfilehash: fe6385ae2e59b9636b82aec250dc3a1f8a40ba5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="7d801-103">Lägga till ett claimable VM tooa labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="7d801-103">Add a claimable VM tooa lab in Azure DevTest Labs</span></span>
<span data-ttu-id="7d801-104">Du lägger till ett claimable VM tooa labb i ett liknande sätt toohow du [lägga till en standard VM](devtest-lab-add-vm.md) – från en *grundläggande* som är antingen en [anpassad avbildning](devtest-lab-create-template.md), [formeln](devtest-lab-manage-formulas.md), eller [Marketplace-avbildning](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="7d801-104">You add a claimable VM tooa lab in a similar manner toohow you [add a standard VM](devtest-lab-add-vm.md) – from a *base* that is either a [custom image](devtest-lab-create-template.md), [formula](devtest-lab-manage-formulas.md), or [Marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="7d801-105">Den här självstudiekursen vägleder dig genom att använda hello Azure portal tooadd ett claimable VM tooa labb i DevTest Labs och visar hello-processen en användare följer tooclaim hello VM.</span><span class="sxs-lookup"><span data-stu-id="7d801-105">This tutorial walks you through using hello Azure portal tooadd a claimable VM tooa lab in DevTest Labs, and shows hello process a user follows tooclaim hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="7d801-106">Om du distribuerar lab virtuella datorer via [Azure Resource Manager-mallar](devtest-lab-create-environment-from-arm.md), du kan skapa claimable virtuella datorer genom att ange hello **allowClaim** egenskapen tootrue i hello properties-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7d801-106">If you deploy lab VMs through [Azure Resource Manager templates](devtest-lab-create-environment-from-arm.md), you can create claimable VMs by setting hello **allowClaim** property tootrue in hello properties section.</span></span>
>
>

## <a name="steps-tooadd-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="7d801-107">Steg tooadd ett claimable VM tooa labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="7d801-107">Steps tooadd a claimable VM tooa lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="7d801-108">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="7d801-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="7d801-109">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="7d801-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
1. <span data-ttu-id="7d801-110">Välj hello lab hello listan övningar som du vill toocreate hello claimable VM.</span><span class="sxs-lookup"><span data-stu-id="7d801-110">From hello list of labs, select hello lab in which you want toocreate hello claimable VM.</span></span>  
1. <span data-ttu-id="7d801-111">På hello lab **översikt** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7d801-111">On hello lab's **Overview** blade, select **+ Add**.</span></span>  

    ![VM-knappen Lägg till](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="7d801-113">På hello **väljer du en bas** bladet Välj en bas för hello VM.</span><span class="sxs-lookup"><span data-stu-id="7d801-113">On hello **Choose a base** blade, select a base for hello VM.</span></span>
1. <span data-ttu-id="7d801-114">På hello **virtuella** bladet, ange ett namn för hello nya virtuella datorn i hello **virtuellt datornamn** textruta.</span><span class="sxs-lookup"><span data-stu-id="7d801-114">On hello **Virtual machine** blade, enter a name for hello new virtual machine in hello **Virtual machine name** text box.</span></span>

    ![Blad för labbet VM](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. <span data-ttu-id="7d801-116">Ange en **användarnamn** som har beviljats administratörsbehörighet på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7d801-116">Enter a **User Name** that is granted administrator privileges on hello virtual machine.</span></span>  
1. <span data-ttu-id="7d801-117">Om du vill toouse ett lösenord som lagras i din [hemliga store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store)väljer **använda en sparad hemlighet**, och ange ett nyckelvärde som motsvarar tooyour hemlighet (lösenord).</span><span class="sxs-lookup"><span data-stu-id="7d801-117">If you want toouse a password stored in your [secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), select **Use a saved secret**, and specify a key value that corresponds tooyour secret (password).</span></span> <span data-ttu-id="7d801-118">Annars kan ange ett lösenord i hello textfält med etiketten **skriver ett värde**.</span><span class="sxs-lookup"><span data-stu-id="7d801-118">Otherwise, enter a password in hello text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="7d801-119">Hej **virtuella disktyp** avgör vilken lagringstyp som disk tillåts för hello virtuella datorer i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="7d801-119">hello **Virtual machine disk type** determines which storage disk type is allowed for hello virtual machines in hello lab.</span></span>
1. <span data-ttu-id="7d801-120">Välj **storlek för virtuell dator** och välj något av hello fördefinierade objekt som anger hello processorkärnor, ram-storlek och hello hårddiskstorlek av hello VM toocreate.</span><span class="sxs-lookup"><span data-stu-id="7d801-120">Select **Virtual machine size** and select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span>
1. <span data-ttu-id="7d801-121">Välj **artefakter** och hello listan över artefakter och välj och konfigurera hello artefakter som du vill tooadd toohello grundläggande bild.</span><span class="sxs-lookup"><span data-stu-id="7d801-121">Select **Artifacts** and from hello list of artifacts, select and configure hello artifacts that you want tooadd toohello base image.</span></span> <span data-ttu-id="7d801-122">Om du nya tooDevTest övningar eller konfigurera artefakter, se toohello [lägga till en befintlig artefakt tooa VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) avsnittet och sedan återvända hit när du är klar.</span><span class="sxs-lookup"><span data-stu-id="7d801-122">If you're new tooDevTest Labs or configuring artifacts, refer toohello [Add an existing artifact tooa VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="7d801-123">Välj **avancerade inställningar** tooconfigure hello VM Nätverksalternativ alternativ och upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="7d801-123">Select **Advanced settings** tooconfigure hello VM's network options and expiration options.</span></span> <span data-ttu-id="7d801-124">Under **anspråk alternativ**, Välj **Ja** toomake hello datorn claimable.</span><span class="sxs-lookup"><span data-stu-id="7d801-124">Under **Claim options**, choose **Yes** toomake hello machine claimable.</span></span>

  ![Välj toomake hello VM claimable.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. <span data-ttu-id="7d801-126">Om du vill tooview eller kopiera hello Azure Resource Manager-mall, se toohello [spara Azure Resource Manager-mall](devtest-lab-add-vm.md#save-azure-resource-manager-template) avsnittet och återvända hit när du är klar.</span><span class="sxs-lookup"><span data-stu-id="7d801-126">If you want tooview or copy hello Azure Resource Manager template, refer toohello [Save Azure Resource Manager template](devtest-lab-add-vm.md#save-azure-resource-manager-template) section, and return here when finished.</span></span>
1. <span data-ttu-id="7d801-127">Välj **skapa** tooadd hello angetts VM toohello labbet.</span><span class="sxs-lookup"><span data-stu-id="7d801-127">Select **Create** tooadd hello specified VM toohello lab.</span></span>
1. <span data-ttu-id="7d801-128">hello blad för labbet visar hello status för skapa hello VM - först som **skapa**, sedan som **kör** efter hello VM har startats.</span><span class="sxs-lookup"><span data-stu-id="7d801-128">hello lab blade displays hello status of hello VM's creation - first as **Creating**, then as **Running** after hello VM has been started.</span></span>


## <a name="using-a-claimable-vm"></a><span data-ttu-id="7d801-129">Med hjälp av en claimable VM</span><span class="sxs-lookup"><span data-stu-id="7d801-129">Using a claimable VM</span></span>

<span data-ttu-id="7d801-130">En användare kan göra anspråk på alla VM hello listan över ”Claimable virtuella datorer” genom att göra något av följande:</span><span class="sxs-lookup"><span data-stu-id="7d801-130">A user can claim any VM from hello list of "Claimable virtual machines" by doing one of these steps:</span></span>

* <span data-ttu-id="7d801-131">Högerklicka på någon av hello virtuella datorer i hello listan hello listan över ”Claimable virtuella datorer” längst ned hello hello lab översikt bladet, och välj **anspråk datorn**.</span><span class="sxs-lookup"><span data-stu-id="7d801-131">From hello list of "Claimable virtual machines" at hello bottom of hello lab's Overview blade, right-click on one of hello VMs in hello list and choose **Claim machine**.</span></span>

 ![Begäran om en specifik claimable VM.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* <span data-ttu-id="7d801-133">Hello överst i hello **översikt** bladet välj **anspråk någon**.</span><span class="sxs-lookup"><span data-stu-id="7d801-133">At hello top of hello **Overview** blade, choose **Claim any**.</span></span> <span data-ttu-id="7d801-134">En slumpmässig virtuell dator har tilldelats hello listan över claimable virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7d801-134">A random virtual machine is assigned from hello list of claimable VMs.</span></span>

 ![Begära alla claimable VM.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


<span data-ttu-id="7d801-136">När en användare anspråk som en virtuell dator, flyttas till sin lista över ”mina virtuella datorer” och är inte längre claimable av någon annan användare.</span><span class="sxs-lookup"><span data-stu-id="7d801-136">After a user claims a VM, it is moved up into their list of "My virtual machines" and is no longer claimable by any other user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d801-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d801-137">Next steps</span></span>
* <span data-ttu-id="7d801-138">En gång hello VM har skapats, du kan ansluta toohello VM genom att välja **Anslut** hello VM-bladet.</span><span class="sxs-lookup"><span data-stu-id="7d801-138">Once hello VM has been created, you can connect toohello VM by selecting **Connect** on hello VM's blade.</span></span>
* <span data-ttu-id="7d801-139">Utforska hello [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span><span class="sxs-lookup"><span data-stu-id="7d801-139">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span></span>
