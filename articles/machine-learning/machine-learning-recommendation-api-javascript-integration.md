---
title: 'Machine Learning rekommendationer: JavaScript Integration | Microsoft Docs'
description: "Azure Machine Learning rekommendationer - integrering för JavaScript - dokumentation"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: bbbb5bb6-489d-4a62-a2ae-f36237e9e2e1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 8f27962d097bffc2a03de80244ae41d6573a4bf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a><span data-ttu-id="29a11-103">Azure Machine Learning-rekommendationer – JavaScript-integrering</span><span class="sxs-lookup"><span data-stu-id="29a11-103">Azure Machine Learning Recommendations - JavaScript Integration</span></span>
> [!NOTE]
> <span data-ttu-id="29a11-104">Du bör börja använda kognitiva Rekommendationstjänsten API i stället för den här versionen.</span><span class="sxs-lookup"><span data-stu-id="29a11-104">You should start using the Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="29a11-105">Kognitiva Rekommendationstjänsten ersätter den här tjänsten och de nya funktionerna det kommer fram.</span><span class="sxs-lookup"><span data-stu-id="29a11-105">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="29a11-106">Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.</span><span class="sxs-lookup"><span data-stu-id="29a11-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="29a11-107">Lär dig mer om [migrera till den nya kognitiva tjänsten](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="29a11-107">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="29a11-108">Det här dokumentet beskriver hur du integrerar din webbplats med JavaScript.</span><span class="sxs-lookup"><span data-stu-id="29a11-108">This document depict how to integrate your site using JavaScript.</span></span> <span data-ttu-id="29a11-109">JavaScript kan du skicka händelser för datainsamling och använda rekommendationer när du skapar en rekommendation modell.</span><span class="sxs-lookup"><span data-stu-id="29a11-109">The JavaScript enables you to send Data Acquisition events and to consume recommendations once you build a recommendation model.</span></span> <span data-ttu-id="29a11-110">Alla åtgärder via JS kan även göras från serversidan.</span><span class="sxs-lookup"><span data-stu-id="29a11-110">All operations done via JS can be also done from server side.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="29a11-111">1. Allmänt: översikt</span><span class="sxs-lookup"><span data-stu-id="29a11-111">1. General Overview</span></span>
<span data-ttu-id="29a11-112">Integrera din webbplats med Azure ML rekommendationer består på 2 faser:</span><span class="sxs-lookup"><span data-stu-id="29a11-112">Integrating your site with Azure ML Recommendations consist on 2 Phases:</span></span>

1. <span data-ttu-id="29a11-113">Skicka händelser till Azure ML rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="29a11-113">Send Events into Azure ML Recommendations.</span></span> <span data-ttu-id="29a11-114">Detta gör att du skapar en rekommendation modell.</span><span class="sxs-lookup"><span data-stu-id="29a11-114">This will enable to build a recommendation model.</span></span>
2. <span data-ttu-id="29a11-115">Använda rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="29a11-115">Consume the recommendations.</span></span> <span data-ttu-id="29a11-116">När modellen har skapats kan du använda rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="29a11-116">After the model is built you can consume the recommendations.</span></span> <span data-ttu-id="29a11-117">(Det här dokumentet inte beskriver hur du skapar en modell, Läs snabbstartsguiden vill ha mer information om hur).</span><span class="sxs-lookup"><span data-stu-id="29a11-117">(This document does not explain how to build a model, read the quick start guide to get more information on how).</span></span>

<span data-ttu-id="29a11-118"><ins>Fas I</ins></span><span class="sxs-lookup"><span data-stu-id="29a11-118"><ins>Phase I</ins></span></span>

<span data-ttu-id="29a11-119">I den första fasen in html-sidor en liten JavaScript-bibliotek som gör att sidan för att skicka händelser när de inträffar på HTML-sidan i Azure ML rekommendationer servrar (via Data marknaden):</span><span class="sxs-lookup"><span data-stu-id="29a11-119">In the first phase you insert into your html pages a small JavaScript library that enables the page to send events as they occur on the html page into Azure ML Recommendations servers (via Data Market):</span></span>

![Drawing1][1]

<span data-ttu-id="29a11-121"><ins>Fas II</ins></span><span class="sxs-lookup"><span data-stu-id="29a11-121"><ins>Phase II</ins></span></span>

<span data-ttu-id="29a11-122">I den andra fasen när du vill visa rekommendationerna på sidan Välj något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="29a11-122">In the second phase when you want to show the recommendations on the page you select one of the following options:</span></span>

<span data-ttu-id="29a11-123">1. servern (på i vilken fas av rendering på sidan) anropar Azure ML rekommendationer Server (via Data marknaden) för att få rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="29a11-123">1.Your server (on the phase of page rendering) calls Azure ML Recommendations Server (via Data Market) to get recommendations.</span></span> <span data-ttu-id="29a11-124">Resultatet innehåller en lista med objekt-ID: t.</span><span class="sxs-lookup"><span data-stu-id="29a11-124">The results include a list of items id.</span></span> <span data-ttu-id="29a11-125">Servern behöver förbättra resultatet med objekt metadata (t.ex. bilder, beskrivning) och skicka sidan som du har skapat till webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="29a11-125">Your server needs to enrich the results with the items Meta data (e.g. images, description) and send the created page to the browser.</span></span>

![Drawing2][2]

<span data-ttu-id="29a11-127">2. det andra alternativet är att använda små JavaScript-filen från den första fasen för att hämta en enkel lista över rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="29a11-127">2.The other option is to use the small JavaScript file from phase one to get a simple list of recommended items.</span></span> <span data-ttu-id="29a11-128">Mottagna här data är smidigare än det i det första alternativet.</span><span class="sxs-lookup"><span data-stu-id="29a11-128">The data received here is leaner than the one in the first option.</span></span>

![Drawing3][3]

## <a name="2-prerequisites"></a><span data-ttu-id="29a11-130">2. Krav</span><span class="sxs-lookup"><span data-stu-id="29a11-130">2. Prerequisites</span></span>
1. <span data-ttu-id="29a11-131">Skapa en ny modell som använder API: erna.</span><span class="sxs-lookup"><span data-stu-id="29a11-131">Create a new model using the APIs.</span></span> <span data-ttu-id="29a11-132">Om hur du gör finns i snabbstartsguiden.</span><span class="sxs-lookup"><span data-stu-id="29a11-132">See the Quick start guide on how to do it.</span></span>
2. <span data-ttu-id="29a11-133">Koda din &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; med base64.</span><span class="sxs-lookup"><span data-stu-id="29a11-133">Encode your &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; with base64.</span></span> <span data-ttu-id="29a11-134">(Detta ska användas för grundläggande autentisering för att aktivera JS-kod för att anropa API: er).</span><span class="sxs-lookup"><span data-stu-id="29a11-134">(This will be used for the basic authentication to enable the JS code to call the APIs).</span></span>

## <a name="3-send-data-acquisition-events-using-javascript"></a><span data-ttu-id="29a11-135">3. Skicka datainsamling händelser med hjälp av JavaScript</span><span class="sxs-lookup"><span data-stu-id="29a11-135">3. Send Data Acquisition events using JavaScript</span></span>
<span data-ttu-id="29a11-136">Följande steg underlätta skicka händelser:</span><span class="sxs-lookup"><span data-stu-id="29a11-136">The following steps facilitate sending events:</span></span>

1. <span data-ttu-id="29a11-137">Inkludera JQuery biblioteket i din kod.</span><span class="sxs-lookup"><span data-stu-id="29a11-137">Include JQuery library in your code.</span></span> <span data-ttu-id="29a11-138">Du kan ladda ned det från nuget i följande URL.</span><span class="sxs-lookup"><span data-stu-id="29a11-138">You can download it from nuget in the following URL.</span></span>
   
     <span data-ttu-id="29a11-139">http://www.nuget.org/Packages/jQuery/1.8.2</span><span class="sxs-lookup"><span data-stu-id="29a11-139">http://www.nuget.org/packages/jQuery/1.8.2</span></span>
2. <span data-ttu-id="29a11-140">Innehåller rekommendationer JavaScript-bibliotek från följande URL: http://aka.ms/RecoJSLib1</span><span class="sxs-lookup"><span data-stu-id="29a11-140">Include the Recommendations Java Script library from the following URL: http://aka.ms/RecoJSLib1</span></span>
3. <span data-ttu-id="29a11-141">Starta Azure ML rekommendationer biblioteket med lämpliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="29a11-141">Initialize Azure ML Recommendations library with the appropriate parameters.</span></span>
   
     <span data-ttu-id="29a11-142"><script>AzureMLRecommendationsStart (”<base64encoding of username:key>”, ”< model_id >”); </script> 
4. Skicka en händelse.</span><span class="sxs-lookup"><span data-stu-id="29a11-142"><script> AzureMLRecommendationsStart("<base64encoding of username:key>", "<model_id>"); </script>
4. Send the appropriate event.</span></span> <span data-ttu-id="29a11-143">Detaljerad avsnittet nedan på alla typer av händelser (exempel på Välj händelsen) <script> om (typeof AzureMLRecommendationsEvent == ”Odefinierad”) {</span><span class="sxs-lookup"><span data-stu-id="29a11-143">See detailed section below on all type of events (example of click event)  <script> if (typeof AzureMLRecommendationsEvent=="undefined") {</span></span>         
                     <span data-ttu-id="29a11-144">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script></span><span class="sxs-lookup"><span data-stu-id="29a11-144">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" }); </script></span></span>

### <a name="31----limitations-and-browser-support"></a><span data-ttu-id="29a11-145">3.1.</span><span class="sxs-lookup"><span data-stu-id="29a11-145">3.1.</span></span>    <span data-ttu-id="29a11-146">Begränsningar och stöd för webbläsare</span><span class="sxs-lookup"><span data-stu-id="29a11-146">Limitations and Browser Support</span></span>
<span data-ttu-id="29a11-147">Detta är en referensimplementering och ges eftersom.</span><span class="sxs-lookup"><span data-stu-id="29a11-147">This is a reference implementation and it is given as is.</span></span> <span data-ttu-id="29a11-148">Det ska ha stöd för alla större webbläsare.</span><span class="sxs-lookup"><span data-stu-id="29a11-148">It should support all major browsers.</span></span>

### <a name="32----type-of-events"></a><span data-ttu-id="29a11-149">3.2.</span><span class="sxs-lookup"><span data-stu-id="29a11-149">3.2.</span></span>    <span data-ttu-id="29a11-150">Typ av händelser</span><span class="sxs-lookup"><span data-stu-id="29a11-150">Type of Events</span></span>
<span data-ttu-id="29a11-151">Det finns 5 typer av händelser som har stöd för biblioteket: på, rekommendation klickar på Lägg till till köp vagnen tas bort från köp kundvagn och inköp.</span><span class="sxs-lookup"><span data-stu-id="29a11-151">There are 5 types of event that the library supports: Click, Recommendation Click, Add to Shop Cart, Remove from Shop Cart and Purchase.</span></span> <span data-ttu-id="29a11-152">Det finns ytterligare en händelse som används för att ange användarkontext kallas inloggningen.</span><span class="sxs-lookup"><span data-stu-id="29a11-152">There is an additional event that is used to set the user context called Login.</span></span>

#### <a name="321-click-event"></a><span data-ttu-id="29a11-153">3.2.1.</span><span class="sxs-lookup"><span data-stu-id="29a11-153">3.2.1.</span></span> <span data-ttu-id="29a11-154">Klicka på händelse</span><span class="sxs-lookup"><span data-stu-id="29a11-154">Click Event</span></span>
<span data-ttu-id="29a11-155">Den här händelsen ska användas när en användare klickade på ett objekt.</span><span class="sxs-lookup"><span data-stu-id="29a11-155">This event should be used any time a user clicked on an item.</span></span> <span data-ttu-id="29a11-156">Vanligtvis när användaren klickar på ett objekt öppnas en ny sida med detaljer om objektet; den här händelsen ska utlösas på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="29a11-156">Usually when user clicks on an item a new page is opened with the item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="29a11-157">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="29a11-157">Parameters:</span></span>

* <span data-ttu-id="29a11-158">händelse (sträng, obligatoriska) ”Klicka”</span><span class="sxs-lookup"><span data-stu-id="29a11-158">event (string, mandatory) - “click”</span></span>
* <span data-ttu-id="29a11-159">objekt (sträng, obligatoriska) - Unik identifierare för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-159">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="29a11-160">itemName (sträng, valfritt) - namnet på objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-160">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="29a11-161">itemDescription (sträng, valfritt) – beskrivning av artikeln</span><span class="sxs-lookup"><span data-stu-id="29a11-161">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="29a11-162">itemCategory (sträng, valfritt) - kategorin för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-162">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

<span data-ttu-id="29a11-163">Eller med valfria data:</span><span class="sxs-lookup"><span data-stu-id="29a11-163">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a><span data-ttu-id="29a11-164">3.2.2.</span><span class="sxs-lookup"><span data-stu-id="29a11-164">3.2.2.</span></span> <span data-ttu-id="29a11-165">Rekommendation klickar du på händelse</span><span class="sxs-lookup"><span data-stu-id="29a11-165">Recommendation Click Event</span></span>
<span data-ttu-id="29a11-166">Den här händelsen ska användas när en användare klickade på ett objekt som tagits emot från Azure ML rekommendationer som ett rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="29a11-166">This event should be used any time a user clicked on an item that was received from Azure ML Recommendations as a recommended item.</span></span> <span data-ttu-id="29a11-167">Vanligtvis när användaren klickar på ett objekt öppnas en ny sida med detaljer om objektet; den här händelsen ska utlösas på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="29a11-167">Usually when user clicks on an item a new page is opened with the item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="29a11-168">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="29a11-168">Parameters:</span></span>

* <span data-ttu-id="29a11-169">händelse (sträng, obligatoriska) ”recommendationclick”</span><span class="sxs-lookup"><span data-stu-id="29a11-169">event (string, mandatory) - “recommendationclick”</span></span>
* <span data-ttu-id="29a11-170">objekt (sträng, obligatoriska) - Unik identifierare för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-170">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="29a11-171">itemName (sträng, valfritt) - namnet på objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-171">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="29a11-172">itemDescription (sträng, valfritt) – beskrivning av artikeln</span><span class="sxs-lookup"><span data-stu-id="29a11-172">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="29a11-173">itemCategory (sträng, valfritt) - kategorin för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-173">itemCategory (string, optional) - the category of the item</span></span>
* <span data-ttu-id="29a11-174">Initierar (Strängmatrisen, valfritt) - frö som genererade rekommendation frågan.</span><span class="sxs-lookup"><span data-stu-id="29a11-174">seeds (string array, optional) - the seeds that generated the recommendation query.</span></span>
* <span data-ttu-id="29a11-175">recoList (Strängmatrisen, valfritt) - resultatet av rekommendation begäran som skapade det objekt som användes.</span><span class="sxs-lookup"><span data-stu-id="29a11-175">recoList (string array, optional) - the result of the recommendation request that generated the item that was clicked.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

<span data-ttu-id="29a11-176">Eller med valfria data:</span><span class="sxs-lookup"><span data-stu-id="29a11-176">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a><span data-ttu-id="29a11-177">3.2.3.</span><span class="sxs-lookup"><span data-stu-id="29a11-177">3.2.3.</span></span> <span data-ttu-id="29a11-178">Lägg till i kundvagn-händelse</span><span class="sxs-lookup"><span data-stu-id="29a11-178">Add Shopping Cart Event</span></span>
<span data-ttu-id="29a11-179">Den här händelsen ska användas när användaren lägger till ett objekt i kundvagnen.</span><span class="sxs-lookup"><span data-stu-id="29a11-179">This event should be used when the user add an item to the shopping cart.</span></span>
<span data-ttu-id="29a11-180">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="29a11-180">Parameters:</span></span>

* <span data-ttu-id="29a11-181">händelse (sträng, obligatoriska) ”addshopcart”</span><span class="sxs-lookup"><span data-stu-id="29a11-181">event (string, mandatory) - “addshopcart”</span></span>
* <span data-ttu-id="29a11-182">objekt (sträng, obligatoriska) - Unik identifierare för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-182">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="29a11-183">itemName (sträng, valfritt) - namnet på objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-183">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="29a11-184">itemDescription (sträng, valfritt) – beskrivning av artikeln</span><span class="sxs-lookup"><span data-stu-id="29a11-184">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="29a11-185">itemCategory (sträng, valfritt) - kategorin för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-185">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a><span data-ttu-id="29a11-186">3.2.4.</span><span class="sxs-lookup"><span data-stu-id="29a11-186">3.2.4.</span></span> <span data-ttu-id="29a11-187">Ta bort Shopping kundvagn händelse</span><span class="sxs-lookup"><span data-stu-id="29a11-187">Remove Shopping Cart Event</span></span>
<span data-ttu-id="29a11-188">Den här händelsen ska användas när användaren tar bort ett objekt i kundvagnen.</span><span class="sxs-lookup"><span data-stu-id="29a11-188">This event should be used when the user removes an item to the shopping cart.</span></span>

<span data-ttu-id="29a11-189">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="29a11-189">Parameters:</span></span>

* <span data-ttu-id="29a11-190">händelse (sträng, obligatoriska) ”removeshopcart”</span><span class="sxs-lookup"><span data-stu-id="29a11-190">event (string, mandatory) - “removeshopcart”</span></span>
* <span data-ttu-id="29a11-191">objekt (sträng, obligatoriska) - Unik identifierare för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-191">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="29a11-192">itemName (sträng, valfritt) - namnet på objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-192">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="29a11-193">itemDescription (sträng, valfritt) – beskrivning av artikeln</span><span class="sxs-lookup"><span data-stu-id="29a11-193">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="29a11-194">itemCategory (sträng, valfritt) - kategorin för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-194">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a><span data-ttu-id="29a11-195">3.2.5.</span><span class="sxs-lookup"><span data-stu-id="29a11-195">3.2.5.</span></span> <span data-ttu-id="29a11-196">Köp händelse</span><span class="sxs-lookup"><span data-stu-id="29a11-196">Purchase Event</span></span>
<span data-ttu-id="29a11-197">Den här händelsen ska användas när användaren har köpt hans kundvagn.</span><span class="sxs-lookup"><span data-stu-id="29a11-197">This event should be used when the user purchased his shopping cart.</span></span>

<span data-ttu-id="29a11-198">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="29a11-198">Parameters:</span></span>

* <span data-ttu-id="29a11-199">händelse (sträng) ”köpa”</span><span class="sxs-lookup"><span data-stu-id="29a11-199">event (string) - “purchase”</span></span>
* <span data-ttu-id="29a11-200">objekt (inköpta []) - matris som innehåller en post för varje objekt har köpt.</span><span class="sxs-lookup"><span data-stu-id="29a11-200">items ( Purchased[] ) - Array holding an entry for each item purchased.</span></span><br><br>
  <span data-ttu-id="29a11-201">Köpta format:</span><span class="sxs-lookup"><span data-stu-id="29a11-201">Purchased format:</span></span>
  * <span data-ttu-id="29a11-202">objekt (sträng) - Unik identifierare för objektet.</span><span class="sxs-lookup"><span data-stu-id="29a11-202">item (string) - Unique identifier of the item.</span></span>
  * <span data-ttu-id="29a11-203">Antal (int eller string) - objekt som har köpts.</span><span class="sxs-lookup"><span data-stu-id="29a11-203">count (int or string) - number of items that were purchased.</span></span>
  * <span data-ttu-id="29a11-204">pris (flyttal eller sträng) - valfria fält - priset för artikeln.</span><span class="sxs-lookup"><span data-stu-id="29a11-204">price (float or string) - optional field - the price of the item.</span></span>

<span data-ttu-id="29a11-205">Exemplet nedan visar inköp av 3 objekt (33, 34, 35), två med alla fält (artikel, antal, pris) och en (artikel 34) utan ett pris.</span><span class="sxs-lookup"><span data-stu-id="29a11-205">The example below shows purchase of 3 items (33, 34, 35), two with all fields populated (item, count, price) and one (item 34) without a price.</span></span>

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a><span data-ttu-id="29a11-206">3.2.6.</span><span class="sxs-lookup"><span data-stu-id="29a11-206">3.2.6.</span></span> <span data-ttu-id="29a11-207">Användaren logga in händelsen</span><span class="sxs-lookup"><span data-stu-id="29a11-207">User Login Event</span></span>
<span data-ttu-id="29a11-208">Azure ML rekommendationer händelse biblioteket skapar och använder en cookie för att identifiera händelser som kommer från samma webbläsare.</span><span class="sxs-lookup"><span data-stu-id="29a11-208">Azure ML Recommendations Event library creates and use a cookie in order to identify events that came from the same browser.</span></span> <span data-ttu-id="29a11-209">För att kunna förbättra modellen resultaten Azure ML rekommendationer kan du ange en unik identifiering för användaren som åsidosätter cookie-användning.</span><span class="sxs-lookup"><span data-stu-id="29a11-209">In order to improve the model results Azure ML Recommendations enables to set a user unique identification that will override the cookie usage.</span></span>

<span data-ttu-id="29a11-210">Den här händelsen ska användas när du loggar in som användare till din webbplats.</span><span class="sxs-lookup"><span data-stu-id="29a11-210">This event should be used after the user login to your site.</span></span>

<span data-ttu-id="29a11-211">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="29a11-211">Parameters:</span></span>

* <span data-ttu-id="29a11-212">händelse (sträng) ”userlogin”</span><span class="sxs-lookup"><span data-stu-id="29a11-212">event (string) - “userlogin”</span></span>
* <span data-ttu-id="29a11-213">användare (sträng) unik identifiering av användaren.</span><span class="sxs-lookup"><span data-stu-id="29a11-213">user (string) - unique identification of the user.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a><span data-ttu-id="29a11-214">4. Använda rekommendationer via JavaScript</span><span class="sxs-lookup"><span data-stu-id="29a11-214">4. Consume Recommendations via JavaScript</span></span>
<span data-ttu-id="29a11-215">Den kod som förbrukar rekommendationen utlöses av vissa JavaScript-händelse av klientens webbsidan.</span><span class="sxs-lookup"><span data-stu-id="29a11-215">The code that consumes the recommendation is triggered by some JavaScript event by the client’s webpage.</span></span> <span data-ttu-id="29a11-216">Rekommendation svaret innehåller rekommenderade objekt-ID, namn och deras klassificeringar.</span><span class="sxs-lookup"><span data-stu-id="29a11-216">The recommendation response includes the recommended items Ids, their names and their ratings.</span></span> <span data-ttu-id="29a11-217">Det är bäst att använda det här alternativet endast för en lista visas de rekommenderade objekten - mer komplexa hantering (till exempel att lägga till objektets metadata) ska göras på server-sida-integration.</span><span class="sxs-lookup"><span data-stu-id="29a11-217">It’s best to use this option only for a list display of the recommended items - more complex handling (such as adding the item’s metadata) should be done on the server side integration.</span></span>

### <a name="41-consume-recommendations"></a><span data-ttu-id="29a11-218">4.1 använda rekommendationer</span><span class="sxs-lookup"><span data-stu-id="29a11-218">4.1 Consume Recommendations</span></span>
<span data-ttu-id="29a11-219">Om du vill använda innehåller rekommendationer som du behöver JavaScript-bibliotek som krävs på sidan och för att anropa AzureMLRecommendationsStart.</span><span class="sxs-lookup"><span data-stu-id="29a11-219">To consume recommendations you need to include the required JavaScript libraries in your page and to call AzureMLRecommendationsStart.</span></span> <span data-ttu-id="29a11-220">Avsnittet 2.</span><span class="sxs-lookup"><span data-stu-id="29a11-220">See section 2.</span></span>

<span data-ttu-id="29a11-221">Att använda rekommendationer för ett eller flera objekt som du behöver anropa en metod som kallas: AzureMLRecommendationsGetI2IRecommendation.</span><span class="sxs-lookup"><span data-stu-id="29a11-221">To consume recommendations for one or more items you need to call a method called: AzureMLRecommendationsGetI2IRecommendation.</span></span>

<span data-ttu-id="29a11-222">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="29a11-222">Parameters:</span></span>

* <span data-ttu-id="29a11-223">objekt (strängmatris) - en eller flera objekt att få rekommendationer för.</span><span class="sxs-lookup"><span data-stu-id="29a11-223">items (array of strings) - One or more items to get recommendations for.</span></span> <span data-ttu-id="29a11-224">Om du använder en Fbt-version kan du ange här bara ett objekt.</span><span class="sxs-lookup"><span data-stu-id="29a11-224">If you consume an Fbt build then you can set here only one item.</span></span>
* <span data-ttu-id="29a11-225">numberOfResults (int) - antal resultat som krävs.</span><span class="sxs-lookup"><span data-stu-id="29a11-225">numberOfResults (int) - number of required results.</span></span>
* <span data-ttu-id="29a11-226">includeMetadata (boolean, valfritt) - om inställd på 'true' innebär att metadata-fältet måste ha fyllts i resultatet.</span><span class="sxs-lookup"><span data-stu-id="29a11-226">includeMetadata (boolean, optional) - if set to ‘true’ indicates that the metadata field must be populated in the result.</span></span>
* <span data-ttu-id="29a11-227">Bearbetning av funktionen - en funktion som hanterar rekommendationerna som returneras.</span><span class="sxs-lookup"><span data-stu-id="29a11-227">Processing function - a function that will handle the recommendations returned.</span></span> <span data-ttu-id="29a11-228">Data returneras en matris med:</span><span class="sxs-lookup"><span data-stu-id="29a11-228">The data is returned as an array of:</span></span>
  * <span data-ttu-id="29a11-229">Artikel - objektet unikt id</span><span class="sxs-lookup"><span data-stu-id="29a11-229">Item - item unique id</span></span>
  * <span data-ttu-id="29a11-230">Name - objektet namn (om det finns i katalogen)</span><span class="sxs-lookup"><span data-stu-id="29a11-230">name - item name (if exist in catalog)</span></span>
  * <span data-ttu-id="29a11-231">klassificeringen - rekommendation klassificering</span><span class="sxs-lookup"><span data-stu-id="29a11-231">rating - recommendation rating</span></span>
  * <span data-ttu-id="29a11-232">metadata - en sträng som representerar metadata för objektet</span><span class="sxs-lookup"><span data-stu-id="29a11-232">metadata - a string that represents the metadata of the item</span></span>

<span data-ttu-id="29a11-233">Exempel: Följande kod begär 8 rekommendationer för objektet ”64f6eb0d-947a-4c18-a16c-888da9e228ba” (och inte anger includeMetadata - implicit står det krävs inga metadata), sammanfoga sedan resultaten i en buffert.</span><span class="sxs-lookup"><span data-stu-id="29a11-233">Example: The following code requests 8 recommendations for item "64f6eb0d-947a-4c18-a16c-888da9e228ba" (and by not specifying includeMetadata - it implicitly says that no metadata is required), it then concatenate the results into a buffer.</span></span>

        <script>
             var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                 var buff = "";
                 for (var ii = 0; ii < reco.length; ii++) {
                       buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                 }
                 alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
