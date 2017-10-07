---
title: "aaaScale kvoter och gränser i ditt labb i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur tooscale ett labb i Azure DevTest Labs"
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
ms.openlocfilehash: 7fb429c0aabdfbce3a4a5aa6d9259daa2ee270d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a><span data-ttu-id="6374e-103">Skala kvoter och gränser i DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="6374e-103">Scale quotas and limits in DevTest Labs</span></span>
<span data-ttu-id="6374e-104">När du arbetar i DevTest Labs märker du att det finns vissa standard gränser toosome Azure-resurser, vilket kan påverka hello DevTest Labs service.</span><span class="sxs-lookup"><span data-stu-id="6374e-104">As you work in DevTest Labs, you might notice that there are certain default limits toosome Azure resources, which can affect hello DevTest Labs service.</span></span> <span data-ttu-id="6374e-105">Dessa gränser är refererad tooas **kvoter**.</span><span class="sxs-lookup"><span data-stu-id="6374e-105">These limits are referred tooas **quotas**.</span></span>

> [!NOTE]
> <span data-ttu-id="6374e-106">Hej DevTest Labs service införa inte kvoter.</span><span class="sxs-lookup"><span data-stu-id="6374e-106">hello DevTest Labs service doesn't impose any quotas.</span></span> <span data-ttu-id="6374e-107">Kvoter som kan uppstå är standardbegränsningar av hello övergripande Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6374e-107">Any quotas you might encounter are default constraints of hello overall Azure subscription.</span></span>

<span data-ttu-id="6374e-108">Du kan använda varje Azure-resurs tills du når sin kvot.</span><span class="sxs-lookup"><span data-stu-id="6374e-108">You can use each Azure resource until you reach its quota.</span></span> <span data-ttu-id="6374e-109">Varje prenumeration har separata kvoter och användning spåras per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6374e-109">Each subscription has separate quotas and usage is tracked per subscription.</span></span>

<span data-ttu-id="6374e-110">Varje prenumeration har till exempel en standardkvot på 20 kärnor.</span><span class="sxs-lookup"><span data-stu-id="6374e-110">For example, each subscription has a default quota of 20 cores.</span></span> <span data-ttu-id="6374e-111">Så om du skapar virtuella datorer i labbet med fyra kärnor, kan du bara skapa fem virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6374e-111">So, if you are creating VMs in your lab with four cores each, then you can only create five VMs.</span></span> 

<span data-ttu-id="6374e-112">[Azure-prenumeration och Tjänstbegränsningarna](https://docs.microsoft.com/azure/azure-subscription-service-limits) listar några av de vanligaste hello-kvoter för Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="6374e-112">[Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits) lists some of hello most common quotas for Azure resources.</span></span> <span data-ttu-id="6374e-113">hello resurser som används mest i ett labb och för vilket du kan stöta på kvoter, inkludera VM kärnor, offentliga IP-adresser, gränssnitt, hanterade diskar, RBAC rolltilldelning och ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="6374e-113">hello resources most commonly used in a lab, and for which you might encounter quotas, include VM cores, public IP addresses, network interface, managed disks, RBAC role assignment, and ExpressRoute circuits.</span></span>

## <a name="view-your-usage-and-quotas"></a><span data-ttu-id="6374e-114">Visa användnings- och kvoter</span><span class="sxs-lookup"><span data-stu-id="6374e-114">View your usage and quotas</span></span>
<span data-ttu-id="6374e-115">Dessa steg visar hur tooview hello aktuella kvoter i din prenumeration för specifika Azure-resurser och toosee vilken procentandel av varje kvot som du har använt.</span><span class="sxs-lookup"><span data-stu-id="6374e-115">These steps show you how tooview hello current quotas in your subscription for specific Azure resources, and toosee what percentage of each quota you have used.</span></span>

1. <span data-ttu-id="6374e-116">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="6374e-116">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="6374e-117">Välj **fler tjänster**, och välj sedan **fakturering** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="6374e-117">Select **More Services**, and then select **Billing** from hello list.</span></span>
1. <span data-ttu-id="6374e-118">Välj en prenumeration i hello fakturerings-bladet.</span><span class="sxs-lookup"><span data-stu-id="6374e-118">In hello Billing blade, select a subscription.</span></span>
4. <span data-ttu-id="6374e-119">Välj **användning + kvoter**.</span><span class="sxs-lookup"><span data-stu-id="6374e-119">Select **Usage + quotas**.</span></span>

   ![Knappen användning och kvoter](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   <span data-ttu-id="6374e-121">Hej användning + kvoter bladet visas med olika resurser som är tillgängliga i den prenumerationen och hello procentandelen hello kvot som används per resurs.</span><span class="sxs-lookup"><span data-stu-id="6374e-121">hello Usage + quotas blade appears, listing different resources available in that subscription and hello percentage of hello quota that is being used per resource.</span></span>

   ![Kvoter och användning](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a><span data-ttu-id="6374e-123">Begär fler resurser i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="6374e-123">Requesting more resources in your subscription</span></span>
<span data-ttu-id="6374e-124">Om du når ett kvoten tak hello Standardgränsen för en resurs i en prenumeration kan du öka upp tooa gränsvärdet, enligt beskrivningen i [Azure-prenumeration och Tjänstbegränsningarna](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span><span class="sxs-lookup"><span data-stu-id="6374e-124">If you reach a quota cap, hello default limit of a resource in a subscription can be increased up tooa maximum limit, as described in [Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span></span>

<span data-ttu-id="6374e-125">Dessa steg visar hur toorequest en kvot öka via hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="6374e-125">These steps show you how toorequest a quota increase through hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="6374e-126">Välj **fler tjänster**väljer **fakturering**, och välj sedan **användning + kvoter**.</span><span class="sxs-lookup"><span data-stu-id="6374e-126">Select **More Services**, select **Billing**, and then select **Usage + quotas**.</span></span>
1. <span data-ttu-id="6374e-127">Välj hello i hello användning + kvoter bladet **begära öka** knappen.</span><span class="sxs-lookup"><span data-stu-id="6374e-127">In hello Usage + quotas blade, select hello **Request Increase** button.</span></span>

   ![Knappen för ökning av begäran](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. <span data-ttu-id="6374e-129">toocomplete och skicka begäran om hello, fylla hello krävs information om alla tre flikar hello **ny supportbegäran** formuläret.</span><span class="sxs-lookup"><span data-stu-id="6374e-129">toocomplete and submit hello request, fill out hello required information on all three tabs of hello **New support request** form.</span></span>

   ![Öka formulär](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

<span data-ttu-id="6374e-131">[Förstå Azure gränser och ökar](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) innehåller mer information om att kontakta Azure-supporten toorequest en kvot ökning.</span><span class="sxs-lookup"><span data-stu-id="6374e-131">[Understanding Azure Limits and Increases](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) provides more information about contacting Azure support toorequest a quota increase.</span></span>



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a><span data-ttu-id="6374e-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6374e-132">Next steps</span></span>
* <span data-ttu-id="6374e-133">Utforska hello [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="6374e-133">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span></span>
