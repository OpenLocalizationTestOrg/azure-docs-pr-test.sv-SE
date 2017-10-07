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
redirect_document_id: True
ms.openlocfilehash: 4c5f0eee4aa04ce823321d52985374c52850f0d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a><span data-ttu-id="8d42f-103">Azure Machine Learning-rekommendationer – JavaScript-integrering</span><span class="sxs-lookup"><span data-stu-id="8d42f-103">Azure Machine Learning Recommendations - JavaScript Integration</span></span>
> [!NOTE]
> <span data-ttu-id="8d42f-104">Du bör börja använda hello kognitiva rekommendationer API-tjänsten i stället för den här versionen.</span><span class="sxs-lookup"><span data-stu-id="8d42f-104">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="8d42f-105">hello kognitiva Rekommendationstjänsten ersätter den här tjänsten och alla nya funktioner för hello det kommer fram.</span><span class="sxs-lookup"><span data-stu-id="8d42f-105">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="8d42f-106">Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.</span><span class="sxs-lookup"><span data-stu-id="8d42f-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="8d42f-107">Lär dig mer om [migrera toohello ny kognitiva tjänst](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="8d42f-107">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="8d42f-108">Det här dokumentet beskriver hur toointegrate webbplatsen med hjälp av JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8d42f-108">This document depict how toointegrate your site using JavaScript.</span></span> <span data-ttu-id="8d42f-109">hello JavaScript kan du toosend datainsamling händelser och tooconsume rekommendationer när du skapar en rekommendation modell.</span><span class="sxs-lookup"><span data-stu-id="8d42f-109">hello JavaScript enables you toosend Data Acquisition events and tooconsume recommendations once you build a recommendation model.</span></span> <span data-ttu-id="8d42f-110">Alla åtgärder via JS kan även göras från serversidan.</span><span class="sxs-lookup"><span data-stu-id="8d42f-110">All operations done via JS can be also done from server side.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="8d42f-111">1. Allmänt: översikt</span><span class="sxs-lookup"><span data-stu-id="8d42f-111">1. General Overview</span></span>
<span data-ttu-id="8d42f-112">Integrera din webbplats med Azure ML rekommendationer består på 2 faser:</span><span class="sxs-lookup"><span data-stu-id="8d42f-112">Integrating your site with Azure ML Recommendations consist on 2 Phases:</span></span>

1. <span data-ttu-id="8d42f-113">Skicka händelser till Azure ML rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="8d42f-113">Send Events into Azure ML Recommendations.</span></span> <span data-ttu-id="8d42f-114">Detta aktiverar toobuild en rekommendation modell.</span><span class="sxs-lookup"><span data-stu-id="8d42f-114">This will enable toobuild a recommendation model.</span></span>
2. <span data-ttu-id="8d42f-115">Använda hello rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="8d42f-115">Consume hello recommendations.</span></span> <span data-ttu-id="8d42f-116">När hello modellen har skapats kan du använda hello rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="8d42f-116">After hello model is built you can consume hello recommendations.</span></span> <span data-ttu-id="8d42f-117">(Det här dokumentet beskriver inte hur hello toobuild en modell, läsa Snabbstart guiden tooget mer information om hur).</span><span class="sxs-lookup"><span data-stu-id="8d42f-117">(This document does not explain how toobuild a model, read hello quick start guide tooget more information on how).</span></span>

<span data-ttu-id="8d42f-118"><ins>Fas I</ins></span><span class="sxs-lookup"><span data-stu-id="8d42f-118"><ins>Phase I</ins></span></span>

<span data-ttu-id="8d42f-119">I hello hello första fasen i en liten JavaScript-bibliotek som gör att infoga i HTML-sidor toosend händelser när de inträffar på hello HTML-sida i Azure ML rekommendationer servrar (via Data marknaden):</span><span class="sxs-lookup"><span data-stu-id="8d42f-119">In hello first phase you insert into your html pages a small JavaScript library that enables hello page toosend events as they occur on hello html page into Azure ML Recommendations servers (via Data Market):</span></span>

![Drawing1][1]

<span data-ttu-id="8d42f-121"><ins>Fas II</ins></span><span class="sxs-lookup"><span data-stu-id="8d42f-121"><ins>Phase II</ins></span></span>

<span data-ttu-id="8d42f-122">I hello välja andra fasen när du vill tooshow hello rekommendationer om hello sidan du något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="8d42f-122">In hello second phase when you want tooshow hello recommendations on hello page you select one of hello following options:</span></span>

<span data-ttu-id="8d42f-123">1. servern (på hello fas av rendering på sidan) anropar Azure ML rekommendationer Server (via Data marknaden) tooget rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="8d42f-123">1.Your server (on hello phase of page rendering) calls Azure ML Recommendations Server (via Data Market) tooget recommendations.</span></span> <span data-ttu-id="8d42f-124">hello resultatet innehåller en lista med objekt-ID: t. Servern måste tooenrich hello resultat med hello objekt metadata (t.ex. bilder, beskrivning) och skicka hello skapade sidan toohello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="8d42f-124">hello results include a list of items id. Your server needs tooenrich hello results with hello items Meta data (e.g. images, description) and send hello created page toohello browser.</span></span>

![Drawing2][2]

<span data-ttu-id="8d42f-126">2. hello är andra alternativet toouse hello små JavaScript-fil från fas en tooget en enkel lista över rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="8d42f-126">2.hello other option is toouse hello small JavaScript file from phase one tooget a simple list of recommended items.</span></span> <span data-ttu-id="8d42f-127">mottagna här hello data är smidigare än hello något i hello första alternativet.</span><span class="sxs-lookup"><span data-stu-id="8d42f-127">hello data received here is leaner than hello one in hello first option.</span></span>

![Drawing3][3]

## <a name="2-prerequisites"></a><span data-ttu-id="8d42f-129">2. Krav</span><span class="sxs-lookup"><span data-stu-id="8d42f-129">2. Prerequisites</span></span>
1. <span data-ttu-id="8d42f-130">Skapa en ny modell med hello API: er.</span><span class="sxs-lookup"><span data-stu-id="8d42f-130">Create a new model using hello APIs.</span></span> <span data-ttu-id="8d42f-131">Om hur finns hello snabbstartsguiden toodo den.</span><span class="sxs-lookup"><span data-stu-id="8d42f-131">See hello Quick start guide on how toodo it.</span></span>
2. <span data-ttu-id="8d42f-132">Koda din &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; med base64.</span><span class="sxs-lookup"><span data-stu-id="8d42f-132">Encode your &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; with base64.</span></span> <span data-ttu-id="8d42f-133">(Detta används för hello grundläggande autentisering tooenable hello JS kod toocall hello API: er).</span><span class="sxs-lookup"><span data-stu-id="8d42f-133">(This will be used for hello basic authentication tooenable hello JS code toocall hello APIs).</span></span>

## <a name="3-send-data-acquisition-events-using-javascript"></a><span data-ttu-id="8d42f-134">3. Skicka datainsamling händelser med hjälp av JavaScript</span><span class="sxs-lookup"><span data-stu-id="8d42f-134">3. Send Data Acquisition events using JavaScript</span></span>
<span data-ttu-id="8d42f-135">hello följande underlätta skicka händelser:</span><span class="sxs-lookup"><span data-stu-id="8d42f-135">hello following steps facilitate sending events:</span></span>

1. <span data-ttu-id="8d42f-136">Inkludera JQuery biblioteket i din kod.</span><span class="sxs-lookup"><span data-stu-id="8d42f-136">Include JQuery library in your code.</span></span> <span data-ttu-id="8d42f-137">Du kan ladda ned det från nuget i hello följande URL: en.</span><span class="sxs-lookup"><span data-stu-id="8d42f-137">You can download it from nuget in hello following URL.</span></span>
   
     <span data-ttu-id="8d42f-138">http://www.nuget.org/Packages/jQuery/1.8.2</span><span class="sxs-lookup"><span data-stu-id="8d42f-138">http://www.nuget.org/packages/jQuery/1.8.2</span></span>
2. <span data-ttu-id="8d42f-139">Inkludera hello rekommendationer JavaScript-bibliotek från hello följande URL: http://aka.ms/RecoJSLib1</span><span class="sxs-lookup"><span data-stu-id="8d42f-139">Include hello Recommendations Java Script library from hello following URL: http://aka.ms/RecoJSLib1</span></span>
3. <span data-ttu-id="8d42f-140">Initiera Azure ML rekommendationer bibliotek med hello lämpliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="8d42f-140">Initialize Azure ML Recommendations library with hello appropriate parameters.</span></span>
   
     <span data-ttu-id="8d42f-141"><script>AzureMLRecommendationsStart (”<base64encoding of username:key>”, ”< model_id >”); </script> 
4. Skicka hello händelse.</span><span class="sxs-lookup"><span data-stu-id="8d42f-141"><script> AzureMLRecommendationsStart("<base64encoding of username:key>", "<model_id>"); </script>
4. Send hello appropriate event.</span></span> <span data-ttu-id="8d42f-142">Detaljerad avsnittet nedan på alla typer av händelser (exempel på Välj händelsen) <script> om (typeof AzureMLRecommendationsEvent == ”Odefinierad”) {</span><span class="sxs-lookup"><span data-stu-id="8d42f-142">See detailed section below on all type of events (example of click event)  <script> if (typeof AzureMLRecommendationsEvent=="undefined") {</span></span>         
                     <span data-ttu-id="8d42f-143">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script></span><span class="sxs-lookup"><span data-stu-id="8d42f-143">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" }); </script></span></span>

### <a name="31----limitations-and-browser-support"></a><span data-ttu-id="8d42f-144">3.1.</span><span class="sxs-lookup"><span data-stu-id="8d42f-144">3.1.</span></span>    <span data-ttu-id="8d42f-145">Begränsningar och stöd för webbläsare</span><span class="sxs-lookup"><span data-stu-id="8d42f-145">Limitations and Browser Support</span></span>
<span data-ttu-id="8d42f-146">Detta är en referensimplementering och ges eftersom.</span><span class="sxs-lookup"><span data-stu-id="8d42f-146">This is a reference implementation and it is given as is.</span></span> <span data-ttu-id="8d42f-147">Det ska ha stöd för alla större webbläsare.</span><span class="sxs-lookup"><span data-stu-id="8d42f-147">It should support all major browsers.</span></span>

### <a name="32----type-of-events"></a><span data-ttu-id="8d42f-148">3.2.</span><span class="sxs-lookup"><span data-stu-id="8d42f-148">3.2.</span></span>    <span data-ttu-id="8d42f-149">Typ av händelser</span><span class="sxs-lookup"><span data-stu-id="8d42f-149">Type of Events</span></span>
<span data-ttu-id="8d42f-150">Det finns 5 typer av händelser som hello biblioteket stöder: klickar du på, rekommendation klickar på Lägg till tooShop kundvagn, ta bort från köp kundvagn och inköp.</span><span class="sxs-lookup"><span data-stu-id="8d42f-150">There are 5 types of event that hello library supports: Click, Recommendation Click, Add tooShop Cart, Remove from Shop Cart and Purchase.</span></span> <span data-ttu-id="8d42f-151">Det finns ytterligare en händelse som används tooset hello användarkontext kallas inloggningen.</span><span class="sxs-lookup"><span data-stu-id="8d42f-151">There is an additional event that is used tooset hello user context called Login.</span></span>

#### <a name="321-click-event"></a><span data-ttu-id="8d42f-152">3.2.1.</span><span class="sxs-lookup"><span data-stu-id="8d42f-152">3.2.1.</span></span> <span data-ttu-id="8d42f-153">Klicka på händelse</span><span class="sxs-lookup"><span data-stu-id="8d42f-153">Click Event</span></span>
<span data-ttu-id="8d42f-154">Den här händelsen ska användas när en användare klickade på ett objekt.</span><span class="sxs-lookup"><span data-stu-id="8d42f-154">This event should be used any time a user clicked on an item.</span></span> <span data-ttu-id="8d42f-155">Vanligtvis när användaren klickar på ett objekt öppnas en ny sida med detaljer om objektet hello; den här händelsen ska utlösas på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="8d42f-155">Usually when user clicks on an item a new page is opened with hello item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="8d42f-156">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="8d42f-156">Parameters:</span></span>

* <span data-ttu-id="8d42f-157">händelse (sträng, obligatoriska) ”Klicka”</span><span class="sxs-lookup"><span data-stu-id="8d42f-157">event (string, mandatory) - “click”</span></span>
* <span data-ttu-id="8d42f-158">objekt (sträng, obligatoriska) - Unik identifierare för hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-158">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="8d42f-159">itemName (sträng, valfritt) - hello namnet på hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-159">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="8d42f-160">itemDescription (sträng, valfritt) - hello beskrivning av hello objektet</span><span class="sxs-lookup"><span data-stu-id="8d42f-160">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="8d42f-161">itemCategory (sträng, valfritt) - hello kategori hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-161">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

<span data-ttu-id="8d42f-162">Eller med valfria data:</span><span class="sxs-lookup"><span data-stu-id="8d42f-162">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a><span data-ttu-id="8d42f-163">3.2.2.</span><span class="sxs-lookup"><span data-stu-id="8d42f-163">3.2.2.</span></span> <span data-ttu-id="8d42f-164">Rekommendation klickar du på händelse</span><span class="sxs-lookup"><span data-stu-id="8d42f-164">Recommendation Click Event</span></span>
<span data-ttu-id="8d42f-165">Den här händelsen ska användas när en användare klickade på ett objekt som tagits emot från Azure ML rekommendationer som ett rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="8d42f-165">This event should be used any time a user clicked on an item that was received from Azure ML Recommendations as a recommended item.</span></span> <span data-ttu-id="8d42f-166">Vanligtvis när användaren klickar på ett objekt öppnas en ny sida med detaljer om objektet hello; den här händelsen ska utlösas på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="8d42f-166">Usually when user clicks on an item a new page is opened with hello item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="8d42f-167">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="8d42f-167">Parameters:</span></span>

* <span data-ttu-id="8d42f-168">händelse (sträng, obligatoriska) ”recommendationclick”</span><span class="sxs-lookup"><span data-stu-id="8d42f-168">event (string, mandatory) - “recommendationclick”</span></span>
* <span data-ttu-id="8d42f-169">objekt (sträng, obligatoriska) - Unik identifierare för hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-169">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="8d42f-170">itemName (sträng, valfritt) - hello namnet på hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-170">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="8d42f-171">itemDescription (sträng, valfritt) - hello beskrivning av hello objektet</span><span class="sxs-lookup"><span data-stu-id="8d42f-171">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="8d42f-172">itemCategory (sträng, valfritt) - hello kategori hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-172">itemCategory (string, optional) - hello category of hello item</span></span>
* <span data-ttu-id="8d42f-173">frö (Strängmatrisen, valfritt) - hello frö som genererade hello rekommendation frågan.</span><span class="sxs-lookup"><span data-stu-id="8d42f-173">seeds (string array, optional) - hello seeds that generated hello recommendation query.</span></span>
* <span data-ttu-id="8d42f-174">recoList (Strängmatrisen, valfritt) - hello resultatet av hello rekommendation begäran som genererade hello-objekt som du har klickat på.</span><span class="sxs-lookup"><span data-stu-id="8d42f-174">recoList (string array, optional) - hello result of hello recommendation request that generated hello item that was clicked.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

<span data-ttu-id="8d42f-175">Eller med valfria data:</span><span class="sxs-lookup"><span data-stu-id="8d42f-175">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a><span data-ttu-id="8d42f-176">3.2.3.</span><span class="sxs-lookup"><span data-stu-id="8d42f-176">3.2.3.</span></span> <span data-ttu-id="8d42f-177">Lägg till i kundvagn-händelse</span><span class="sxs-lookup"><span data-stu-id="8d42f-177">Add Shopping Cart Event</span></span>
<span data-ttu-id="8d42f-178">Den här händelsen ska användas när hello användare lägger till ett objekt toohello kundvagn.</span><span class="sxs-lookup"><span data-stu-id="8d42f-178">This event should be used when hello user add an item toohello shopping cart.</span></span>
<span data-ttu-id="8d42f-179">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="8d42f-179">Parameters:</span></span>

* <span data-ttu-id="8d42f-180">händelse (sträng, obligatoriska) ”addshopcart”</span><span class="sxs-lookup"><span data-stu-id="8d42f-180">event (string, mandatory) - “addshopcart”</span></span>
* <span data-ttu-id="8d42f-181">objekt (sträng, obligatoriska) - Unik identifierare för hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-181">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="8d42f-182">itemName (sträng, valfritt) - hello namnet på hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-182">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="8d42f-183">itemDescription (sträng, valfritt) - hello beskrivning av hello objektet</span><span class="sxs-lookup"><span data-stu-id="8d42f-183">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="8d42f-184">itemCategory (sträng, valfritt) - hello kategori hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-184">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a><span data-ttu-id="8d42f-185">3.2.4.</span><span class="sxs-lookup"><span data-stu-id="8d42f-185">3.2.4.</span></span> <span data-ttu-id="8d42f-186">Ta bort Shopping kundvagn händelse</span><span class="sxs-lookup"><span data-stu-id="8d42f-186">Remove Shopping Cart Event</span></span>
<span data-ttu-id="8d42f-187">Den här händelsen ska användas när hello användaren tar bort en artikel toohello kundvagn.</span><span class="sxs-lookup"><span data-stu-id="8d42f-187">This event should be used when hello user removes an item toohello shopping cart.</span></span>

<span data-ttu-id="8d42f-188">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="8d42f-188">Parameters:</span></span>

* <span data-ttu-id="8d42f-189">händelse (sträng, obligatoriska) ”removeshopcart”</span><span class="sxs-lookup"><span data-stu-id="8d42f-189">event (string, mandatory) - “removeshopcart”</span></span>
* <span data-ttu-id="8d42f-190">objekt (sträng, obligatoriska) - Unik identifierare för hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-190">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="8d42f-191">itemName (sträng, valfritt) - hello namnet på hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-191">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="8d42f-192">itemDescription (sträng, valfritt) - hello beskrivning av hello objektet</span><span class="sxs-lookup"><span data-stu-id="8d42f-192">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="8d42f-193">itemCategory (sträng, valfritt) - hello kategori hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-193">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a><span data-ttu-id="8d42f-194">3.2.5.</span><span class="sxs-lookup"><span data-stu-id="8d42f-194">3.2.5.</span></span> <span data-ttu-id="8d42f-195">Köp händelse</span><span class="sxs-lookup"><span data-stu-id="8d42f-195">Purchase Event</span></span>
<span data-ttu-id="8d42f-196">Den här händelsen ska användas när användaren hello köpte hans kundvagn.</span><span class="sxs-lookup"><span data-stu-id="8d42f-196">This event should be used when hello user purchased his shopping cart.</span></span>

<span data-ttu-id="8d42f-197">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="8d42f-197">Parameters:</span></span>

* <span data-ttu-id="8d42f-198">händelse (sträng) ”köpa”</span><span class="sxs-lookup"><span data-stu-id="8d42f-198">event (string) - “purchase”</span></span>
* <span data-ttu-id="8d42f-199">objekt (inköpta []) - matris som innehåller en post för varje objekt har köpt.</span><span class="sxs-lookup"><span data-stu-id="8d42f-199">items ( Purchased[] ) - Array holding an entry for each item purchased.</span></span><br><br>
  <span data-ttu-id="8d42f-200">Köpta format:</span><span class="sxs-lookup"><span data-stu-id="8d42f-200">Purchased format:</span></span>
  * <span data-ttu-id="8d42f-201">objekt (sträng) - Unik identifierare för hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="8d42f-201">item (string) - Unique identifier of hello item.</span></span>
  * <span data-ttu-id="8d42f-202">Antal (int eller string) - objekt som har köpts.</span><span class="sxs-lookup"><span data-stu-id="8d42f-202">count (int or string) - number of items that were purchased.</span></span>
  * <span data-ttu-id="8d42f-203">pris (flyttal eller sträng) - Fritextfält - hello priset hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="8d42f-203">price (float or string) - optional field - hello price of hello item.</span></span>

<span data-ttu-id="8d42f-204">hello exemplet nedan visar inköp av 3 objekt (33, 34, 35), två med alla fält (artikel, antal, pris) och en (artikel 34) utan ett pris.</span><span class="sxs-lookup"><span data-stu-id="8d42f-204">hello example below shows purchase of 3 items (33, 34, 35), two with all fields populated (item, count, price) and one (item 34) without a price.</span></span>

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a><span data-ttu-id="8d42f-205">3.2.6.</span><span class="sxs-lookup"><span data-stu-id="8d42f-205">3.2.6.</span></span> <span data-ttu-id="8d42f-206">Användaren logga in händelsen</span><span class="sxs-lookup"><span data-stu-id="8d42f-206">User Login Event</span></span>
<span data-ttu-id="8d42f-207">Azure ML rekommendationer händelse biblioteket skapar och Använd en cookie i ordning tooidentify händelser som kommer från hello samma webbläsare.</span><span class="sxs-lookup"><span data-stu-id="8d42f-207">Azure ML Recommendations Event library creates and use a cookie in order tooidentify events that came from hello same browser.</span></span> <span data-ttu-id="8d42f-208">I ordning tooimprove hello modell kan resultat Azure ML rekommendationer tooset en unik identifiering för användaren som åsidosätter hello cookie användning.</span><span class="sxs-lookup"><span data-stu-id="8d42f-208">In order tooimprove hello model results Azure ML Recommendations enables tooset a user unique identification that will override hello cookie usage.</span></span>

<span data-ttu-id="8d42f-209">Den här händelsen ska användas efter hello användaren logga in tooyour.</span><span class="sxs-lookup"><span data-stu-id="8d42f-209">This event should be used after hello user login tooyour site.</span></span>

<span data-ttu-id="8d42f-210">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="8d42f-210">Parameters:</span></span>

* <span data-ttu-id="8d42f-211">händelse (sträng) ”userlogin”</span><span class="sxs-lookup"><span data-stu-id="8d42f-211">event (string) - “userlogin”</span></span>
* <span data-ttu-id="8d42f-212">användare (sträng) unik identifiering av hello användare.</span><span class="sxs-lookup"><span data-stu-id="8d42f-212">user (string) - unique identification of hello user.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a><span data-ttu-id="8d42f-213">4. Använda rekommendationer via JavaScript</span><span class="sxs-lookup"><span data-stu-id="8d42f-213">4. Consume Recommendations via JavaScript</span></span>
<span data-ttu-id="8d42f-214">hello-kod som förbrukar hello rekommendation utlöses av vissa JavaScript-händelse av webbsidan hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="8d42f-214">hello code that consumes hello recommendation is triggered by some JavaScript event by hello client’s webpage.</span></span> <span data-ttu-id="8d42f-215">hello rekommendation svaret innehåller hello rekommenderade objekt-ID: N, deras namn och deras klassificering.</span><span class="sxs-lookup"><span data-stu-id="8d42f-215">hello recommendation response includes hello recommended items Ids, their names and their ratings.</span></span> <span data-ttu-id="8d42f-216">Det är bästa toouse det här alternativet endast för en lista över visning av hello rekommenderade objekt - mer komplexa hantering (till exempel att lägga till hello objektmetadata) ska göras på hello server sida-integration.</span><span class="sxs-lookup"><span data-stu-id="8d42f-216">It’s best toouse this option only for a list display of hello recommended items - more complex handling (such as adding hello item’s metadata) should be done on hello server side integration.</span></span>

### <a name="41-consume-recommendations"></a><span data-ttu-id="8d42f-217">4.1 använda rekommendationer</span><span class="sxs-lookup"><span data-stu-id="8d42f-217">4.1 Consume Recommendations</span></span>
<span data-ttu-id="8d42f-218">tooconsume rekommendationer som du behöver tooinclude hello krävs JavaScript-bibliotek på sidan och toocall AzureMLRecommendationsStart.</span><span class="sxs-lookup"><span data-stu-id="8d42f-218">tooconsume recommendations you need tooinclude hello required JavaScript libraries in your page and toocall AzureMLRecommendationsStart.</span></span> <span data-ttu-id="8d42f-219">Avsnittet 2.</span><span class="sxs-lookup"><span data-stu-id="8d42f-219">See section 2.</span></span>

<span data-ttu-id="8d42f-220">tooconsume rekommendationer för ett eller flera objekt som du behöver toocall anropade en metod: AzureMLRecommendationsGetI2IRecommendation.</span><span class="sxs-lookup"><span data-stu-id="8d42f-220">tooconsume recommendations for one or more items you need toocall a method called: AzureMLRecommendationsGetI2IRecommendation.</span></span>

<span data-ttu-id="8d42f-221">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="8d42f-221">Parameters:</span></span>

* <span data-ttu-id="8d42f-222">objekt (strängmatris) - en eller flera objekt tooget rekommendationer för.</span><span class="sxs-lookup"><span data-stu-id="8d42f-222">items (array of strings) - One or more items tooget recommendations for.</span></span> <span data-ttu-id="8d42f-223">Om du använder en Fbt-version kan du ange här bara ett objekt.</span><span class="sxs-lookup"><span data-stu-id="8d42f-223">If you consume an Fbt build then you can set here only one item.</span></span>
* <span data-ttu-id="8d42f-224">numberOfResults (int) - antal resultat som krävs.</span><span class="sxs-lookup"><span data-stu-id="8d42f-224">numberOfResults (int) - number of required results.</span></span>
* <span data-ttu-id="8d42f-225">includeMetadata (boolean, valfritt) - om ange too'true' anger hello metadata fältet måste ha fyllts i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="8d42f-225">includeMetadata (boolean, optional) - if set too‘true’ indicates that hello metadata field must be populated in hello result.</span></span>
* <span data-ttu-id="8d42f-226">Bearbetning av funktionen - en funktion som hanterar hello rekommendationer returneras.</span><span class="sxs-lookup"><span data-stu-id="8d42f-226">Processing function - a function that will handle hello recommendations returned.</span></span> <span data-ttu-id="8d42f-227">hello data returneras en matris med:</span><span class="sxs-lookup"><span data-stu-id="8d42f-227">hello data is returned as an array of:</span></span>
  * <span data-ttu-id="8d42f-228">Artikel - objektet unikt id</span><span class="sxs-lookup"><span data-stu-id="8d42f-228">Item - item unique id</span></span>
  * <span data-ttu-id="8d42f-229">Name - objektet namn (om det finns i katalogen)</span><span class="sxs-lookup"><span data-stu-id="8d42f-229">name - item name (if exist in catalog)</span></span>
  * <span data-ttu-id="8d42f-230">klassificeringen - rekommendation klassificering</span><span class="sxs-lookup"><span data-stu-id="8d42f-230">rating - recommendation rating</span></span>
  * <span data-ttu-id="8d42f-231">metadata - en sträng som representerar hello metadata för hello objekt</span><span class="sxs-lookup"><span data-stu-id="8d42f-231">metadata - a string that represents hello metadata of hello item</span></span>

<span data-ttu-id="8d42f-232">Exempel: följande kod hello begär 8 rekommendationer för objektet ”64f6eb0d-947a-4c18-a16c-888da9e228ba” (och inte anger includeMetadata - implicit står det krävs inga metadata), sammanfoga sedan hello resultat i en buffert.</span><span class="sxs-lookup"><span data-stu-id="8d42f-232">Example: hello following code requests 8 recommendations for item "64f6eb0d-947a-4c18-a16c-888da9e228ba" (and by not specifying includeMetadata - it implicitly says that no metadata is required), it then concatenate hello results into a buffer.</span></span>

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
