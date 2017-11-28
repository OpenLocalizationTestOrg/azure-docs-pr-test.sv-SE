---
title: "principer för labbet aaaManage i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur toodefine lab principer, till exempel VM storlekar maximala virtuella datorer per användare, och stängning automatisering."
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
ms.openlocfilehash: 351b3645a1fd729455884e5d177877c2986bd853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="065c0-103">Hantera alla principer för ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="065c0-103">Manage all policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="065c0-104">Azure DevTest Labs kan du styra kostnader och minimera skräp i din labs genom att hantera principer (inställningar) för varje labbet.</span><span class="sxs-lookup"><span data-stu-id="065c0-104">Azure DevTest Labs lets you control cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="065c0-105">Den här artikeln beskrivs detaljerat steg för steg hur tooset varje princip.</span><span class="sxs-lookup"><span data-stu-id="065c0-105">This article explains in step-by-step detail how tooset each policy.</span></span>  

## <a name="set-allowed-virtual-machine-sizes"></a><span data-ttu-id="065c0-106">Ange tillåtna storlekar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="065c0-106">Set allowed virtual machine sizes</span></span>
<span data-ttu-id="065c0-107">hello tillåtna princip för inställningen hello storlekar på VM hjälper toominimize lab avfallshantering aktiverar toospecify vilka VM-storlekar tillåts i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="065c0-107">hello policy for setting hello allowed VM sizes helps toominimize lab waste by enabling you toospecify which VM sizes are allowed in hello lab.</span></span> <span data-ttu-id="065c0-108">Om den här principen aktiveras kan endast VM-storlekar från den här listan vara används toocreate virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="065c0-108">If this policy is activated, only VM sizes from this list can be used toocreate VMs.</span></span>

1. <span data-ttu-id="065c0-109">På hello lab **konfiguration och principer** bladet väljer **tillåtna storlekar för virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="065c0-109">On hello lab's **Configuration and policies** blade, select **Allowed virtual machines sizes**.</span></span>
   
    ![Tillåtna virtuella datorer storlekar](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. <span data-ttu-id="065c0-111">Välj **på** tooenable den här principen och **av** toodisable den.</span><span class="sxs-lookup"><span data-stu-id="065c0-111">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="065c0-112">Om du aktiverar den här principen väljer du en eller flera VM-storlekar som kan skapas i labbet.</span><span class="sxs-lookup"><span data-stu-id="065c0-112">If you enable this policy, select one or more VM sizes that can be created in your lab.</span></span>

1. <span data-ttu-id="065c0-113">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="065c0-113">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="065c0-114">Ange virtuella datorer per användare</span><span class="sxs-lookup"><span data-stu-id="065c0-114">Set virtual machines per user</span></span>
<span data-ttu-id="065c0-115">Hej princip för **virtuella datorer per användare** kan du toospecify hello maximalt antal virtuella datorer som kan skapas av en enskild användare.</span><span class="sxs-lookup"><span data-stu-id="065c0-115">hello policy for **Virtual machines per user** allows you toospecify hello maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="065c0-116">Om en användare försöker toocreate eller anspråk en virtuell dator när hello gränsen för textmeddelandeanvändare har uppfyllts, anger ett felmeddelande som hello inte VM går att skapa/begära.</span><span class="sxs-lookup"><span data-stu-id="065c0-116">If a user attempts toocreate or claim a VM when hello user limit has been met, an error message indicates that hello VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="065c0-117">På hello lab **konfiguration och principer** väljer du **virtuella datorer per användare**.</span><span class="sxs-lookup"><span data-stu-id="065c0-117">On hello lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuella datorer per användare](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="065c0-119">Välj **Ja** toolimit hello antal virtuella datorer per användare.</span><span class="sxs-lookup"><span data-stu-id="065c0-119">Select **Yes** toolimit hello number of VMs per user.</span></span> <span data-ttu-id="065c0-120">Om du inte vill att toolimit hello antal virtuella datorer per användare väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="065c0-120">If you do not want toolimit hello number of VMs per user, select **No**.</span></span> <span data-ttu-id="065c0-121">Om du väljer **Ja**, ange ett numeriskt värde som anger hello maximalt antal virtuella datorer som kan skapas eller ägs av en användare.</span><span class="sxs-lookup"><span data-stu-id="065c0-121">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="065c0-122">Välj **Ja** toolimit hello antal virtuella datorer som kan använda SSD (SSD disk).</span><span class="sxs-lookup"><span data-stu-id="065c0-122">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="065c0-123">Om du inte vill att toolimit hello antal virtuella datorer som kan använda SSD väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="065c0-123">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="065c0-124">Om du väljer **Ja**, ange ett värde som anger hello maximalt antal virtuella datorer som kan skapas med SSD.</span><span class="sxs-lookup"><span data-stu-id="065c0-124">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="065c0-125">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="065c0-125">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-lab"></a><span data-ttu-id="065c0-126">Ange virtuella datorer per labb</span><span class="sxs-lookup"><span data-stu-id="065c0-126">Set virtual machines per lab</span></span>
<span data-ttu-id="065c0-127">Hej princip för **virtuella datorer per labb** kan du toospecify hello maximalt antal virtuella datorer som kan skapas för hello aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="065c0-127">hello policy for **Virtual machines per lab** allows you toospecify hello maximum number of VMs that can be created for hello current lab.</span></span> <span data-ttu-id="065c0-128">Om en användare försöker toocreate en virtuell dator när hello lab gränsen har uppfyllts, anger ett felmeddelande som hello VM inte kan skapas.</span><span class="sxs-lookup"><span data-stu-id="065c0-128">If a user attempts toocreate a VM when hello lab limit has been met, an error message indicates that hello VM cannot be created.</span></span> 

1. <span data-ttu-id="065c0-129">På hello lab **konfiguration och principer** väljer du **virtuella datorer per labb**.</span><span class="sxs-lookup"><span data-stu-id="065c0-129">On hello lab's **Configuration and policies** menu, select **Virtual machines per lab**.</span></span>
   
    ![Virtuella datorer per labb](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. <span data-ttu-id="065c0-131">Välj **Ja** toolimit hello antal virtuella datorer per labb.</span><span class="sxs-lookup"><span data-stu-id="065c0-131">Select **Yes** toolimit hello number of VMs per lab.</span></span> <span data-ttu-id="065c0-132">Om du inte vill att toolimit hello antal virtuella datorer per labb väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="065c0-132">If you do not want toolimit hello number of VMs per lab, select **No**.</span></span> <span data-ttu-id="065c0-133">Om du väljer **Ja**, ange ett numeriskt värde som anger hello maximalt antal virtuella datorer som kan skapas eller ägs av en användare.</span><span class="sxs-lookup"><span data-stu-id="065c0-133">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="065c0-134">Välj **Ja** toolimit hello antal virtuella datorer som kan använda SSD (SSD disk).</span><span class="sxs-lookup"><span data-stu-id="065c0-134">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="065c0-135">Om du inte vill att toolimit hello antal virtuella datorer som kan använda SSD väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="065c0-135">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="065c0-136">Om du väljer **Ja**, ange ett värde som anger hello maximalt antal virtuella datorer som kan skapas med SSD.</span><span class="sxs-lookup"><span data-stu-id="065c0-136">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="065c0-137">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="065c0-137">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="065c0-138">Ange automatisk avstängning</span><span class="sxs-lookup"><span data-stu-id="065c0-138">Set auto-shutdown</span></span>
<span data-ttu-id="065c0-139">hello automatisk avstängning principen ser toominimize lab avfallshantering genom att låta dig toospecify hello tid som den här övningen VMs stängs.</span><span class="sxs-lookup"><span data-stu-id="065c0-139">hello auto-shutdown policy helps toominimize lab waste by allowing you toospecify hello time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="065c0-140">På hello lab **konfiguration och principer** bladet väljer **automatisk avstängning**.</span><span class="sxs-lookup"><span data-stu-id="065c0-140">On hello lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Automatisk avstängning](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="065c0-142">Välj **på** tooenable den här principen och **av** toodisable den.</span><span class="sxs-lookup"><span data-stu-id="065c0-142">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="065c0-143">Om du aktiverar den här principen kan du ange hello tid (och tidszon) tooshut ned alla virtuella datorer i hello aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="065c0-143">If you enable this policy, specify hello time (and time zone) tooshut down all VMs in hello current lab.</span></span>

1. <span data-ttu-id="065c0-144">Ange **Ja** eller **nr** ett meddelande 15 minuter tidigare toohello angetts för hello alternativet toosend automatisk avstängning tid.</span><span class="sxs-lookup"><span data-stu-id="065c0-144">Specify **Yes** or **No** for hello option toosend a notification 15 minutes prior toohello specified auto-shutdown time.</span></span> <span data-ttu-id="065c0-145">Om du anger **Ja**, ange ett webhook URL endpoint tooreceive hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="065c0-145">If you specify **Yes**, enter a webhook URL endpoint tooreceive hello notification.</span></span> <span data-ttu-id="065c0-146">Läs mer om webhooks [skapa en webhook eller API Azure-funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="065c0-146">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="065c0-147">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="065c0-147">Select **Save**.</span></span>

    <span data-ttu-id="065c0-148">Den här principen gäller tooall virtuella datorer i hello aktuella labbet när aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="065c0-148">By default, once enabled, this policy applies tooall VMs in hello current lab.</span></span> <span data-ttu-id="065c0-149">Öppna tooremove-inställningen från en specifik VM hello VM bladet och ändrar dess **automatisk avstängning** inställning</span><span class="sxs-lookup"><span data-stu-id="065c0-149">tooremove this setting from a specific VM, open hello VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="065c0-150">Ange automatisk start</span><span class="sxs-lookup"><span data-stu-id="065c0-150">Set auto-start</span></span>
<span data-ttu-id="065c0-151">hello Autostart principen kan du toospecify när hello virtuella datorer i hello aktuella labbet ska startas.</span><span class="sxs-lookup"><span data-stu-id="065c0-151">hello auto-start policy allows you toospecify when hello VMs in hello current lab should be started.</span></span>  

1. <span data-ttu-id="065c0-152">På hello lab **konfiguration och principer** bladet väljer **Autostart**.</span><span class="sxs-lookup"><span data-stu-id="065c0-152">On hello lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatisk start](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="065c0-154">Välj **på** tooenable den här principen och **av** toodisable den.</span><span class="sxs-lookup"><span data-stu-id="065c0-154">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

3. <span data-ttu-id="065c0-155">Om du aktiverar den här principen kan ange hello schemalagda starttiden, tidszon och hello veckodagar hello vilka hello tid gäller.</span><span class="sxs-lookup"><span data-stu-id="065c0-155">If you enable this policy, specify hello scheduled start time, time zone, and hello days of hello week for which hello time applies.</span></span> 

4. <span data-ttu-id="065c0-156">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="065c0-156">Select **Save**.</span></span>

    <span data-ttu-id="065c0-157">När du har aktiverat, är den här principen inte automatiskt tillämpade tooany virtuella datorer i hello aktuella labbet.</span><span class="sxs-lookup"><span data-stu-id="065c0-157">Once enabled, this policy is not automatically applied tooany VMs in hello current lab.</span></span> <span data-ttu-id="065c0-158">tooapply den här inställningen tooa specifik VM, öppna hello VM bladet och ändrar dess **Autostart** inställning</span><span class="sxs-lookup"><span data-stu-id="065c0-158">tooapply this setting tooa specific VM, open hello VM's blade and change its **Auto-start** setting</span></span> 

## <a name="set-expiration-date"></a><span data-ttu-id="065c0-159">Ange förfallodatum</span><span class="sxs-lookup"><span data-stu-id="065c0-159">Set expiration date</span></span>
<span data-ttu-id="065c0-160">Du kan ange ett förfallodatum när du [skapa hello VM](devtest-lab-add-vm.md).</span><span class="sxs-lookup"><span data-stu-id="065c0-160">You can set an expiration date when you [create hello VM](devtest-lab-add-vm.md).</span></span> <span data-ttu-id="065c0-161">I **avancerade inställningar**, Välj hello Kalender ikonen toospecify ett datum då hello VM tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="065c0-161">In **Advanced settings**, choose hello calendar icon toospecify a date on which hello VM will be automatically deleted.</span></span>  <span data-ttu-id="065c0-162">Som standard upphör aldrig hello VM.</span><span class="sxs-lookup"><span data-stu-id="065c0-162">By default, hello VM will never expire.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="065c0-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="065c0-163">Next steps</span></span>
<span data-ttu-id="065c0-164">När du har definierat och tillämpas hello olika inställningar för virtuell dator för ditt labb, kan vissa saker tootry bredvid:</span><span class="sxs-lookup"><span data-stu-id="065c0-164">Once you've defined and applied hello various VM policy settings for your lab, here are some things tootry next:</span></span>

* <span data-ttu-id="065c0-165">[Förstå delade IP-adresser](devtest-lab-shared-ip.md) -förklarar hur delade IP-adresserna används i DevTest Labs toominimize hello antal offentliga IP-adresser krävs tooconnect tooyour lab virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="065c0-165">[Understand shared IP addresses](devtest-lab-shared-ip.md) - Explains how shared IP addresses are used in DevTest Labs toominimize hello number of public IP addresses required tooconnect tooyour lab VMs.</span></span>
* <span data-ttu-id="065c0-166">[Konfigurera hantering av kostnaden](devtest-lab-configure-cost-management.md) -illustrerar hur toouse hello **månatlig uppskattad Kostnadstrend** diagram</span><span class="sxs-lookup"><span data-stu-id="065c0-166">[Configure cost management](devtest-lab-configure-cost-management.md) - Illustrates how toouse hello **Monthly Estimated Cost Trend** chart</span></span>  
  <span data-ttu-id="065c0-167">tooview hello aktuella månaden uppskattade kostnaden-till-date och hello planerat sista månad kostnaden.</span><span class="sxs-lookup"><span data-stu-id="065c0-167">tooview hello current month's estimated cost-to-date and hello projected end-of-month cost.</span></span>
* <span data-ttu-id="065c0-168">[Skapa den anpassade bilden](devtest-lab-create-template.md) – när du skapar en virtuell dator, anger du en bas som kan vara antingen en anpassad avbildning eller en Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="065c0-168">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="065c0-169">Den här artikeln visar hur toocreate en anpassad avbildning från en VHD-fil.</span><span class="sxs-lookup"><span data-stu-id="065c0-169">This article illustrates how toocreate a custom image from a VHD file.</span></span>
* <span data-ttu-id="065c0-170">[Konfigurera Marketplace-bilder](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs stöder skapandet av virtuella datorer baserat på Azure Marketplace-bilder.</span><span class="sxs-lookup"><span data-stu-id="065c0-170">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="065c0-171">Den här artikeln beskrivs hur toospecify som eventuellt Azure Marketplace-bilder kan vara används när du skapar virtuella datorer i ett labb.</span><span class="sxs-lookup"><span data-stu-id="065c0-171">This article illustrates how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="065c0-172">[Skapa en virtuell dator i ett labb](devtest-lab-add-vm-with-artifacts.md) -illustrerar hur toocreate en virtuell dator från en grundläggande bild (antingen anpassad eller Marketplace), och hur toowork med artefakter i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="065c0-172">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how toocreate a VM from a base image (either custom or Marketplace), and how toowork with artifacts in your VM.</span></span>

