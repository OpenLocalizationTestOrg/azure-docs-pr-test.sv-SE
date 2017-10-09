---
title: "principer för aaaManage grundläggande labbet i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur tooset vissa hello grundläggande principer (inställningar) för ett labb i DevTest Labs"
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
ms.openlocfilehash: 792c0d19cfe73964be9c03d3de7751a868fd86e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="4b2dd-103">Hantera grundläggande principer för ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="4b2dd-103">Manage basic policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="4b2dd-104">Azure DevTest Labs kan du toocontrol kostnad och minimera skräp i din labs genom att hantera principer (inställningar) för varje labbet.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-104">Azure DevTest Labs enables you toocontrol cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="4b2dd-105">I den här artikeln får du komma igång med principer genom att lära dig hur tooset två viktigaste principer hello - begränsa hello antalet virtuella datorer (VM) som kan skapas eller ägs av en enskild användare och konfigurera automatisk avstängning.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-105">In this article, you get started with policies by learning how tooset two of hello most critical policies - limiting hello number of virtual machines (VM) that can be created or claimed by a single user, and configuring auto-shutdown.</span></span> <span data-ttu-id="4b2dd-106">tooview hur tooset varje lab princip finns hello artikeln [Definiera principer för labbet i Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="4b2dd-106">tooview how tooset every lab policy, see hello article, [Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span></span>  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a><span data-ttu-id="4b2dd-107">Åtkomst till ett labb principer i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="4b2dd-107">Accessing a lab's policies in Azure DevTest Labs</span></span>
<span data-ttu-id="4b2dd-108">hello hur följande steg du konfigurerar principer för ett labb i Azure DevTest Labs:</span><span class="sxs-lookup"><span data-stu-id="4b2dd-108">hello following steps guide you through setting up policies for a lab in Azure DevTest Labs:</span></span>

<span data-ttu-id="4b2dd-109">tooview (och ändra) hello principer för ett labb så här:</span><span class="sxs-lookup"><span data-stu-id="4b2dd-109">tooview (and change) hello policies for a lab, follow these steps:</span></span>

1. <span data-ttu-id="4b2dd-110">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="4b2dd-110">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="4b2dd-111">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-111">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="4b2dd-112">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-112">From hello list of labs, select hello desired lab.</span></span>   

1. <span data-ttu-id="4b2dd-113">Välj **konfiguration och principer**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-113">Select **Configuration and policies**.</span></span>

    ![Principen inställningsbladet](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. <span data-ttu-id="4b2dd-115">Hej **konfiguration och principer** bladet innehåller en meny med inställningar som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-115">hello **Configuration and policies** blade contains a menu of settings that you can specify.</span></span> <span data-ttu-id="4b2dd-116">Den här artikeln beskriver bara hello inställningar för **virtuella datorer per användare** och **automatisk avstängning**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-116">This article covers only hello settings for **Virtual machines per user** and **Auto-shutdown**.</span></span> <span data-ttu-id="4b2dd-117">toolearn om hello återstående inställningar, se [hantera alla principer för ett labb i Azure DevTest Labs](./devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="4b2dd-117">toolearn about hello remaining settings, see [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md).</span></span> 
   
## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="4b2dd-118">Ange virtuella datorer per användare</span><span class="sxs-lookup"><span data-stu-id="4b2dd-118">Set virtual machines per user</span></span>
<span data-ttu-id="4b2dd-119">Hej princip för **virtuella datorer per användare** kan du toospecify hello maximalt antal virtuella datorer som kan skapas av en enskild användare.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-119">hello policy for **Virtual machines per user** allows you toospecify hello maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="4b2dd-120">Om en användare försöker toocreate eller anspråk en virtuell dator när hello gränsen för textmeddelandeanvändare har uppfyllts, anger ett felmeddelande som hello inte VM går att skapa/begära.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-120">If a user attempts toocreate or claim a VM when hello user limit has been met, an error message indicates that hello VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="4b2dd-121">På hello lab **konfiguration och principer** väljer du **virtuella datorer per användare**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-121">On hello lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuella datorer per användare](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="4b2dd-123">Välj **Ja** toolimit hello antal virtuella datorer per användare.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-123">Select **Yes** toolimit hello number of VMs per user.</span></span> <span data-ttu-id="4b2dd-124">Om du inte vill att toolimit hello antal virtuella datorer per användare väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-124">If you do not want toolimit hello number of VMs per user, select **No**.</span></span> <span data-ttu-id="4b2dd-125">Om du väljer **Ja**, ange ett numeriskt värde som anger hello maximalt antal virtuella datorer som kan skapas eller ägs av en användare.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-125">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="4b2dd-126">Välj **Ja** toolimit hello antal virtuella datorer som kan använda SSD (SSD disk).</span><span class="sxs-lookup"><span data-stu-id="4b2dd-126">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="4b2dd-127">Om du inte vill att toolimit hello antal virtuella datorer som kan använda SSD väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-127">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="4b2dd-128">Om du väljer **Ja**, ange ett värde som anger hello maximalt antal virtuella datorer som kan skapas med SSD.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-128">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="4b2dd-129">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-129">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="4b2dd-130">Ange automatisk avstängning</span><span class="sxs-lookup"><span data-stu-id="4b2dd-130">Set auto-shutdown</span></span>
<span data-ttu-id="4b2dd-131">hello automatisk avstängning principen ser toominimize lab avfallshantering genom att låta dig toospecify hello tid som den här övningen VMs stängs.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-131">hello auto-shutdown policy helps toominimize lab waste by allowing you toospecify hello time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="4b2dd-132">På hello lab **konfiguration och principer** bladet väljer **automatisk avstängning**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-132">On hello lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Automatisk avstängning](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="4b2dd-134">Välj **på** tooenable den här principen och **av** toodisable den.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-134">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="4b2dd-135">Om du aktiverar den här principen kan du ange hello tid (och tidszon) tooshut ned alla virtuella datorer i hello aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-135">If you enable this policy, specify hello time (and time zone) tooshut down all VMs in hello current lab.</span></span>

1. <span data-ttu-id="4b2dd-136">Ange **Ja** eller **nr** ett meddelande 15 minuter tidigare toohello angetts för hello alternativet toosend automatisk avstängning tid.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-136">Specify **Yes** or **No** for hello option toosend a notification 15 minutes prior toohello specified auto-shutdown time.</span></span> <span data-ttu-id="4b2dd-137">Om du anger **Ja**, ange ett webhook URL endpoint tooreceive hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-137">If you specify **Yes**, enter a webhook URL endpoint tooreceive hello notification.</span></span> <span data-ttu-id="4b2dd-138">Läs mer om webhooks [skapa en webhook eller API Azure-funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="4b2dd-138">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="4b2dd-139">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-139">Select **Save**.</span></span>

    <span data-ttu-id="4b2dd-140">Den här principen gäller tooall virtuella datorer i hello aktuella labbet när aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-140">By default, once enabled, this policy applies tooall VMs in hello current lab.</span></span> <span data-ttu-id="4b2dd-141">Öppna tooremove-inställningen från en specifik VM hello VM bladet och ändrar dess **automatisk avstängning** inställning</span><span class="sxs-lookup"><span data-stu-id="4b2dd-141">tooremove this setting from a specific VM, open hello VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="4b2dd-142">Ange automatisk start</span><span class="sxs-lookup"><span data-stu-id="4b2dd-142">Set auto-start</span></span>
<span data-ttu-id="4b2dd-143">hello Autostart principen kan du toospecify när hello virtuella datorer i hello aktuella labbet ska startas.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-143">hello auto-start policy allows you toospecify when hello VMs in hello current lab should be started.</span></span>  

1. <span data-ttu-id="4b2dd-144">På hello lab **konfiguration och principer** bladet väljer **Autostart**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-144">On hello lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatisk start](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="4b2dd-146">Välj **på** tooenable den här principen och **av** toodisable den.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-146">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

3. <span data-ttu-id="4b2dd-147">Om du aktiverar den här principen kan ange hello schemalagda starttiden, tidszon och hello veckodagar hello vilka hello tid gäller.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-147">If you enable this policy, specify hello scheduled start time, time zone, and hello days of hello week for which hello time applies.</span></span> 

4. <span data-ttu-id="4b2dd-148">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-148">Select **Save**.</span></span>

    <span data-ttu-id="4b2dd-149">När du har aktiverat, är den här principen inte automatiskt tillämpade tooany virtuella datorer i hello aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="4b2dd-149">Once enabled, this policy is not automatically applied tooany VMs in hello current lab.</span></span> <span data-ttu-id="4b2dd-150">tooapply den här inställningen tooa specifik VM, öppna hello VM bladet och ändrar dess **Autostart** inställning</span><span class="sxs-lookup"><span data-stu-id="4b2dd-150">tooapply this setting tooa specific VM, open hello VM's blade and change its **Auto-start** setting</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4b2dd-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b2dd-151">Next steps</span></span>

- <span data-ttu-id="4b2dd-152">[Definiera principer för labbet i Azure DevTest Labs](devtest-lab-set-lab-policy.md) – Lär dig hur toomodify andra principer för labbet</span><span class="sxs-lookup"><span data-stu-id="4b2dd-152">[Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md) - Learn how toomodify other lab policies</span></span> 
