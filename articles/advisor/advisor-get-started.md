---
title: "aaaGet igång med Azure Advisor | Microsoft Docs"
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
ms.openlocfilehash: 30fc8b8f3823f6f047e46cb9000189f3ccb3d514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-advisor"></a><span data-ttu-id="3d204-103">Kom igång med Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="3d204-103">Get started with Azure Advisor</span></span>

<span data-ttu-id="3d204-104">Lär dig hur tooaccess Advisor via hello Azure-portalen get rekommendationer implementera rekommendationer, söka efter rekommendationer och uppdatera rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="3d204-104">Learn how tooaccess Advisor through hello Azure portal, get recommendations, implement recommendations, search for recommendations, and refresh recommendations.</span></span>

## <a name="get-advisor-recommendations"></a><span data-ttu-id="3d204-105">Få Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3d204-105">Get Advisor recommendations</span></span>

1. <span data-ttu-id="3d204-106">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3d204-106">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="3d204-107">Hello vänster klickar du på **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="3d204-107">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="3d204-108">I hello tjänsten menyn rutan under **övervakning och hantering av**, klickar du på **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="3d204-108">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="3d204-109">hello Advisor instrumentpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="3d204-109">hello Advisor dashboard is displayed.</span></span>

   ![Åtkomst till Azure Advisor med hello Azure-portalen](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. <span data-ttu-id="3d204-111">Välj hello prenumerationen för vilken du vill tooreceive rekommendationer på hello Advisor instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3d204-111">On hello Advisor dashboard, select hello subscription for which you want tooreceive recommendations.</span></span>  
<span data-ttu-id="3d204-112">hello Advisor instrumentpanelen visar anpassade rekommendationer för den valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="3d204-112">hello Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

5. <span data-ttu-id="3d204-113">tooget rekommendationer för en viss kategori, klicka på någon av flikarna hello: **hög tillgänglighet**, **säkerhet**, **prestanda**, eller **kostnaden**.</span><span class="sxs-lookup"><span data-stu-id="3d204-113">tooget recommendations for a particular category, click one of hello tabs: **High Availability**, **Security**, **Performance**, or **Cost**.</span></span>
 
> [!NOTE]
> <span data-ttu-id="3d204-114">tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="3d204-114">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="3d204-115">En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="3d204-115">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="3d204-116">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="3d204-116">This is a *one-time operation*.</span></span> <span data-ttu-id="3d204-117">När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="3d204-117">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

  ![Azure Advisor-instrumentpanelen](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a><span data-ttu-id="3d204-119">Hämta information om Advisor rekommendation och implementera en lösning</span><span class="sxs-lookup"><span data-stu-id="3d204-119">Get Advisor recommendation details, and implement a solution</span></span>

<span data-ttu-id="3d204-120">Hej **rekommendation** bladet i Advisor erbjuder ytterligare information om hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="3d204-120">hello **Recommendation** blade in Advisor offers additional information about hello recommendation.</span></span> 

1. <span data-ttu-id="3d204-121">Logga in toohello [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="3d204-121">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="3d204-122">På hello **Advisor-rekommendationer** instrumentpanelen, klickar du på **få rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="3d204-122">On hello **Advisor recommendations** dashboard, click **Get recommendations**.</span></span>

3. <span data-ttu-id="3d204-123">Klicka på en rekommendation som du vill tooreview i detalj i hello lista över rekommendationerna.</span><span class="sxs-lookup"><span data-stu-id="3d204-123">In hello list of recommendations, click a recommendation that you want tooreview in detail.</span></span>  
<span data-ttu-id="3d204-124">Hej **rekommendation** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="3d204-124">hello **Recommendation** blade is displayed.</span></span>

4. <span data-ttu-id="3d204-125">På hello **rekommendationer** bladet granska information om åtgärder du kan utföra tooresolve potentiella problem eller dra nytta av en kostnad spara affärsmöjlighet.</span><span class="sxs-lookup"><span data-stu-id="3d204-125">On hello **Recommendations** blade, review information about actions that you can perform tooresolve a potential issue, or take advantage of a cost-saving opportunity.</span></span> 
  
  ![hello Advisor rekommendation bladet](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a><span data-ttu-id="3d204-127">Sök efter Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3d204-127">Search for Advisor recommendations</span></span>

<span data-ttu-id="3d204-128">Du kan söka efter rekommendationer för en viss grupp av prenumerationen eller resursen.</span><span class="sxs-lookup"><span data-stu-id="3d204-128">You can search for recommendations for a particular subscription or resource group.</span></span> <span data-ttu-id="3d204-129">Du kan också söka efter rekommendationer efter status.</span><span class="sxs-lookup"><span data-stu-id="3d204-129">You can also search for recommendations by status.</span></span>

1. <span data-ttu-id="3d204-130">Logga in toohello [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="3d204-130">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="3d204-131">Sök efter rekommendationer genom att filtrera för prenumerationer och resursgrupper rekommendation status (**Active** eller **Snoozed**).</span><span class="sxs-lookup"><span data-stu-id="3d204-131">Search for recommendations by filtering for subscriptions, resource groups, and recommendation status (**Active** or **Snoozed**).</span></span>

3. <span data-ttu-id="3d204-132">toodisplay en lista med Advisor-rekommendationer som baseras på dina sökfilter kriterier klickar du på **få rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="3d204-132">toodisplay a list of Advisor recommendations that are based on your search-filter criteria, click **Get recommendations**.</span></span>

  ![Advisor sökfilter villkor](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a><span data-ttu-id="3d204-134">Viloläge eller ignorera Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3d204-134">Snooze or dismiss Advisor recommendations</span></span>

1. <span data-ttu-id="3d204-135">Logga in toohello [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="3d204-135">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="3d204-136">Klicka på **få rekommendationer**, och i hello lista över rekommendationerna, klickar du på en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="3d204-136">Click **Get recommendations**, and then, in hello list of recommendations, click a recommendation.</span></span>

3. <span data-ttu-id="3d204-137">På hello **rekommendation** bladet, klickar du på **viloläge**.</span><span class="sxs-lookup"><span data-stu-id="3d204-137">On hello **Recommendation** blade, click **Snooze**.</span></span>  

   ![Advisor rekommendation åtgärd exempel](./media/advisor-get-started/advisor-snooze.png)

4. <span data-ttu-id="3d204-139">Ange en viloläge tidsperiod, eller välj **aldrig** toodismiss hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="3d204-139">Specify a snooze time period, or select **Never** toodismiss hello recommendation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3d204-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d204-140">Next steps</span></span>

<span data-ttu-id="3d204-141">toolearn mer om Advisor, se:</span><span class="sxs-lookup"><span data-stu-id="3d204-141">toolearn more about Advisor, see:</span></span>
* [<span data-ttu-id="3d204-142">Introduktion tooAzure Advisor</span><span class="sxs-lookup"><span data-stu-id="3d204-142">Introduction tooAzure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="3d204-143">Advisor-rekommendationer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="3d204-143">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="3d204-144">Advisor säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="3d204-144">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
-  [<span data-ttu-id="3d204-145">Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3d204-145">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="3d204-146">Kostnad Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3d204-146">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
