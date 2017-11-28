---
title: "Jämföra anpassade avbildningar och formler i DevTest Labs | Microsoft Docs"
description: "Lär dig mer om skillnaderna mellan anpassade avbildningar och formler som VM baserar så att du kan bestämma vilket som passar din miljö."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tarcher
ms.openlocfilehash: ff771abc26c08f0adb977c29739d2f5c91924b21
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a><span data-ttu-id="e9049-103">Jämföra anpassade avbildningar och formler i DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="e9049-103">Comparing custom images and formulas in DevTest Labs</span></span>
<span data-ttu-id="e9049-104">Båda [anpassade avbildningar](devtest-lab-create-template.md) och [formler](devtest-lab-manage-formulas.md) kan användas som grund för [skapa nya virtuella datorer](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="e9049-104">Both [custom images](devtest-lab-create-template.md) and [formulas](devtest-lab-manage-formulas.md) can be used as bases for [created new VMs](devtest-lab-add-vm-with-artifacts.md).</span></span> <span data-ttu-id="e9049-105">Dock viktiga skillnaden mellan anpassade avbildningar och formler är att en anpassad avbildning är helt enkelt en avbildning baserat på en virtuell Hårddisk, medan en formel är en avbildning baserat på en virtuell Hårddisk *förutom* förkonfigurerade inställningar – till exempel VM-storlek, virtuella nätverk, undernät och artefakter.</span><span class="sxs-lookup"><span data-stu-id="e9049-105">However, the key distinction between custom images and formulas is that a custom image is simply an image based on a VHD, while a formula is an image based on a VHD *in addition to* preconfigured settings - such as VM Size, virtual network, subnet, and artifacts.</span></span> <span data-ttu-id="e9049-106">Inställningarna förinställda ställs in med standardvärden som kan åsidosättas vid tidpunkten för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e9049-106">These preconfigured settings are set up with default values that can be overridden at the time of VM creation.</span></span> <span data-ttu-id="e9049-107">Den här artikeln förklarar några av (tekniker) och nackdelar (nackdelar) till att använda anpassade avbildningar jämfört med formler.</span><span class="sxs-lookup"><span data-stu-id="e9049-107">This article explains some of the advantages (pros) and disadvantages (cons) to using custom images versus using formulas.</span></span>

## <a name="custom-image-pros-and-cons"></a><span data-ttu-id="e9049-108">Anpassad bild- och nackdelar</span><span class="sxs-lookup"><span data-stu-id="e9049-108">Custom image pros and cons</span></span>
<span data-ttu-id="e9049-109">Anpassade avbildningar är statisk, ändras kan du skapa virtuella datorer från en önskad miljö.</span><span class="sxs-lookup"><span data-stu-id="e9049-109">Custom images provide a static, immutable way to create VMs from a desired environment.</span></span> 

<span data-ttu-id="e9049-110">**Tekniker**</span><span class="sxs-lookup"><span data-stu-id="e9049-110">**Pros**</span></span>

* <span data-ttu-id="e9049-111">Etablering av virtuell dator från en anpassad avbildning är snabb eftersom inget ändras när den virtuella datorn är de från avbildningen.</span><span class="sxs-lookup"><span data-stu-id="e9049-111">VM provisioning from a custom image is fast as nothing changes after the VM is spun up from the image.</span></span> <span data-ttu-id="e9049-112">Med andra ord, finns det inga inställningar att tillämpa den anpassade avbildningen är en bild utan inställningar.</span><span class="sxs-lookup"><span data-stu-id="e9049-112">In other words, there are no settings to apply as the custom image is just an image without settings.</span></span> 
* <span data-ttu-id="e9049-113">Virtuella datorer skapas från en anpassad bild är identiska.</span><span class="sxs-lookup"><span data-stu-id="e9049-113">VMs created from a single custom image are identical.</span></span>

<span data-ttu-id="e9049-114">**Nackdelar**</span><span class="sxs-lookup"><span data-stu-id="e9049-114">**Cons**</span></span>

* <span data-ttu-id="e9049-115">Om du behöver uppdatera viss aspekt av den anpassade avbildningen måste du återskapa avbildningen.</span><span class="sxs-lookup"><span data-stu-id="e9049-115">If you need to update some aspect of the custom image, the image must be recreated.</span></span>  

## <a name="formula-pros-and-cons"></a><span data-ttu-id="e9049-116">Formeln- och nackdelar</span><span class="sxs-lookup"><span data-stu-id="e9049-116">Formula pros and cons</span></span>
<span data-ttu-id="e9049-117">Formler ger ett dynamiskt sätt att skapa virtuella datorer från önskade konfigurationsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="e9049-117">Formulas provide a dynamic way to create VMs from the desired configuration/settings.</span></span>

<span data-ttu-id="e9049-118">**Tekniker**</span><span class="sxs-lookup"><span data-stu-id="e9049-118">**Pros**</span></span>

* <span data-ttu-id="e9049-119">Ändringar i miljön kan hämtas direkt via artefakter.</span><span class="sxs-lookup"><span data-stu-id="e9049-119">Changes in the environment can be captured on the fly via artifacts.</span></span> <span data-ttu-id="e9049-120">Om du vill att en virtuell dator som installerats med den senaste bits från din pipeline versionen eller registrera senaste koden från din lagringsplats kan du bara ange en artefakt som distribuerar den senaste bits eller anlitar senaste koden i formeln tillsammans med en mål-basavbildning.</span><span class="sxs-lookup"><span data-stu-id="e9049-120">For example, if you want a VM installed with the latest bits from your release pipeline or enlist the latest code from your repository, you can simply specify an artifact that deploys the latest bits or enlists the latest code in the formula together with a target base image.</span></span> <span data-ttu-id="e9049-121">När den här formeln används för att skapa virtuella datorer, är den senaste bits/kod distribueras/registrerad till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e9049-121">Whenever this formula is used to create VMs, the latest bits/code are deployed/enlisted to the VM.</span></span> 
* <span data-ttu-id="e9049-122">Formler kan definiera standardinställningar som anpassade avbildningar inte kan tillhandahålla - som VM-storlekar och inställningarna för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e9049-122">Formulas can define default settings that custom images cannot provide - such as VM sizes and virtual network settings.</span></span> 
* <span data-ttu-id="e9049-123">Inställningarna sparas i en formel som standardvärden, men kan ändras när den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="e9049-123">The settings saved in a formula are shown as default values, but can be modified when the VM is created.</span></span> 

<span data-ttu-id="e9049-124">**Nackdelar**</span><span class="sxs-lookup"><span data-stu-id="e9049-124">**Cons**</span></span>

* <span data-ttu-id="e9049-125">Skapa en virtuell dator från en formel kan det ta längre tid än att skapa en virtuell dator från en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="e9049-125">Creating a VM from a formula can take more time than creating a VM from a custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="e9049-126">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="e9049-126">Related blog posts</span></span>
* [<span data-ttu-id="e9049-127">Anpassade avbildningar eller formler?</span><span class="sxs-lookup"><span data-stu-id="e9049-127">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="e9049-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9049-128">Next steps</span></span>
- [<span data-ttu-id="e9049-129">DevTest Labs vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="e9049-129">DevTest Labs FAQ</span></span>](devtest-lab-faq.md)