---
title: "Hantera principer för labbet i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur du definierar principer för labbet som VM-storlekar och maximala virtuella datorer per användare och avstängning automation."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 328a4d893637d7150807855e118b485a2c3bbfc5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="95902-103">Hantera alla principer för ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="95902-103">Manage all policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="95902-104">Azure DevTest Labs kan du styra kostnader och minimera skräp i din labs genom att hantera principer (inställningar) för varje labbet.</span><span class="sxs-lookup"><span data-stu-id="95902-104">Azure DevTest Labs lets you control cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="95902-105">Den här artikeln förklarar i detalj steg för steg hur du ställer in varje princip.</span><span class="sxs-lookup"><span data-stu-id="95902-105">This article explains in step-by-step detail how to set each policy.</span></span>  

## <a name="set-allowed-virtual-machine-sizes"></a><span data-ttu-id="95902-106">Ange tillåtna storlekar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="95902-106">Set allowed virtual machine sizes</span></span>
<span data-ttu-id="95902-107">Principen för att ange tillåtna VM-storlekar hjälper till att minimera skräp lab genom att du kan ange vilka VM-storlekar som tillåts i labbet.</span><span class="sxs-lookup"><span data-stu-id="95902-107">The policy for setting the allowed VM sizes helps to minimize lab waste by enabling you to specify which VM sizes are allowed in the lab.</span></span> <span data-ttu-id="95902-108">Om den här principen aktiveras användas endast VM-storlekar från den här listan för att skapa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="95902-108">If this policy is activated, only VM sizes from this list can be used to create VMs.</span></span>

1. <span data-ttu-id="95902-109">På testmiljön **konfiguration och principer** bladet väljer **tillåtna storlekar för virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="95902-109">On the lab's **Configuration and policies** blade, select **Allowed virtual machines sizes**.</span></span>
   
    ![Tillåtna virtuella datorer storlekar](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. <span data-ttu-id="95902-111">Välj **på** att aktivera den här principen och **av** du inaktivera den.</span><span class="sxs-lookup"><span data-stu-id="95902-111">Select **On** to enable this policy, and **Off** to disable it.</span></span>

1. <span data-ttu-id="95902-112">Om du aktiverar den här principen väljer du en eller flera VM-storlekar som kan skapas i labbet.</span><span class="sxs-lookup"><span data-stu-id="95902-112">If you enable this policy, select one or more VM sizes that can be created in your lab.</span></span>

1. <span data-ttu-id="95902-113">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="95902-113">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="95902-114">Ange virtuella datorer per användare</span><span class="sxs-lookup"><span data-stu-id="95902-114">Set virtual machines per user</span></span>
<span data-ttu-id="95902-115">Principen för **virtuella datorer per användare** kan du ange det maximala antalet virtuella datorer som kan skapas av en enskild användare.</span><span class="sxs-lookup"><span data-stu-id="95902-115">The policy for **Virtual machines per user** allows you to specify the maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="95902-116">Om en användare försöker skapa eller begära en virtuell dator när antalet användare som har uppfyllts, ett felmeddelande som anger att den virtuella datorn inte kan skapas/anspråk.</span><span class="sxs-lookup"><span data-stu-id="95902-116">If a user attempts to create or claim a VM when the user limit has been met, an error message indicates that the VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="95902-117">På testmiljön **konfiguration och principer** väljer du **virtuella datorer per användare**.</span><span class="sxs-lookup"><span data-stu-id="95902-117">On the lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuella datorer per användare](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="95902-119">Välj **Ja** begränsar antalet virtuella datorer per användare.</span><span class="sxs-lookup"><span data-stu-id="95902-119">Select **Yes** to limit the number of VMs per user.</span></span> <span data-ttu-id="95902-120">Om du inte vill begränsa antalet virtuella datorer per användare väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="95902-120">If you do not want to limit the number of VMs per user, select **No**.</span></span> <span data-ttu-id="95902-121">Om du väljer **Ja**, ange ett numeriskt värde som anger maximalt antal virtuella datorer som kan skapas eller ägs av en användare.</span><span class="sxs-lookup"><span data-stu-id="95902-121">If you select **Yes**, enter a numeric value indicating the maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="95902-122">Välj **Ja** begränsar antalet virtuella datorer som kan använda SSD (SSD disk).</span><span class="sxs-lookup"><span data-stu-id="95902-122">Select **Yes** to limit the number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="95902-123">Om du inte vill begränsa antalet virtuella datorer som kan använda SSD, väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="95902-123">If you do not want to limit the number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="95902-124">Om du väljer **Ja**, ange ett värde som anger maximalt antal virtuella datorer som kan skapas med SSD.</span><span class="sxs-lookup"><span data-stu-id="95902-124">If you select **Yes**, enter a value indicating the maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="95902-125">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="95902-125">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-lab"></a><span data-ttu-id="95902-126">Ange virtuella datorer per labb</span><span class="sxs-lookup"><span data-stu-id="95902-126">Set virtual machines per lab</span></span>
<span data-ttu-id="95902-127">Principen för **virtuella datorer per labb** kan du ange det maximala antalet virtuella datorer som kan skapas för det aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="95902-127">The policy for **Virtual machines per lab** allows you to specify the maximum number of VMs that can be created for the current lab.</span></span> <span data-ttu-id="95902-128">Om en användare försöker skapa en virtuell dator när lab gränsen har uppfyllts, ett felmeddelande som anger att det inte går att skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="95902-128">If a user attempts to create a VM when the lab limit has been met, an error message indicates that the VM cannot be created.</span></span> 

1. <span data-ttu-id="95902-129">På testmiljön **konfiguration och principer** väljer du **virtuella datorer per labb**.</span><span class="sxs-lookup"><span data-stu-id="95902-129">On the lab's **Configuration and policies** menu, select **Virtual machines per lab**.</span></span>
   
    ![Virtuella datorer per labb](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. <span data-ttu-id="95902-131">Välj **Ja** begränsar antalet virtuella datorer per labb.</span><span class="sxs-lookup"><span data-stu-id="95902-131">Select **Yes** to limit the number of VMs per lab.</span></span> <span data-ttu-id="95902-132">Om du inte vill begränsa antalet virtuella datorer per labb väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="95902-132">If you do not want to limit the number of VMs per lab, select **No**.</span></span> <span data-ttu-id="95902-133">Om du väljer **Ja**, ange ett numeriskt värde som anger maximalt antal virtuella datorer som kan skapas eller ägs av en användare.</span><span class="sxs-lookup"><span data-stu-id="95902-133">If you select **Yes**, enter a numeric value indicating the maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="95902-134">Välj **Ja** begränsar antalet virtuella datorer som kan använda SSD (SSD disk).</span><span class="sxs-lookup"><span data-stu-id="95902-134">Select **Yes** to limit the number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="95902-135">Om du inte vill begränsa antalet virtuella datorer som kan använda SSD, väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="95902-135">If you do not want to limit the number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="95902-136">Om du väljer **Ja**, ange ett värde som anger maximalt antal virtuella datorer som kan skapas med SSD.</span><span class="sxs-lookup"><span data-stu-id="95902-136">If you select **Yes**, enter a value indicating the maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="95902-137">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="95902-137">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="95902-138">Ange automatisk avstängning</span><span class="sxs-lookup"><span data-stu-id="95902-138">Set auto-shutdown</span></span>
<span data-ttu-id="95902-139">Principen för automatisk avstängning hjälper till att minimera skräp lab genom att du kan ange hur lång tid som den här övningen VMs stängs.</span><span class="sxs-lookup"><span data-stu-id="95902-139">The auto-shutdown policy helps to minimize lab waste by allowing you to specify the time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="95902-140">På testmiljön **konfiguration och principer** bladet väljer **automatisk avstängning**.</span><span class="sxs-lookup"><span data-stu-id="95902-140">On the lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Automatisk avstängning](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="95902-142">Välj **på** att aktivera den här principen och **av** du inaktivera den.</span><span class="sxs-lookup"><span data-stu-id="95902-142">Select **On** to enable this policy, and **Off** to disable it.</span></span>

1. <span data-ttu-id="95902-143">Om du aktiverar den här principen kan du ange tid (och tidszon) för att stänga av alla virtuella datorer i det aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="95902-143">If you enable this policy, specify the time (and time zone) to shut down all VMs in the current lab.</span></span>

1. <span data-ttu-id="95902-144">Ange **Ja** eller **nr** för alternativet att skicka en avisering 15 minuter innan den angivna automatisk avstängning tid.</span><span class="sxs-lookup"><span data-stu-id="95902-144">Specify **Yes** or **No** for the option to send a notification 15 minutes prior to the specified auto-shutdown time.</span></span> <span data-ttu-id="95902-145">Om du anger **Ja**, ange en webhook URL-slutpunkt för att ta emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="95902-145">If you specify **Yes**, enter a webhook URL endpoint to receive the notification.</span></span> <span data-ttu-id="95902-146">Läs mer om webhooks [skapa en webhook eller API Azure-funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="95902-146">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="95902-147">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="95902-147">Select **Save**.</span></span>

    <span data-ttu-id="95902-148">Som standard när du har aktiverat, den här principen gäller för alla virtuella datorer i det aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="95902-148">By default, once enabled, this policy applies to all VMs in the current lab.</span></span> <span data-ttu-id="95902-149">Om du vill ta bort den här inställningen från en specifik VM, öppna bladet för den virtuella datorn och ändra dess **automatisk avstängning** inställning</span><span class="sxs-lookup"><span data-stu-id="95902-149">To remove this setting from a specific VM, open the VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="95902-150">Ange automatisk start</span><span class="sxs-lookup"><span data-stu-id="95902-150">Set auto-start</span></span>
<span data-ttu-id="95902-151">Principen för automatisk start kan du ange när de virtuella datorerna i det aktuella labbet ska startas.</span><span class="sxs-lookup"><span data-stu-id="95902-151">The auto-start policy allows you to specify when the VMs in the current lab should be started.</span></span>  

1. <span data-ttu-id="95902-152">På testmiljön **konfiguration och principer** bladet väljer **Autostart**.</span><span class="sxs-lookup"><span data-stu-id="95902-152">On the lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatisk start](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="95902-154">Välj **på** att aktivera den här principen och **av** du inaktivera den.</span><span class="sxs-lookup"><span data-stu-id="95902-154">Select **On** to enable this policy, and **Off** to disable it.</span></span>

3. <span data-ttu-id="95902-155">Om du aktiverar den här principen kan du ange den schemalagda starttiden, tidszon och veckodagar som gäller tid.</span><span class="sxs-lookup"><span data-stu-id="95902-155">If you enable this policy, specify the scheduled start time, time zone, and the days of the week for which the time applies.</span></span> 

4. <span data-ttu-id="95902-156">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="95902-156">Select **Save**.</span></span>

    <span data-ttu-id="95902-157">När du har aktiverat, tillämpas inte principen automatiskt till alla virtuella datorer i det aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="95902-157">Once enabled, this policy is not automatically applied to any VMs in the current lab.</span></span> <span data-ttu-id="95902-158">Öppna bladet för den virtuella datorn för att använda den här inställningen för en specifik VM, och ändra dess **Autostart** inställning</span><span class="sxs-lookup"><span data-stu-id="95902-158">To apply this setting to a specific VM, open the VM's blade and change its **Auto-start** setting</span></span> 

## <a name="set-expiration-date"></a><span data-ttu-id="95902-159">Ange förfallodatum</span><span class="sxs-lookup"><span data-stu-id="95902-159">Set expiration date</span></span>
<span data-ttu-id="95902-160">Du kan ange ett förfallodatum när du [skapa den virtuella datorn](devtest-lab-add-vm.md).</span><span class="sxs-lookup"><span data-stu-id="95902-160">You can set an expiration date when you [create the VM](devtest-lab-add-vm.md).</span></span> <span data-ttu-id="95902-161">I **avancerade inställningar**, väljer du kalenderikonen för att ange ett datum då den virtuella datorn tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="95902-161">In **Advanced settings**, choose the calendar icon to specify a date on which the VM will be automatically deleted.</span></span>  <span data-ttu-id="95902-162">Som standard upphör aldrig den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="95902-162">By default, the VM will never expire.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="95902-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95902-163">Next steps</span></span>
<span data-ttu-id="95902-164">När du har definierat och tillämpas olika VM-principinställningarna för övningen, är här några saker att försöka:</span><span class="sxs-lookup"><span data-stu-id="95902-164">Once you've defined and applied the various VM policy settings for your lab, here are some things to try next:</span></span>

* <span data-ttu-id="95902-165">[Förstå delade IP-adresser](devtest-lab-shared-ip.md) -förklarar hur delade IP-adresserna används i DevTest Labs för att minimera antalet offentliga IP-adresser som krävs för att ansluta till ditt labb virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="95902-165">[Understand shared IP addresses](devtest-lab-shared-ip.md) - Explains how shared IP addresses are used in DevTest Labs to minimize the number of public IP addresses required to connect to your lab VMs.</span></span>
* <span data-ttu-id="95902-166">[Konfigurera hantering av kostnaden](devtest-lab-configure-cost-management.md) -illustrerar hur du använder den **månatlig uppskattad Kostnadstrend** diagram</span><span class="sxs-lookup"><span data-stu-id="95902-166">[Configure cost management](devtest-lab-configure-cost-management.md) - Illustrates how to use the **Monthly Estimated Cost Trend** chart</span></span>  
  <span data-ttu-id="95902-167">Om du vill visa den aktuella månaden är uppskattade kostnaden-till-date och planerade sista månad kostnaden.</span><span class="sxs-lookup"><span data-stu-id="95902-167">to view the current month's estimated cost-to-date and the projected end-of-month cost.</span></span>
* <span data-ttu-id="95902-168">[Skapa den anpassade bilden](devtest-lab-create-template.md) – när du skapar en virtuell dator, anger du en bas som kan vara antingen en anpassad avbildning eller en Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="95902-168">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="95902-169">Den här artikeln beskrivs hur du skapar en anpassad avbildning från en VHD-fil.</span><span class="sxs-lookup"><span data-stu-id="95902-169">This article illustrates how to create a custom image from a VHD file.</span></span>
* <span data-ttu-id="95902-170">[Konfigurera Marketplace-bilder](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs stöder skapandet av virtuella datorer baserat på Azure Marketplace-bilder.</span><span class="sxs-lookup"><span data-stu-id="95902-170">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="95902-171">Den här artikeln visar hur du kan ange vilka eventuella Azure Marketplace-bilder kan användas när du skapar virtuella datorer i ett labb.</span><span class="sxs-lookup"><span data-stu-id="95902-171">This article illustrates how to specify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="95902-172">[Skapa en virtuell dator i ett labb](devtest-lab-add-vm-with-artifacts.md) -illustrerar hur du skapar en virtuell dator från en grundläggande bild (antingen anpassad eller Marketplace), och hur du arbetar med artefakter i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="95902-172">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how to create a VM from a base image (either custom or Marketplace), and how to work with artifacts in your VM.</span></span>

