---
title: "Skala kvoter och gränser i ditt labb i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur du skala ett labb i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: tarcher
ms.openlocfilehash: f11ed42b474e4f208eac92689bfa33fb188d15a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a><span data-ttu-id="50684-103">Skala kvoter och gränser i DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="50684-103">Scale quotas and limits in DevTest Labs</span></span>
<span data-ttu-id="50684-104">När du arbetar i DevTest Labs märker du att det inte finns några Azure-resurser, vilket kan påverka tjänsten DevTest Labs vissa standardgränser.</span><span class="sxs-lookup"><span data-stu-id="50684-104">As you work in DevTest Labs, you might notice that there are certain default limits to some Azure resources, which can affect the DevTest Labs service.</span></span> <span data-ttu-id="50684-105">Dessa gränser kallas **kvoter**.</span><span class="sxs-lookup"><span data-stu-id="50684-105">These limits are referred to as **quotas**.</span></span>

> [!NOTE]
> <span data-ttu-id="50684-106">Tjänsten DevTest Labs införa inte kvoter.</span><span class="sxs-lookup"><span data-stu-id="50684-106">The DevTest Labs service doesn't impose any quotas.</span></span> <span data-ttu-id="50684-107">Kvoter som kan uppstå är standardbegränsningar av övergripande Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="50684-107">Any quotas you might encounter are default constraints of the overall Azure subscription.</span></span>

<span data-ttu-id="50684-108">Du kan använda varje Azure-resurs tills du når sin kvot.</span><span class="sxs-lookup"><span data-stu-id="50684-108">You can use each Azure resource until you reach its quota.</span></span> <span data-ttu-id="50684-109">Varje prenumeration har separata kvoter och användning spåras per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="50684-109">Each subscription has separate quotas and usage is tracked per subscription.</span></span>

<span data-ttu-id="50684-110">Varje prenumeration har till exempel en standardkvot på 20 kärnor.</span><span class="sxs-lookup"><span data-stu-id="50684-110">For example, each subscription has a default quota of 20 cores.</span></span> <span data-ttu-id="50684-111">Så om du skapar virtuella datorer i labbet med fyra kärnor, kan du bara skapa fem virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="50684-111">So, if you are creating VMs in your lab with four cores each, then you can only create five VMs.</span></span> 

<span data-ttu-id="50684-112">[Azure-prenumeration och Tjänstbegränsningarna](https://docs.microsoft.com/azure/azure-subscription-service-limits) listar några av de vanligaste kvoterna för Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="50684-112">[Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits) lists some of the most common quotas for Azure resources.</span></span> <span data-ttu-id="50684-113">Resurserna som används mest i ett labb och för vilket du kan stöta på kvoter, innehåller VM kärnor, offentliga IP-adresser, gränssnitt, hanterade diskar, RBAC rolltilldelning och ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="50684-113">The resources most commonly used in a lab, and for which you might encounter quotas, include VM cores, public IP addresses, network interface, managed disks, RBAC role assignment, and ExpressRoute circuits.</span></span>

## <a name="view-your-usage-and-quotas"></a><span data-ttu-id="50684-114">Visa användnings- och kvoter</span><span class="sxs-lookup"><span data-stu-id="50684-114">View your usage and quotas</span></span>
<span data-ttu-id="50684-115">Dessa steg visar hur att visa de aktuella kvoterna i din prenumeration för specifika Azure-resurser och se hur många procent av varje kvot som du har använt.</span><span class="sxs-lookup"><span data-stu-id="50684-115">These steps show you how to view the current quotas in your subscription for specific Azure resources, and to see what percentage of each quota you have used.</span></span>

1. <span data-ttu-id="50684-116">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="50684-116">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="50684-117">Välj **fler tjänster**, och välj sedan **fakturering** från listan.</span><span class="sxs-lookup"><span data-stu-id="50684-117">Select **More Services**, and then select **Billing** from the list.</span></span>
1. <span data-ttu-id="50684-118">Välj en prenumeration i bladet fakturering.</span><span class="sxs-lookup"><span data-stu-id="50684-118">In the Billing blade, select a subscription.</span></span>
4. <span data-ttu-id="50684-119">Välj **användning + kvoter**.</span><span class="sxs-lookup"><span data-stu-id="50684-119">Select **Usage + quotas**.</span></span>

   ![Knappen användning och kvoter](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   <span data-ttu-id="50684-121">Användning + kvoter bladet visas, lista över olika resurser som är tillgängliga i den prenumerationen och procentandelen kvoten som används per resurs.</span><span class="sxs-lookup"><span data-stu-id="50684-121">The Usage + quotas blade appears, listing different resources available in that subscription and the percentage of the quota that is being used per resource.</span></span>

   ![Kvoter och användning](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a><span data-ttu-id="50684-123">Begär fler resurser i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="50684-123">Requesting more resources in your subscription</span></span>
<span data-ttu-id="50684-124">Om du når ett kvoten tak Standardgränsen för en resurs i en prenumeration kan du öka upp till en övre gräns, enligt beskrivningen i [Azure-prenumeration och Tjänstbegränsningarna](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span><span class="sxs-lookup"><span data-stu-id="50684-124">If you reach a quota cap, the default limit of a resource in a subscription can be increased up to a maximum limit, as described in [Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span></span>

<span data-ttu-id="50684-125">Dessa steg visar hur du begära en ökad kvot via den [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="50684-125">These steps show you how to request a quota increase through the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="50684-126">Välj **fler tjänster**väljer **fakturering**, och välj sedan **användning + kvoter**.</span><span class="sxs-lookup"><span data-stu-id="50684-126">Select **More Services**, select **Billing**, and then select **Usage + quotas**.</span></span>
1. <span data-ttu-id="50684-127">I användning + kvoter bladet väljer den **begära öka** knappen.</span><span class="sxs-lookup"><span data-stu-id="50684-127">In the Usage + quotas blade, select the **Request Increase** button.</span></span>

   ![Knappen för ökning av begäran](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. <span data-ttu-id="50684-129">För att slutföra och skicka begäran fyller i informationen som krävs på de tre flikarna i den **ny supportbegäran** formuläret.</span><span class="sxs-lookup"><span data-stu-id="50684-129">To complete and submit the request, fill out the required information on all three tabs of the **New support request** form.</span></span>

   ![Öka formulär](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

<span data-ttu-id="50684-131">[Förstå Azure gränser och ökar](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) innehåller mer information om att kontakta Azure-supporten om du vill begära en ökad kvot.</span><span class="sxs-lookup"><span data-stu-id="50684-131">[Understanding Azure Limits and Increases](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) provides more information about contacting Azure support to request a quota increase.</span></span>



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a><span data-ttu-id="50684-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50684-132">Next steps</span></span>
* <span data-ttu-id="50684-133">Utforska den [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="50684-133">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span></span>
