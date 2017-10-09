---
title: aaaTroubleshoot distribution av problem med Windows virtuell dator i Azure | Microsoft Docs
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
ms.openlocfilehash: 30de25827c050cc266761cfc14548bcc64237dac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a><span data-ttu-id="49b36-103">Felsöka distribution problem med Windows virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="49b36-103">Troubleshoot deploying Windows virtual machine issues in Azure</span></span>

<span data-ttu-id="49b36-104">tootroubleshoot virtuell dator (VM) distributionsproblem i Azure, granska hello [vanliga problem](#top-issues) för vanliga fel och lösningar.</span><span class="sxs-lookup"><span data-stu-id="49b36-104">tootroubleshoot virtual machine (VM) deployment issues in Azure, review hello [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="49b36-105">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="49b36-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="49b36-106">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="49b36-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="49b36-107">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.</span><span class="sxs-lookup"><span data-stu-id="49b36-107">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="49b36-108">De främsta problemen</span><span class="sxs-lookup"><span data-stu-id="49b36-108">Top issues</span></span>
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a><span data-ttu-id="49b36-109">hello kluster stöder inte hello begärda VM-storlek</span><span class="sxs-lookup"><span data-stu-id="49b36-109">hello cluster cannot support hello requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="49b36-110">Försök hello förfrågan med en mindre VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="49b36-110">Retry hello request using a smaller VM size.</span></span>
- <span data-ttu-id="49b36-111">Om hello efterfrågade storleken hello VM inte kan ändras:</span><span class="sxs-lookup"><span data-stu-id="49b36-111">If hello size of hello requested VM cannot be changed:</span></span>
    - <span data-ttu-id="49b36-112">Stoppa alla hello virtuella datorer i hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="49b36-112">Stop all hello VMs in hello availability set.</span></span> <span data-ttu-id="49b36-113">Klicka på **resursgrupper** > resursgruppen > **resurser** > din tillgänglighetsuppsättning > **virtuella datorer** > din virtuella dator >  **Stoppa**.</span><span class="sxs-lookup"><span data-stu-id="49b36-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="49b36-114">När alla hello stoppa virtuella datorer, skapa hello VM hello önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="49b36-114">After all hello VMs stop, create hello VM in hello desired size.</span></span>
    - <span data-ttu-id="49b36-115">Starta hello nya virtuella datorn först och sedan väljer av hello stoppade virtuella datorer och klicka på Start.</span><span class="sxs-lookup"><span data-stu-id="49b36-115">Start hello new VM first, and then select each of hello stopped VMs and click Start.</span></span>


## <a name="hello-cluster-does-not-have-free-resources"></a><span data-ttu-id="49b36-116">hello klustret har inte lediga resurser</span><span class="sxs-lookup"><span data-stu-id="49b36-116">hello cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="49b36-117">Försök igen med hello begäran senare.</span><span class="sxs-lookup"><span data-stu-id="49b36-117">Retry hello request later.</span></span>
- <span data-ttu-id="49b36-118">Om hello ny virtuell dator kan vara en del av en annan tillgänglighetsuppsättning in</span><span class="sxs-lookup"><span data-stu-id="49b36-118">If hello new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="49b36-119">Skapa en virtuell dator i en annan tillgänglighetsuppsättning (i hello samma region).</span><span class="sxs-lookup"><span data-stu-id="49b36-119">Create a VM in a different availability set (in hello same region).</span></span>
    - <span data-ttu-id="49b36-120">Lägg till hello nya VM toohello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="49b36-120">Add hello new VM toohello same virtual network.</span></span>

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a><span data-ttu-id="49b36-121">Hur kan använda och distribuera en avbildning för windows-klienten till Azure?</span><span class="sxs-lookup"><span data-stu-id="49b36-121">How can I use and deploy a windows client image into Azure?</span></span>

<span data-ttu-id="49b36-122">Du kan använda Windows 7, Windows 8 eller Windows 10 i Azure för utveckling och testning scenarier om du har en lämplig Visual Studio (tidigare MSDN)-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="49b36-122">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios if you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> <span data-ttu-id="49b36-123">Detta [artikel](client-images.md) beskrivs hello behörighetskraven för att köra Windows-klienten i Azure och användning av hello Azure-galleriet bilder.</span><span class="sxs-lookup"><span data-stu-id="49b36-123">This [article](client-images.md) outlines hello eligibility requirements for running Windows client in Azure and uses of hello Azure Gallery images.</span></span>

## <a name="how-can-i-deploy-a-virtual-machine-using-hello-hybrid-use-benefit-hub"></a><span data-ttu-id="49b36-124">Hur kan jag distribuera en virtuell dator med hello Hybrid Använd förmånen (NAV)?</span><span class="sxs-lookup"><span data-stu-id="49b36-124">How can I deploy a virtual machine using hello Hybrid Use Benefit (HUB)?</span></span>

<span data-ttu-id="49b36-125">Det finns ett par olika sätt toodeploy virtuella Windows-datorer med hello Azure Hybrid Använd förmånen.</span><span class="sxs-lookup"><span data-stu-id="49b36-125">There are a couple of different ways toodeploy Windows virtual machines with hello Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="49b36-126">För en prenumeration för Enterprise-avtal:</span><span class="sxs-lookup"><span data-stu-id="49b36-126">For an Enterprise Agreement subscription:</span></span>

<span data-ttu-id="49b36-127">• Distribuera virtuella datorer från specifika Marketplace-avbildningar som är förkonfigurerad med Azure Hybrid Använd förmån.</span><span class="sxs-lookup"><span data-stu-id="49b36-127">•   Deploy VMs from specific Marketplace images that are pre-configured with Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="49b36-128">För Enterprise-avtal:</span><span class="sxs-lookup"><span data-stu-id="49b36-128">For Enterprise agreement:</span></span>

<span data-ttu-id="49b36-129">• Ladda upp en egen virtuell dator och distribuera med hjälp av en Resource Manager-mall eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49b36-129">•   Upload a custom VM and deploy using a Resource Manager template or Azure PowerShell.</span></span>

<span data-ttu-id="49b36-130">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="49b36-130">For more information, see hello following resources:</span></span>

 - [<span data-ttu-id="49b36-131">Översikt över Azure Hybrid Använd förmån</span><span class="sxs-lookup"><span data-stu-id="49b36-131">Azure Hybrid Use Benefit overview </span></span>](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [<span data-ttu-id="49b36-132">Nedladdningsbar vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="49b36-132">Downloadable FAQ</span></span>](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - <span data-ttu-id="49b36-133">[Hybridrapportering i Azure används förmån för Windows Server och Windows-klient](hybrid-use-benefit-licensing.md).</span><span class="sxs-lookup"><span data-stu-id="49b36-133">[Azure Hybrid Use Benefit for Windows Server and Windows Client](hybrid-use-benefit-licensing.md).</span></span>

 - [<span data-ttu-id="49b36-134">Hur kan jag använda hello Hybrid använda förmån i Azure</span><span class="sxs-lookup"><span data-stu-id="49b36-134">How can I use hello Hybrid Use Benefit in Azure</span></span>](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="49b36-135">Hur aktiverar jag min månadskredit för Visual studio Enterprise (BizSpark)</span><span class="sxs-lookup"><span data-stu-id="49b36-135">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="49b36-136">tooactivate din månatliga krediter, se den här [artikel](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="49b36-136">tooactivate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="how-tooadd-enterprise-devtest-toomy-enterprise-agreement-ea-tooget-access-toowindow-client-images"></a><span data-ttu-id="49b36-137">Hur åt tooadd utveckling och testning Enterprise toomy Enterprise-avtal (EA) tooget tooWindow klientavbildningar?</span><span class="sxs-lookup"><span data-stu-id="49b36-137">How tooadd Enterprise Dev/Test toomy Enterprise Agreement (EA) tooget access tooWindow client images?</span></span>

<span data-ttu-id="49b36-138">hello möjlighet toocreate prenumerationer baserat på hello utveckling och testning Enterprise erbjuder är begränsad tooAccount ägare som har tilldelats behörighet toodo så som företagsadministratör.</span><span class="sxs-lookup"><span data-stu-id="49b36-138">hello ability toocreate subscriptions based on hello Enterprise Dev/Test offer is restricted tooAccount Owners who have been given permission toodo so by an Enterprise Administrator.</span></span> <span data-ttu-id="49b36-139">Hej Kontoägaren skapar prenumerationer via hello Azure-Kontoportalen och sedan ska lägga till aktiva Visual Studio-prenumeranter som medadministratörer.</span><span class="sxs-lookup"><span data-stu-id="49b36-139">hello Account Owner creates subscriptions via hello Azure Account Portal, and then should add active Visual Studio subscribers as co-administrators.</span></span> <span data-ttu-id="49b36-140">Så att de kan hantera och använda hello-resurser som krävs för utveckling och testning.</span><span class="sxs-lookup"><span data-stu-id="49b36-140">So that they can manage and use hello resources needed for development and testing.</span></span> <span data-ttu-id="49b36-141">Mer information finns i [utveckling och testning Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/).</span><span class="sxs-lookup"><span data-stu-id="49b36-141">For more information, see [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/).</span></span>

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a><span data-ttu-id="49b36-142">Drivrutinerna saknas för den virtuella datorn N-serien Windows</span><span class="sxs-lookup"><span data-stu-id="49b36-142">My drivers are missing for my Windows N-Series VM</span></span>

<span data-ttu-id="49b36-143">Drivrutiner för Windows-baserade virtuella datorer är placerade [här](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="49b36-143">Drivers for Windows-based VMs are located [here](n-series-driver-setup.md).</span></span>

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="49b36-144">Jag kan inte hitta en GPU-instans i den virtuella datorn N-serien</span><span class="sxs-lookup"><span data-stu-id="49b36-144">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="49b36-145">tootake nytta av hello GPU-funktionerna i Azure N-serien virtuella datorer som kör Windows Server 2016 eller Windows Server 2012 R2, måste du installera drivrutinerna grafik på varje virtuell dator efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="49b36-145">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="49b36-146">Drivrutinen installationsinformationen är tillgängliga för [virtuella Windows-datorer](n-series-driver-setup.md) och [virtuella Linux-datorer](../linux/n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="49b36-146">Driver setup information is available for [Windows VMs](n-series-driver-setup.md) and [Linux VMs](../linux/n-series-driver-setup.md).</span></span>

## <a name="are-client-images-supported-for-n-series"></a><span data-ttu-id="49b36-147">Stöds klientavbildningar för N-serien?</span><span class="sxs-lookup"><span data-stu-id="49b36-147">Are client images supported for N-Series?</span></span>

<span data-ttu-id="49b36-148">Azure stöder för närvarande endast N-serien på virtuella datorer som kör Windows Server- och Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="49b36-148">Currently, Azure only supports N-Series on VMs running Windows Server and Linux operating systems.</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="49b36-149">Är N-serien virtuella datorer i min region?</span><span class="sxs-lookup"><span data-stu-id="49b36-149">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="49b36-150">Du kan kontrollera hello tillgänglighet från hello [produkter som är tillgängliga efter region tabell](https://azure.microsoft.com/regions/services), och prissättning [här](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="49b36-150">You can check hello availability from hello [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-tooi-get-them"></a><span data-ttu-id="49b36-151">Vilka klientavbildningar kan jag använda och distribuera i Azure och hur tooI få dem?</span><span class="sxs-lookup"><span data-stu-id="49b36-151">What client images can I use and deploy in Azure, and how tooI get them?</span></span>

<span data-ttu-id="49b36-152">Du kan använda Windows 7, Windows 8 eller Windows 10 i Azure för utveckling och testning scenarier som du har en lämplig Visual Studio (tidigare MSDN)-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="49b36-152">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios provided you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> 

- <span data-ttu-id="49b36-153">Windows 10-avbildningar som är tillgängliga från hello Azure-galleriet inom [berättigade utveckling och testning erbjuder](client-images.md#eligible-offers).</span><span class="sxs-lookup"><span data-stu-id="49b36-153">Windows 10 images are available from hello Azure Gallery within [eligible dev/test offers](client-images.md#eligible-offers).</span></span> 
- <span data-ttu-id="49b36-154">Visual Studio-prenumeranter inom någon typ av erbjudandet kan också [på lämpligt sätt förbereda och skapa](prepare-for-upload-vhd-image.md) en 64-bitars Windows 7, Windows 8 eller Windows 10-avbildning och sedan [överför tooAzure](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="49b36-154">Visual Studio subscribers within any type of offer can also [adequately prepare and create](prepare-for-upload-vhd-image.md) a 64-bit Windows 7, Windows 8, or Windows 10 image and then [upload tooAzure](upload-generalized-managed.md).</span></span> <span data-ttu-id="49b36-155">hello används fortfarande begränsat toodev och testning av aktiva Visual Studio-prenumeranter.</span><span class="sxs-lookup"><span data-stu-id="49b36-155">hello use remains limited toodev/test by active Visual Studio subscribers.</span></span>

<span data-ttu-id="49b36-156">Detta [artikel](client-images.md) beskrivs hello behörighetskraven för att köra Windows-klienten i Azure och användning av hello Azure-galleriet bilder.</span><span class="sxs-lookup"><span data-stu-id="49b36-156">This [article](client-images.md) outlines hello eligibility requirements for running Windows client in Azure and use of hello Azure Gallery images.</span></span>

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="49b36-157">Jag vill inte kan toosee VM-storlek familj som jag vill när du ändrar storlek på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="49b36-157">I am not able toosee VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="49b36-158">När en virtuell dator körs, är det distribuerade tooa fysisk server.</span><span class="sxs-lookup"><span data-stu-id="49b36-158">When a VM is running, it is deployed tooa physical server.</span></span> <span data-ttu-id="49b36-159">hello fysiska servrar i Azure-regioner grupperas i kluster av vanliga fysisk maskinvara.</span><span class="sxs-lookup"><span data-stu-id="49b36-159">hello physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="49b36-160">Ändra storlek på en virtuell dator som kräver hello VM toobe flyttas toodifferent maskinvara kluster är olika beroende på vilken distributionsmodell har använt toodeploy hello VM.</span><span class="sxs-lookup"><span data-stu-id="49b36-160">Resizing a VM that requires hello VM toobe moved toodifferent hardware clusters is different depending on which deployment model was used toodeploy hello VM.</span></span>

- <span data-ttu-id="49b36-161">Virtuella datorer som distribueras i klassiska distributionsmodellen hello cloud service-distributionen måste tas bort och toochange hello VMs tooa storlek i en annan familj för storlek på nytt.</span><span class="sxs-lookup"><span data-stu-id="49b36-161">VMs deployed in Classic deployment model, hello cloud service deployment must be removed and redeployed toochange hello VMs tooa size in another size family.</span></span>

- <span data-ttu-id="49b36-162">Virtuella datorer som distribueras i Resource Manager-distributionsmodellen, måste du stoppa alla virtuella datorer i hello tillgänglighetsuppsättning innan du ändrar hello storleken på någon virtuell dator i hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="49b36-162">VMs deployed in Resource Manager deployment model, you must stop all VMs in hello availability set before changing hello size of any VM in hello availability set.</span></span>

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="49b36-163">hello stöds listade VM-storlek inte vid distribution i Tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="49b36-163">hello listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="49b36-164">Välj en storlek som stöds för hello tillgänglighet set-klustret.</span><span class="sxs-lookup"><span data-stu-id="49b36-164">Choose a size that is supported on hello availability set's cluster.</span></span> <span data-ttu-id="49b36-165">Det rekommenderas när du skapar en tillgänglighetsuppsättning toochoose hello största VM-storlek du tror att du behöver och ha som ditt första distributionen toohello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="49b36-165">It is recommended when creating an availability set toochoose hello largest VM size you think you need, and have that be your first deployment toohello Availability set.</span></span>

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a><span data-ttu-id="49b36-166">Kan jag lägga till en befintlig tillgänglighetsuppsättning för klassiska Virtuella tooan?</span><span class="sxs-lookup"><span data-stu-id="49b36-166">Can I add an existing Classic VM tooan availability set?</span></span>

<span data-ttu-id="49b36-167">Ja.</span><span class="sxs-lookup"><span data-stu-id="49b36-167">Yes.</span></span> <span data-ttu-id="49b36-168">Du kan lägga till en befintlig klassiska Virtuella tooa ny eller befintlig Tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="49b36-168">You can add an existing classic VM tooa new or existing Availability Set.</span></span> <span data-ttu-id="49b36-169">Mer information finns i [lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning](classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="49b36-169">For more information see [Add an existing virtual machine tooan availability set](classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="49b36-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49b36-170">Next steps</span></span>
<span data-ttu-id="49b36-171">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="49b36-171">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="49b36-172">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="49b36-172">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="49b36-173">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.</span><span class="sxs-lookup"><span data-stu-id="49b36-173">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
