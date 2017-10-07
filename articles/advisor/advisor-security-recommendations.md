---
title: "aaaAzure Advisor säkerhetsrekommendationer | Microsoft Docs"
description: "Använd Azure Advisor toohelp förbättra hello säkerheten för din Azure-distributioner."
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
ms.openlocfilehash: e01ac29eb6e02bff0b1e846e320e7c36f85c7343
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-security-recommendations"></a><span data-ttu-id="e3fac-103">Advisor säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="e3fac-103">Advisor Security recommendations</span></span>

<span data-ttu-id="e3fac-104">Azure Advisor ger en konsekvent konsoliderad vy över rekommendationer för alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e3fac-104">Azure Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="e3fac-105">Den kan integreras med Azure Security Center toobring du säkerhetsrekommendationer.</span><span class="sxs-lookup"><span data-stu-id="e3fac-105">It integrates with Azure Security Center toobring you security recommendations.</span></span> <span data-ttu-id="e3fac-106">Du kan hämta säkerhetsrekommendationer från hello **säkerhet** fliken på instrumentpanelen för hello Advisor.</span><span class="sxs-lookup"><span data-stu-id="e3fac-106">You can get security recommendations from hello **Security** tab on hello Advisor dashboard.</span></span>

![knappen för hello Advisor-säkerhet](./media/advisor-security-recommendations/advisor-security-tab.png)

<span data-ttu-id="e3fac-108">Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e3fac-108">Security Center helps you prevent, detect, and respond toothreats with increased visibility into and control over hello security of your Azure resources.</span></span> <span data-ttu-id="e3fac-109">Funktionen analyserar regelbundet hello säkerhetstillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e3fac-109">It periodically analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="e3fac-110">När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="e3fac-110">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="e3fac-111">hello rekommendationer leder dig igenom hello konfigureringen av hello-kontroller som du behöver.</span><span class="sxs-lookup"><span data-stu-id="e3fac-111">hello recommendations guide you through hello process of configuring hello controls you need.</span></span> 

![hello Advisor säkerhetsfliken](./media/advisor-security-recommendations/advisor-security-recommendations.png)

<span data-ttu-id="e3fac-113">Mer information om säkerhetsrekommendationer finns [hantera säkerhetsrekommendationer i Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span><span class="sxs-lookup"><span data-stu-id="e3fac-113">For more information about security recommendations, see [Managing security recommendations in Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span></span>

## <a name="how-tooaccess-security-recommendations-in-azure-advisor"></a><span data-ttu-id="e3fac-114">Hur tooaccess säkerhetsrekommendationer i Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="e3fac-114">How tooaccess Security recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="e3fac-115">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e3fac-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="e3fac-116">Hello vänster klickar du på **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="e3fac-116">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="e3fac-117">I hello tjänsten menyn rutan under **övervakning och hantering av**, klickar du på **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="e3fac-117">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="e3fac-118">hello Advisor instrumentpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="e3fac-118">hello Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="e3fac-119">Klicka på hello Advisor instrumentpanelen hello **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="e3fac-119">On hello Advisor dashboard, click hello **Security** tab.</span></span>

5. <span data-ttu-id="e3fac-120">Välj hello prenumeration som du vill tooreceive rekommendationer och klicka sedan på **få rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="e3fac-120">Select hello subscription for which you want tooreceive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="e3fac-121">tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="e3fac-121">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="e3fac-122">En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="e3fac-122">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="e3fac-123">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="e3fac-123">This is a *one-time operation*.</span></span> <span data-ttu-id="e3fac-124">När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="e3fac-124">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3fac-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3fac-125">Next steps</span></span>

<span data-ttu-id="e3fac-126">toolearn mer om Advisor-rekommendationer finns:</span><span class="sxs-lookup"><span data-stu-id="e3fac-126">toolearn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="e3fac-127">Introduktion tooAdvisor</span><span class="sxs-lookup"><span data-stu-id="e3fac-127">Introduction tooAdvisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="e3fac-128">Kom igång med Advisor</span><span class="sxs-lookup"><span data-stu-id="e3fac-128">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="e3fac-129">Kostnad Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="e3fac-129">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="e3fac-130">Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="e3fac-130">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="e3fac-131">Advisor-rekommendationer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="e3fac-131">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)


 
