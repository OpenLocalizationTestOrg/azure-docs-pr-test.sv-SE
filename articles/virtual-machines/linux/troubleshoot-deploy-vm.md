---
title: aaaTroubleshoot distribuera Linux virtuella problem i Azure | Microsoft Docs
description: "Felsöka distribuera Linux virtuella problem i Azurethe Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
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
ms.openlocfilehash: d1092ca3d9d51af7510b19c8c9a79e6dda588697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a><span data-ttu-id="5ab2c-103">Felsöka distribuera virtuella Linux-problem i Azure</span><span class="sxs-lookup"><span data-stu-id="5ab2c-103">Troubleshoot deploying Linux virtual machine issues in Azure</span></span>

<span data-ttu-id="5ab2c-104">tootroubleshoot virtuell dator (VM) distributionsproblem i Azure, granska hello [vanliga problem](#top-issues) för vanliga fel och lösningar.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-104">tootroubleshoot virtual machine (VM) deployment issues in Azure, review hello [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="5ab2c-105">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="5ab2c-106">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="5ab2c-107">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-107">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="5ab2c-108">De främsta problemen</span><span class="sxs-lookup"><span data-stu-id="5ab2c-108">Top issues</span></span>
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a><span data-ttu-id="5ab2c-109">hello kluster stöder inte hello begärda VM-storlek</span><span class="sxs-lookup"><span data-stu-id="5ab2c-109">hello cluster cannot support hello requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="5ab2c-110">Försök hello förfrågan med en mindre VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-110">Retry hello request using a smaller VM size.</span></span>
- <span data-ttu-id="5ab2c-111">Om hello efterfrågade storleken hello VM inte kan ändras:</span><span class="sxs-lookup"><span data-stu-id="5ab2c-111">If hello size of hello requested VM cannot be changed:</span></span>
    - <span data-ttu-id="5ab2c-112">Stoppa alla hello virtuella datorer i hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-112">Stop all hello VMs in hello availability set.</span></span> <span data-ttu-id="5ab2c-113">Klicka på **resursgrupper** > resursgruppen > **resurser** > din tillgänglighetsuppsättning > **virtuella datorer** > din virtuella dator >  **Stoppa**.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="5ab2c-114">När alla hello stoppa virtuella datorer, skapa hello VM hello önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-114">After all hello VMs stop, create hello VM in hello desired size.</span></span>
    - <span data-ttu-id="5ab2c-115">Starta hello nya virtuella datorn först och sedan väljer av hello stoppade virtuella datorer och klicka på Start.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-115">Start hello new VM first, and then select each of hello stopped VMs and click Start.</span></span>


## <a name="hello-cluster-does-not-have-free-resources"></a><span data-ttu-id="5ab2c-116">hello klustret har inte lediga resurser</span><span class="sxs-lookup"><span data-stu-id="5ab2c-116">hello cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="5ab2c-117">Försök igen med hello begäran senare.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-117">Retry hello request later.</span></span>
- <span data-ttu-id="5ab2c-118">Om hello ny virtuell dator kan vara en del av en annan tillgänglighetsuppsättning in</span><span class="sxs-lookup"><span data-stu-id="5ab2c-118">If hello new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="5ab2c-119">Skapa en virtuell dator i en annan tillgänglighetsuppsättning (i hello samma region).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-119">Create a VM in a different availability set (in hello same region).</span></span>
    - <span data-ttu-id="5ab2c-120">Lägg till hello nya VM toohello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-120">Add hello new VM toohello same virtual network.</span></span>

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="5ab2c-121">Hur aktiverar jag min månadskredit för Visual studio Enterprise (BizSpark)</span><span class="sxs-lookup"><span data-stu-id="5ab2c-121">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="5ab2c-122">tooactivate din månatliga krediter, se den här [artikel](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-122">tooactivate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="why-can-i-not-install-hello-gpu-driver-for-an-ubuntu-nv-vm"></a><span data-ttu-id="5ab2c-123">Varför kan jag inte installera hello GPU-drivrutinen för en virtuell dator NV Ubuntu?</span><span class="sxs-lookup"><span data-stu-id="5ab2c-123">Why can I not install hello GPU driver for an Ubuntu NV VM?</span></span>

<span data-ttu-id="5ab2c-124">För närvarande finns endast stöd för Linux GPU i Azure NC virtuella datorer som kör Ubuntu Server 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-124">Currently, Linux GPU support is only available on Azure NC VMs running Ubuntu Server 16.04 LTS.</span></span> <span data-ttu-id="5ab2c-125">Mer information finns i [ställa in GPU drivrutiner för N-serien virtuella datorer som kör Linux](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-125">For more information, see [Set up GPU drivers for N-series VMs running Linux](n-series-driver-setup.md).</span></span>

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a><span data-ttu-id="5ab2c-126">Drivrutinerna saknas för den virtuella datorn N-serien Linux</span><span class="sxs-lookup"><span data-stu-id="5ab2c-126">My drivers are missing for my Linux N-Series VM</span></span>

<span data-ttu-id="5ab2c-127">Drivrutiner för Linux-baserade virtuella datorer är placerade [här](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-127">Drivers for Linux-based VMs are located [here](n-series-driver-setup.md).</span></span> 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="5ab2c-128">Jag kan inte hitta en GPU-instans i den virtuella datorn N-serien</span><span class="sxs-lookup"><span data-stu-id="5ab2c-128">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="5ab2c-129">tootake nytta av hello GPU-funktionerna i Azure N-serien virtuella datorer som kör Windows Server 2016 eller Windows Server 2012 R2, måste du installera drivrutinerna grafik på varje virtuell dator efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-129">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="5ab2c-130">Drivrutinen installationsinformationen är tillgängliga för [virtuella Windows-datorer](../windows/n-series-driver-setup.md) och [virtuella Linux-datorer](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-130">Driver setup information is available for [Windows VMs](../windows/n-series-driver-setup.md) and [Linux VMs](n-series-driver-setup.md).</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="5ab2c-131">Är N-serien virtuella datorer i min region?</span><span class="sxs-lookup"><span data-stu-id="5ab2c-131">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="5ab2c-132">Du kan kontrollera hello tillgänglighet från hello [produkter som är tillgängliga efter region tabell](https://azure.microsoft.com/regions/services), och prissättning [här](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-132">You can check hello availability from hello [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="5ab2c-133">Jag vill inte kan toosee VM-storlek familj som jag vill när du ändrar storlek på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-133">I am not able toosee VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="5ab2c-134">När en virtuell dator körs, är det distribuerade tooa fysisk server.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-134">When a VM is running, it is deployed tooa physical server.</span></span> <span data-ttu-id="5ab2c-135">hello fysiska servrar i Azure-regioner grupperas i kluster av vanliga fysisk maskinvara.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-135">hello physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="5ab2c-136">Ändra storlek på en virtuell dator som kräver hello VM toobe flyttas toodifferent maskinvara kluster är olika beroende på vilken distributionsmodell har använt toodeploy hello VM.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-136">Resizing a VM that requires hello VM toobe moved toodifferent hardware clusters is different depending on which deployment model was used toodeploy hello VM.</span></span>

- <span data-ttu-id="5ab2c-137">Virtuella datorer som distribueras i klassiska distributionsmodellen hello cloud service-distributionen måste tas bort och toochange hello VMs tooa storlek i en annan familj för storlek på nytt.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-137">VMs deployed in Classic deployment model, hello cloud service deployment must be removed and redeployed toochange hello VMs tooa size in another size family.</span></span>

- <span data-ttu-id="5ab2c-138">Virtuella datorer som distribueras i Resource Manager-distributionsmodellen, måste du stoppa alla virtuella datorer i hello tillgänglighetsuppsättning innan du ändrar hello storleken på någon virtuell dator i hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-138">VMs deployed in Resource Manager deployment model, you must stop all VMs in hello availability set before changing hello size of any VM in hello availability set.</span></span>

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="5ab2c-139">hello stöds listade VM-storlek inte vid distribution i Tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-139">hello listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="5ab2c-140">Välj en storlek som stöds för hello tillgänglighet set-klustret.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-140">Choose a size that is supported on hello availability set's cluster.</span></span> <span data-ttu-id="5ab2c-141">Det rekommenderas när du skapar en tillgänglighetsuppsättning toochoose hello största VM-storlek du tror att du behöver och ha som ditt första distributionen toohello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-141">It is recommended when creating an availability set toochoose hello largest VM size you think you need, and have that be your first deployment toohello Availability set.</span></span>

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a><span data-ttu-id="5ab2c-142">Vilka Linux-distributioner /-versioner som stöds på Azure?</span><span class="sxs-lookup"><span data-stu-id="5ab2c-142">What Linux distributions/versions are supported on Azure?</span></span>

<span data-ttu-id="5ab2c-143">Du hittar Linux hello listan på [Azure-Endorsed distributioner](endorsed-distros.md).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-143">You can find hello list at Linux on [Azure-Endorsed Distributions](endorsed-distros.md).</span></span>

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a><span data-ttu-id="5ab2c-144">Kan jag lägga till en befintlig tillgänglighetsuppsättning för klassiska Virtuella tooan?</span><span class="sxs-lookup"><span data-stu-id="5ab2c-144">Can I add an existing Classic VM tooan availability set?</span></span>

<span data-ttu-id="5ab2c-145">Ja.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-145">Yes.</span></span> <span data-ttu-id="5ab2c-146">Du kan lägga till en befintlig klassiska Virtuella tooa ny eller befintlig Tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-146">You can add an existing classic VM tooa new or existing Availability Set.</span></span> <span data-ttu-id="5ab2c-147">Mer information finns i [lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning](../windows/classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-147">For more information see [Add an existing virtual machine tooan availability set](../windows/classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="5ab2c-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5ab2c-148">Next steps</span></span>
<span data-ttu-id="5ab2c-149">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="5ab2c-149">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="5ab2c-150">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-150">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="5ab2c-151">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.</span><span class="sxs-lookup"><span data-stu-id="5ab2c-151">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
