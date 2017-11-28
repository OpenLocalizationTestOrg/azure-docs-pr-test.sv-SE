---
title: aaaInteract med rapporter med hello JavaScript API | Microsoft Docs
description: Power BI Embedded, interagera med rapporter med hello JavaScript API
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: bdd885d3-1b00-4dcf-bdff-531eb1f97bfb
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: 657e4d5cee031bdda173ab3f451cc19b93ddb17b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="interact-with-power-bi-reports-using-hello-javascript-api"></a><span data-ttu-id="412a1-103">Interagera med Power BI-rapporter med hello JavaScript API</span><span class="sxs-lookup"><span data-stu-id="412a1-103">Interact with Power BI reports using hello JavaScript API</span></span>
<span data-ttu-id="412a1-104">hello Power BI JavaScript API som ger du tooeasily bädda in Power BI-rapporter i dina program.</span><span class="sxs-lookup"><span data-stu-id="412a1-104">hello Power BI JavaScript API enables you tooeasily embed Power BI reports into your applications.</span></span> <span data-ttu-id="412a1-105">Med hello API interagera dina program med annan rapportelement som sidor och filter.</span><span class="sxs-lookup"><span data-stu-id="412a1-105">With hello API, your applications can programmatically interact with different report elements like pages and filters.</span></span> <span data-ttu-id="412a1-106">Den här interaktiviteten gör Power BI-rapporter till en mer integrerad del av ditt program.</span><span class="sxs-lookup"><span data-stu-id="412a1-106">This interactivity makes Power BI reports a more integrated part of your application.</span></span>

<span data-ttu-id="412a1-107">Du bädda in en Power BI-rapport i ditt program med en iframe som finns som en del av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="412a1-107">You embed a Power BI report in your application by using an iframe that is hosted as part of hello application.</span></span> <span data-ttu-id="412a1-108">hello iframe fungerar som en gräns mellan program och hello rapport som du ser i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="412a1-108">hello iframe acts as a boundary between your application and hello report, as you can see in hello following image.</span></span> 

![Power BI inbäddad iframe utan Javascript API](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-1.png)

<span data-ttu-id="412a1-110">hello iframe gör hello bädda in processen mycket enklare, men utan hello JavaScript API hello rapporten och programmet samverka inte med varandra.</span><span class="sxs-lookup"><span data-stu-id="412a1-110">hello iframe makes hello embedding process a lot easier, but without hello JavaScript API hello report and your application can't interact with each other.</span></span> <span data-ttu-id="412a1-111">Den här bristen på interaktionen kan det vara känner hello rapporten inte är verkligen en del av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="412a1-111">This lack of interaction can make it feel like hello report is not really part of hello application.</span></span> <span data-ttu-id="412a1-112">hello rapporten och program du verkligen behöver toocommunicate fram och tillbaka som hello följande bild.</span><span class="sxs-lookup"><span data-stu-id="412a1-112">hello report and application really need toocommunicate back and forth, as in hello following image.</span></span>

![Power BI inbäddad iframe med Javascript API](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-2.png)

<span data-ttu-id="412a1-114">hello Power BI JavaScript API kan du toowrite kod som på ett säkert sätt kan passera hello iframe gräns.</span><span class="sxs-lookup"><span data-stu-id="412a1-114">hello Power BI JavaScript API enables you toowrite code that can securely pass through hello iframe boundary.</span></span> <span data-ttu-id="412a1-115">Det här kan ditt program tooprogrammatically utföra en åtgärd i en rapport och toolisten händelser från åtgärder som användare gör inom hello rapport.</span><span class="sxs-lookup"><span data-stu-id="412a1-115">This enables your application tooprogrammatically perform an action in a report, and toolisten for events from actions that users make within hello report.</span></span>

## <a name="what-can-you-do-with-hello-power-bi-javascript-api"></a><span data-ttu-id="412a1-116">Vad kan du göra med hello Power BI JavaScript API?</span><span class="sxs-lookup"><span data-stu-id="412a1-116">What can you do with hello Power BI JavaScript API?</span></span>
<span data-ttu-id="412a1-117">Med hello JavaScript API kan du hantera rapporter, navigera toopages i en rapport, filtrera en rapport och hantera bädda in händelser.</span><span class="sxs-lookup"><span data-stu-id="412a1-117">With hello JavaScript API you can manage reports, navigate toopages in a report, filter a report, and handle embedding events.</span></span> <span data-ttu-id="412a1-118">hello visar följande diagram hello strukturen för hello API.</span><span class="sxs-lookup"><span data-stu-id="412a1-118">hello following diagram shows hello structure of hello API.</span></span>

![Power BI JavaScript API-diagram](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-3.png)

### <a name="manage-reports"></a><span data-ttu-id="412a1-120">Hantera rapporter</span><span class="sxs-lookup"><span data-stu-id="412a1-120">Manage reports</span></span>
<span data-ttu-id="412a1-121">hello Javascript API kan du toomanage beteende på hello rapport- och:</span><span class="sxs-lookup"><span data-stu-id="412a1-121">hello Javascript API enables you toomanage behavior at hello report and page level:</span></span>

* <span data-ttu-id="412a1-122">Bädda in en specifik Power BI-rapport på ett säkert sätt i ditt program - försök hello [bädda in demo-program](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)</span><span class="sxs-lookup"><span data-stu-id="412a1-122">Embed a specific Power BI Report securely in your application - try hello [embed demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)</span></span>
  * <span data-ttu-id="412a1-123">Ange åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="412a1-123">Set access token</span></span>
* <span data-ttu-id="412a1-124">Konfigurera hello-rapport</span><span class="sxs-lookup"><span data-stu-id="412a1-124">Configure hello report</span></span>
  * <span data-ttu-id="412a1-125">Aktivera och inaktivera hello filterfönstret och navigeringsfönstret - försök hello [uppdatera inställningar demo-program](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)</span><span class="sxs-lookup"><span data-stu-id="412a1-125">Enable and disable hello filter pane and page navigation pane - try hello [update settings demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)</span></span>
  * <span data-ttu-id="412a1-126">Ange standardinställningar för sidor och filter - försök hello [uppsättning standarder demo](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)</span><span class="sxs-lookup"><span data-stu-id="412a1-126">Set defaults for pages and filters - try hello [set defaults demo](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)</span></span>
* <span data-ttu-id="412a1-127">Gå in och ur fullskärmsläge</span><span class="sxs-lookup"><span data-stu-id="412a1-127">Enter and exit full screen mode</span></span>

[<span data-ttu-id="412a1-128">Läs mer om att bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="412a1-128">Learn more about embedding a report</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)

### <a name="navigate-toopages-in-a-report"></a><span data-ttu-id="412a1-129">Navigera toopages i en rapport</span><span class="sxs-lookup"><span data-stu-id="412a1-129">Navigate toopages in a report</span></span>
<span data-ttu-id="412a1-130">hello JavaScript API enbales du toodiscover alla sidor i en rapport och tooset hello sidan.</span><span class="sxs-lookup"><span data-stu-id="412a1-130">hello JavaScript API enbales you toodiscover all pages in a report and tooset hello current page.</span></span> <span data-ttu-id="412a1-131">Försök hello [navigering demo programmet](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).</span><span class="sxs-lookup"><span data-stu-id="412a1-131">Try hello [navigation demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).</span></span>

[<span data-ttu-id="412a1-132">Läs mer om sidnavigering</span><span class="sxs-lookup"><span data-stu-id="412a1-132">Learn more about page navigation</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a><span data-ttu-id="412a1-133">Filtrera en rapport</span><span class="sxs-lookup"><span data-stu-id="412a1-133">Filter a report</span></span>
<span data-ttu-id="412a1-134">hello JavaScript API ger grundläggande och avancerade filtreringsfunktioner för inbäddade rapporter och rapportsidor.</span><span class="sxs-lookup"><span data-stu-id="412a1-134">hello JavaScript API provides basic and advanced filtering capabilities for embedded reports and report pages.</span></span> <span data-ttu-id="412a1-135">Försök hello [filtrering demo programmet](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), och granska vissa inledande koden här.</span><span class="sxs-lookup"><span data-stu-id="412a1-135">Try hello [filtering demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), and review some introductory code here.</span></span>  

#### <a name="basic-filters"></a><span data-ttu-id="412a1-136">Grundläggande filter</span><span class="sxs-lookup"><span data-stu-id="412a1-136">Basic filters</span></span>
<span data-ttu-id="412a1-137">Ett grundläggande filter placeras på en kolumn eller hierarki nivå och innehåller en lista över värden tooinclude eller utelämna.</span><span class="sxs-lookup"><span data-stu-id="412a1-137">A basic filter is placed on a column or hierarchy level and contains a list of values tooinclude or exclude.</span></span>

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a><span data-ttu-id="412a1-138">Avancerade filter</span><span class="sxs-lookup"><span data-stu-id="412a1-138">Advanced filters</span></span>
<span data-ttu-id="412a1-139">Avancerade filter använder hello logisk operator och eller eller och acceptera en eller två villkor, var och en med sin egen operator och ett värde.</span><span class="sxs-lookup"><span data-stu-id="412a1-139">Advanced filters use hello logical operator AND or OR, and accept one or two conditions, each with their own operator and value.</span></span> <span data-ttu-id="412a1-140">Villkor som stöds är:</span><span class="sxs-lookup"><span data-stu-id="412a1-140">Supported conditions are:</span></span>

* <span data-ttu-id="412a1-141">Ingen</span><span class="sxs-lookup"><span data-stu-id="412a1-141">None</span></span>
* <span data-ttu-id="412a1-142">LessThan</span><span class="sxs-lookup"><span data-stu-id="412a1-142">LessThan</span></span>
* <span data-ttu-id="412a1-143">LessThanOrEqual</span><span class="sxs-lookup"><span data-stu-id="412a1-143">LessThanOrEqual</span></span>
* <span data-ttu-id="412a1-144">GreaterThan</span><span class="sxs-lookup"><span data-stu-id="412a1-144">GreaterThan</span></span>
* <span data-ttu-id="412a1-145">GreaterThanOrEqual</span><span class="sxs-lookup"><span data-stu-id="412a1-145">GreaterThanOrEqual</span></span>
* <span data-ttu-id="412a1-146">Contains</span><span class="sxs-lookup"><span data-stu-id="412a1-146">Contains</span></span>
* <span data-ttu-id="412a1-147">DoesNotContain</span><span class="sxs-lookup"><span data-stu-id="412a1-147">DoesNotContain</span></span>
* <span data-ttu-id="412a1-148">StartsWith</span><span class="sxs-lookup"><span data-stu-id="412a1-148">StartsWith</span></span>
* <span data-ttu-id="412a1-149">DoesNotStartWith</span><span class="sxs-lookup"><span data-stu-id="412a1-149">DoesNotStartWith</span></span>
* <span data-ttu-id="412a1-150">Is</span><span class="sxs-lookup"><span data-stu-id="412a1-150">Is</span></span>
* <span data-ttu-id="412a1-151">IsNot</span><span class="sxs-lookup"><span data-stu-id="412a1-151">IsNot</span></span>
* <span data-ttu-id="412a1-152">IsBlank</span><span class="sxs-lookup"><span data-stu-id="412a1-152">IsBlank</span></span>
* <span data-ttu-id="412a1-153">IsNotBlank</span><span class="sxs-lookup"><span data-stu-id="412a1-153">IsNotBlank</span></span>

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[<span data-ttu-id="412a1-154">Läs mer om filtrering</span><span class="sxs-lookup"><span data-stu-id="412a1-154">Learn more about filtering</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)

### <a name="handling-events"></a><span data-ttu-id="412a1-155">Hantera händelser</span><span class="sxs-lookup"><span data-stu-id="412a1-155">Handling events</span></span>
<span data-ttu-id="412a1-156">Dessutom toosending information till hello iframe ditt program kan också ta emot information om hello följande händelser som kommer från hello iframe:</span><span class="sxs-lookup"><span data-stu-id="412a1-156">In addition toosending information into hello iframe, your application can also receive information on hello following events coming from hello iframe:</span></span>

* <span data-ttu-id="412a1-157">Bädda in</span><span class="sxs-lookup"><span data-stu-id="412a1-157">Embed</span></span>
  * <span data-ttu-id="412a1-158">inläst</span><span class="sxs-lookup"><span data-stu-id="412a1-158">loaded</span></span>
  * <span data-ttu-id="412a1-159">fel</span><span class="sxs-lookup"><span data-stu-id="412a1-159">error</span></span>
* <span data-ttu-id="412a1-160">Rapporter</span><span class="sxs-lookup"><span data-stu-id="412a1-160">Reports</span></span>
  * <span data-ttu-id="412a1-161">pageChanged</span><span class="sxs-lookup"><span data-stu-id="412a1-161">pageChanged</span></span>
  * <span data-ttu-id="412a1-162">dataSelected (kommer snart)</span><span class="sxs-lookup"><span data-stu-id="412a1-162">dataSelected (coming soon)</span></span>

[<span data-ttu-id="412a1-163">Läs mer om att hantera händelser</span><span class="sxs-lookup"><span data-stu-id="412a1-163">Learn more about handling events</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)

## <a name="next-steps"></a><span data-ttu-id="412a1-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="412a1-164">Next steps</span></span>
<span data-ttu-id="412a1-165">Mer information om hello Power BI JavaScript API för utcheckning hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="412a1-165">For more information about hello Power BI JavaScript API, check out hello following links:</span></span>

* [<span data-ttu-id="412a1-166">JavaScript API Wiki</span><span class="sxs-lookup"><span data-stu-id="412a1-166">JavaScript API Wiki</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
* [<span data-ttu-id="412a1-167">Referens för objektmodell</span><span class="sxs-lookup"><span data-stu-id="412a1-167">Object model reference</span></span>](https://microsoft.github.io/powerbi-models/modules/_models_.html)
* <span data-ttu-id="412a1-168">Exempel</span><span class="sxs-lookup"><span data-stu-id="412a1-168">Samples</span></span>
  * [<span data-ttu-id="412a1-169">Angular</span><span class="sxs-lookup"><span data-stu-id="412a1-169">Angular</span></span>](http://azure-samples.github.io/powerbi-angular-client)
  * [<span data-ttu-id="412a1-170">Ember</span><span class="sxs-lookup"><span data-stu-id="412a1-170">Ember</span></span>](https://github.com/Microsoft/powerbi-ember)
* [<span data-ttu-id="412a1-171">Live-demo</span><span class="sxs-lookup"><span data-stu-id="412a1-171">Live demo</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)

