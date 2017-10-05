---
title: Skapa ett labb i Azure DevTest Labs | Microsoft Docs
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
ms.openlocfilehash: 265a968f902f53c7561c8c7e937f8eacfdb37167
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="43438-103">Skapa ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="43438-103">Create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="43438-104">Ett labb i Azure DevTest Labs är den infrastruktur som omfattar en grupp med resurser, till exempel virtuella datorer (VM), som låter dig hantera resurserna genom att ange gränser och kvoter.</span><span class="sxs-lookup"><span data-stu-id="43438-104">A lab in Azure DevTest Labs is the infrastructure that encompasses a group of resources, such as Virtual Machines (VMs), that lets you better manage those resources by specifying limits and quotas.</span></span> <span data-ttu-id="43438-105">Den här artikeln beskriver steg för steg hur du skapar ett labb med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="43438-105">This article walks you through the process of creating a lab using the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43438-106">Krav</span><span class="sxs-lookup"><span data-stu-id="43438-106">Prerequisites</span></span>
<span data-ttu-id="43438-107">Du behöver följande om du vill skapa ett labb:</span><span class="sxs-lookup"><span data-stu-id="43438-107">To create a lab, you need:</span></span>

* <span data-ttu-id="43438-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="43438-108">An Azure subscription.</span></span> <span data-ttu-id="43438-109">Mer information om köpalternativ för Azure finns i [Så här köper du Azure](https://azure.microsoft.com/pricing/purchase-options/) eller [Kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43438-109">To learn about Azure purchase options, see [How to buy Azure](https://azure.microsoft.com/pricing/purchase-options/) or [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="43438-110">För att kunna skapa labbet måste du vara prenumerationens ägare.</span><span class="sxs-lookup"><span data-stu-id="43438-110">You must be the owner of the subscription to create the lab.</span></span>

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="43438-111">Steg för att skapa ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="43438-111">Steps to create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="43438-112">Följande steg illustrerar hur du använder Azure-portalen för att skapa ett labb i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="43438-112">The following steps illustrate how to use the Azure portal to create a lab in Azure DevTest Labs.</span></span> 

1. <span data-ttu-id="43438-113">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="43438-113">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="43438-114">På huvudmenyn på vänster sida väljer du **Fler tjänster** (längst ned i listan).</span><span class="sxs-lookup"><span data-stu-id="43438-114">From the main menu on the left side, select **More Services** (at the bottom of the list).</span></span>

    ![Menyalternativet Fler tjänster](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. <span data-ttu-id="43438-116">I listan över tillgängliga tjänster: **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="43438-116">From the list of available services, **DevTest Labs**.</span></span>
1. <span data-ttu-id="43438-117">På bladet **DevTest Labs** väljer du **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="43438-117">On the **DevTest Labs** blade, select **Add**.</span></span>
   
    ![Lägga till ett labb](./media/devtest-lab-create-lab/add-lab-button.png)

1. <span data-ttu-id="43438-119">På bladet **Skapa ett DevTest-labb**:</span><span class="sxs-lookup"><span data-stu-id="43438-119">On the **Create a DevTest Lab** blade:</span></span>
   
    1. <span data-ttu-id="43438-120">Ange **Labbnamn** för det nya labbet.</span><span class="sxs-lookup"><span data-stu-id="43438-120">Enter a **Lab Name** for the new lab.</span></span>
    2. <span data-ttu-id="43438-121">Välj den **prenumeration** som du vill koppla till labbet.</span><span class="sxs-lookup"><span data-stu-id="43438-121">Select the **Subscription** to associate with the lab.</span></span>
    3. <span data-ttu-id="43438-122">Välj en **plats** där du vill lagra labbet.</span><span class="sxs-lookup"><span data-stu-id="43438-122">Select a **Location** in which to store the lab.</span></span>
    4. <span data-ttu-id="43438-123">Välj **Automatisk avstängning** för att ange om du vill aktivera, och definiera parametrar för, automatisk avstängning av alla labbets virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="43438-123">Select **Auto-shutdown** to specify if you want to enable - and define the parameters for - the automatic shutting down of all the lab's VMs.</span></span> <span data-ttu-id="43438-124">Funktionen automatisk avstängning är främst en kostnadfunktion där du kan ange om du vill att den virtuella datorn ska stängas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="43438-124">The auto-shutdown feature is mainly a cost-saving feature whereby you can specify when you want the VM to automatically be shut down.</span></span> <span data-ttu-id="43438-125">Du kan ändra inställningarna för automatisk avstängning när du har skapat labbet genom att följa anvisningarna i artikeln [Hantera alla principer för ett labb i Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span><span class="sxs-lookup"><span data-stu-id="43438-125">You can change auto-shutdown settings after creating the lab by following the steps outlined in the article, [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span></span>
    5. <span data-ttu-id="43438-126">Välj **Fäst på instrumentpanelen** om du vill att en genväg till labbet ska visas på instrumentpanelen i portalen.</span><span class="sxs-lookup"><span data-stu-id="43438-126">Select **Pin to Dashboard** if you want a shortcut of the lab to appear on the portal dashboard.</span></span>
    6. <span data-ttu-id="43438-127">Välj **Automatiseringsalternativ** för att hämta Azure Resource Manager-mallar för konfigurationsautomatisering.</span><span class="sxs-lookup"><span data-stu-id="43438-127">Select **Automation options** to get Azure Resource Manager templates for configuration automation.</span></span> 
    7. <span data-ttu-id="43438-128">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="43438-128">Select **Create**.</span></span> <span data-ttu-id="43438-129">När du har valt **Skapa** visas bladet**DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="43438-129">After selecting **Create**, the **DevTest Labs** blade displays.</span></span> <span data-ttu-id="43438-130">Du kan övervaka statusen för labbprocessen genom att titta på **meddelande**området.</span><span class="sxs-lookup"><span data-stu-id="43438-130">You can monitor the status of the lab creation process by watching the **Notifications** area.</span></span> <span data-ttu-id="43438-131">Uppdatera sidan om du vill se den nyligen skapade testmiljön i listan över labbar när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="43438-131">Once completed, refresh the page to see the newly created lab in the list of labs.</span></span>  
    
    ![Skapa ett blad för labbet](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="43438-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43438-133">Next steps</span></span>
<span data-ttu-id="43438-134">När du har skapat labbet kan du fundera på följande steg:</span><span class="sxs-lookup"><span data-stu-id="43438-134">Once you've created your lab, here are some next steps to consider:</span></span>

* <span data-ttu-id="43438-135">[Säker åtkomst till ett labb](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="43438-135">[Secure access to a lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="43438-136">[Ange principer för labbet](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="43438-136">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="43438-137">[Skapa en labbmall](devtest-lab-create-template.md).</span><span class="sxs-lookup"><span data-stu-id="43438-137">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="43438-138">[Skapa anpassade artefakter för dina virtuella datorer](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="43438-138">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="43438-139">[Lägga till en virtuell dator med artefakter i ett labb](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span><span class="sxs-lookup"><span data-stu-id="43438-139">[Add a VM with artifacts to a lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span></span>

