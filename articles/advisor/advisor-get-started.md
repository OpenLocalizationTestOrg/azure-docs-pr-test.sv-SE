---
title: "Kom igång med Azure Advisor | Microsoft Docs"
description: "Kom igång med Azure Advisor."
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: a662841bebda460d4225e080f16705b3f16fdc46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-advisor"></a><span data-ttu-id="3cc5e-103">Kom igång med Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="3cc5e-103">Get started with Azure Advisor</span></span>

<span data-ttu-id="3cc5e-104">Lär dig att komma åt Advisor via Azure portal, få rekommendationer, implementera rekommendationer, söka efter rekommendationer och uppdatera rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-104">Learn how to access Advisor through the Azure portal, get recommendations, implement recommendations, search for recommendations, and refresh recommendations.</span></span>

## <a name="get-advisor-recommendations"></a><span data-ttu-id="3cc5e-105">Få Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3cc5e-105">Get Advisor recommendations</span></span>

1. <span data-ttu-id="3cc5e-106">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3cc5e-106">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="3cc5e-107">I den vänstra rutan klickar du på **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-107">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="3cc5e-108">I fönstret service menyn under **övervakning och hantering av**, klickar du på **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-108">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="3cc5e-109">Advisor-instrumentpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-109">The Advisor dashboard is displayed.</span></span>

   ![Klassificering av åtkomst till Azure med Azure-portalen](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. <span data-ttu-id="3cc5e-111">Välj den prenumeration som du vill få rekommendationer på Advisor-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-111">On the Advisor dashboard, select the subscription for which you want to receive recommendations.</span></span>  
<span data-ttu-id="3cc5e-112">Advisor-instrumentpanelen innehåller anpassad rekommendationer för den valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-112">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

5. <span data-ttu-id="3cc5e-113">Få rekommendationer för en viss kategori klickar du på någon av flikarna: **hög tillgänglighet**, **säkerhet**, **prestanda**, eller **kostnaden**.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-113">To get recommendations for a particular category, click one of the tabs: **High Availability**, **Security**, **Performance**, or **Cost**.</span></span>
 
> [!NOTE]
> <span data-ttu-id="3cc5e-114">Om du vill komma åt Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-114">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="3cc5e-115">En prenumeration registreras när en *prenumeration ägare* startar Advisor instrumentpanelen och klickar på den **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-115">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="3cc5e-116">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-116">This is a *one-time operation*.</span></span> <span data-ttu-id="3cc5e-117">När prenumerationen har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-117">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

  ![Azure Advisor-instrumentpanelen](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a><span data-ttu-id="3cc5e-119">Hämta information om Advisor rekommendation och implementera en lösning</span><span class="sxs-lookup"><span data-stu-id="3cc5e-119">Get Advisor recommendation details, and implement a solution</span></span>

<span data-ttu-id="3cc5e-120">Den **rekommendation** bladet i Advisor innehåller ytterligare information om hur rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-120">The **Recommendation** blade in Advisor offers additional information about the recommendation.</span></span> 

1. <span data-ttu-id="3cc5e-121">Logga in på den [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="3cc5e-121">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="3cc5e-122">På den **Advisor-rekommendationer** instrumentpanelen, klickar du på **få rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-122">On the **Advisor recommendations** dashboard, click **Get recommendations**.</span></span>

3. <span data-ttu-id="3cc5e-123">Klicka på en rekommendation som du vill granska i detalj i listan över rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-123">In the list of recommendations, click a recommendation that you want to review in detail.</span></span>  
<span data-ttu-id="3cc5e-124">Den **rekommendation** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-124">The **Recommendation** blade is displayed.</span></span>

4. <span data-ttu-id="3cc5e-125">På den **rekommendationer** bladet granska information om åtgärder du kan utföra för att lösa eventuella problem eller dra nytta av en kostnad spara affärsmöjlighet.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-125">On the **Recommendations** blade, review information about actions that you can perform to resolve a potential issue, or take advantage of a cost-saving opportunity.</span></span> 
  
  ![Bladet Advisor-rekommendationer](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a><span data-ttu-id="3cc5e-127">Sök efter Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3cc5e-127">Search for Advisor recommendations</span></span>

<span data-ttu-id="3cc5e-128">Du kan söka efter rekommendationer för en viss grupp av prenumerationen eller resursen.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-128">You can search for recommendations for a particular subscription or resource group.</span></span> <span data-ttu-id="3cc5e-129">Du kan också söka efter rekommendationer efter status.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-129">You can also search for recommendations by status.</span></span>

1. <span data-ttu-id="3cc5e-130">Logga in på den [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="3cc5e-130">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="3cc5e-131">Sök efter rekommendationer genom att filtrera för prenumerationer och resursgrupper rekommendation status (**Active** eller **Snoozed**).</span><span class="sxs-lookup"><span data-stu-id="3cc5e-131">Search for recommendations by filtering for subscriptions, resource groups, and recommendation status (**Active** or **Snoozed**).</span></span>

3. <span data-ttu-id="3cc5e-132">Om du vill visa en lista med Advisor-rekommendationer som baseras på dina sökfilter kriterier klickar du på **få rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-132">To display a list of Advisor recommendations that are based on your search-filter criteria, click **Get recommendations**.</span></span>

  ![Advisor sökfilter villkor](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a><span data-ttu-id="3cc5e-134">Viloläge eller ignorera Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3cc5e-134">Snooze or dismiss Advisor recommendations</span></span>

1. <span data-ttu-id="3cc5e-135">Logga in på den [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="3cc5e-135">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="3cc5e-136">Klicka på **få rekommendationer**, och klicka sedan på en rekommendation av rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-136">Click **Get recommendations**, and then, in the list of recommendations, click a recommendation.</span></span>

3. <span data-ttu-id="3cc5e-137">På den **rekommendation** bladet, klickar du på **viloläge**.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-137">On the **Recommendation** blade, click **Snooze**.</span></span>  

   ![Advisor rekommendation åtgärd exempel](./media/advisor-get-started/advisor-snooze.png)

4. <span data-ttu-id="3cc5e-139">Ange en viloläge tidsperiod, eller välj **aldrig** stänga rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="3cc5e-139">Specify a snooze time period, or select **Never** to dismiss the recommendation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3cc5e-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3cc5e-140">Next steps</span></span>

<span data-ttu-id="3cc5e-141">Mer information om Advisor finns:</span><span class="sxs-lookup"><span data-stu-id="3cc5e-141">To learn more about Advisor, see:</span></span>
* [<span data-ttu-id="3cc5e-142">Introduktion till Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="3cc5e-142">Introduction to Azure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="3cc5e-143">Advisor-rekommendationer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="3cc5e-143">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="3cc5e-144">Advisor säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="3cc5e-144">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
-  [<span data-ttu-id="3cc5e-145">Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3cc5e-145">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="3cc5e-146">Kostnad Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3cc5e-146">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
