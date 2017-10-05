---
title: "Säkerhetsrekommendationer i Azure Advisor | Microsoft Docs"
description: "Använda Azure Advisor för att förbättra säkerheten för din Azure-distributioner."
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
ms.openlocfilehash: 53be05593c3733ccb27979a3a026414013125779
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-security-recommendations"></a><span data-ttu-id="85359-103">Advisor säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="85359-103">Advisor Security recommendations</span></span>

<span data-ttu-id="85359-104">Azure Advisor ger en konsekvent konsoliderad vy över rekommendationer för alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="85359-104">Azure Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="85359-105">Den kan integreras med Azure Security Center så att du säkerhetsrekommendationer.</span><span class="sxs-lookup"><span data-stu-id="85359-105">It integrates with Azure Security Center to bring you security recommendations.</span></span> <span data-ttu-id="85359-106">Du kan hämta säkerhetsrekommendationer från den **säkerhet** på Advisor-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="85359-106">You can get security recommendations from the **Security** tab on the Advisor dashboard.</span></span>

![Knappen Advisor-säkerhet](./media/advisor-security-recommendations/advisor-security-tab.png)

<span data-ttu-id="85359-108">Med hjälp av Security Center kan du förebygga, upptäcka och åtgärda hot med bättre överblick och kontroll över säkerheten för dina resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="85359-108">Security Center helps you prevent, detect, and respond to threats with increased visibility into and control over the security of your Azure resources.</span></span> <span data-ttu-id="85359-109">Funktionen analyserar regelbundet säkerhetstillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="85359-109">It periodically analyzes the security state of your Azure resources.</span></span> <span data-ttu-id="85359-110">När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="85359-110">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="85359-111">Rekommendationerna som leder dig genom processen att konfigurera kontroller som du behöver.</span><span class="sxs-lookup"><span data-stu-id="85359-111">The recommendations guide you through the process of configuring the controls you need.</span></span> 

![Fliken Advisor-säkerhet](./media/advisor-security-recommendations/advisor-security-recommendations.png)

<span data-ttu-id="85359-113">Mer information om säkerhetsrekommendationer finns [hantera säkerhetsrekommendationer i Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span><span class="sxs-lookup"><span data-stu-id="85359-113">For more information about security recommendations, see [Managing security recommendations in Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span></span>

## <a name="how-to-access-security-recommendations-in-azure-advisor"></a><span data-ttu-id="85359-114">Hur du kommer åt säkerhetsrekommendationer i Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="85359-114">How to access Security recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="85359-115">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="85359-115">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="85359-116">I den vänstra rutan klickar du på **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="85359-116">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="85359-117">I fönstret service menyn under **övervakning och hantering av**, klickar du på **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="85359-117">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="85359-118">Advisor-instrumentpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="85359-118">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="85359-119">Advisor-instrumentpanelen, klicka på den **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="85359-119">On the Advisor dashboard, click the **Security** tab.</span></span>

5. <span data-ttu-id="85359-120">Välj den prenumeration som du vill få rekommendationer och klicka sedan på **få rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="85359-120">Select the subscription for which you want to receive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="85359-121">Om du vill komma åt Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="85359-121">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="85359-122">En prenumeration registreras när en *prenumeration ägare* startar Advisor instrumentpanelen och klickar på den **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="85359-122">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="85359-123">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="85359-123">This is a *one-time operation*.</span></span> <span data-ttu-id="85359-124">När prenumerationen har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="85359-124">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85359-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85359-125">Next steps</span></span>

<span data-ttu-id="85359-126">Mer information om Advisor-rekommendationer finns:</span><span class="sxs-lookup"><span data-stu-id="85359-126">To learn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="85359-127">Introduktion till Advisor</span><span class="sxs-lookup"><span data-stu-id="85359-127">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="85359-128">Kom igång med Advisor</span><span class="sxs-lookup"><span data-stu-id="85359-128">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="85359-129">Kostnad Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="85359-129">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="85359-130">Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="85359-130">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="85359-131">Advisor-rekommendationer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="85359-131">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)


 
