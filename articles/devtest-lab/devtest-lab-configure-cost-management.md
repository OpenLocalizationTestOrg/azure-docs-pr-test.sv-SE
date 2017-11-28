---
title: "Visa de månatliga kostnadstrend beräknade labbkostnaden i Azure DevTest Labs | Microsoft Docs"
description: "Läs mer om Azure DevTest Labs månatliga uppskattade kostnaden trend diagrammet."
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
ms.openlocfilehash: b3ad1ead522908d4b41b7cca98d20ac91664998e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a><span data-ttu-id="81670-103">Visa de månatliga kostnadstrend beräknade labbkostnaden i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="81670-103">View the monthly estimated lab cost trend in Azure DevTest Labs</span></span>
<span data-ttu-id="81670-104">Funktionen hantering av kostnaden för DevTest Labs hjälper dig att spåra kostnaden för ditt labb.</span><span class="sxs-lookup"><span data-stu-id="81670-104">The Cost Management feature of DevTest Labs helps you track the cost of your lab.</span></span> <span data-ttu-id="81670-105">Den här artikeln beskrivs hur du använder den **månatlig uppskattad Kostnadstrend** diagram om du vill visa den aktuella kalendermånaden uppskattade kostnaden-till-date och planerade sista månad kostnaden för den aktuella kalendermånaden.</span><span class="sxs-lookup"><span data-stu-id="81670-105">This article illustrates how to use the **Monthly Estimated Cost Trend** chart to view the current calendar month's estimated cost-to-date and the projected end-of-month cost for the current calendar month.</span></span> <span data-ttu-id="81670-106">I den här artikeln får lära du att visa diagrammet trend månatliga uppskattade kostnaden i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="81670-106">In this article, you learn how to view the monthly estimated cost trend chart in the Azure portal.</span></span>

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a><span data-ttu-id="81670-107">Visa diagrammet månatlig uppskattad Kostnadstrend</span><span class="sxs-lookup"><span data-stu-id="81670-107">Viewing the Monthly Estimated Cost Trend chart</span></span>
<span data-ttu-id="81670-108">Följ dessa steg om du vill visa månatlig uppskattad Kostnadstrend diagrammet:</span><span class="sxs-lookup"><span data-stu-id="81670-108">To view the Monthly Estimated Cost Trend chart, follow these steps:</span></span> 

1. <span data-ttu-id="81670-109">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="81670-109">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="81670-110">Välj **fler tjänster**, och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="81670-110">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="81670-111">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="81670-111">From the list of labs, select the desired lab.</span></span>   
4. <span data-ttu-id="81670-112">På den testmiljön bladet välj **kostnad inställningar**.</span><span class="sxs-lookup"><span data-stu-id="81670-112">On the lab's blade, select **Cost settings**.</span></span>
5. <span data-ttu-id="81670-113">På testmiljön **kostnad inställningar** bladet väljer **Lab kostnadstrend**.</span><span class="sxs-lookup"><span data-stu-id="81670-113">On the lab's **Cost settings** blade, select **Lab cost trend**.</span></span>
6. <span data-ttu-id="81670-114">Följande skärmbild visar ett exempel på ett diagram med kostnaden.</span><span class="sxs-lookup"><span data-stu-id="81670-114">The following screen shot shows an example of a cost chart.</span></span> 
   
    ![Kostnad diagram](./media/devtest-lab-configure-cost-management/graph.png)

<span data-ttu-id="81670-116">Den **uppskattade kostnaden** värdet är den aktuella kalendermånaden uppskattade kostnaden hittills.</span><span class="sxs-lookup"><span data-stu-id="81670-116">The **Estimated cost** value is the current calendar month's estimated cost-to-date.</span></span> <span data-ttu-id="81670-117">Den **planerad kostnad** är uppskattade kostnaden för hela aktuell kalendermånad, beräknas med hjälp av lab kostnaden för föregående fem dagar.</span><span class="sxs-lookup"><span data-stu-id="81670-117">The **Projected cost** is the estimated cost for the entire current calendar month, calculated using the lab cost for the previous five days.</span></span>

<span data-ttu-id="81670-118">Kostnadsbelopp som avrundat till närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="81670-118">The cost amounts are rounded up to the next whole number.</span></span> <span data-ttu-id="81670-119">Exempel:</span><span class="sxs-lookup"><span data-stu-id="81670-119">For example:</span></span> 

* <span data-ttu-id="81670-120">5.01 Avrundar upp till 6</span><span class="sxs-lookup"><span data-stu-id="81670-120">5.01 rounds up to 6</span></span> 
* <span data-ttu-id="81670-121">5.50 Avrundar upp till 6</span><span class="sxs-lookup"><span data-stu-id="81670-121">5.50 rounds up to 6</span></span>
* <span data-ttu-id="81670-122">5.99 Avrundar upp till 6</span><span class="sxs-lookup"><span data-stu-id="81670-122">5.99 rounds up to 6</span></span>

<span data-ttu-id="81670-123">Eftersom den anger ovanför diagrammet kostnaderna som du ser i diagrammet är *uppskattade* kostnader med [betala per användning](https://azure.microsoft.com/offers/ms-azr-0003p/) erbjuda priser.</span><span class="sxs-lookup"><span data-stu-id="81670-123">As it states above the chart, the costs you see in the chart are *estimated* costs using [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) offer rates.</span></span>
<span data-ttu-id="81670-124">Dessutom kan följande är *inte* med i beräkningen:</span><span class="sxs-lookup"><span data-stu-id="81670-124">Additionally, the following are *not* included in the cost calculation:</span></span>

* <span data-ttu-id="81670-125">CSP och Dreamspark-prenumerationer stöds inte för närvarande eftersom Azure DevTest Labs använder den [Azure fakturering API: er](../billing/billing-usage-rate-card-overview.md) att beräkna labbet kostnad, som inte stöder CSP eller Dreamspark-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="81670-125">CSP and Dreamspark subscriptions are currently not supported as Azure DevTest Labs uses the [Azure billing APIs](../billing/billing-usage-rate-card-overview.md) to calculate the lab cost, which does not support CSP or Dreamspark subscriptions.</span></span>
* <span data-ttu-id="81670-126">Allteftersom-taxan.</span><span class="sxs-lookup"><span data-stu-id="81670-126">Your offer rates.</span></span> <span data-ttu-id="81670-127">Vi är för närvarande inte kan använda din allteftersom-taxan (visas i din prenumeration) att du har förhandlats med Microsoft eller Microsoft partner.</span><span class="sxs-lookup"><span data-stu-id="81670-127">Currently, we are not able to use your offer rates (shown under your subscription) that you have negotiated with Microsoft or Microsoft partners.</span></span> <span data-ttu-id="81670-128">Vi använder betala per användning priser.</span><span class="sxs-lookup"><span data-stu-id="81670-128">We are using Pay-As-You-Go rates.</span></span>
* <span data-ttu-id="81670-129">Din skatter</span><span class="sxs-lookup"><span data-stu-id="81670-129">Your taxes</span></span>
* <span data-ttu-id="81670-130">Din rabatt</span><span class="sxs-lookup"><span data-stu-id="81670-130">Your discounts</span></span>
* <span data-ttu-id="81670-131">Valuta.</span><span class="sxs-lookup"><span data-stu-id="81670-131">Your billing currency.</span></span> <span data-ttu-id="81670-132">För närvarande visas lab kostnaden bara i USD valuta.</span><span class="sxs-lookup"><span data-stu-id="81670-132">Currently, the lab cost is displayed only in USD currency.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="81670-133">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="81670-133">Related blog posts</span></span>
* [<span data-ttu-id="81670-134">Två saker att hålla dina kostnader på Spåra i DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="81670-134">Two more things to keep your cost on track in DevTest Labs</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [<span data-ttu-id="81670-135">Varför kostnad tröskelvärden?</span><span class="sxs-lookup"><span data-stu-id="81670-135">Why Cost Thresholds?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a><span data-ttu-id="81670-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81670-136">Next steps</span></span>
<span data-ttu-id="81670-137">Här följer några saker att försöka:</span><span class="sxs-lookup"><span data-stu-id="81670-137">Here are some things to try next:</span></span>

* <span data-ttu-id="81670-138">[Definiera principer för labbet](devtest-lab-set-lab-policy.md) – Lär dig att ange olika principer som används för att styra hur ditt labb och dess virtuella datorer används.</span><span class="sxs-lookup"><span data-stu-id="81670-138">[Define lab policies](devtest-lab-set-lab-policy.md) - Learn how to set the various policies used to govern how your lab and its VMs are used.</span></span> 
* <span data-ttu-id="81670-139">[Skapa den anpassade bilden](devtest-lab-create-template.md) – när du skapar en virtuell dator, anger du en bas som kan vara antingen en anpassad avbildning eller en Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="81670-139">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="81670-140">Den här artikeln beskrivs hur du skapar en anpassad avbildning från en VHD-fil.</span><span class="sxs-lookup"><span data-stu-id="81670-140">This article illustrates how to create a custom image from a VHD file.</span></span>
* <span data-ttu-id="81670-141">[Konfigurera Marketplace-bilder](devtest-lab-configure-marketplace-images.md) - DevTest Labs stöder skapandet av virtuella datorer baserat på Azure Marketplace-bilder.</span><span class="sxs-lookup"><span data-stu-id="81670-141">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="81670-142">Den här artikeln visar hur du kan ange vilka eventuella Azure Marketplace-bilder kan användas när du skapar virtuella datorer i ett labb.</span><span class="sxs-lookup"><span data-stu-id="81670-142">This article illustrates how to specify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="81670-143">[Skapa en virtuell dator i ett labb](devtest-lab-add-vm-with-artifacts.md) -illustrerar hur du skapar en virtuell dator från en grundläggande bild (antingen anpassad eller Marketplace), och hur du arbetar med artefakter i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="81670-143">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how to create a VM from a base image (either custom or Marketplace), and how to work with artifacts in your VM.</span></span>

