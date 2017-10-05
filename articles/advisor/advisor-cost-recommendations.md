---
title: "Azure kostnaden för Advisor-rekommendationer | Microsoft Docs"
description: "Använd Azure Advisor för att optimera kostnader för Azure-distributioner."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 5eef2116f238b477fa8de46ce7b25728c393739c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-cost-recommendations"></a><span data-ttu-id="aa987-103">Kostnad Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="aa987-103">Advisor Cost recommendations</span></span>

<span data-ttu-id="aa987-104">Advisor hjälper dig att optimera och minska din övergripande Azure tillbringar med att identifiera inaktiv och underutnyttjade resurser.</span><span class="sxs-lookup"><span data-stu-id="aa987-104">Advisor helps you optimize and reduce your overall Azure spend by identifying idle and underutilized resources.</span></span> <span data-ttu-id="aa987-105">Du kan hämta cost rekommendationerna från den **kostnaden** på Advisor-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="aa987-105">You can get cost recommendations from the **Cost** tab on the Advisor dashboard.</span></span>

![Advisor kostnaden fliken](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a><span data-ttu-id="aa987-107">Optimera virtuella tillbringar genom att ändra storlek underutnyttjade instanser</span><span class="sxs-lookup"><span data-stu-id="aa987-107">Optimize virtual machine spend by resizing underutilized instances</span></span> 
<span data-ttu-id="aa987-108">Även om vissa Programscenarier kan resultera i lågt utnyttjande av design, kan du ofta spara pengar genom att hantera storlek och antalet virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="aa987-108">Although certain application scenarios can result in low utilization by design, you can often save money by managing the size and number of your virtual machines.</span></span> <span data-ttu-id="aa987-109">Advisor övervakar din användning av virtuella datorer på 14 dagar och identifierar låg belastning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="aa987-109">Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines.</span></span> <span data-ttu-id="aa987-110">Virtuella datorer vars CPU-användning är 5 procent eller mindre och nätverksanvändning är 7 MB eller mindre för fyra eller flera dagar betraktas som låg belastning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="aa987-110">Virtual machines whose CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.</span></span>

<span data-ttu-id="aa987-111">Advisor visar de uppskattade kostnaderna för fortsätter att köra den virtuella datorn så att du kan välja att stänga av eller ändra storlek på den.</span><span class="sxs-lookup"><span data-stu-id="aa987-111">Advisor shows you the estimated cost of continuing to run your virtual machine, so that you can choose to shut it down or resize it.</span></span>  

![Advisor kostnad rekommendationer för storleksändring av virtuella datorer](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a><span data-ttu-id="aa987-113">Använda en kostnadseffektiv lösning för att hantera prestandamål av flera SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="aa987-113">Use a cost effective solution to manage performance goals of multiple SQL databases</span></span>
<span data-ttu-id="aa987-114">Advisor identifierar SQL server-instanser som kan dra nytta av att skapa elastiska databaspooler.</span><span class="sxs-lookup"><span data-stu-id="aa987-114">Advisor identifies SQL server instances that can benefit from creating elastic database pools.</span></span> <span data-ttu-id="aa987-115">Elastiska databaspooler ger en enkel och kostnadseffektiv lösning för att hantera prestandamål av flera databaser som har olika användningsmönster.</span><span class="sxs-lookup"><span data-stu-id="aa987-115">Elastic database pools provide a simple, cost-effective solution to manage the performance goals of multiple databases that have varying usage patterns.</span></span> <span data-ttu-id="aa987-116">Läs mer om Azure elastiska pooler [vad är en Azure elastisk pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span><span class="sxs-lookup"><span data-stu-id="aa987-116">For more information about Azure elastic pools, see [What is an Azure Elastic pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span></span>

![Advisor kostnad rekommendationer för elastiska databaspooler](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a><span data-ttu-id="aa987-118">Hur du kommer åt kostnaden rekommendationerna i Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="aa987-118">How to access cost recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="aa987-119">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa987-119">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="aa987-120">I den vänstra rutan klickar du på **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="aa987-120">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="aa987-121">I fönstret service menyn under **övervakning och hantering av**, klickar du på **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="aa987-121">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="aa987-122">Advisor-instrumentpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="aa987-122">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="aa987-123">Advisor-instrumentpanelen, klicka på den **kostnaden** fliken.</span><span class="sxs-lookup"><span data-stu-id="aa987-123">On the Advisor dashboard, click the **Cost** tab.</span></span>

5. <span data-ttu-id="aa987-124">Välj den prenumeration som du vill få rekommendationer och klicka sedan på **få rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="aa987-124">Select the subscription for which you want to receive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="aa987-125">Om du vill komma åt Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="aa987-125">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="aa987-126">En prenumeration registreras när en *prenumeration ägare* startar Advisor instrumentpanelen och klickar på den **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="aa987-126">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="aa987-127">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="aa987-127">This is a *one-time operation*.</span></span> <span data-ttu-id="aa987-128">När prenumerationen har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="aa987-128">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa987-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa987-129">Next steps</span></span>

<span data-ttu-id="aa987-130">Mer information om Advisor-rekommendationer finns:</span><span class="sxs-lookup"><span data-stu-id="aa987-130">To learn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="aa987-131">Introduktion till Advisor</span><span class="sxs-lookup"><span data-stu-id="aa987-131">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="aa987-132">Kom igång</span><span class="sxs-lookup"><span data-stu-id="aa987-132">Get Started</span></span>](advisor-get-started.md)
* [<span data-ttu-id="aa987-133">Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="aa987-133">Advisor Performance recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="aa987-134">Advisor-rekommendationer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="aa987-134">Advisor High Availability recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="aa987-135">Advisor säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="aa987-135">Advisor Security recommendations</span></span>](advisor-cost-recommendations.md)
