---
title: "aaaUse Azure DevTest Labs för virtuella datorn och PaaS testmiljöer | Microsoft Docs"
description: "Lär dig hur toouse Azure DevTest Labs för virtuella datorn och PaaS testa scenarier för miljön."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: d4e2c334-643a-40c9-9051-625b8f39fc86
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tarcher
ms.openlocfilehash: 9285090da768491e1275942318b094fae89e3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a><span data-ttu-id="de87d-103">Använd Azure DevTest Labs för virtuella datorn och PaaS testmiljöer</span><span class="sxs-lookup"><span data-stu-id="de87d-103">Use Azure DevTest Labs for VM and PaaS test environments</span></span>

<span data-ttu-id="de87d-104">Du kan använda Azure DevTest Labs tooimplement många viktiga scenarier, men en primär scenariet inbegriper med DevTest Labs toohost datorer för Testare.</span><span class="sxs-lookup"><span data-stu-id="de87d-104">You can use Azure DevTest Labs tooimplement many key scenarios, but a primary scenario involves using DevTest Labs toohost machines for testers.</span></span> 

<span data-ttu-id="de87d-105">I det här scenariot har DevTest Labs följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="de87d-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="de87d-106">Testare testa hello senaste versionen av sina program genom att snabbt etablera Windows och Linux-miljöer med hjälp av återanvändbara mallar och artefakter.</span><span class="sxs-lookup"><span data-stu-id="de87d-106">Testers can test hello latest version of their application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span>
- <span data-ttu-id="de87d-107">Testare kan skala upp sina belastningen testar genom att etablera flera test-agenter.</span><span class="sxs-lookup"><span data-stu-id="de87d-107">Testers can scale up their load testing by provisioning multiple test agents.</span></span>
- <span data-ttu-id="de87d-108">Administratörer kan styra kostnader genom att säkerställa att:</span><span class="sxs-lookup"><span data-stu-id="de87d-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="de87d-109">Testare kan inte hämta flera virtuella datorer än de behöver.</span><span class="sxs-lookup"><span data-stu-id="de87d-109">Testers cannot get more VMs than they need.</span></span>
  - <span data-ttu-id="de87d-110">Virtuella datorer är avstängd när inte i användning.</span><span class="sxs-lookup"><span data-stu-id="de87d-110">VMs are shut down when not in use.</span></span>

![Använda DevTest Labs för träning](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="de87d-112">I den här artikeln lär dig mer om olika Azure DevTest Labs funktioner som används toomeet tester krav och hello detaljerade anvisningar toofollow tooset in ett labb.</span><span class="sxs-lookup"><span data-stu-id="de87d-112">In this article, you learn about various Azure DevTest Labs features used toomeet tester requirements and hello detailed steps toofollow tooset up a lab.</span></span>

## <a name="implementing-test-environments-with-azure-devtest-labs"></a><span data-ttu-id="de87d-113">Implementera testmiljöer med Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="de87d-113">Implementing test environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="de87d-114">**Skapa hello labb**</span><span class="sxs-lookup"><span data-stu-id="de87d-114">**Create hello lab**</span></span> 
   
    <span data-ttu-id="de87d-115">Labs är hello startpunkt i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="de87d-115">Labs are hello starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="de87d-116">När du skapar ett labb kan utföra du uppgifter som att lägga till användare (Testare) toohello lab, inställningen principer toocontrol kostnader, definiera VM-avbildningar som kan skapa snabbt, med mera.</span><span class="sxs-lookup"><span data-stu-id="de87d-116">Once you create a lab, you can perform tasks such as adding users (testers) toohello lab, setting policies toocontrol costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="de87d-117">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-117">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-118">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-118">Task</span></span> | <span data-ttu-id="de87d-119">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-120">Skapa ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="de87d-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="de87d-121">Lär dig hur toocreate ett labb i Azure DevTest Labs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="de87d-121">Learn how toocreate a lab in Azure DevTest Labs in hello Azure portal.</span></span> |
2. <span data-ttu-id="de87d-122">**Skapa virtuella datorer i minuter med hjälp av fördefinierade marketplace-bilder och anpassade avbildningar**</span><span class="sxs-lookup"><span data-stu-id="de87d-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="de87d-123">Du kan plocka färdiga avbildningar från en mängd bilder i hello Azure Marketplace och göra dem tillgängliga i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="de87d-123">You can pick ready-made images from a wide variety of images in hello Azure Marketplace and make them available in hello lab.</span></span> <span data-ttu-id="de87d-124">Om hello färdiga bilder inte uppfyller dina krav kan skapa du en anpassad avbildning genom att skapa ett labb VM som använder en färdiga avbildning från Azure Marketplace, installera alla hello-programvara som du behöver, och sparar hello VM som en anpassad avbildning i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="de87d-124">If hello ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all hello software that you need, and saving hello VM as a custom image in hello lab.</span></span>

    <span data-ttu-id="de87d-125">Om du kommer att använda anpassade avbildningar, Överväg att använda en avbildning factory toocreate och distribuera bilderna.</span><span class="sxs-lookup"><span data-stu-id="de87d-125">If you will be using custom images, consider using an image factory toocreate and distribute your images.</span></span> <span data-ttu-id="de87d-126">En avbildning fabriken är en lösning för som konfigurationskod som regelbundet skapar och distribuerar bilderna konfigurerade automatiskt.</span><span class="sxs-lookup"><span data-stu-id="de87d-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="de87d-127">Detta sparar hello tid som krävs för toomanually konfigurera hello system när en virtuell dator har skapats med hello basera OS.</span><span class="sxs-lookup"><span data-stu-id="de87d-127">This saves hello time required toomanually configure hello system after a VM has been created with hello base OS.</span></span>
  
    <span data-ttu-id="de87d-128">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-128">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-129">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-129">Task</span></span> | <span data-ttu-id="de87d-130">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-131">Konfigurera Azure Marketplace-bilder</span><span class="sxs-lookup"><span data-stu-id="de87d-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="de87d-132">Lär dig hur du kan godkända Azure Marketplace-bilder, gör tillgängliga för val av endast hello bilder du vill använda för hello Testare.</span><span class="sxs-lookup"><span data-stu-id="de87d-132">Learn how you can whitelist Azure Marketplace images, making available for selection only hello images you want for hello testers.</span></span>|
   | [<span data-ttu-id="de87d-133">Skapa en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="de87d-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="de87d-134">Skapa en anpassad avbildning genom att installera före hello-programvara som du behöver så att Testare kan snabbt skapa en virtuell dator med hjälp av hello anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="de87d-134">Create a custom image by pre-installing hello software you need so that testers can quickly create a VM using hello custom image.</span></span>|
   | [<span data-ttu-id="de87d-135">Lär dig mer om image fabriken</span><span class="sxs-lookup"><span data-stu-id="de87d-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="de87d-136">Se en video som beskriver hur tooset upp och använda en avbildning fabriken.</span><span class="sxs-lookup"><span data-stu-id="de87d-136">Watch a video that describes how tooset up and use an image factory.</span></span>|

3. <span data-ttu-id="de87d-137">**Skapa återanvändbara mallar för testdatorer**</span><span class="sxs-lookup"><span data-stu-id="de87d-137">**Create reusable templates for test machines**</span></span> 
   
    <span data-ttu-id="de87d-138">En formel i Azure DevTest Labs är en lista över egenskapsvärden som standard används toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="de87d-138">A formula in Azure DevTest Labs is a list of default property values used toocreate a VM.</span></span> <span data-ttu-id="de87d-139">Du kan skapa en formel i hello labbet genom att välja en bild, en VM-storlek (en kombination av Processorn och RAM-minne) och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="de87d-139">You can create a formula in hello lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="de87d-140">Varje tester kan hello formeln visas i hello lab och använda den toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="de87d-140">Each tester can see hello formula in hello lab and use it toocreate a VM.</span></span> 
   
    <span data-ttu-id="de87d-141">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-141">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-142">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-142">Task</span></span> | <span data-ttu-id="de87d-143">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-144">Hantera DevTest Labs formler toocreate virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="de87d-144">Manage DevTest Labs formulas toocreate VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="de87d-145">Lär dig hur du kan skapa en formel genom att välja en bild, VM-storlek (kombination av Processorn och RAM-minne) och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="de87d-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

3. <span data-ttu-id="de87d-146">**Skapa flera Virtuella testmiljöer**</span><span class="sxs-lookup"><span data-stu-id="de87d-146">**Create multi-VM test environments**</span></span> 
   
    <span data-ttu-id="de87d-147">Du kan använda Azure Resource Manager mallar toodefine hello infrastrukturen och konfigurationen av din lösning för Azure och upprepade gånger distribuera flera test virtuella datorer i ett konsekvent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="de87d-147">You can use Azure Resource Manager templates toodefine hello infrastructure and configuration of your Azure solution and repeatedly deploy multiple test VMs in a consistent state.</span></span>

    <span data-ttu-id="de87d-148">Azure PaaS-resurser kan etableras i en miljö med en Resource Manager-mall och visas i Kostnadsuppföljning.</span><span class="sxs-lookup"><span data-stu-id="de87d-148">Azure PaaS resources can be provisioned in an environment from a Resource Manager template and appear in cost tracking.</span></span> <span data-ttu-id="de87d-149">Dock gäller inte automatisk avstängning på VM tooPaaS resurser.</span><span class="sxs-lookup"><span data-stu-id="de87d-149">However, VM auto shutdown does not apply tooPaaS resources.</span></span>

    <span data-ttu-id="de87d-150">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-150">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-151">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-151">Task</span></span> | <span data-ttu-id="de87d-152">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-152">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-153">Skapa miljöer med flera virtuella datorer och PaaS-resurser med Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="de87d-153">Create multi-VM environments and PaaS resources with Azure Resource Manager templates</span></span>](devtest-lab-create-environment-from-arm.md) |<span data-ttu-id="de87d-154">Lär dig hur du kan distribuera flera virtuella datorer i ett konsekvent tillstånd för en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="de87d-154">Learn how you can deploy multiple VMs in a consistent state for your test environment.</span></span>|

4. <span data-ttu-id="de87d-155">**Skapa artefakter tooenable flexibel VM anpassning**</span><span class="sxs-lookup"><span data-stu-id="de87d-155">**Create artifacts tooenable flexible VM customization**</span></span>

   <span data-ttu-id="de87d-156">Artefakter är används toodeploy och konfigurera ditt program när en virtuell dator har etablerats.</span><span class="sxs-lookup"><span data-stu-id="de87d-156">Artifacts are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="de87d-157">Artefakter kan vara:</span><span class="sxs-lookup"><span data-stu-id="de87d-157">Artifacts can be:</span></span>

   - <span data-ttu-id="de87d-158">Verktyg som du vill tooinstall på hello virtuell dator, till exempel agenter, Fiddler och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de87d-158">Tools that you want tooinstall on hello VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="de87d-159">Åtgärder som du vill toorun på hello virtuell dator, t.ex att klona en repo.</span><span class="sxs-lookup"><span data-stu-id="de87d-159">Actions that you want toorun on hello VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="de87d-160">Program som du vill tootest.</span><span class="sxs-lookup"><span data-stu-id="de87d-160">Applications that you want tootest.</span></span>

   <span data-ttu-id="de87d-161">Många artefakter är redan tillgänglig out-of-the-box.</span><span class="sxs-lookup"><span data-stu-id="de87d-161">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="de87d-162">Men om du vill ha mer anpassning för dina specifika behov kan du skapa egna anpassade artefakter.</span><span class="sxs-lookup"><span data-stu-id="de87d-162">But if you want more customization for your specific needs, you can create your own custom artifacts.</span></span>

   <span data-ttu-id="de87d-163">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-163">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-164">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-164">Task</span></span> | <span data-ttu-id="de87d-165">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-165">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-166">Skapa anpassade artefakter för den virtuella datorn med DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="de87d-166">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="de87d-167">Skapa egna anpassade artefakter för hello virtuella datorer i labbet.</span><span class="sxs-lookup"><span data-stu-id="de87d-167">Create your own custom artifacts for hello virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="de87d-168">Lägg till en Git-lagringsplatsen toostore anpassade artefakter och Azure Resource Manager-mallar för användning i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="de87d-168">Add a Git repository toostore custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="de87d-169">Lär dig hur toostore din anpassade artefakter i din egen privata Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="de87d-169">Learn how toostore your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="de87d-170">**Kontrollera kostnader**</span><span class="sxs-lookup"><span data-stu-id="de87d-170">**Control costs**</span></span>
   
    <span data-ttu-id="de87d-171">Azure DevTest Labs tillåter tooset en princip i hello lab toospecify hello maximalt antal virtuella datorer som kan skapas med en tester i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="de87d-171">Azure DevTest Labs allows you tooset a policy in hello lab toospecify hello maximum number of VMs that can be created by a tester in hello lab.</span></span> 
   
    <span data-ttu-id="de87d-172">Om test-teamet har en uppsättning arbetsschema och du vill toostop alla hello virtuella datorer på en viss tid hello och automatiskt starta om dem hello efter dag, kan du enkelt göra som genom inställningen principer i hello labbet automatisk avstängning och automatisk start.</span><span class="sxs-lookup"><span data-stu-id="de87d-172">If your test team has a set work schedule and you want toostop all hello VMs at a particular time of hello day and then automatically restart them hello following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in hello lab.</span></span> 
   
    <span data-ttu-id="de87d-173">Slutligen när app-utveckling är klar, du kan ta bort alla hello virtuella datorer samtidigt genom att köra en enda PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="de87d-173">Finally, when app development is complete, you can delete all hello VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="de87d-174">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-174">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-175">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-175">Task</span></span> | <span data-ttu-id="de87d-176">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-176">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-177">Definiera labbprinciper</span><span class="sxs-lookup"><span data-stu-id="de87d-177">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="de87d-178">Kontrollera kostnader genom att ange principer i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="de87d-178">Control costs by setting policies in hello lab.</span></span> |
   | [<span data-ttu-id="de87d-179">Ta bort alla hello lab virtuella datorer med ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="de87d-179">Delete all hello lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="de87d-180">Ta bort alla hello labs i en enda åtgärd när testet är klart.</span><span class="sxs-lookup"><span data-stu-id="de87d-180">Delete all hello labs in one operation when testing is complete.</span></span>|

1. <span data-ttu-id="de87d-181">**Lägg till ett virtuellt nätverk tooa labb**</span><span class="sxs-lookup"><span data-stu-id="de87d-181">**Add a virtual network tooa Lab**</span></span> 
   
    <span data-ttu-id="de87d-182">DevTest Labs skapar ett nytt virtuellt nätverk (VNET) när ett labb skapas.</span><span class="sxs-lookup"><span data-stu-id="de87d-182">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="de87d-183">Om du har konfigurerat dina egna virtuella nätverk – till exempel med hjälp av ExpressRoute eller plats-till-plats VPN – kan du lägga till inställningar för den här VNET tooyour övningen virtuella nätverk så att den är tillgänglig när du skapar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="de87d-183">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET tooyour lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="de87d-184">Dessutom är en Azure Active Directory-domän koppling artefakt tillgängliga som ansluter till en domän för VM-tooa när hello VM skapas.</span><span class="sxs-lookup"><span data-stu-id="de87d-184">In addition, there is an Azure Active Directory domain join artifact available that joins a VM tooa domain when hello VM is being created.</span></span> 
   
    <span data-ttu-id="de87d-185">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-185">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-186">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-186">Task</span></span> | <span data-ttu-id="de87d-187">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-188">Konfigurera ett virtuellt nätverk i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="de87d-188">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="de87d-189">Lär dig hur tooconfigure ett virtuellt nätverk i Azure DevTest Labs med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="de87d-189">Learn how tooconfigure a virtual network in Azure DevTest Labs using hello Azure portal.</span></span>|

6. <span data-ttu-id="de87d-190">**Dela hello testlabb med varje tester**</span><span class="sxs-lookup"><span data-stu-id="de87d-190">**Share hello lab with each tester**</span></span>
   
    <span data-ttu-id="de87d-191">Övningar kan nås direkt med en länk som du delar med din Testare.</span><span class="sxs-lookup"><span data-stu-id="de87d-191">Labs can be directly accessed using a link that you share with your testers.</span></span> <span data-ttu-id="de87d-192">De inte ännu har toohave ett Azure-konto så länge som de har en [Microsoft-konto](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="de87d-192">They don't even have toohave an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="de87d-193">Testare kan inte se virtuella datorer som skapats av andra Testare.</span><span class="sxs-lookup"><span data-stu-id="de87d-193">Testers cannot see VMs created by other testers.</span></span>  
   
    <span data-ttu-id="de87d-194">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-194">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-195">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-195">Task</span></span> | <span data-ttu-id="de87d-196">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-196">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-197">Lägga till ett tester tooa labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="de87d-197">Add a tester tooa lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="de87d-198">Använd hello Azure portal tooadd Testare tooyour labbet.</span><span class="sxs-lookup"><span data-stu-id="de87d-198">Use hello Azure portal tooadd testers tooyour lab.</span></span>|
   | [<span data-ttu-id="de87d-199">Lägg till Testare toohello labbet använder ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="de87d-199">Add testers toohello lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="de87d-200">Använd PowerShell tooautomate lägger till Testare tooyour labbet.</span><span class="sxs-lookup"><span data-stu-id="de87d-200">Use PowerShell tooautomate adding testers tooyour lab.</span></span> |
   | [<span data-ttu-id="de87d-201">Hämta en länk toohello labb</span><span class="sxs-lookup"><span data-stu-id="de87d-201">Get a link toohello lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="de87d-202">Lär dig hur Testare direkt åtkomst till ett labb via en hyperlänk.</span><span class="sxs-lookup"><span data-stu-id="de87d-202">Learn how testers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="de87d-203">**Automatisera genereringen av testlabb för flera team**</span><span class="sxs-lookup"><span data-stu-id="de87d-203">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="de87d-204">Du kan automatisera genereringen av lab, inklusive anpassade inställningar genom att skapa en mall för Resource Manager och använder den toocreate identiska labs och om igen.</span><span class="sxs-lookup"><span data-stu-id="de87d-204">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it toocreate identical labs again and again.</span></span> 
   
    <span data-ttu-id="de87d-205">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="de87d-205">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="de87d-206">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="de87d-206">Task</span></span> | <span data-ttu-id="de87d-207">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="de87d-207">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="de87d-208">Skapa ett labb som använder en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="de87d-208">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="de87d-209">Skapa labb i Azure DevTest Labs med Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="de87d-209">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

