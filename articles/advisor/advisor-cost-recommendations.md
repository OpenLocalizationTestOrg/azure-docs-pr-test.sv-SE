---
title: "aaaAzure kostnaden för Advisor-rekommendationer | Microsoft Docs"
description: "Använd Azure Advisor toooptimize hello kostnaden för Azure-distributioner."
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
ms.openlocfilehash: 50f70c33a17f550c8753795435cdfddd51e409f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-cost-recommendations"></a><span data-ttu-id="8eb6c-103">Kostnad Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="8eb6c-103">Advisor Cost recommendations</span></span>

<span data-ttu-id="8eb6c-104">Advisor hjälper dig att optimera och minska din övergripande Azure tillbringar med att identifiera inaktiv och underutnyttjade resurser.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-104">Advisor helps you optimize and reduce your overall Azure spend by identifying idle and underutilized resources.</span></span> <span data-ttu-id="8eb6c-105">Du kan hämta cost rekommendationerna från hello **kostnaden** fliken på instrumentpanelen för hello Advisor.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-105">You can get cost recommendations from hello **Cost** tab on hello Advisor dashboard.</span></span>

![Advisor kostnaden fliken](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a><span data-ttu-id="8eb6c-107">Optimera virtuella tillbringar genom att ändra storlek underutnyttjade instanser</span><span class="sxs-lookup"><span data-stu-id="8eb6c-107">Optimize virtual machine spend by resizing underutilized instances</span></span> 
<span data-ttu-id="8eb6c-108">Även om vissa Programscenarier kan resultera i lågt utnyttjande av design, kan du ofta spara pengar genom att hantera hello storlek och antalet virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-108">Although certain application scenarios can result in low utilization by design, you can often save money by managing hello size and number of your virtual machines.</span></span> <span data-ttu-id="8eb6c-109">Advisor övervakar din användning av virtuella datorer på 14 dagar och identifierar låg belastning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-109">Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines.</span></span> <span data-ttu-id="8eb6c-110">Virtuella datorer vars CPU-användning är 5 procent eller mindre och nätverksanvändning är 7 MB eller mindre för fyra eller flera dagar betraktas som låg belastning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-110">Virtual machines whose CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.</span></span>

<span data-ttu-id="8eb6c-111">Advisor visar du hello uppskattade kostnaden för fortsätter toorun den virtuella datorn så att du kan välja tooshut den ned eller ändra storlek på den.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-111">Advisor shows you hello estimated cost of continuing toorun your virtual machine, so that you can choose tooshut it down or resize it.</span></span>  

![Advisor kostnad rekommendationer för storleksändring av virtuella datorer](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-toomanage-performance-goals-of-multiple-sql-databases"></a><span data-ttu-id="8eb6c-113">Använd en kostnadseffektiv lösning toomanage prestandamål av flera SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="8eb6c-113">Use a cost effective solution toomanage performance goals of multiple SQL databases</span></span>
<span data-ttu-id="8eb6c-114">Advisor identifierar SQL server-instanser som kan dra nytta av att skapa elastiska databaspooler.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-114">Advisor identifies SQL server instances that can benefit from creating elastic database pools.</span></span> <span data-ttu-id="8eb6c-115">Elastiska databaspooler ger en enkel och kostnadseffektiv lösning toomanage hello prestandamål av flera databaser som har olika användningsmönster.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-115">Elastic database pools provide a simple, cost-effective solution toomanage hello performance goals of multiple databases that have varying usage patterns.</span></span> <span data-ttu-id="8eb6c-116">Läs mer om Azure elastiska pooler [vad är en Azure elastisk pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span><span class="sxs-lookup"><span data-stu-id="8eb6c-116">For more information about Azure elastic pools, see [What is an Azure Elastic pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span></span>

![Advisor kostnad rekommendationer för elastiska databaspooler](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-tooaccess-cost-recommendations-in-azure-advisor"></a><span data-ttu-id="8eb6c-118">Hur tooaccess kostnad rekommendationerna i Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="8eb6c-118">How tooaccess cost recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="8eb6c-119">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8eb6c-119">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="8eb6c-120">Hello vänster klickar du på **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-120">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="8eb6c-121">I hello tjänsten menyn rutan under **övervakning och hantering av**, klickar du på **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-121">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="8eb6c-122">hello Advisor instrumentpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-122">hello Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="8eb6c-123">Klicka på hello Advisor instrumentpanelen hello **kostnaden** fliken.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-123">On hello Advisor dashboard, click hello **Cost** tab.</span></span>

5. <span data-ttu-id="8eb6c-124">Välj hello prenumeration som du vill tooreceive rekommendationer och klicka sedan på **få rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-124">Select hello subscription for which you want tooreceive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="8eb6c-125">tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-125">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="8eb6c-126">En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-126">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="8eb6c-127">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-127">This is a *one-time operation*.</span></span> <span data-ttu-id="8eb6c-128">När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="8eb6c-128">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8eb6c-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8eb6c-129">Next steps</span></span>

<span data-ttu-id="8eb6c-130">toolearn mer om Advisor-rekommendationer finns:</span><span class="sxs-lookup"><span data-stu-id="8eb6c-130">toolearn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="8eb6c-131">Introduktion tooAdvisor</span><span class="sxs-lookup"><span data-stu-id="8eb6c-131">Introduction tooAdvisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="8eb6c-132">Kom igång</span><span class="sxs-lookup"><span data-stu-id="8eb6c-132">Get Started</span></span>](advisor-get-started.md)
* [<span data-ttu-id="8eb6c-133">Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="8eb6c-133">Advisor Performance recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="8eb6c-134">Advisor-rekommendationer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="8eb6c-134">Advisor High Availability recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="8eb6c-135">Advisor säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="8eb6c-135">Advisor Security recommendations</span></span>](advisor-cost-recommendations.md)
