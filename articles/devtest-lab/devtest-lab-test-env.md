---
title: "Använd Azure DevTest Labs för virtuella datorn och PaaS testmiljöer | Microsoft Docs"
description: "Lär dig hur du använder Azure DevTest Labs för den virtuella datorn och PaaS testa scenarier för miljön."
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
ms.openlocfilehash: a556cee9d7b665cd7df23c97e7e2c8c2afabbe58
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a><span data-ttu-id="1fb9e-103">Använd Azure DevTest Labs för virtuella datorn och PaaS testmiljöer</span><span class="sxs-lookup"><span data-stu-id="1fb9e-103">Use Azure DevTest Labs for VM and PaaS test environments</span></span>

<span data-ttu-id="1fb9e-104">Du kan använda Azure DevTest Labs för att implementera många viktiga scenarier, men en primär scenariet inbegriper med DevTest Labs för värddatorerna för Testare.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-104">You can use Azure DevTest Labs to implement many key scenarios, but a primary scenario involves using DevTest Labs to host machines for testers.</span></span> 

<span data-ttu-id="1fb9e-105">I det här scenariot har DevTest Labs följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="1fb9e-106">Testare kan testa den senaste versionen av sina program genom att snabbt etablera Windows och Linux-miljöer med hjälp av återanvändbara mallar och artefakter.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-106">Testers can test the latest version of their application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span>
- <span data-ttu-id="1fb9e-107">Testare kan skala upp sina belastningen testar genom att etablera flera test-agenter.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-107">Testers can scale up their load testing by provisioning multiple test agents.</span></span>
- <span data-ttu-id="1fb9e-108">Administratörer kan styra kostnader genom att säkerställa att:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="1fb9e-109">Testare kan inte hämta flera virtuella datorer än de behöver.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-109">Testers cannot get more VMs than they need.</span></span>
  - <span data-ttu-id="1fb9e-110">Virtuella datorer är avstängd när inte i användning.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-110">VMs are shut down when not in use.</span></span>

![Använda DevTest Labs för träning](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="1fb9e-112">I den här artikeln får du lära dig om olika Azure DevTest Labs funktioner som används för att uppfylla krav på tester och detaljerade anvisningar följer för att ställa in ett labb.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-112">In this article, you learn about various Azure DevTest Labs features used to meet tester requirements and the detailed steps to follow to set up a lab.</span></span>

## <a name="implementing-test-environments-with-azure-devtest-labs"></a><span data-ttu-id="1fb9e-113">Implementera testmiljöer med Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1fb9e-113">Implementing test environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="1fb9e-114">**Skapa labbet**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-114">**Create the lab**</span></span> 
   
    <span data-ttu-id="1fb9e-115">Labs utgör startpunkten i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-115">Labs are the starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="1fb9e-116">När du skapar ett labb kan utföra du uppgifter som att lägga till användare (Testare) i labbet, ange principer för att styra kostnader, definiera VM-avbildningar som kan skapa snabbare och mer.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-116">Once you create a lab, you can perform tasks such as adding users (testers) to the lab, setting policies to control costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="1fb9e-117">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-117">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-118">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-118">Task</span></span> | <span data-ttu-id="1fb9e-119">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-120">Skapa ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1fb9e-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="1fb9e-121">Lär dig mer om att skapa ett labb i Azure DevTest Labs i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-121">Learn how to create a lab in Azure DevTest Labs in the Azure portal.</span></span> |
2. <span data-ttu-id="1fb9e-122">**Skapa virtuella datorer i minuter med hjälp av fördefinierade marketplace-bilder och anpassade avbildningar**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="1fb9e-123">Du kan välja färdiga avbildningar från en mängd olika avbildningar i Azure Marketplace och göra dem tillgängliga i labbet.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-123">You can pick ready-made images from a wide variety of images in the Azure Marketplace and make them available in the lab.</span></span> <span data-ttu-id="1fb9e-124">Om de färdiga bilderna inte uppfyller dina krav kan skapa du en anpassad avbildning genom att skapa ett labb VM med en färdiga avbildning från Azure Marketplace, installera den programvara som du behöver och sparar den virtuella datorn som en anpassad avbildning i labbet.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-124">If the ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all the software that you need, and saving the VM as a custom image in the lab.</span></span>

    <span data-ttu-id="1fb9e-125">Om du kommer att använda anpassade avbildningar, Överväg att använda en avbildning factory att skapa och distribuera bilderna.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-125">If you will be using custom images, consider using an image factory to create and distribute your images.</span></span> <span data-ttu-id="1fb9e-126">En avbildning fabriken är en lösning för som konfigurationskod som regelbundet skapar och distribuerar bilderna konfigurerade automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="1fb9e-127">Detta sparar tid som krävs för att manuellt konfigurera systemet efter att en virtuell dator har skapats med det grundläggande Operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-127">This saves the time required to manually configure the system after a VM has been created with the base OS.</span></span>
  
    <span data-ttu-id="1fb9e-128">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-128">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-129">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-129">Task</span></span> | <span data-ttu-id="1fb9e-130">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-131">Konfigurera Azure Marketplace-bilder</span><span class="sxs-lookup"><span data-stu-id="1fb9e-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="1fb9e-132">Lär dig hur du kan godkända Azure Marketplace-bilder, gör att välja endast avbildningarna du vill använda för Testare.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-132">Learn how you can whitelist Azure Marketplace images, making available for selection only the images you want for the testers.</span></span>|
   | [<span data-ttu-id="1fb9e-133">Skapa en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="1fb9e-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="1fb9e-134">Skapa en anpassad avbildning genom att installera före den programvara du behöver så att Testare kan snabbt skapa en virtuell dator med hjälp av den anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-134">Create a custom image by pre-installing the software you need so that testers can quickly create a VM using the custom image.</span></span>|
   | [<span data-ttu-id="1fb9e-135">Lär dig mer om image fabriken</span><span class="sxs-lookup"><span data-stu-id="1fb9e-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="1fb9e-136">Se en video som beskriver hur du skapar och använder en bild-fabriken.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-136">Watch a video that describes how to set up and use an image factory.</span></span>|

3. <span data-ttu-id="1fb9e-137">**Skapa återanvändbara mallar för testdatorer**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-137">**Create reusable templates for test machines**</span></span> 
   
    <span data-ttu-id="1fb9e-138">En formel i Azure DevTest Labs är en lista över egenskapen standardvärden används för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-138">A formula in Azure DevTest Labs is a list of default property values used to create a VM.</span></span> <span data-ttu-id="1fb9e-139">Du kan skapa en formel i labbet genom att välja en bild, en VM-storlek (en kombination av Processorn och RAM-minne) och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-139">You can create a formula in the lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="1fb9e-140">Varje tester kan se formeln i labbet och använda den för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-140">Each tester can see the formula in the lab and use it to create a VM.</span></span> 
   
    <span data-ttu-id="1fb9e-141">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-141">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-142">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-142">Task</span></span> | <span data-ttu-id="1fb9e-143">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-144">Hantera DevTest Labs formler för att skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1fb9e-144">Manage DevTest Labs formulas to create VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="1fb9e-145">Lär dig hur du kan skapa en formel genom att välja en bild, VM-storlek (kombination av Processorn och RAM-minne) och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

3. <span data-ttu-id="1fb9e-146">**Skapa flera Virtuella testmiljöer**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-146">**Create multi-VM test environments**</span></span> 
   
    <span data-ttu-id="1fb9e-147">Du kan använda Azure Resource Manager-mallar för att definiera infrastrukturen och konfigurationen av din lösning för Azure och upprepade gånger distribuera flera test virtuella datorer i ett konsekvent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-147">You can use Azure Resource Manager templates to define the infrastructure and configuration of your Azure solution and repeatedly deploy multiple test VMs in a consistent state.</span></span>

    <span data-ttu-id="1fb9e-148">Azure PaaS-resurser kan etableras i en miljö med en Resource Manager-mall och visas i Kostnadsuppföljning.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-148">Azure PaaS resources can be provisioned in an environment from a Resource Manager template and appear in cost tracking.</span></span> <span data-ttu-id="1fb9e-149">Automatisk avstängning på VM gäller dock inte för PaaS-resurser.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-149">However, VM auto shutdown does not apply to PaaS resources.</span></span>

    <span data-ttu-id="1fb9e-150">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-150">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-151">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-151">Task</span></span> | <span data-ttu-id="1fb9e-152">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-152">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-153">Skapa miljöer med flera virtuella datorer och PaaS-resurser med Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="1fb9e-153">Create multi-VM environments and PaaS resources with Azure Resource Manager templates</span></span>](devtest-lab-create-environment-from-arm.md) |<span data-ttu-id="1fb9e-154">Lär dig hur du kan distribuera flera virtuella datorer i ett konsekvent tillstånd för en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-154">Learn how you can deploy multiple VMs in a consistent state for your test environment.</span></span>|

4. <span data-ttu-id="1fb9e-155">**Skapa de artefakter för att aktivera flexibel VM-anpassning**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-155">**Create artifacts to enable flexible VM customization**</span></span>

   <span data-ttu-id="1fb9e-156">Artefakter används för att distribuera och konfigurera ditt program när en virtuell dator har etablerats.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-156">Artifacts are used to deploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="1fb9e-157">Artefakter kan vara:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-157">Artifacts can be:</span></span>

   - <span data-ttu-id="1fb9e-158">Verktyg som du vill installera på den virtuella datorn – t.ex agenter, Fiddler och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-158">Tools that you want to install on the VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="1fb9e-159">Åtgärder som du vill köra på den virtuella datorn – t.ex att klona en repo.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-159">Actions that you want to run on the VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="1fb9e-160">Program som du vill testa.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-160">Applications that you want to test.</span></span>

   <span data-ttu-id="1fb9e-161">Många artefakter är redan tillgänglig out-of-the-box.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-161">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="1fb9e-162">Men om du vill ha mer anpassning för dina specifika behov kan du skapa egna anpassade artefakter.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-162">But if you want more customization for your specific needs, you can create your own custom artifacts.</span></span>

   <span data-ttu-id="1fb9e-163">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-163">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-164">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-164">Task</span></span> | <span data-ttu-id="1fb9e-165">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-165">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-166">Skapa anpassade artefakter för den virtuella datorn med DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1fb9e-166">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="1fb9e-167">Skapa egna anpassade artefakter för virtuella datorer i labbet.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-167">Create your own custom artifacts for the virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="1fb9e-168">Lägg till en Git-lagringsplats för att lagra anpassade artefakter och Azure Resource Manager-mallar för användning i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1fb9e-168">Add a Git repository to store custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="1fb9e-169">Lär dig mer om att lagra dina anpassade artefakter i din egen privata Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-169">Learn how to store your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="1fb9e-170">**Kontrollera kostnader**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-170">**Control costs**</span></span>
   
    <span data-ttu-id="1fb9e-171">Azure DevTest Labs kan du ange en princip i labbet för att ange det maximala antalet virtuella datorer som kan skapas med en tester i labbet.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-171">Azure DevTest Labs allows you to set a policy in the lab to specify the maximum number of VMs that can be created by a tester in the lab.</span></span> 
   
    <span data-ttu-id="1fb9e-172">Om din test-grupp har en uppsättning fungerar schema och du vill stoppa alla virtuella datorer på en viss tid på dagen och sedan automatiskt starta om dem följande dag, kan du enkelt göra det genom att ange principer för automatisk avstängning och automatisk start i labbet.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-172">If your test team has a set work schedule and you want to stop all the VMs at a particular time of the day and then automatically restart them the following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in the lab.</span></span> 
   
    <span data-ttu-id="1fb9e-173">Slutligen när app-utveckling är klar, du kan ta bort alla virtuella datorer samtidigt genom att köra en enda PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-173">Finally, when app development is complete, you can delete all the VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="1fb9e-174">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-174">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-175">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-175">Task</span></span> | <span data-ttu-id="1fb9e-176">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-176">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-177">Definiera labbprinciper</span><span class="sxs-lookup"><span data-stu-id="1fb9e-177">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="1fb9e-178">Kontrollera kostnader genom att ange principer i labbet.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-178">Control costs by setting policies in the lab.</span></span> |
   | [<span data-ttu-id="1fb9e-179">Ta bort alla labbet virtuella datorer med ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="1fb9e-179">Delete all the lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="1fb9e-180">Ta bort alla labs i en enda åtgärd när testet är klart.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-180">Delete all the labs in one operation when testing is complete.</span></span>|

1. <span data-ttu-id="1fb9e-181">**Lägg till ett virtuellt nätverk i ett labb**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-181">**Add a virtual network to a Lab**</span></span> 
   
    <span data-ttu-id="1fb9e-182">DevTest Labs skapar ett nytt virtuellt nätverk (VNET) när ett labb skapas.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-182">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="1fb9e-183">Om du har konfigurerat dina egna virtuella nätverk – till exempel med hjälp av ExpressRoute eller plats-till-plats VPN – du kan lägga till detta virtuella nätverk inställningarna för ditt virtuella nätverk så att den är tillgänglig när du skapar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-183">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET to your lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="1fb9e-184">Dessutom är en Azure Active Directory-domän koppling artefakt tillgängliga som ansluter till en virtuell dator till en domän när den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-184">In addition, there is an Azure Active Directory domain join artifact available that joins a VM to a domain when the VM is being created.</span></span> 
   
    <span data-ttu-id="1fb9e-185">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-185">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-186">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-186">Task</span></span> | <span data-ttu-id="1fb9e-187">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-188">Konfigurera ett virtuellt nätverk i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1fb9e-188">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="1fb9e-189">Lär dig hur du konfigurerar ett virtuellt nätverk i Azure DevTest Labs med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-189">Learn how to configure a virtual network in Azure DevTest Labs using the Azure portal.</span></span>|

6. <span data-ttu-id="1fb9e-190">**Dela labbet med varje tester**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-190">**Share the lab with each tester**</span></span>
   
    <span data-ttu-id="1fb9e-191">Övningar kan nås direkt med en länk som du delar med din Testare.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-191">Labs can be directly accessed using a link that you share with your testers.</span></span> <span data-ttu-id="1fb9e-192">De behöver inte ha en Azure-konto så länge som de har en [Microsoft-konto](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="1fb9e-192">They don't even have to have an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="1fb9e-193">Testare kan inte se virtuella datorer som skapats av andra Testare.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-193">Testers cannot see VMs created by other testers.</span></span>  
   
    <span data-ttu-id="1fb9e-194">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-194">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-195">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-195">Task</span></span> | <span data-ttu-id="1fb9e-196">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-196">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-197">Lägg till en tester i ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1fb9e-197">Add a tester to a lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="1fb9e-198">Använd Azure-portalen för att lägga till Testare ditt labb.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-198">Use the Azure portal to add testers to your lab.</span></span>|
   | [<span data-ttu-id="1fb9e-199">Lägg till Testare i labbet använder ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="1fb9e-199">Add testers to the lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="1fb9e-200">Använd PowerShell för att automatisera att lägga till Testare att ditt labb.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-200">Use PowerShell to automate adding testers to your lab.</span></span> |
   | [<span data-ttu-id="1fb9e-201">Hämta en länk till labbet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-201">Get a link to the lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="1fb9e-202">Lär dig hur Testare direkt åtkomst till ett labb via en hyperlänk.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-202">Learn how testers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="1fb9e-203">**Automatisera genereringen av testlabb för flera team**</span><span class="sxs-lookup"><span data-stu-id="1fb9e-203">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="1fb9e-204">Du kan automatisera genereringen av lab, inklusive anpassade inställningar genom att skapa en mall för hanteraren för filserverresurser och använder den för att skapa identiska labs och om igen.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-204">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it to create identical labs again and again.</span></span> 
   
    <span data-ttu-id="1fb9e-205">Lär dig mer genom att klicka på länkarna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="1fb9e-205">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="1fb9e-206">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1fb9e-206">Task</span></span> | <span data-ttu-id="1fb9e-207">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1fb9e-207">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1fb9e-208">Skapa ett labb som använder en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="1fb9e-208">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="1fb9e-209">Skapa labb i Azure DevTest Labs med Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-209">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

