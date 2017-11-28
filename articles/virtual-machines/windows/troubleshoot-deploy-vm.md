---
title: "Felsöka distribution problem med Windows virtuell dator i Azure | Microsoft Docs"
description: "Felsöka distribution problem med Windows virtuell dator i Azurethe Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: genli
ms.openlocfilehash: 6800c137097c2803f28dec7365f6d3f2a173b411
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a><span data-ttu-id="035f1-103">Felsöka distribution problem med Windows virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="035f1-103">Troubleshoot deploying Windows virtual machine issues in Azure</span></span>

<span data-ttu-id="035f1-104">Om du vill felsöka distributionsproblem för virtuell dator (VM) i Azure, granska den [vanliga problem](#top-issues) för vanliga fel och lösningar.</span><span class="sxs-lookup"><span data-stu-id="035f1-104">To troubleshoot virtual machine (VM) deployment issues in Azure, review the [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="035f1-105">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta Azure-experter på [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="035f1-105">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="035f1-106">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="035f1-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="035f1-107">Gå till den [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.</span><span class="sxs-lookup"><span data-stu-id="035f1-107">Go to the [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="035f1-108">De främsta problemen</span><span class="sxs-lookup"><span data-stu-id="035f1-108">Top issues</span></span>
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a><span data-ttu-id="035f1-109">Klustret har inte stöd för den begärda VM-storleken</span><span class="sxs-lookup"><span data-stu-id="035f1-109">The cluster cannot support the requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="035f1-110">Försök igen med begäran med en mindre VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="035f1-110">Retry the request using a smaller VM size.</span></span>
- <span data-ttu-id="035f1-111">Om storleken på den begärda virtuella datorn inte kan ändras:</span><span class="sxs-lookup"><span data-stu-id="035f1-111">If the size of the requested VM cannot be changed:</span></span>
    - <span data-ttu-id="035f1-112">Stoppa alla virtuella datorer i tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="035f1-112">Stop all the VMs in the availability set.</span></span> <span data-ttu-id="035f1-113">Klicka på **resursgrupper** > resursgruppen > **resurser** > din tillgänglighetsuppsättning > **virtuella datorer** > din virtuella dator >  **Stoppa**.</span><span class="sxs-lookup"><span data-stu-id="035f1-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="035f1-114">När alla virtuella datorer har slutat, kan du skapa den virtuella datorn i önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="035f1-114">After all the VMs stop, create the VM in the desired size.</span></span>
    - <span data-ttu-id="035f1-115">Starta den nya virtuella datorn först och markera varje stoppad virtuell dator och klicka på Start.</span><span class="sxs-lookup"><span data-stu-id="035f1-115">Start the new VM first, and then select each of the stopped VMs and click Start.</span></span>


## <a name="the-cluster-does-not-have-free-resources"></a><span data-ttu-id="035f1-116">Klustret har inte frigöra resurser</span><span class="sxs-lookup"><span data-stu-id="035f1-116">The cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="035f1-117">Försöka senare.</span><span class="sxs-lookup"><span data-stu-id="035f1-117">Retry the request later.</span></span>
- <span data-ttu-id="035f1-118">Om den nya virtuella datorn kan vara en del av en annan tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="035f1-118">If the new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="035f1-119">Skapa en virtuell dator i en annan tillgänglighetsuppsättning (i samma region).</span><span class="sxs-lookup"><span data-stu-id="035f1-119">Create a VM in a different availability set (in the same region).</span></span>
    - <span data-ttu-id="035f1-120">Lägg till ny virtuell dator i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="035f1-120">Add the new VM to the same virtual network.</span></span>

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a><span data-ttu-id="035f1-121">Hur kan använda och distribuera en avbildning för windows-klienten till Azure?</span><span class="sxs-lookup"><span data-stu-id="035f1-121">How can I use and deploy a windows client image into Azure?</span></span>

<span data-ttu-id="035f1-122">Du kan använda Windows 7, Windows 8 eller Windows 10 i Azure för utveckling och testning scenarier om du har en lämplig Visual Studio (tidigare MSDN)-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="035f1-122">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios if you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> <span data-ttu-id="035f1-123">Detta [artikel](client-images.md) beskrivs behörighetskraven för Windows-klienter som körs i Azure och använder Azure-galleriet bilder.</span><span class="sxs-lookup"><span data-stu-id="035f1-123">This [article](client-images.md) outlines the eligibility requirements for running Windows client in Azure and uses of the Azure Gallery images.</span></span>

## <a name="how-can-i-deploy-a-virtual-machine-using-the-hybrid-use-benefit-hub"></a><span data-ttu-id="035f1-124">Hur kan jag distribuera en virtuell dator med Hybrid Använd förmånen (NAV)?</span><span class="sxs-lookup"><span data-stu-id="035f1-124">How can I deploy a virtual machine using the Hybrid Use Benefit (HUB)?</span></span>

<span data-ttu-id="035f1-125">Det finns ett par olika sätt att distribuera Windows-datorer och de fördelar som Azure Hybrid användning.</span><span class="sxs-lookup"><span data-stu-id="035f1-125">There are a couple of different ways to deploy Windows virtual machines with the Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="035f1-126">För en prenumeration för Enterprise-avtal:</span><span class="sxs-lookup"><span data-stu-id="035f1-126">For an Enterprise Agreement subscription:</span></span>

<span data-ttu-id="035f1-127">• Distribuera virtuella datorer från specifika Marketplace-avbildningar som är förkonfigurerad med Azure Hybrid Använd förmån.</span><span class="sxs-lookup"><span data-stu-id="035f1-127">•   Deploy VMs from specific Marketplace images that are pre-configured with Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="035f1-128">För Enterprise-avtal:</span><span class="sxs-lookup"><span data-stu-id="035f1-128">For Enterprise agreement:</span></span>

<span data-ttu-id="035f1-129">• Ladda upp en egen virtuell dator och distribuera med hjälp av en Resource Manager-mall eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="035f1-129">•   Upload a custom VM and deploy using a Resource Manager template or Azure PowerShell.</span></span>

<span data-ttu-id="035f1-130">Mer information finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="035f1-130">For more information, see the following resources:</span></span>

 - [<span data-ttu-id="035f1-131">Översikt över Azure Hybrid Använd förmån</span><span class="sxs-lookup"><span data-stu-id="035f1-131">Azure Hybrid Use Benefit overview </span></span>](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [<span data-ttu-id="035f1-132">Nedladdningsbar vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="035f1-132">Downloadable FAQ</span></span>](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - <span data-ttu-id="035f1-133">[Hybridrapportering i Azure används förmån för Windows Server och Windows-klient](hybrid-use-benefit-licensing.md).</span><span class="sxs-lookup"><span data-stu-id="035f1-133">[Azure Hybrid Use Benefit for Windows Server and Windows Client](hybrid-use-benefit-licensing.md).</span></span>

 - [<span data-ttu-id="035f1-134">Hur kan jag använda Hybrid använda fördelen i Azure</span><span class="sxs-lookup"><span data-stu-id="035f1-134">How can I use the Hybrid Use Benefit in Azure</span></span>](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="035f1-135">Hur aktiverar jag min månadskredit för Visual studio Enterprise (BizSpark)</span><span class="sxs-lookup"><span data-stu-id="035f1-135">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="035f1-136">Om du vill aktivera din månadskredit finns [artikel](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="035f1-136">To activate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="how-to-add-enterprise-devtest-to-my-enterprise-agreement-ea-to-get-access-to-window-client-images"></a><span data-ttu-id="035f1-137">Hur du lägger till utveckling och testning Enterprise till mitt Enterprise avtal (EA) att få tillgång till fönstret klientavbildningar?</span><span class="sxs-lookup"><span data-stu-id="035f1-137">How to add Enterprise Dev/Test to my Enterprise Agreement (EA) to get access to Window client images?</span></span>

<span data-ttu-id="035f1-138">Möjligheten att skapa prenumerationer baserat på utveckling och testning Enterprise erbjudandet är begränsad till kontot ägare som har behörighet att göra det genom att företagsadministratör.</span><span class="sxs-lookup"><span data-stu-id="035f1-138">The ability to create subscriptions based on the Enterprise Dev/Test offer is restricted to Account Owners who have been given permission to do so by an Enterprise Administrator.</span></span> <span data-ttu-id="035f1-139">Kontoägaren skapar prenumerationer via Azure-Kontoportalen och sedan ska lägga till aktiva Visual Studio-prenumeranter som medadministratörer.</span><span class="sxs-lookup"><span data-stu-id="035f1-139">The Account Owner creates subscriptions via the Azure Account Portal, and then should add active Visual Studio subscribers as co-administrators.</span></span> <span data-ttu-id="035f1-140">Så att de kan hantera och använda de resurser som krävs för utveckling och testning.</span><span class="sxs-lookup"><span data-stu-id="035f1-140">So that they can manage and use the resources needed for development and testing.</span></span> <span data-ttu-id="035f1-141">Mer information finns i [utveckling och testning Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/).</span><span class="sxs-lookup"><span data-stu-id="035f1-141">For more information, see [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/).</span></span>

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a><span data-ttu-id="035f1-142">Drivrutinerna saknas för den virtuella datorn N-serien Windows</span><span class="sxs-lookup"><span data-stu-id="035f1-142">My drivers are missing for my Windows N-Series VM</span></span>

<span data-ttu-id="035f1-143">Drivrutiner för Windows-baserade virtuella datorer är placerade [här](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="035f1-143">Drivers for Windows-based VMs are located [here](n-series-driver-setup.md).</span></span>

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="035f1-144">Jag kan inte hitta en GPU-instans i den virtuella datorn N-serien</span><span class="sxs-lookup"><span data-stu-id="035f1-144">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="035f1-145">Om du vill dra nytta av GPU-funktionerna i Azure N-serien virtuella datorer som kör Windows Server 2016 eller Windows Server 2012 R2, måste du installera drivrutinerna grafik på varje virtuell dator efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="035f1-145">To take advantage of the GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="035f1-146">Drivrutinen installationsinformationen är tillgängliga för [virtuella Windows-datorer](n-series-driver-setup.md) och [virtuella Linux-datorer](../linux/n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="035f1-146">Driver setup information is available for [Windows VMs](n-series-driver-setup.md) and [Linux VMs](../linux/n-series-driver-setup.md).</span></span>

## <a name="are-client-images-supported-for-n-series"></a><span data-ttu-id="035f1-147">Stöds klientavbildningar för N-serien?</span><span class="sxs-lookup"><span data-stu-id="035f1-147">Are client images supported for N-Series?</span></span>

<span data-ttu-id="035f1-148">Azure stöder för närvarande endast N-serien på virtuella datorer som kör Windows Server- och Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="035f1-148">Currently, Azure only supports N-Series on VMs running Windows Server and Linux operating systems.</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="035f1-149">Är N-serien virtuella datorer i min region?</span><span class="sxs-lookup"><span data-stu-id="035f1-149">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="035f1-150">Du kan kontrollera tillgänglighet från den [produkter som är tillgängliga efter region tabell](https://azure.microsoft.com/regions/services), och prissättning [här](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="035f1-150">You can check the availability from the [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-to-i-get-them"></a><span data-ttu-id="035f1-151">Vilka klientavbildningar kan jag använda och distribuera i Azure och hur att jag dem?</span><span class="sxs-lookup"><span data-stu-id="035f1-151">What client images can I use and deploy in Azure, and how to I get them?</span></span>

<span data-ttu-id="035f1-152">Du kan använda Windows 7, Windows 8 eller Windows 10 i Azure för utveckling och testning scenarier som du har en lämplig Visual Studio (tidigare MSDN)-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="035f1-152">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios provided you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> 

- <span data-ttu-id="035f1-153">Windows 10-avbildningar som är tillgängliga från Azure-galleriet inom [berättigade utveckling och testning erbjuder](client-images.md#eligible-offers).</span><span class="sxs-lookup"><span data-stu-id="035f1-153">Windows 10 images are available from the Azure Gallery within [eligible dev/test offers](client-images.md#eligible-offers).</span></span> 
- <span data-ttu-id="035f1-154">Visual Studio-prenumeranter inom någon typ av erbjudandet kan också [på lämpligt sätt förbereda och skapa](prepare-for-upload-vhd-image.md) en 64-bitars Windows 7, Windows 8 eller Windows 10-avbildning och sedan [Överför till Azure](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="035f1-154">Visual Studio subscribers within any type of offer can also [adequately prepare and create](prepare-for-upload-vhd-image.md) a 64-bit Windows 7, Windows 8, or Windows 10 image and then [upload to Azure](upload-generalized-managed.md).</span></span> <span data-ttu-id="035f1-155">Användningen är begränsad till utveckling och testning av aktiva Visual Studio-prenumeranter.</span><span class="sxs-lookup"><span data-stu-id="035f1-155">The use remains limited to dev/test by active Visual Studio subscribers.</span></span>

<span data-ttu-id="035f1-156">Detta [artikel](client-images.md) beskrivs behörighetskraven för Windows-klienter som körs i Azure och användning av Azure-galleriet bilder.</span><span class="sxs-lookup"><span data-stu-id="035f1-156">This [article](client-images.md) outlines the eligibility requirements for running Windows client in Azure and use of the Azure Gallery images.</span></span>

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="035f1-157">Jag kan inte se VM-storlek familj som jag vill när du ändrar storlek på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="035f1-157">I am not able to see VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="035f1-158">När en virtuell dator körs distribueras den till en fysisk server.</span><span class="sxs-lookup"><span data-stu-id="035f1-158">When a VM is running, it is deployed to a physical server.</span></span> <span data-ttu-id="035f1-159">De fysiska servrarna i Azure-regioner grupperas i kluster av vanliga fysisk maskinvara.</span><span class="sxs-lookup"><span data-stu-id="035f1-159">The physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="035f1-160">Ändra storlek på en virtuell dator som kräver att den virtuella datorn flyttas till annan maskinvara kluster är olika beroende på vilken distributionsmodell som användes för att distribuera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="035f1-160">Resizing a VM that requires the VM to be moved to different hardware clusters is different depending on which deployment model was used to deploy the VM.</span></span>

- <span data-ttu-id="035f1-161">Virtuella datorer som distribueras i klassiska distributionsmodellen cloud service-distributionen måste tas bort och för att ändra de virtuella datorerna till en storlek i en annan familj för storlek på nytt.</span><span class="sxs-lookup"><span data-stu-id="035f1-161">VMs deployed in Classic deployment model, the cloud service deployment must be removed and redeployed to change the VMs to a size in another size family.</span></span>

- <span data-ttu-id="035f1-162">Virtuella datorer som distribueras i Resource Manager-distributionsmodellen, måste du stoppa alla virtuella datorer i tillgänglighetsuppsättning innan du ändrar storleken på någon virtuell dator i tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="035f1-162">VMs deployed in Resource Manager deployment model, you must stop all VMs in the availability set before changing the size of any VM in the availability set.</span></span>

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="035f1-163">Den angivna VM-storleken stöds inte vid distribution i Tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="035f1-163">The listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="035f1-164">Välj en storlek som stöds för den tillgänglighetsuppsättningen klustret.</span><span class="sxs-lookup"><span data-stu-id="035f1-164">Choose a size that is supported on the availability set's cluster.</span></span> <span data-ttu-id="035f1-165">Det rekommenderas när du skapar en tillgänglighet för att välja den största VM storlek som du tror att du behöver och ha som ditt första distributionen till tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="035f1-165">It is recommended when creating an availability set to choose the largest VM size you think you need, and have that be your first deployment to the Availability set.</span></span>

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a><span data-ttu-id="035f1-166">Kan jag lägga till en befintlig klassiska virtuell dator till en tillgänglighetsuppsättning?</span><span class="sxs-lookup"><span data-stu-id="035f1-166">Can I add an existing Classic VM to an availability set?</span></span>

<span data-ttu-id="035f1-167">Ja.</span><span class="sxs-lookup"><span data-stu-id="035f1-167">Yes.</span></span> <span data-ttu-id="035f1-168">Du kan lägga till en befintlig klassiska virtuell dator till en ny eller befintlig Tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="035f1-168">You can add an existing classic VM to a new or existing Availability Set.</span></span> <span data-ttu-id="035f1-169">Mer information finns i [lägga till en befintlig virtuell dator i en tillgänglighetsuppsättning](classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="035f1-169">For more information see [Add an existing virtual machine to an availability set](classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="035f1-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="035f1-170">Next steps</span></span>
<span data-ttu-id="035f1-171">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta Azure-experter på [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="035f1-171">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="035f1-172">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="035f1-172">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="035f1-173">Gå till den [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.</span><span class="sxs-lookup"><span data-stu-id="035f1-173">Go to the [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
