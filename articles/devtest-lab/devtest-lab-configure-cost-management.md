---
title: "aaaView hello månatliga beräknade labbkostnaden kostnadstrend i Azure DevTest Labs | Microsoft Docs"
description: "Läs mer om hello Azure DevTest Labs månatliga uppskattade kostnaden trenddiagram."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 47c7dd4cf91b76de74b502c50f05e79cd501ee35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-hello-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a><span data-ttu-id="a5e47-103">Visa hello månatliga beräknade labbkostnaden kostnadstrend i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="a5e47-103">View hello monthly estimated lab cost trend in Azure DevTest Labs</span></span>
<span data-ttu-id="a5e47-104">hanteringsfunktionen för hello kostnaden för DevTest Labs hjälper dig att spåra hello kostnaden för ditt labb.</span><span class="sxs-lookup"><span data-stu-id="a5e47-104">hello Cost Management feature of DevTest Labs helps you track hello cost of your lab.</span></span> <span data-ttu-id="a5e47-105">Den här artikeln beskrivs hur toouse hello **månatlig uppskattad Kostnadstrend** diagram tooview hello aktuella kalendermånaden uppskattade kostnaden-till-date och hello planerade sista månad kostnaden för hello aktuella kalendermånaden.</span><span class="sxs-lookup"><span data-stu-id="a5e47-105">This article illustrates how toouse hello **Monthly Estimated Cost Trend** chart tooview hello current calendar month's estimated cost-to-date and hello projected end-of-month cost for hello current calendar month.</span></span> <span data-ttu-id="a5e47-106">I den här artikeln lär du dig hur tooview hello månatliga uppskattade kostnaden trenddiagram i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a5e47-106">In this article, you learn how tooview hello monthly estimated cost trend chart in hello Azure portal.</span></span>

## <a name="viewing-hello-monthly-estimated-cost-trend-chart"></a><span data-ttu-id="a5e47-107">Visar hello månatlig uppskattad Kostnadstrend diagram</span><span class="sxs-lookup"><span data-stu-id="a5e47-107">Viewing hello Monthly Estimated Cost Trend chart</span></span>
<span data-ttu-id="a5e47-108">tooview hello månatlig uppskattad Kostnadstrend diagram, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="a5e47-108">tooview hello Monthly Estimated Cost Trend chart, follow these steps:</span></span> 

1. <span data-ttu-id="a5e47-109">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="a5e47-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="a5e47-110">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="a5e47-110">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="a5e47-111">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="a5e47-111">From hello list of labs, select hello desired lab.</span></span>   
4. <span data-ttu-id="a5e47-112">På bladet hello lab väljer **kostnad inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a5e47-112">On hello lab's blade, select **Cost settings**.</span></span>
5. <span data-ttu-id="a5e47-113">På hello lab **kostnad inställningar** bladet väljer **Lab kostnadstrend**.</span><span class="sxs-lookup"><span data-stu-id="a5e47-113">On hello lab's **Cost settings** blade, select **Lab cost trend**.</span></span>
6. <span data-ttu-id="a5e47-114">hello visar följande skärmbild ett exempel på ett diagram med kostnaden.</span><span class="sxs-lookup"><span data-stu-id="a5e47-114">hello following screen shot shows an example of a cost chart.</span></span> 
   
    ![Kostnad diagram](./media/devtest-lab-configure-cost-management/graph.png)

<span data-ttu-id="a5e47-116">Hej **uppskattade kostnaden** värde är hello aktuella kalendermånaden uppskattade kostnaden hittills.</span><span class="sxs-lookup"><span data-stu-id="a5e47-116">hello **Estimated cost** value is hello current calendar month's estimated cost-to-date.</span></span> <span data-ttu-id="a5e47-117">Hej **planerad kostnad** hello uppskattade kostnaden för hela hello aktuella kalendermånaden beräknas med hjälp av hello lab kostnaden för hello tidigare fem dagar.</span><span class="sxs-lookup"><span data-stu-id="a5e47-117">hello **Projected cost** is hello estimated cost for hello entire current calendar month, calculated using hello lab cost for hello previous five days.</span></span>

<span data-ttu-id="a5e47-118">hello kostnadsbelopp avrundas toohello närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="a5e47-118">hello cost amounts are rounded up toohello next whole number.</span></span> <span data-ttu-id="a5e47-119">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a5e47-119">For example:</span></span> 

* <span data-ttu-id="a5e47-120">5.01 Avrundar uppåt too6</span><span class="sxs-lookup"><span data-stu-id="a5e47-120">5.01 rounds up too6</span></span> 
* <span data-ttu-id="a5e47-121">5.50 Avrundar uppåt too6</span><span class="sxs-lookup"><span data-stu-id="a5e47-121">5.50 rounds up too6</span></span>
* <span data-ttu-id="a5e47-122">5.99 Avrundar uppåt too6</span><span class="sxs-lookup"><span data-stu-id="a5e47-122">5.99 rounds up too6</span></span>

<span data-ttu-id="a5e47-123">Eftersom den anger över hello diagram hello kostnader som du ser i diagrammet hello är *uppskattade* kostnader med [betala per användning](https://azure.microsoft.com/offers/ms-azr-0003p/) erbjuda priser.</span><span class="sxs-lookup"><span data-stu-id="a5e47-123">As it states above hello chart, hello costs you see in hello chart are *estimated* costs using [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) offer rates.</span></span>
<span data-ttu-id="a5e47-124">Dessutom hello följande är *inte* med i beräkningen hello:</span><span class="sxs-lookup"><span data-stu-id="a5e47-124">Additionally, hello following are *not* included in hello cost calculation:</span></span>

* <span data-ttu-id="a5e47-125">CSP och Dreamspark-prenumerationer stöds inte för närvarande eftersom Azure DevTest Labs använder hello [Azure fakturering API: er](../billing/billing-usage-rate-card-overview.md) toocalculate hello lab kostnad, som inte stöder CSP eller Dreamspark-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="a5e47-125">CSP and Dreamspark subscriptions are currently not supported as Azure DevTest Labs uses hello [Azure billing APIs](../billing/billing-usage-rate-card-overview.md) toocalculate hello lab cost, which does not support CSP or Dreamspark subscriptions.</span></span>
* <span data-ttu-id="a5e47-126">Allteftersom-taxan.</span><span class="sxs-lookup"><span data-stu-id="a5e47-126">Your offer rates.</span></span> <span data-ttu-id="a5e47-127">Vi är för närvarande inte kan toouse din allteftersom-taxan (visas i din prenumeration) att du har förhandlats med Microsoft eller Microsoft partner.</span><span class="sxs-lookup"><span data-stu-id="a5e47-127">Currently, we are not able toouse your offer rates (shown under your subscription) that you have negotiated with Microsoft or Microsoft partners.</span></span> <span data-ttu-id="a5e47-128">Vi använder betala per användning priser.</span><span class="sxs-lookup"><span data-stu-id="a5e47-128">We are using Pay-As-You-Go rates.</span></span>
* <span data-ttu-id="a5e47-129">Din skatter</span><span class="sxs-lookup"><span data-stu-id="a5e47-129">Your taxes</span></span>
* <span data-ttu-id="a5e47-130">Din rabatt</span><span class="sxs-lookup"><span data-stu-id="a5e47-130">Your discounts</span></span>
* <span data-ttu-id="a5e47-131">Valuta.</span><span class="sxs-lookup"><span data-stu-id="a5e47-131">Your billing currency.</span></span> <span data-ttu-id="a5e47-132">För närvarande visas hello lab kostnaden endast i USD valuta.</span><span class="sxs-lookup"><span data-stu-id="a5e47-132">Currently, hello lab cost is displayed only in USD currency.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="a5e47-133">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="a5e47-133">Related blog posts</span></span>
* [<span data-ttu-id="a5e47-134">Två flera saker tookeep dina kostnader på spår i DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="a5e47-134">Two more things tookeep your cost on track in DevTest Labs</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [<span data-ttu-id="a5e47-135">Varför kostnad tröskelvärden?</span><span class="sxs-lookup"><span data-stu-id="a5e47-135">Why Cost Thresholds?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a><span data-ttu-id="a5e47-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5e47-136">Next steps</span></span>
<span data-ttu-id="a5e47-137">Här följer några saker tootry bredvid:</span><span class="sxs-lookup"><span data-stu-id="a5e47-137">Here are some things tootry next:</span></span>

* <span data-ttu-id="a5e47-138">[Definiera principer för labbet](devtest-lab-set-lab-policy.md) – Lär dig hur tooset hello olika principer används toogovern hur ditt labb och dess virtuella datorer används.</span><span class="sxs-lookup"><span data-stu-id="a5e47-138">[Define lab policies](devtest-lab-set-lab-policy.md) - Learn how tooset hello various policies used toogovern how your lab and its VMs are used.</span></span> 
* <span data-ttu-id="a5e47-139">[Skapa den anpassade bilden](devtest-lab-create-template.md) – när du skapar en virtuell dator, anger du en bas som kan vara antingen en anpassad avbildning eller en Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="a5e47-139">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="a5e47-140">Den här artikeln visar hur toocreate en anpassad avbildning från en VHD-fil.</span><span class="sxs-lookup"><span data-stu-id="a5e47-140">This article illustrates how toocreate a custom image from a VHD file.</span></span>
* <span data-ttu-id="a5e47-141">[Konfigurera Marketplace-bilder](devtest-lab-configure-marketplace-images.md) - DevTest Labs stöder skapandet av virtuella datorer baserat på Azure Marketplace-bilder.</span><span class="sxs-lookup"><span data-stu-id="a5e47-141">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="a5e47-142">Den här artikeln beskrivs hur toospecify som eventuellt Azure Marketplace-bilder kan vara används när du skapar virtuella datorer i ett labb.</span><span class="sxs-lookup"><span data-stu-id="a5e47-142">This article illustrates how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="a5e47-143">[Skapa en virtuell dator i ett labb](devtest-lab-add-vm-with-artifacts.md) -illustrerar hur toocreate en virtuell dator från en grundläggande bild (antingen anpassad eller Marketplace), och hur toowork med artefakter i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a5e47-143">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how toocreate a VM from a base image (either custom or Marketplace), and how toowork with artifacts in your VM.</span></span>

