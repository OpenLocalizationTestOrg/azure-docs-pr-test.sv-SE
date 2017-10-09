---
title: aaaComparing anpassade avbildningar och formler i DevTest Labs | Microsoft Docs
description: "Läs mer om hello skillnaderna mellan anpassade avbildningar och formler som VM baserar så att du kan bestämma vilket som passar din miljö."
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
ms.openlocfilehash: 3c1d88dfe0ff94b8e825bb7a0b4aca3341c9330d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a><span data-ttu-id="81386-103">Jämföra anpassade avbildningar och formler i DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="81386-103">Comparing custom images and formulas in DevTest Labs</span></span>
<span data-ttu-id="81386-104">Båda [anpassade avbildningar](devtest-lab-create-template.md) och [formler](devtest-lab-manage-formulas.md) kan användas som grund för [skapa nya virtuella datorer](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="81386-104">Both [custom images](devtest-lab-create-template.md) and [formulas](devtest-lab-manage-formulas.md) can be used as bases for [created new VMs](devtest-lab-add-vm-with-artifacts.md).</span></span> <span data-ttu-id="81386-105">Hello viktiga åtskillnad mellan anpassade avbildningar och formler är dock att en anpassad avbildning är helt enkelt en avbildning baserat på en virtuell Hårddisk, medan en formel är en avbildning baserat på en virtuell Hårddisk *förutom* förkonfigurerade inställningar – till exempel VM-storlek, virtuella nätverk undernätet och artefakter.</span><span class="sxs-lookup"><span data-stu-id="81386-105">However, hello key distinction between custom images and formulas is that a custom image is simply an image based on a VHD, while a formula is an image based on a VHD *in addition to* preconfigured settings - such as VM Size, virtual network, subnet, and artifacts.</span></span> <span data-ttu-id="81386-106">Inställningarna förinställda ställs in med standardvärden som kan åsidosättas när hello dags att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="81386-106">These preconfigured settings are set up with default values that can be overridden at hello time of VM creation.</span></span> <span data-ttu-id="81386-107">Den här artikeln förklarar några av fördelarna hello (tekniker) och nackdelar (nackdelar) toousing anpassade avbildningar jämfört med formler.</span><span class="sxs-lookup"><span data-stu-id="81386-107">This article explains some of hello advantages (pros) and disadvantages (cons) toousing custom images versus using formulas.</span></span>

## <a name="custom-image-pros-and-cons"></a><span data-ttu-id="81386-108">Anpassad bild- och nackdelar</span><span class="sxs-lookup"><span data-stu-id="81386-108">Custom image pros and cons</span></span>
<span data-ttu-id="81386-109">Anpassade avbildningar innehåller en statisk, ändras sätt toocreate virtuella datorer från en önskad miljö.</span><span class="sxs-lookup"><span data-stu-id="81386-109">Custom images provide a static, immutable way toocreate VMs from a desired environment.</span></span> 

<span data-ttu-id="81386-110">**Tekniker**</span><span class="sxs-lookup"><span data-stu-id="81386-110">**Pros**</span></span>

* <span data-ttu-id="81386-111">Etablering från en anpassad avbildning av virtuell dator är snabb eftersom inget ändras efter hello VM är de från hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="81386-111">VM provisioning from a custom image is fast as nothing changes after hello VM is spun up from hello image.</span></span> <span data-ttu-id="81386-112">Det finns med andra ord inga inställningar tooapply som hello anpassad avbildning är en bild utan inställningar.</span><span class="sxs-lookup"><span data-stu-id="81386-112">In other words, there are no settings tooapply as hello custom image is just an image without settings.</span></span> 
* <span data-ttu-id="81386-113">Virtuella datorer skapas från en anpassad bild är identiska.</span><span class="sxs-lookup"><span data-stu-id="81386-113">VMs created from a single custom image are identical.</span></span>

<span data-ttu-id="81386-114">**Nackdelar**</span><span class="sxs-lookup"><span data-stu-id="81386-114">**Cons**</span></span>

* <span data-ttu-id="81386-115">Om du behöver tooupdate någon aspekt av hello anpassad avbildning måste du återskapa hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="81386-115">If you need tooupdate some aspect of hello custom image, hello image must be recreated.</span></span>  

## <a name="formula-pros-and-cons"></a><span data-ttu-id="81386-116">Formeln- och nackdelar</span><span class="sxs-lookup"><span data-stu-id="81386-116">Formula pros and cons</span></span>
<span data-ttu-id="81386-117">Formler innehåller ett dynamiskt sätt toocreate virtuella datorer från hello önskad konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="81386-117">Formulas provide a dynamic way toocreate VMs from hello desired configuration/settings.</span></span>

<span data-ttu-id="81386-118">**Tekniker**</span><span class="sxs-lookup"><span data-stu-id="81386-118">**Pros**</span></span>

* <span data-ttu-id="81386-119">Ändringar i hello miljö kan hämtas på hello direkt via artefakter.</span><span class="sxs-lookup"><span data-stu-id="81386-119">Changes in hello environment can be captured on hello fly via artifacts.</span></span> <span data-ttu-id="81386-120">Till exempel om du vill att en virtuell dator som installerats med hello senaste bits från din pipeline versionen eller registrera hello senaste koden från din lagringsplats du bara ange en artefakt som distribuerar hello senaste bitar eller anlitar hello senaste koden i hello formeln tillsammans med en mål basavbildning.</span><span class="sxs-lookup"><span data-stu-id="81386-120">For example, if you want a VM installed with hello latest bits from your release pipeline or enlist hello latest code from your repository, you can simply specify an artifact that deploys hello latest bits or enlists hello latest code in hello formula together with a target base image.</span></span> <span data-ttu-id="81386-121">När den här formeln används toocreate virtuella datorer är hello senaste bits/kod distribueras/registrerad toohello VM.</span><span class="sxs-lookup"><span data-stu-id="81386-121">Whenever this formula is used toocreate VMs, hello latest bits/code are deployed/enlisted toohello VM.</span></span> 
* <span data-ttu-id="81386-122">Formler kan definiera standardinställningar som anpassade avbildningar inte kan tillhandahålla - som VM-storlekar och inställningarna för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="81386-122">Formulas can define default settings that custom images cannot provide - such as VM sizes and virtual network settings.</span></span> 
* <span data-ttu-id="81386-123">hello inställningarna sparas i en formel som standardvärden, men kan ändras när hello VM skapas.</span><span class="sxs-lookup"><span data-stu-id="81386-123">hello settings saved in a formula are shown as default values, but can be modified when hello VM is created.</span></span> 

<span data-ttu-id="81386-124">**Nackdelar**</span><span class="sxs-lookup"><span data-stu-id="81386-124">**Cons**</span></span>

* <span data-ttu-id="81386-125">Skapa en virtuell dator från en formel kan det ta längre tid än att skapa en virtuell dator från en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="81386-125">Creating a VM from a formula can take more time than creating a VM from a custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="81386-126">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="81386-126">Related blog posts</span></span>
* [<span data-ttu-id="81386-127">Anpassade avbildningar eller formler?</span><span class="sxs-lookup"><span data-stu-id="81386-127">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="81386-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81386-128">Next steps</span></span>
- [<span data-ttu-id="81386-129">DevTest Labs vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="81386-129">DevTest Labs FAQ</span></span>](devtest-lab-faq.md)