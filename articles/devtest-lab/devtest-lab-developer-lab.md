---
title: "aaaUse Azure DevTest Labs för utvecklare | Microsoft Docs"
description: "Lär dig hur toouse Azure DevTest Labs för utvecklare scenarier."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 22e070e5-3d1a-49fe-9d4c-5e07cb0b7fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: tarcher
ms.openlocfilehash: 16a3ef47c9fcbca3050dd50db5b472a9a1e3c62c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-developers"></a><span data-ttu-id="1cf27-103">Använd Azure DevTest Labs för utvecklare</span><span class="sxs-lookup"><span data-stu-id="1cf27-103">Use Azure DevTest Labs for developers</span></span>
<span data-ttu-id="1cf27-104">Azure DevTest Labs kan vara används tooimplement många viktiga scenarier, men en av hello huvuduppgifterna är med hjälp av DevTest Labs toohost development datorer för utvecklare.</span><span class="sxs-lookup"><span data-stu-id="1cf27-104">Azure DevTest Labs can be used tooimplement many key scenarios, but one of hello primary scenarios involves using DevTest Labs toohost development machines for developers.</span></span> <span data-ttu-id="1cf27-105">I det här scenariot har DevTest Labs följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1cf27-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="1cf27-106">Utvecklare kan snabbt etablera deras utveckling datorer på begäran.</span><span class="sxs-lookup"><span data-stu-id="1cf27-106">Developers can quickly provision their development machines on demand.</span></span>
- <span data-ttu-id="1cf27-107">Utvecklare kan enkelt anpassa sina development datorer när så behövs.</span><span class="sxs-lookup"><span data-stu-id="1cf27-107">Developers can easily customize their development machines whenever needed.</span></span>
- <span data-ttu-id="1cf27-108">Administratörer kan styra kostnader genom att säkerställa att:</span><span class="sxs-lookup"><span data-stu-id="1cf27-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="1cf27-109">Utvecklare kan få fler virtuella datorer än de behöver för utveckling.</span><span class="sxs-lookup"><span data-stu-id="1cf27-109">Developers cannot get more VMs than they need for development.</span></span>
  - <span data-ttu-id="1cf27-110">Virtuella datorer är avstängd när inte i användning.</span><span class="sxs-lookup"><span data-stu-id="1cf27-110">VMs are shut down when not in use.</span></span> 

![Använda DevTest Labs för träning](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="1cf27-112">I den här artikeln får du lära dig om olika funktioner i Azure DevTest Labs som kan använda toomeet developer krav och hello detaljerade anvisningar för att du kan följa tooset in ett labb.</span><span class="sxs-lookup"><span data-stu-id="1cf27-112">In this article, you learn about various Azure DevTest Labs features that can be used toomeet developer requirements and hello detailed steps that you can follow tooset up a lab.</span></span>

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a><span data-ttu-id="1cf27-113">Implementera developer miljöer med Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1cf27-113">Implementing developer environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="1cf27-114">**Skapa hello labb**</span><span class="sxs-lookup"><span data-stu-id="1cf27-114">**Create hello lab**</span></span> 
   
    <span data-ttu-id="1cf27-115">Labs är hello startpunkt i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="1cf27-115">Labs are hello starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="1cf27-116">När du skapar ett labb kan utföra du uppgifter som att lägga till användare (utvecklare) toohello lab, inställningen principer toocontrol kostnader, definiera VM-avbildningar som kan skapa snabbt, med mera.</span><span class="sxs-lookup"><span data-stu-id="1cf27-116">Once you create a lab, you can perform tasks such as adding users (developers) toohello lab, setting policies toocontrol costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="1cf27-117">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1cf27-117">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="1cf27-118">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1cf27-118">Task</span></span> | <span data-ttu-id="1cf27-119">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1cf27-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1cf27-120">Skapa ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1cf27-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="1cf27-121">Lär dig hur toocreate ett labb i Azure DevTest Labs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1cf27-121">Learn how toocreate a lab in Azure DevTest Labs in hello Azure portal.</span></span> |
2. <span data-ttu-id="1cf27-122">**Skapa virtuella datorer i minuter med hjälp av fördefinierade marketplace-bilder och anpassade avbildningar**</span><span class="sxs-lookup"><span data-stu-id="1cf27-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="1cf27-123">Du kan plocka färdiga avbildningar från en mängd bilder i hello Azure Marketplace och göra dem tillgängliga i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="1cf27-123">You can pick ready-made images from a wide variety of images in hello Azure Marketplace and make them available in hello lab.</span></span> <span data-ttu-id="1cf27-124">Om hello färdiga bilder inte uppfyller dina krav kan skapa du en anpassad avbildning genom att skapa ett labb VM som använder en färdiga avbildning från Azure Marketplace, installera alla hello-programvara som du behöver, och sparar hello VM som en anpassad avbildning i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="1cf27-124">If hello ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all hello software that you need, and saving hello VM as a custom image in hello lab.</span></span>

    <span data-ttu-id="1cf27-125">Om du kommer att använda anpassade avbildningar, Överväg att använda en avbildning factory toocreate och distribuera bilderna.</span><span class="sxs-lookup"><span data-stu-id="1cf27-125">If you will be using custom images, consider using an image factory toocreate and distribute your images.</span></span> <span data-ttu-id="1cf27-126">En avbildning fabriken är en lösning för som konfigurationskod som regelbundet skapar och distribuerar bilderna konfigurerade automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1cf27-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="1cf27-127">Detta sparar hello tid som krävs för toomanually konfigurera hello system när en virtuell dator har skapats med hello basera OS.</span><span class="sxs-lookup"><span data-stu-id="1cf27-127">This saves hello time required toomanually configure hello system after a VM has been created with hello base OS.</span></span>
  
    <span data-ttu-id="1cf27-128">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1cf27-128">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="1cf27-129">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1cf27-129">Task</span></span> | <span data-ttu-id="1cf27-130">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1cf27-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1cf27-131">Konfigurera Azure Marketplace-bilder</span><span class="sxs-lookup"><span data-stu-id="1cf27-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="1cf27-132">Lär dig hur du kan godkända Azure Marketplace-bilder, gör tillgängliga för val av endast hello bilder du vill använda för hello-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="1cf27-132">Learn how you can whitelist Azure Marketplace images, making available for selection only hello images you want for hello developers.</span></span>|
   | [<span data-ttu-id="1cf27-133">Skapa en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="1cf27-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="1cf27-134">Skapa en anpassad avbildning genom att installera före hello-programvara som du behöver så att utvecklare kan snabbt skapa en virtuell dator med hjälp av hello anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="1cf27-134">Create a custom image by pre-installing hello software you need so that developers can quickly create a VM using hello custom image.</span></span>|
   | [<span data-ttu-id="1cf27-135">Lär dig mer om image fabriken</span><span class="sxs-lookup"><span data-stu-id="1cf27-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="1cf27-136">Se en video som beskriver hur tooset upp och använda en avbildning fabriken.</span><span class="sxs-lookup"><span data-stu-id="1cf27-136">Watch a video that describes how tooset up and use an image factory.</span></span>|

3. <span data-ttu-id="1cf27-137">**Skapa återanvändbara mallar för utvecklare datorer**</span><span class="sxs-lookup"><span data-stu-id="1cf27-137">**Create reusable templates for developer machines**</span></span> 
   
    <span data-ttu-id="1cf27-138">En formel i Azure DevTest Labs är en lista över egenskapsvärden som standard används toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cf27-138">A formula in Azure DevTest Labs is a list of default property values used toocreate a VM.</span></span> <span data-ttu-id="1cf27-139">Du kan skapa en formel i hello labbet genom att välja en bild, en VM-storlek (en kombination av Processorn och RAM-minne) och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1cf27-139">You can create a formula in hello lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="1cf27-140">Varje utvecklare kan hello formeln visas i hello lab och använda den toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cf27-140">Each developer can see hello formula in hello lab and use it toocreate a VM.</span></span> 
   
    <span data-ttu-id="1cf27-141">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1cf27-141">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="1cf27-142">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1cf27-142">Task</span></span> | <span data-ttu-id="1cf27-143">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1cf27-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1cf27-144">Hantera DevTest Labs formler toocreate virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1cf27-144">Manage DevTest Labs formulas toocreate VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="1cf27-145">Lär dig hur du kan skapa en formel genom att välja en bild, VM-storlek (kombination av Processorn och RAM-minne) och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1cf27-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

4. <span data-ttu-id="1cf27-146">**Skapa artefakter tooenable flexibel VM anpassning**</span><span class="sxs-lookup"><span data-stu-id="1cf27-146">**Create artifacts tooenable flexible VM customization**</span></span>

   <span data-ttu-id="1cf27-147">Artefakter är används toodeploy och konfigurera ditt program när en virtuell dator har etablerats.</span><span class="sxs-lookup"><span data-stu-id="1cf27-147">Artifacts are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="1cf27-148">Artefakter kan vara:</span><span class="sxs-lookup"><span data-stu-id="1cf27-148">Artifacts can be:</span></span>

   - <span data-ttu-id="1cf27-149">Verktyg som du vill tooinstall på hello virtuell dator, till exempel agenter, Fiddler och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cf27-149">Tools that you want tooinstall on hello VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="1cf27-150">Åtgärder som du vill toorun på hello virtuell dator, t.ex att klona en repo.</span><span class="sxs-lookup"><span data-stu-id="1cf27-150">Actions that you want toorun on hello VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="1cf27-151">Program som du vill tootest.</span><span class="sxs-lookup"><span data-stu-id="1cf27-151">Applications that you want tootest.</span></span>

   <span data-ttu-id="1cf27-152">Många artefakter är redan tillgänglig out-of-the-box.</span><span class="sxs-lookup"><span data-stu-id="1cf27-152">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="1cf27-153">Du kan skapa egna anpassade artefakter om du vill ha mer anpassning för dina specifika behov.</span><span class="sxs-lookup"><span data-stu-id="1cf27-153">You can create your own custom artifacts if you want more customization for your specific needs.</span></span>

   <span data-ttu-id="1cf27-154">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1cf27-154">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="1cf27-155">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1cf27-155">Task</span></span> | <span data-ttu-id="1cf27-156">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1cf27-156">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1cf27-157">Skapa anpassade artefakter för den virtuella datorn med DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1cf27-157">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="1cf27-158">Skapa egna anpassade artefakter för hello virtuella datorer i labbet.</span><span class="sxs-lookup"><span data-stu-id="1cf27-158">Create your own custom artifacts for hello virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="1cf27-159">Lägg till en Git-lagringsplatsen toostore anpassade artefakter och Azure Resource Manager-mallar för användning i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1cf27-159">Add a Git repository toostore custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="1cf27-160">Lär dig hur toostore din anpassade artefakter i din egen privata Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="1cf27-160">Learn how toostore your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="1cf27-161">**Kontrollera kostnader**</span><span class="sxs-lookup"><span data-stu-id="1cf27-161">**Control costs**</span></span>
   
    <span data-ttu-id="1cf27-162">Azure DevTest Labs tillåter tooset en princip i hello lab toospecify hello maximalt antal virtuella datorer som kan skapas av en utvecklare i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="1cf27-162">Azure DevTest Labs allows you tooset a policy in hello lab toospecify hello maximum number of VMs that can be created by a developer in hello lab.</span></span> 
   
    <span data-ttu-id="1cf27-163">Om din developer-grupp har en uppsättning arbetsschema och du vill toostop alla hello virtuella datorer på en viss tid hello och automatiskt starta om dem hello efter dag, kan enkelt utföra som av inställningen principer i hello lab automatisk avstängning och automatisk start.</span><span class="sxs-lookup"><span data-stu-id="1cf27-163">If your developer team has a set work schedule and you want toostop all hello VMs at a particular time of hello day and then automatically restart them hello following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in hello lab.</span></span> 
   
    <span data-ttu-id="1cf27-164">Slutligen när app-utveckling är klar, du kan ta bort alla hello virtuella datorer samtidigt genom att köra en enda PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="1cf27-164">Finally, when app development is complete, you can delete all hello VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="1cf27-165">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1cf27-165">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="1cf27-166">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1cf27-166">Task</span></span> | <span data-ttu-id="1cf27-167">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1cf27-167">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1cf27-168">Definiera labbprinciper</span><span class="sxs-lookup"><span data-stu-id="1cf27-168">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="1cf27-169">Kontrollera kostnader genom att ange principer i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="1cf27-169">Control costs by setting policies in hello lab.</span></span> |
   | [<span data-ttu-id="1cf27-170">Ta bort alla hello lab virtuella datorer med ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="1cf27-170">Delete all hello lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="1cf27-171">Ta bort alla hello labs i en enda åtgärd när är slutförda.</span><span class="sxs-lookup"><span data-stu-id="1cf27-171">Delete all hello labs in one operation when development is complete.</span></span>|

1. <span data-ttu-id="1cf27-172">**Lägg till ett virtuellt nätverk tooa VM**</span><span class="sxs-lookup"><span data-stu-id="1cf27-172">**Add a virtual network tooa VM**</span></span> 
   
    <span data-ttu-id="1cf27-173">DevTest Labs skapar ett nytt virtuellt nätverk (VNET) när ett labb skapas.</span><span class="sxs-lookup"><span data-stu-id="1cf27-173">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="1cf27-174">Om du har konfigurerat dina egna virtuella nätverk – till exempel med hjälp av ExpressRoute eller plats-till-plats VPN – kan du lägga till inställningar för den här VNET tooyour övningen virtuella nätverk så att den är tillgänglig när du skapar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1cf27-174">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET tooyour lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="1cf27-175">Dessutom är en Azure Active Directory-domän koppling artefakt tillgängligt som kommer att ansluta till en domän för VM-tooa när hello VM skapas.</span><span class="sxs-lookup"><span data-stu-id="1cf27-175">In addition, there is an Azure Active Directory domain join artifact available that will join a VM tooa domain when hello VM is being created.</span></span> 
   
    <span data-ttu-id="1cf27-176">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1cf27-176">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="1cf27-177">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1cf27-177">Task</span></span> | <span data-ttu-id="1cf27-178">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1cf27-178">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1cf27-179">Konfigurera ett virtuellt nätverk i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1cf27-179">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="1cf27-180">Lär dig hur tooconfigure ett virtuellt nätverk i Azure DevTest Labs med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1cf27-180">Learn how tooconfigure a virtual network in Azure DevTest Labs using hello Azure portal.</span></span>|

6. <span data-ttu-id="1cf27-181">**Dela hello testlabb med varje utvecklare**</span><span class="sxs-lookup"><span data-stu-id="1cf27-181">**Share hello lab with each developer**</span></span>
   
    <span data-ttu-id="1cf27-182">Övningar kan nås direkt med en länk som du delar med utvecklarna.</span><span class="sxs-lookup"><span data-stu-id="1cf27-182">Labs can be directly accessed using a link that you share with your developers.</span></span> <span data-ttu-id="1cf27-183">De inte ännu har toohave ett Azure-konto så länge som de har en [Microsoft-konto](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="1cf27-183">They don't even have toohave an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="1cf27-184">Utvecklare kan inte se virtuella datorer som skapats av andra utvecklare.</span><span class="sxs-lookup"><span data-stu-id="1cf27-184">Developers cannot see VMs created by other developers.</span></span>  
   
    <span data-ttu-id="1cf27-185">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1cf27-185">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="1cf27-186">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1cf27-186">Task</span></span> | <span data-ttu-id="1cf27-187">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1cf27-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1cf27-188">Lägga till ett developer tooa labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1cf27-188">Add a developer tooa lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="1cf27-189">Använd hello Azure portal tooadd utvecklare tooyour labbet.</span><span class="sxs-lookup"><span data-stu-id="1cf27-189">Use hello Azure portal tooadd developers tooyour lab.</span></span>|
   | [<span data-ttu-id="1cf27-190">Lägg till utvecklare toohello labbet använder ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="1cf27-190">Add developers toohello lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="1cf27-191">Använd PowerShell tooautomate lägger till utvecklare tooyour labbet.</span><span class="sxs-lookup"><span data-stu-id="1cf27-191">Use PowerShell tooautomate adding developers tooyour lab.</span></span> |
   | [<span data-ttu-id="1cf27-192">Hämta en länk toohello labb</span><span class="sxs-lookup"><span data-stu-id="1cf27-192">Get a link toohello lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="1cf27-193">Lär dig hur utvecklare kan direkt åtkomst till ett labb via en hyperlänk.</span><span class="sxs-lookup"><span data-stu-id="1cf27-193">Learn how developers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="1cf27-194">**Automatisera genereringen av testlabb för flera team**</span><span class="sxs-lookup"><span data-stu-id="1cf27-194">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="1cf27-195">Du kan automatisera genereringen av lab, inklusive anpassade inställningar genom att skapa en mall för Resource Manager och använder den toocreate identiska labs och om igen.</span><span class="sxs-lookup"><span data-stu-id="1cf27-195">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it toocreate identical labs again and again.</span></span> 
   
    <span data-ttu-id="1cf27-196">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1cf27-196">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="1cf27-197">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="1cf27-197">Task</span></span> | <span data-ttu-id="1cf27-198">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1cf27-198">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="1cf27-199">Skapa ett labb som använder en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="1cf27-199">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="1cf27-200">Skapa labb i Azure DevTest Labs med Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="1cf27-200">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

