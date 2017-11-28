---
title: "aaaUse Azure DevTest Labs för träning | Microsoft Docs"
description: "Lär dig hur toouse Azure DevTest Labs för utbildning-scenarier."
services: devtest-lab,virtual-machines
documentationcenter: na
author: steved0x
manager: douge
editor: 
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: sdanie
ms.openlocfilehash: 4a5f44a282d8f6a58849c730ff89237ccff39ca8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-training"></a><span data-ttu-id="f76a7-103">Använda Azure DevTest Labs för träning</span><span class="sxs-lookup"><span data-stu-id="f76a7-103">Use Azure DevTest Labs for training</span></span>
<span data-ttu-id="f76a7-104">Azure DevTest Labs kan vara används tooimplement många viktiga scenarier i tillägget toodev och testning.</span><span class="sxs-lookup"><span data-stu-id="f76a7-104">Azure DevTest Labs can be used tooimplement many key scenarios in addition toodev/test.</span></span> <span data-ttu-id="f76a7-105">En av dessa scenarier är tooset in ett labb för utbildning.</span><span class="sxs-lookup"><span data-stu-id="f76a7-105">One of those scenarios is tooset up a lab for training.</span></span> <span data-ttu-id="f76a7-106">Azure DevTest Labs kan du toocreate ett labb där du kan ange anpassade mallar som varje äga rum kan använda toocreate identiska och isolerade miljöer för utbildning.</span><span class="sxs-lookup"><span data-stu-id="f76a7-106">Azure DevTest Labs allows you toocreate a lab where you can provide custom templates that each trainee can use toocreate identical and isolated environments for training.</span></span> <span data-ttu-id="f76a7-107">Du kan tillämpa principer tooensure att utbildning miljöer är tillgängliga tooeach äga rum när de behöver dem och innehåller tillräckligt med resurser – till exempel virtuella datorer - krävs för hello utbildning.</span><span class="sxs-lookup"><span data-stu-id="f76a7-107">You can apply policies tooensure that training environments are available tooeach trainee only when they need them and contain enough resources - such as virtual machines - required for hello training.</span></span> <span data-ttu-id="f76a7-108">Slutligen kan du enkelt dela hello testlabb med praktikanter som de kan komma åt med ett klick.</span><span class="sxs-lookup"><span data-stu-id="f76a7-108">Finally, you can easily share hello lab with trainees, which they can access in one click.</span></span>

![Använda DevTest Labs för träning](./media/devtest-lab-training-lab/devtest-lab-training.png)

<span data-ttu-id="f76a7-110">Azure DevTest Labs uppfyller hello följa kraven som är nödvändiga tooconduct utbildning i en virtuell miljö:</span><span class="sxs-lookup"><span data-stu-id="f76a7-110">Azure DevTest Labs meets hello following requirements that are required tooconduct training in any virtual environment:</span></span> 

* <span data-ttu-id="f76a7-111">Praktikanter kan inte se virtuella datorer som skapats av andra praktikanter</span><span class="sxs-lookup"><span data-stu-id="f76a7-111">Trainees cannot see VMs created by other trainees</span></span>
* <span data-ttu-id="f76a7-112">Varje dator utbildningen ska vara identiska</span><span class="sxs-lookup"><span data-stu-id="f76a7-112">Every training machine should be identical</span></span>
* <span data-ttu-id="f76a7-113">Praktikanter kan snabbt tillhandahålla sina utbildning-miljöer</span><span class="sxs-lookup"><span data-stu-id="f76a7-113">Trainees can quickly provision their training environments</span></span>
* <span data-ttu-id="f76a7-114">Kontrollera kostnaden genom att säkerställa att praktikanter inte kan hämta flera virtuella datorer än de behöver för hello utbildning och Stäng virtuella datorer när de inte använder dem</span><span class="sxs-lookup"><span data-stu-id="f76a7-114">Control cost by ensuring that trainees cannot get more VMs than they need for hello training and also shutdown VMs when they are not using them</span></span>
* <span data-ttu-id="f76a7-115">Enkelt dela hello utbildning testlabb med varje äga rum</span><span class="sxs-lookup"><span data-stu-id="f76a7-115">Easily share hello training lab with each trainee</span></span>
* <span data-ttu-id="f76a7-116">Återanvända hello utbildning lab och om igen</span><span class="sxs-lookup"><span data-stu-id="f76a7-116">Reuse hello training lab again and again</span></span>

<span data-ttu-id="f76a7-117">I den här artikeln får information du om olika funktioner som kan användas i Azure DevTest Labs toomeet hello som beskrevs tidigare utbildningskrav och detaljerade anvisningar för att du kan följa tooset in ett labb för utbildning.</span><span class="sxs-lookup"><span data-stu-id="f76a7-117">In this article, you learn about various Azure DevTest Labs features that can be used toomeet hello previously described training requirements and detailed steps that you can follow tooset up a lab for training.</span></span>  

## <a name="implementing-training-with-azure-devtest-labs"></a><span data-ttu-id="f76a7-118">Implementera utbildning med Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="f76a7-118">Implementing training with Azure DevTest Labs</span></span>
1. <span data-ttu-id="f76a7-119">**Skapa hello labb**</span><span class="sxs-lookup"><span data-stu-id="f76a7-119">**Create hello lab**</span></span> 
   
    <span data-ttu-id="f76a7-120">Labs är hello startpunkt i Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="f76a7-120">Labs are hello starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="f76a7-121">När du skapar ett labb kan utföra du uppgifter som exempelvis lägga till användare (praktikanter) toohello övningen uppsättning principer toocontrol kostnader, definiera VM-avbildningar som kan snabbt skapa och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="f76a7-121">Once you create a lab, you can perform tasks such as add users (trainees) toohello lab, set policies toocontrol costs, define VM images that can create quickly, and more.</span></span>   
   
    <span data-ttu-id="f76a7-122">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="f76a7-122">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f76a7-123">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="f76a7-123">Task</span></span> | <span data-ttu-id="f76a7-124">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="f76a7-124">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f76a7-125">Skapa ett labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="f76a7-125">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="f76a7-126">Lär dig hur toocreate ett labb i Azure DevTest Labs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f76a7-126">Learn how toocreate a lab in Azure DevTest Labs in hello Azure portal.</span></span> |
2. <span data-ttu-id="f76a7-127">**Skapa utbildning virtuella datorer i minuter med hjälp av fördefinierade marketplace-bilder och anpassade avbildningar**</span><span class="sxs-lookup"><span data-stu-id="f76a7-127">**Create training VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="f76a7-128">Du kan plocka färdiga avbildningar från en mängd bilder i hello Azure Marketplace och göra dem tillgängliga för hello praktikanter i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="f76a7-128">You can pick ready-made images from a wide variety of images in hello Azure Marketplace and make them available for hello trainees in hello lab.</span></span> <span data-ttu-id="f76a7-129">Om hello färdiga bilder inte uppfyller dina krav kan skapa du en anpassad avbildning genom att skapa ett labb VM med en färdig bild från Azure Marketplace, installera alla hello-programvara som du behöver för hello utbildning och sparar hello VM som anpassad avbildning i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="f76a7-129">If hello ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all hello software that you need for hello training, and saving hello VM as custom image in hello lab.</span></span> 
   
    <span data-ttu-id="f76a7-130">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="f76a7-130">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f76a7-131">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="f76a7-131">Task</span></span> | <span data-ttu-id="f76a7-132">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="f76a7-132">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f76a7-133">Konfigurera Azure Marketplace-bilder</span><span class="sxs-lookup"><span data-stu-id="f76a7-133">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="f76a7-134">Lär dig hur du kan godkända Azure Marketplace-bilder. Gör tillgängligt för val av endast hello bilder du vill använda för hello utbildning.</span><span class="sxs-lookup"><span data-stu-id="f76a7-134">Learn how you can whitelist Azure Marketplace images; making available for selection only hello images you want for hello training.</span></span> |
   | [<span data-ttu-id="f76a7-135">Skapa en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="f76a7-135">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="f76a7-136">Skapa en anpassad avbildning genom att installera före hello programvara du behöver för hello utbildningen så att praktikanter kan snabbt skapa en virtuell dator med hjälp av hello anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="f76a7-136">Create a custom image by pre-installing hello software you need for hello training so that trainees can quickly create a VM using hello custom image.</span></span> |
3. <span data-ttu-id="f76a7-137">**Skapa återanvändbara mallar för utbildning-datorer**</span><span class="sxs-lookup"><span data-stu-id="f76a7-137">**Create reusable templates for training machines**</span></span> 
   
    <span data-ttu-id="f76a7-138">En formel i Azure DevTest Labs är en lista över egenskapsvärden som standard används toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f76a7-138">A formula in Azure DevTest Labs is a list of default property values used toocreate a VM.</span></span> <span data-ttu-id="f76a7-139">Du kan skapa en formel i hello labbet genom att välja en bild, en VM-storlek (en kombination av Processorn och RAM-minne) och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f76a7-139">You can create a formula in hello lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="f76a7-140">Varje äga rum kan hello formeln visas i hello lab och använda den toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f76a7-140">Each trainee can see hello formula in hello lab and use it toocreate a VM.</span></span> 
   
    <span data-ttu-id="f76a7-141">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="f76a7-141">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f76a7-142">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="f76a7-142">Task</span></span> | <span data-ttu-id="f76a7-143">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="f76a7-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f76a7-144">Hantera DevTest Labs formler toocreate virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f76a7-144">Manage DevTest Labs formulas toocreate VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="f76a7-145">Lär dig hur du kan skapa en formel genom att välja en bild, VM-storlek (kombination av Processorn och RAM-minne) och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f76a7-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span> |
4. <span data-ttu-id="f76a7-146">**Kontrollera kostnader**</span><span class="sxs-lookup"><span data-stu-id="f76a7-146">**Control costs**</span></span>
   
    <span data-ttu-id="f76a7-147">Azure DevTest Labs tillåter tooset en princip i hello lab toospecify hello maximalt antal virtuella datorer som kan skapas med en ges i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="f76a7-147">Azure DevTest Labs allows you tooset a policy in hello lab toospecify hello maximum number of VMs that can be created by a trainee in hello lab.</span></span> 
   
    <span data-ttu-id="f76a7-148">Om du gör flera dagars utbildning och vill toostop alla hello virtuella datorer på en viss tid hello och automatiskt starta om dem hello efter dag, kan du enkelt göra det genom att ställa in automatisk avstängning och automatisk start principer i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="f76a7-148">If you are conducting multi-day training and want toostop all hello VMs at a particular time of hello day and then automatically restart them hello following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in hello lab.</span></span> 
   
    <span data-ttu-id="f76a7-149">Slutligen när utbildning är klar du kan ta bort alla hello virtuella datorer samtidigt genom att köra en enda PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="f76a7-149">Finally, when training is complete you can delete all hello VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="f76a7-150">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="f76a7-150">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f76a7-151">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="f76a7-151">Task</span></span> | <span data-ttu-id="f76a7-152">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="f76a7-152">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f76a7-153">Definiera labbprinciper</span><span class="sxs-lookup"><span data-stu-id="f76a7-153">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="f76a7-154">Kontrollera kostnader genom att ange principer i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="f76a7-154">Control costs by setting policies in hello lab.</span></span> |
   | [<span data-ttu-id="f76a7-155">Ta bort alla hello lab virtuella datorer med ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="f76a7-155">Delete all hello lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="f76a7-156">Ta bort alla hello labs i en enda åtgärd när hello utbildning är klar.</span><span class="sxs-lookup"><span data-stu-id="f76a7-156">Delete all hello labs in one operation when hello training is complete.</span></span> |
5. <span data-ttu-id="f76a7-157">**Dela hello testlabb med varje äga rum**</span><span class="sxs-lookup"><span data-stu-id="f76a7-157">**Share hello lab with each trainee**</span></span>
   
    <span data-ttu-id="f76a7-158">Övningar kan nås direkt med en länk som du delar med din praktikanter.</span><span class="sxs-lookup"><span data-stu-id="f76a7-158">Labs can be directly accessed using a link that you share with your trainees.</span></span> <span data-ttu-id="f76a7-159">Din praktikanter inte ännu har toohave ett Azure-konto så länge som de har en [Microsoft-konto](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="f76a7-159">Your trainees don't even have toohave an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="f76a7-160">Praktikanter kan inte se virtuella datorer som skapats av andra praktikanter.</span><span class="sxs-lookup"><span data-stu-id="f76a7-160">Trainees cannot see VMs created by other trainees.</span></span>  
   
    <span data-ttu-id="f76a7-161">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="f76a7-161">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f76a7-162">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="f76a7-162">Task</span></span> | <span data-ttu-id="f76a7-163">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="f76a7-163">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f76a7-164">Lägga till ett äga rum tooa labb i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="f76a7-164">Add a trainee tooa lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="f76a7-165">Använd hello Azure portal tooadd praktikanter tooyour utbildning labbet.</span><span class="sxs-lookup"><span data-stu-id="f76a7-165">Use hello Azure portal tooadd trainees tooyour training lab.</span></span> |
   | [<span data-ttu-id="f76a7-166">Lägg till praktikanter toohello labbet använder ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="f76a7-166">Add trainees toohello lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="f76a7-167">Använd PowerShell tooautomate lägger till praktikanter tooyour utbildning labbet.</span><span class="sxs-lookup"><span data-stu-id="f76a7-167">Use PowerShell tooautomate adding trainees tooyour training lab.</span></span> |
   | [<span data-ttu-id="f76a7-168">Hämta en länk toohello labb</span><span class="sxs-lookup"><span data-stu-id="f76a7-168">Get a link toohello lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="f76a7-169">Lär dig hur ett labb kan nås direkt via en hyperlänk.</span><span class="sxs-lookup"><span data-stu-id="f76a7-169">Learn how a lab can be directly accessed via a hyperlink.</span></span> |
6. <span data-ttu-id="f76a7-170">**Återanvända hello lab och om igen**</span><span class="sxs-lookup"><span data-stu-id="f76a7-170">**Reuse hello lab again and again**</span></span> 
   
    <span data-ttu-id="f76a7-171">Du kan automatisera genereringen av lab, inklusive anpassade inställningar genom att skapa en mall för Resource Manager och använder den toocreate identiska labs och om igen.</span><span class="sxs-lookup"><span data-stu-id="f76a7-171">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it toocreate identical labs again and again.</span></span> 
   
    <span data-ttu-id="f76a7-172">Lär dig mer genom att klicka på hello länkarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="f76a7-172">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f76a7-173">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="f76a7-173">Task</span></span> | <span data-ttu-id="f76a7-174">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="f76a7-174">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f76a7-175">Skapa ett labb som använder en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="f76a7-175">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="f76a7-176">Skapa labb i Azure DevTest Labs med Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="f76a7-176">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

