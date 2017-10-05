---
title: "Hantera principer för grundläggande labbet i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur du ställer in några av de grundläggande principerna (inställningar) för ett labb i DevTest Labs"
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
ms.date: 06/29/2017
ms.author: tarcher
ms.openlocfilehash: ed35d081b191ec41ed9e5970515057a4715c0d59
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="328bc-103">Hantera grundläggande principer för ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="328bc-103">Manage basic policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="328bc-104">Azure DevTest Labs kan du styra kostnader och minimera skräp i din labs genom att hantera principer (inställningar) för varje labbet.</span><span class="sxs-lookup"><span data-stu-id="328bc-104">Azure DevTest Labs enables you to control cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="328bc-105">I den här artikeln får komma du igång med principer genom att lära dig hur du ställer in två av de viktigaste principerna - begränsning av antalet virtuella datorer (VM) som kan skapas eller ägs av en enskild användare och konfigurera automatisk avstängning.</span><span class="sxs-lookup"><span data-stu-id="328bc-105">In this article, you get started with policies by learning how to set two of the most critical policies - limiting the number of virtual machines (VM) that can be created or claimed by a single user, and configuring auto-shutdown.</span></span> <span data-ttu-id="328bc-106">Så här visar hur du ställer in varje lab princip finns i artikeln [Definiera principer för labbet i Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="328bc-106">To view how to set every lab policy, see the article, [Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span></span>  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a><span data-ttu-id="328bc-107">Åtkomst till ett labb principer i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="328bc-107">Accessing a lab's policies in Azure DevTest Labs</span></span>
<span data-ttu-id="328bc-108">Följande steg hur du konfigurerar principer för ett labb i Azure DevTest Labs:</span><span class="sxs-lookup"><span data-stu-id="328bc-108">The following steps guide you through setting up policies for a lab in Azure DevTest Labs:</span></span>

<span data-ttu-id="328bc-109">Om du vill visa, och ändra principer för ett labb, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="328bc-109">To view (and change) the policies for a lab, follow these steps:</span></span>

1. <span data-ttu-id="328bc-110">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="328bc-110">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="328bc-111">Välj **Fler tjänster** och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="328bc-111">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="328bc-112">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="328bc-112">From the list of labs, select the desired lab.</span></span>   

1. <span data-ttu-id="328bc-113">Välj **konfiguration och principer**.</span><span class="sxs-lookup"><span data-stu-id="328bc-113">Select **Configuration and policies**.</span></span>

    ![Principen inställningsbladet](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. <span data-ttu-id="328bc-115">Den **konfiguration och principer** bladet innehåller en meny med inställningar som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="328bc-115">The **Configuration and policies** blade contains a menu of settings that you can specify.</span></span> <span data-ttu-id="328bc-116">Den här artikeln beskriver bara till inställningarna för **virtuella datorer per användare** och **automatisk avstängning**.</span><span class="sxs-lookup"><span data-stu-id="328bc-116">This article covers only the settings for **Virtual machines per user** and **Auto-shutdown**.</span></span> <span data-ttu-id="328bc-117">Läs om de återstående inställningarna i [hantera alla principer för ett labb i Azure DevTest Labs](./devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="328bc-117">To learn about the remaining settings, see [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md).</span></span> 
   
## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="328bc-118">Ange virtuella datorer per användare</span><span class="sxs-lookup"><span data-stu-id="328bc-118">Set virtual machines per user</span></span>
<span data-ttu-id="328bc-119">Principen för **virtuella datorer per användare** kan du ange det maximala antalet virtuella datorer som kan skapas av en enskild användare.</span><span class="sxs-lookup"><span data-stu-id="328bc-119">The policy for **Virtual machines per user** allows you to specify the maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="328bc-120">Om en användare försöker skapa eller begära en virtuell dator när antalet användare som har uppfyllts, ett felmeddelande som anger att den virtuella datorn inte kan skapas/anspråk.</span><span class="sxs-lookup"><span data-stu-id="328bc-120">If a user attempts to create or claim a VM when the user limit has been met, an error message indicates that the VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="328bc-121">På testmiljön **konfiguration och principer** väljer du **virtuella datorer per användare**.</span><span class="sxs-lookup"><span data-stu-id="328bc-121">On the lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuella datorer per användare](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="328bc-123">Välj **Ja** begränsar antalet virtuella datorer per användare.</span><span class="sxs-lookup"><span data-stu-id="328bc-123">Select **Yes** to limit the number of VMs per user.</span></span> <span data-ttu-id="328bc-124">Om du inte vill begränsa antalet virtuella datorer per användare väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="328bc-124">If you do not want to limit the number of VMs per user, select **No**.</span></span> <span data-ttu-id="328bc-125">Om du väljer **Ja**, ange ett numeriskt värde som anger maximalt antal virtuella datorer som kan skapas eller ägs av en användare.</span><span class="sxs-lookup"><span data-stu-id="328bc-125">If you select **Yes**, enter a numeric value indicating the maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="328bc-126">Välj **Ja** begränsar antalet virtuella datorer som kan använda SSD (SSD disk).</span><span class="sxs-lookup"><span data-stu-id="328bc-126">Select **Yes** to limit the number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="328bc-127">Om du inte vill begränsa antalet virtuella datorer som kan använda SSD, väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="328bc-127">If you do not want to limit the number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="328bc-128">Om du väljer **Ja**, ange ett värde som anger maximalt antal virtuella datorer som kan skapas med SSD.</span><span class="sxs-lookup"><span data-stu-id="328bc-128">If you select **Yes**, enter a value indicating the maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="328bc-129">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="328bc-129">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="328bc-130">Ange automatisk avstängning</span><span class="sxs-lookup"><span data-stu-id="328bc-130">Set auto-shutdown</span></span>
<span data-ttu-id="328bc-131">Principen för automatisk avstängning hjälper till att minimera skräp lab genom att du kan ange hur lång tid som den här övningen VMs stängs.</span><span class="sxs-lookup"><span data-stu-id="328bc-131">The auto-shutdown policy helps to minimize lab waste by allowing you to specify the time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="328bc-132">På testmiljön **konfiguration och principer** bladet väljer **automatisk avstängning**.</span><span class="sxs-lookup"><span data-stu-id="328bc-132">On the lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Automatisk avstängning](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="328bc-134">Välj **på** att aktivera den här principen och **av** du inaktivera den.</span><span class="sxs-lookup"><span data-stu-id="328bc-134">Select **On** to enable this policy, and **Off** to disable it.</span></span>

1. <span data-ttu-id="328bc-135">Om du aktiverar den här principen kan du ange tid (och tidszon) för att stänga av alla virtuella datorer i det aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="328bc-135">If you enable this policy, specify the time (and time zone) to shut down all VMs in the current lab.</span></span>

1. <span data-ttu-id="328bc-136">Ange **Ja** eller **nr** för alternativet att skicka en avisering 15 minuter innan den angivna automatisk avstängning tid.</span><span class="sxs-lookup"><span data-stu-id="328bc-136">Specify **Yes** or **No** for the option to send a notification 15 minutes prior to the specified auto-shutdown time.</span></span> <span data-ttu-id="328bc-137">Om du anger **Ja**, ange en webhook URL-slutpunkt för att ta emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="328bc-137">If you specify **Yes**, enter a webhook URL endpoint to receive the notification.</span></span> <span data-ttu-id="328bc-138">Läs mer om webhooks [skapa en webhook eller API Azure-funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="328bc-138">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="328bc-139">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="328bc-139">Select **Save**.</span></span>

    <span data-ttu-id="328bc-140">Som standard när du har aktiverat, den här principen gäller för alla virtuella datorer i det aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="328bc-140">By default, once enabled, this policy applies to all VMs in the current lab.</span></span> <span data-ttu-id="328bc-141">Om du vill ta bort den här inställningen från en specifik VM, öppna bladet för den virtuella datorn och ändra dess **automatisk avstängning** inställning</span><span class="sxs-lookup"><span data-stu-id="328bc-141">To remove this setting from a specific VM, open the VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="328bc-142">Ange automatisk start</span><span class="sxs-lookup"><span data-stu-id="328bc-142">Set auto-start</span></span>
<span data-ttu-id="328bc-143">Principen för automatisk start kan du ange när de virtuella datorerna i det aktuella labbet ska startas.</span><span class="sxs-lookup"><span data-stu-id="328bc-143">The auto-start policy allows you to specify when the VMs in the current lab should be started.</span></span>  

1. <span data-ttu-id="328bc-144">På testmiljön **konfiguration och principer** bladet väljer **Autostart**.</span><span class="sxs-lookup"><span data-stu-id="328bc-144">On the lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatisk start](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="328bc-146">Välj **på** att aktivera den här principen och **av** du inaktivera den.</span><span class="sxs-lookup"><span data-stu-id="328bc-146">Select **On** to enable this policy, and **Off** to disable it.</span></span>

3. <span data-ttu-id="328bc-147">Om du aktiverar den här principen kan du ange den schemalagda starttiden, tidszon och veckodagar som gäller tid.</span><span class="sxs-lookup"><span data-stu-id="328bc-147">If you enable this policy, specify the scheduled start time, time zone, and the days of the week for which the time applies.</span></span> 

4. <span data-ttu-id="328bc-148">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="328bc-148">Select **Save**.</span></span>

    <span data-ttu-id="328bc-149">När du har aktiverat, tillämpas inte principen automatiskt till alla virtuella datorer i det aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="328bc-149">Once enabled, this policy is not automatically applied to any VMs in the current lab.</span></span> <span data-ttu-id="328bc-150">Öppna bladet för den virtuella datorn för att använda den här inställningen för en specifik VM, och ändra dess **Autostart** inställning</span><span class="sxs-lookup"><span data-stu-id="328bc-150">To apply this setting to a specific VM, open the VM's blade and change its **Auto-start** setting</span></span> 

## <a name="next-steps"></a><span data-ttu-id="328bc-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="328bc-151">Next steps</span></span>

- <span data-ttu-id="328bc-152">[Definiera principer för labbet i Azure DevTest Labs](devtest-lab-set-lab-policy.md) – Lär dig hur du ändrar andra principer för labbet</span><span class="sxs-lookup"><span data-stu-id="328bc-152">[Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md) - Learn how to modify other lab policies</span></span> 
