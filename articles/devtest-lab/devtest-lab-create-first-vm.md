---
title: "aaaCreate din första virtuella datorn i ett labb i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur toocreate din första virtuella datorn i ett labb i Azure DevTest Labs"
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
ms.openlocfilehash: 4c3257efca9be6fdd190eaac1db731464e07fcfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="34d56-103">Skapa din första virtuella datorn i ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="34d56-103">Create your first VM in a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="34d56-104">När du först komma åt DevTest Labs och vill toocreate din första virtuella datorn, kommer troligen gör du det med hjälp av en förinstallerade [grundläggande marketplace-avbildning](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="34d56-104">When you initially access DevTest Labs and want toocreate your first VM, you will likely do so using a pre-loaded [base marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="34d56-105">Vid ett senare tillfälle du även att kunna toochoose från en [anpassad avbildning och en formel](devtest-lab-add-vm.md) när du skapar flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="34d56-105">Later on, you'll also be able toochoose from a [custom image and a formula](devtest-lab-add-vm.md) when creating more VMs.</span></span> 

<span data-ttu-id="34d56-106">Den här självstudiekursen vägleder dig genom med hello Azure portal tooadd ditt första VM tooa labb i DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="34d56-106">This tutorial walks you through using hello Azure portal tooadd your first VM tooa lab in DevTest Labs.</span></span>

## <a name="steps-tooadd-your-first-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="34d56-107">Steg tooadd ditt första VM tooa labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="34d56-107">Steps tooadd your first VM tooa lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="34d56-108">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="34d56-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="34d56-109">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="34d56-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
1. <span data-ttu-id="34d56-110">Välj hello labb som du vill toocreate hello VM hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="34d56-110">From hello list of labs, select hello lab in which you want toocreate hello VM.</span></span>  
1. <span data-ttu-id="34d56-111">På hello lab **översikt** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="34d56-111">On hello lab's **Overview** blade, select **+ Add**.</span></span>  

    ![VM-knappen Lägg till](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="34d56-113">På hello **väljer du en bas** bladet Välj en marketplace-avbildning för hello VM.</span><span class="sxs-lookup"><span data-stu-id="34d56-113">On hello **Choose a base** blade, select a marketplace image for hello VM.</span></span>
1. <span data-ttu-id="34d56-114">På hello **virtuella** bladet, ange ett namn för hello nya virtuella datorn i hello **virtuellt datornamn** textruta.</span><span class="sxs-lookup"><span data-stu-id="34d56-114">On hello **Virtual machine** blade, enter a name for hello new virtual machine in hello **Virtual machine name** text box.</span></span>

    ![Blad för labbet VM](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. <span data-ttu-id="34d56-116">Ange en **användarnamn** som har beviljats administratörsbehörighet på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="34d56-116">Enter a **User Name** that is granted administrator privileges on hello virtual machine.</span></span>  
1. <span data-ttu-id="34d56-117">Ange ett lösenord i hello textfält med etiketten **skriver ett värde**.</span><span class="sxs-lookup"><span data-stu-id="34d56-117">Enter a password in hello text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="34d56-118">Hej **virtuella disktyp** avgör vilken lagringstyp som disk tillåts för hello virtuella datorer i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="34d56-118">hello **Virtual machine disk type** determines which storage disk type is allowed for hello virtual machines in hello lab.</span></span>
1. <span data-ttu-id="34d56-119">Välj **storlek för virtuell dator** och välj något av hello fördefinierade objekt som anger hello processorkärnor, ram-storlek och hello hårddiskstorlek av hello VM toocreate.</span><span class="sxs-lookup"><span data-stu-id="34d56-119">Select **Virtual machine size** and select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span>
1. <span data-ttu-id="34d56-120">Välj **artefakter** och - hello listan med artefakter - Välj och konfigurera hello artefakter som du vill tooadd toohello grundläggande bild.</span><span class="sxs-lookup"><span data-stu-id="34d56-120">Select **Artifacts** and - from hello list of artifacts - select and configure hello artifacts that you want tooadd toohello base image.</span></span>
    <span data-ttu-id="34d56-121">**Obs:** om du nya tooDevTest övningar eller konfigurera artefakter, se toohello [lägga till en befintlig artefakt tooa VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) avsnittet och sedan återvända hit när du är klar.</span><span class="sxs-lookup"><span data-stu-id="34d56-121">**Note:** If you're new tooDevTest Labs or configuring artifacts, refer toohello [Add an existing artifact tooa VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="34d56-122">Välj **skapa** tooadd hello angetts VM toohello labbet.</span><span class="sxs-lookup"><span data-stu-id="34d56-122">Select **Create** tooadd hello specified VM toohello lab.</span></span>

   <span data-ttu-id="34d56-123">hello blad för labbet visar hello status för skapa hello VM - först som **skapa**, sedan som **kör** efter hello VM har startats.</span><span class="sxs-lookup"><span data-stu-id="34d56-123">hello lab blade displays hello status of hello VM's creation - first as **Creating**, then as **Running** after hello VM has been started.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34d56-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34d56-124">Next steps</span></span>
* <span data-ttu-id="34d56-125">En gång hello VM har skapats, du kan ansluta toohello VM genom att välja **Anslut** hello VM-bladet.</span><span class="sxs-lookup"><span data-stu-id="34d56-125">Once hello VM has been created, you can connect toohello VM by selecting **Connect** on hello VM's blade.</span></span>
* <span data-ttu-id="34d56-126">Checka ut [lägga till ett labb för VM-tooa](devtest-lab-add-vm.md) fullständig information om hur du lägger till följande virtuella datorer i labbet.</span><span class="sxs-lookup"><span data-stu-id="34d56-126">Check out [Add a VM tooa lab](devtest-lab-add-vm.md) for more complete info about adding subsequent VMs in your lab.</span></span>
* <span data-ttu-id="34d56-127">Utforska hello [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span><span class="sxs-lookup"><span data-stu-id="34d56-127">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span></span>
