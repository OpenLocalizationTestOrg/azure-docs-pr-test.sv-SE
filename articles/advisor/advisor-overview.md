---
title: Introduktion till Azure Advisor | Microsoft Docs
description: "Använd Azure Advisor för att optimera din Azure-distributioner."
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
ms.openlocfilehash: 35678142550f9f887562f311a5e7d9516495cf53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-azure-advisor"></a><span data-ttu-id="d198a-103">Introduktion till Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="d198a-103">Introduction to Azure Advisor</span></span>

<span data-ttu-id="d198a-104">Lär dig mer om Azure Advisor och de viktigaste funktionerna och få svar på vanliga frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="d198a-104">Learn about Azure Advisor and its key capabilities, and get answers to frequently asked questions.</span></span>

## <a name="what-is-advisor"></a><span data-ttu-id="d198a-105">Vad är Advisor?</span><span class="sxs-lookup"><span data-stu-id="d198a-105">What is Advisor?</span></span>
<span data-ttu-id="d198a-106">Advisor är en anpassad cloud konsult som hjälper dig att följa de bästa metoderna för att optimera din Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="d198a-106">Advisor is a personalized cloud consultant that helps you follow best practices to optimize your Azure deployments.</span></span> <span data-ttu-id="d198a-107">Den analyserar dina resurskonfigurationen och telemetri och rekommenderar lösningar som hjälper dig att förbättra kostnadseffektivitet, prestanda, hög tillgänglighet och säkerhet för dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="d198a-107">It analyzes your resource configuration and usage telemetry and then recommends solutions that can help you improve the cost effectiveness, performance, high availability, and security of your Azure resources.</span></span>

<span data-ttu-id="d198a-108">Med Advisor kan du:</span><span class="sxs-lookup"><span data-stu-id="d198a-108">With Advisor, you can:</span></span>
* <span data-ttu-id="d198a-109">Hämta proaktiv, tillförlitlig och anpassade rekommendationer och metodtips.</span><span class="sxs-lookup"><span data-stu-id="d198a-109">Get proactive, actionable, and personalized best practices recommendations.</span></span> 
* <span data-ttu-id="d198a-110">Förbättra prestanda, säkerhet och hög tillgänglighet för dina resurser, som du identifiera möjligheter att minska den totala Azure tillbringar.</span><span class="sxs-lookup"><span data-stu-id="d198a-110">Improve the performance, security, and high availability of your resources, as you identify opportunities to reduce your overall Azure spend.</span></span>
* <span data-ttu-id="d198a-111">Få rekommendationer med infogad föreslagna åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d198a-111">Get recommendations with proposed actions inline.</span></span>

<span data-ttu-id="d198a-112">Du kan komma åt Advisor i den [Azure-portalen](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="d198a-112">You can access Advisor through the [Azure portal](https://aka.ms/azureadvisordashboard).</span></span> <span data-ttu-id="d198a-113">Logga in på den [portal](https://portal.azure.com)väljer **Bläddra**, och bläddra sedan till **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="d198a-113">Sign in to the [portal](https://portal.azure.com), select **Browse**, and then scroll to **Azure Advisor**.</span></span> <span data-ttu-id="d198a-114">Advisor-instrumentpanelen innehåller anpassad rekommendationer för den valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="d198a-114">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

<span data-ttu-id="d198a-115">Rekommendationerna delas in i fyra kategorier:</span><span class="sxs-lookup"><span data-stu-id="d198a-115">The recommendations are divided into four categories:</span></span> 

* <span data-ttu-id="d198a-116">**Hög tillgänglighet**: att säkerställa och förbättra kontinuiteten i dina verksamhetskritiska program.</span><span class="sxs-lookup"><span data-stu-id="d198a-116">**High Availability**: To ensure and improve the continuity of your business-critical applications.</span></span> <span data-ttu-id="d198a-117">Mer information finns i [hög tillgänglighet för Advisor-rekommendationer](advisor-high-availability-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="d198a-117">For more information, see [Advisor High Availability recommendations](advisor-high-availability-recommendations.md).</span></span>

* <span data-ttu-id="d198a-118">**Säkerhet**: identifiera hot och säkerhetsrisker som kan leda till säkerhetsintrång.</span><span class="sxs-lookup"><span data-stu-id="d198a-118">**Security**: To detect threats and vulnerabilities that might lead to security breaches.</span></span> <span data-ttu-id="d198a-119">Mer information finns i [Advisor säkerhetsrekommendationer](advisor-security-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="d198a-119">For more information, see [Advisor Security recommendations](advisor-security-recommendations.md).</span></span>

* <span data-ttu-id="d198a-120">**Prestanda**: att förbättra hastighet för dina program.</span><span class="sxs-lookup"><span data-stu-id="d198a-120">**Performance**: To improve the speed of your applications.</span></span> <span data-ttu-id="d198a-121">Mer information finns i [Advisor-rekommendationer](advisor-performance-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="d198a-121">For more information, see [Advisor Performance recommendations](advisor-performance-recommendations.md).</span></span>

* <span data-ttu-id="d198a-122">**Kostnad**: för att optimera och minska din övergripande Azure tillbringar.</span><span class="sxs-lookup"><span data-stu-id="d198a-122">**Cost**: To optimize and reduce your overall Azure spend.</span></span> <span data-ttu-id="d198a-123">Mer information finns i [kostnaden för Advisor-rekommendationer](advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="d198a-123">For more information, see [Advisor Cost recommendations](advisor-cost-recommendations.md).</span></span>

  ![Typer för Advisor-rekommendationer](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> <span data-ttu-id="d198a-125">Om du vill komma åt Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="d198a-125">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="d198a-126">En prenumeration registreras när en *prenumeration ägare* startar Advisor instrumentpanelen och klickar på den **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="d198a-126">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="d198a-127">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="d198a-127">This is a *one-time operation*.</span></span> <span data-ttu-id="d198a-128">När prenumerationen har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="d198a-128">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

<span data-ttu-id="d198a-129">Du kan klicka på en rekommendation om du vill veta mer om den.</span><span class="sxs-lookup"><span data-stu-id="d198a-129">You can click a recommendation to learn more about it.</span></span> <span data-ttu-id="d198a-130">Du kan också information om åtgärder som du kan utföra för att dra nytta av en affärsmöjlighet eller lösa ett problem.</span><span class="sxs-lookup"><span data-stu-id="d198a-130">You can also learn about actions that you can perform to take advantage of an opportunity or resolve an issue.</span></span> 

<span data-ttu-id="d198a-131">Advisor-rekommendationer med infogad åtgärder eller dokumentationen länkar.</span><span class="sxs-lookup"><span data-stu-id="d198a-131">Advisor offers recommendations with inline actions or documentation links.</span></span> <span data-ttu-id="d198a-132">Klicka på en infogad åtgärd tar dig igenom en ”interaktiv användare resa” att genomföra den.</span><span class="sxs-lookup"><span data-stu-id="d198a-132">Clicking an inline action takes you through a “guided user journey” to implement it.</span></span> <span data-ttu-id="d198a-133">Klicka på en länk i dokumentationen hänvisar till dokumentationen som beskriver hur du implementerar åtgärden manuellt.</span><span class="sxs-lookup"><span data-stu-id="d198a-133">Clicking a documentation link points you to documentation that describes how to manually implement the action.</span></span> 

<span data-ttu-id="d198a-134">Advisor uppdaterar rekommendationer varje timme.</span><span class="sxs-lookup"><span data-stu-id="d198a-134">Advisor updates recommendations hourly.</span></span> <span data-ttu-id="d198a-135">Om du inte tänker vidta omedelbara åtgärder på en rekommendation du stänga den viloläge för en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="d198a-135">If you don’t intend to take immediate action on a recommendation, you can snooze it for a specified time period or dismiss it.</span></span> 

## <a name="frequently-asked-questions"></a><span data-ttu-id="d198a-136">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="d198a-136">Frequently asked questions</span></span>

### <a name="how-do-i-access-advisor"></a><span data-ttu-id="d198a-137">Hur kommer jag åt Advisor?</span><span class="sxs-lookup"><span data-stu-id="d198a-137">How do I access Advisor?</span></span>
<span data-ttu-id="d198a-138">Du kan komma åt Advisor i den [Azure-portalen](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="d198a-138">You can access Advisor through the [Azure portal](https://aka.ms/azureadvisordashboard).</span></span> <span data-ttu-id="d198a-139">Logga in på den [portal](https://portal.azure.com)väljer **Bläddra**, och bläddra sedan till **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="d198a-139">Sign in to the [portal](https://portal.azure.com), select **Browse**, and then scroll to **Azure Advisor**.</span></span> <span data-ttu-id="d198a-140">Advisor-instrumentpanelen innehåller anpassad rekommendationer för den valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="d198a-140">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

<span data-ttu-id="d198a-141">Du kan också visa Advisor-rekommendationer via resursbladet för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d198a-141">You can also view Advisor recommendations through the virtual machine resource blade.</span></span> <span data-ttu-id="d198a-142">Välj en virtuell dator och bläddrar sedan till Advisor-rekommendationer i menyn.</span><span class="sxs-lookup"><span data-stu-id="d198a-142">Choose a virtual machine, and then scroll to Advisor recommendations in the menu.</span></span> 

### <a name="what-permissions-do-i-need-to-access-advisor"></a><span data-ttu-id="d198a-143">Vilka behörigheter som behöver att komma åt Advisor?</span><span class="sxs-lookup"><span data-stu-id="d198a-143">What permissions do I need to access Advisor?</span></span>

<span data-ttu-id="d198a-144">Om du vill komma åt Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="d198a-144">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="d198a-145">En prenumeration registreras när en *prenumeration ägare* startar Advisor instrumentpanelen och klickar på den **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="d198a-145">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="d198a-146">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="d198a-146">This is a *one-time operation*.</span></span> <span data-ttu-id="d198a-147">När prenumerationen har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="d198a-147">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

### <a name="how-often-are-advisor-recommendations-updated"></a><span data-ttu-id="d198a-148">Hur ofta uppdateras Advisor-rekommendationer uppdateras?</span><span class="sxs-lookup"><span data-stu-id="d198a-148">How often are Advisor recommendations updated?</span></span>

<span data-ttu-id="d198a-149">Advisor-rekommendationer uppdateras varje timme.</span><span class="sxs-lookup"><span data-stu-id="d198a-149">Advisor recommendations are updated hourly.</span></span>

### <a name="what-resources-does-advisor-provide-recommendations-for"></a><span data-ttu-id="d198a-150">Vilka resurser Advisor ger rekommendationer för?</span><span class="sxs-lookup"><span data-stu-id="d198a-150">What resources does Advisor provide recommendations for?</span></span>

<span data-ttu-id="d198a-151">Advisor innehåller rekommendationer för virtuella datorer, tillgänglighetsuppsättningar, programgatewayer, Apptjänster, SQL-servrar, SQL-databaser och Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="d198a-151">Advisor provides recommendations for virtual machines, availability sets, application gateways, App Services, SQL servers, SQL databases, and Redis Cache.</span></span>

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a><span data-ttu-id="d198a-152">Kan jag viloläge eller ignorera en rekommendation?</span><span class="sxs-lookup"><span data-stu-id="d198a-152">Can I snooze or dismiss a recommendation?</span></span>

<span data-ttu-id="d198a-153">Viloläge eller ignorera en rekommendation genom att klicka på den **viloläge** knappen eller länken.</span><span class="sxs-lookup"><span data-stu-id="d198a-153">To snooze or dismiss a recommendation, click the **Snooze** button or link.</span></span> <span data-ttu-id="d198a-154">Du kan ange en tid för viloläge period eller välja **aldrig** stänga rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="d198a-154">You can specify a snooze time period or select **Never** to dismiss the recommendation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d198a-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d198a-155">Next steps</span></span>

<span data-ttu-id="d198a-156">Mer information om Advisor-rekommendationer finns:</span><span class="sxs-lookup"><span data-stu-id="d198a-156">To learn more about Advisor recommendations, see:</span></span>

* [<span data-ttu-id="d198a-157">Kom igång med Advisor</span><span class="sxs-lookup"><span data-stu-id="d198a-157">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="d198a-158">Advisor-rekommendationer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="d198a-158">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="d198a-159">Advisor säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="d198a-159">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
* [<span data-ttu-id="d198a-160">Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="d198a-160">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="d198a-161">Kostnad Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="d198a-161">Advisor Cost recommendations</span></span>](advisor-cost-recommendations.md)
