---
title: aaaCreate ett labb i Azure DevTest Labs | Microsoft Docs
description: "Skapa ett labb i Azure DevTest Labs för virtuella datorer"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/30/2017
ms.author: tarcher
ms.openlocfilehash: 2ec5498f10e0cc06a196a71e319e340937abb1a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="7fbf3-103">Skapa ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="7fbf3-103">Create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="7fbf3-104">Ett labb i Azure DevTest Labs är hello-infrastruktur som omfattar en grupp med resurser, t.ex. virtuella datorer (VM), som låter dig bättre hantera resurserna genom att ange gränser och kvoter.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-104">A lab in Azure DevTest Labs is hello infrastructure that encompasses a group of resources, such as Virtual Machines (VMs), that lets you better manage those resources by specifying limits and quotas.</span></span> <span data-ttu-id="7fbf3-105">Den här artikeln vägleder dig genom hello processen att skapa ett testlabb med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-105">This article walks you through hello process of creating a lab using hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7fbf3-106">Krav</span><span class="sxs-lookup"><span data-stu-id="7fbf3-106">Prerequisites</span></span>
<span data-ttu-id="7fbf3-107">toocreate ett labb behöver du:</span><span class="sxs-lookup"><span data-stu-id="7fbf3-107">toocreate a lab, you need:</span></span>

* <span data-ttu-id="7fbf3-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-108">An Azure subscription.</span></span> <span data-ttu-id="7fbf3-109">toolearn om köpalternativ för Azure, se [hur toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) eller [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-109">toolearn about Azure purchase options, see [How toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) or [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="7fbf3-110">Du måste vara hello prenumeration toocreate hello lab hello ägare.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-110">You must be hello owner of hello subscription toocreate hello lab.</span></span>

## <a name="steps-toocreate-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="7fbf3-111">Steg toocreate ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="7fbf3-111">Steps toocreate a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="7fbf3-112">hello följande steg visar hur hello toouse Azure portal toocreate ett labb i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-112">hello following steps illustrate how toouse hello Azure portal toocreate a lab in Azure DevTest Labs.</span></span> 

1. <span data-ttu-id="7fbf3-113">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-113">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="7fbf3-114">Hello huvudmenyn hello vänster, Välj **fler tjänster** (längst ned hello hello listan).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-114">From hello main menu on hello left side, select **More Services** (at hello bottom of hello list).</span></span>

    ![Menyalternativet Fler tjänster](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. <span data-ttu-id="7fbf3-116">Hello listan över tillgängliga tjänster **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-116">From hello list of available services, **DevTest Labs**.</span></span>
1. <span data-ttu-id="7fbf3-117">På hello **DevTest Labs** bladet väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-117">On hello **DevTest Labs** blade, select **Add**.</span></span>
   
    ![Lägga till ett labb](./media/devtest-lab-create-lab/add-lab-button.png)

1. <span data-ttu-id="7fbf3-119">På hello **skapa ett DevTest-labb** bladet:</span><span class="sxs-lookup"><span data-stu-id="7fbf3-119">On hello **Create a DevTest Lab** blade:</span></span>
   
    1. <span data-ttu-id="7fbf3-120">Ange en **Labbnamnet** för hello nya labbet.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-120">Enter a **Lab Name** for hello new lab.</span></span>
    2. <span data-ttu-id="7fbf3-121">Välj hello **prenumeration** tooassociate med hello labbet.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-121">Select hello **Subscription** tooassociate with hello lab.</span></span>
    3. <span data-ttu-id="7fbf3-122">Välj en **plats** i vilka toostore hello labbet.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-122">Select a **Location** in which toostore hello lab.</span></span>
    4. <span data-ttu-id="7fbf3-123">Välj **automatisk avstängning** toospecify om du vill tooenable - och definiera hello parametrar för - hello automatisk avstängning av alla hello lab virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-123">Select **Auto-shutdown** toospecify if you want tooenable - and define hello parameters for - hello automatic shutting down of all hello lab's VMs.</span></span> <span data-ttu-id="7fbf3-124">hello automatisk avstängning funktionen är främst en kostnad spara där du kan ange när du vill hello VM tooautomatically stängas av.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-124">hello auto-shutdown feature is mainly a cost-saving feature whereby you can specify when you want hello VM tooautomatically be shut down.</span></span> <span data-ttu-id="7fbf3-125">Du kan ändra inställningarna för automatisk avstängning när du har skapat hello lab genom att följa hello stegen som beskrivs i artikeln hello [hantera alla principer för ett labb i Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-125">You can change auto-shutdown settings after creating hello lab by following hello steps outlined in hello article, [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span></span>
    5. <span data-ttu-id="7fbf3-126">Välj **PIN-kod tooDashboard** om du vill att en genväg för hello lab tooappear på hello portalens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-126">Select **Pin tooDashboard** if you want a shortcut of hello lab tooappear on hello portal dashboard.</span></span>
    6. <span data-ttu-id="7fbf3-127">Välj **automatiseringsalternativ** tooget Azure Resource Manager-mallar för automatisering av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-127">Select **Automation options** tooget Azure Resource Manager templates for configuration automation.</span></span> 
    7. <span data-ttu-id="7fbf3-128">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-128">Select **Create**.</span></span> <span data-ttu-id="7fbf3-129">När du har valt **skapa**, hello **DevTest Labs** bladet visar.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-129">After selecting **Create**, hello **DevTest Labs** blade displays.</span></span> <span data-ttu-id="7fbf3-130">Du kan övervaka hello status för hello lab processen genom att titta på hello **meddelanden** område.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-130">You can monitor hello status of hello lab creation process by watching hello **Notifications** area.</span></span> <span data-ttu-id="7fbf3-131">Uppdatera hello sidan toosee hello nyskapad labb i hello lista över labs när slutförd.</span><span class="sxs-lookup"><span data-stu-id="7fbf3-131">Once completed, refresh hello page toosee hello newly created lab in hello list of labs.</span></span>  
    
    ![Skapa ett blad för labbet](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="7fbf3-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7fbf3-133">Next steps</span></span>
<span data-ttu-id="7fbf3-134">När du har skapat ditt labb följer här några tooconsider för nästa steg:</span><span class="sxs-lookup"><span data-stu-id="7fbf3-134">Once you've created your lab, here are some next steps tooconsider:</span></span>

* <span data-ttu-id="7fbf3-135">[Säker åtkomst tooa lab](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-135">[Secure access tooa lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="7fbf3-136">[Ange principer för labbet](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-136">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="7fbf3-137">[Skapa en labbmall](devtest-lab-create-template.md).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-137">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="7fbf3-138">[Skapa anpassade artefakter för dina virtuella datorer](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-138">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="7fbf3-139">[Lägg till en virtuell dator med artefakter tooa lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span><span class="sxs-lookup"><span data-stu-id="7fbf3-139">[Add a VM with artifacts tooa lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span></span>

