---
title: Rensa en Azure CDN-slutpunkt | Microsoft Docs
description: "Lär dig mer om att rensa allt cachelagrat innehåll från en Azure CDN-slutpunkt."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: b035c232bb58d653960190d4974cc3789d55a51d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a><span data-ttu-id="aade5-103">Rensa en Azure CDN-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="aade5-103">Purge an Azure CDN endpoint</span></span>
## <a name="overview"></a><span data-ttu-id="aade5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="aade5-104">Overview</span></span>
<span data-ttu-id="aade5-105">Azure CDN edge noder cachelagras tillgångar tills tillgångens time to live (TTL) upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="aade5-105">Azure CDN edge nodes will cache assets until the asset's time-to-live (TTL) expires.</span></span>  <span data-ttu-id="aade5-106">När tillgångens TTL upphör att gälla, när en klient begär tillgången från kantnoden, kantnoden hämtar en ny uppdaterad kopia av tillgången som fungerar klientbegäran och store uppdatera cachen.</span><span class="sxs-lookup"><span data-stu-id="aade5-106">After the asset's TTL expires, when a client requests the asset from the edge node, the edge node will retrieve a new updated copy of the asset to serve the client request and store refresh the cache.</span></span>

<span data-ttu-id="aade5-107">Bäst att kontrollera att användarna hämta alltid den senaste kopian av dina tillgångar är att version dina tillgångar för varje uppdatering och publicera dem som nya URL: er.</span><span class="sxs-lookup"><span data-stu-id="aade5-107">The best practice to make sure your users always obtain the latest copy of your assets is to version your assets for each update and publish them as new URLs.</span></span>  <span data-ttu-id="aade5-108">CDN kommer omedelbart att hämta nya tillgångar efter nästa klientbegäranden.</span><span class="sxs-lookup"><span data-stu-id="aade5-108">CDN will immediately retrieve the new assets for the next client requests.</span></span>  <span data-ttu-id="aade5-109">Ibland kanske du vill ta bort cachelagrat innehåll från alla noder i kant och tvinga att alla ska hämta nya uppdaterade tillgångar.</span><span class="sxs-lookup"><span data-stu-id="aade5-109">Sometimes you may wish to purge cached content from all edge nodes and force them all to retrieve new updated assets.</span></span>  <span data-ttu-id="aade5-110">Detta kan bero på uppdateringar att ditt webbprogram eller för att snabbt uppdatera tillgångar som innehåller felaktig information.</span><span class="sxs-lookup"><span data-stu-id="aade5-110">This might be due to updates to your web application, or to quickly update assets that contain incorrect information.</span></span>

> [!TIP]
> <span data-ttu-id="aade5-111">Observera att rensa endast tar bort cachelagrat innehåll på CDN edge-servrar.</span><span class="sxs-lookup"><span data-stu-id="aade5-111">Note that purging only clears the cached content on the CDN edge servers.</span></span>  <span data-ttu-id="aade5-112">Alla underordnade cacheminnen, till exempel proxyservrar och lokala browser cacheminnen kan fortfarande håller en cachelagrad kopia av filen.</span><span class="sxs-lookup"><span data-stu-id="aade5-112">Any downstream caches, such as proxy servers and local browser caches, may still hold a cached copy of the file.</span></span>  <span data-ttu-id="aade5-113">Det är viktigt att komma ihåg detta när du ställer in en fil time-to-live.</span><span class="sxs-lookup"><span data-stu-id="aade5-113">It's important to remember this when you set a file's time-to-live.</span></span>  <span data-ttu-id="aade5-114">Du kan tvinga en underordnad klienten begära den senaste versionen av filen genom att ge den ett unikt namn varje gång du uppdaterar den eller genom att utnyttja [fråga cachelagring av frågesträngar](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="aade5-114">You can force a downstream client to request the latest version of your file by giving it a unique name every time you update it, or by taking advantage of [query string caching](cdn-query-string.md).</span></span>  
> 
> 

<span data-ttu-id="aade5-115">Den här självstudiekursen vägleder dig genom att rensa tillgångar från alla noder i kanten av en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="aade5-115">This tutorial walks you through purging assets from all edge nodes of an endpoint.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="aade5-116">Genomgång</span><span class="sxs-lookup"><span data-stu-id="aade5-116">Walkthrough</span></span>
1. <span data-ttu-id="aade5-117">I den [Azure Portal](https://portal.azure.com), bläddra till CDN-profilen som innehåller den slutpunkt som du vill rensa.</span><span class="sxs-lookup"><span data-stu-id="aade5-117">In the [Azure Portal](https://portal.azure.com), browse to the CDN profile containing the endpoint you wish to purge.</span></span>
2. <span data-ttu-id="aade5-118">Klicka på Rensa i bladet CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="aade5-118">From the CDN profile blade, click the purge button.</span></span>
   
    ![CDN-profilbladet](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    <span data-ttu-id="aade5-120">Rensa blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="aade5-120">The Purge blade opens.</span></span>
   
    ![CDN Rensa bladet](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. <span data-ttu-id="aade5-122">Välj serviceadressen som du vill rensa i listrutan URL på bladet Rensa.</span><span class="sxs-lookup"><span data-stu-id="aade5-122">On the Purge blade, select the service address you wish to purge from the URL dropdown.</span></span>
   
    ![Rensa formulär](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > <span data-ttu-id="aade5-124">Du kan också öppna bladet Rensa genom att klicka på den **Rensa** knappen på bladet CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="aade5-124">You can also get to the Purge blade by clicking the **Purge** button on the CDN endpoint blade.</span></span>  <span data-ttu-id="aade5-125">I så fall den **URL** fältet kommer att vara förifyllda med serviceadressen för den specifika slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="aade5-125">In that case, the **URL** field will be pre-populated with the service address of that specific endpoint.</span></span>
   > 
   > 
4. <span data-ttu-id="aade5-126">Välj vilka resurser som du vill ta bort från noderna kant.</span><span class="sxs-lookup"><span data-stu-id="aade5-126">Select what assets you wish to purge from the edge nodes.</span></span>  <span data-ttu-id="aade5-127">Om du vill rensa alla tillgångar, klickar du på den **Rensa alla** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="aade5-127">If you wish to clear all assets, click the **Purge all** checkbox.</span></span>  <span data-ttu-id="aade5-128">Annars anger du sökvägen till varje tillgång som du vill rensa i den **sökväg** textruta.</span><span class="sxs-lookup"><span data-stu-id="aade5-128">Otherwise, type the path of each asset you wish to purge in the **Path** textbox.</span></span> <span data-ttu-id="aade5-129">Under format stöds i sökvägen.</span><span class="sxs-lookup"><span data-stu-id="aade5-129">Below formats are supported in the path.</span></span>
    1. <span data-ttu-id="aade5-130">**Den enda URL Rensa**: Rensa enskilda tillgångar genom att ange den fullständiga URL, med eller utan filtillägget, t.ex.`/pictures/strasbourg.png`;`/pictures/strasbourg`</span><span class="sxs-lookup"><span data-stu-id="aade5-130">**Single URL purge**: Purge individual asset by specifying the full URL, with or without the file extension, e.g.,`/pictures/strasbourg.png`; `/pictures/strasbourg`</span></span>
    2. <span data-ttu-id="aade5-131">**Jokertecken Rensa**: Asterisk (\*) kan användas som jokertecken.</span><span class="sxs-lookup"><span data-stu-id="aade5-131">**Wildcard purge**: Asterisk (\*) may be used as a wildcard.</span></span> <span data-ttu-id="aade5-132">Rensa alla mappar, undermappar och filer på en slutpunkt med `/*` i sökvägen eller rensa alla undermappar och filer under en viss mapp genom att ange mappen följt av `/*`, t.ex.`/pictures/*`.</span><span class="sxs-lookup"><span data-stu-id="aade5-132">Purge all folders, sub-folders and files under an endpoint with `/*` in the path or purge all sub-folders and files under a specific folder by specifying the folder followed by `/*`, e.g.,`/pictures/*`.</span></span>  <span data-ttu-id="aade5-133">Observera att rensa jokertecken inte stöds av Azure CDN från Akamai för närvarande.</span><span class="sxs-lookup"><span data-stu-id="aade5-133">Note that wildcard purge is not supported by Azure CDN from Akamai currently.</span></span> 
    3. <span data-ttu-id="aade5-134">**Roten domän Rensa**: Rensa roten för slutpunkten med ”/” i sökvägen.</span><span class="sxs-lookup"><span data-stu-id="aade5-134">**Root domain purge**: Purge the root of the endpoint with "/" in the path.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="aade5-135">Sökvägar måste anges för rensning och måste vara en relativ URL som passar följande [reguljärt uttryck](https://msdn.microsoft.com/library/az24scfc.aspx).</span><span class="sxs-lookup"><span data-stu-id="aade5-135">Paths must be specified for purge and must be a relative URL that fit the following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx).</span></span> <span data-ttu-id="aade5-136">**Rensa alla** och **jokertecken Rensa** stöds inte av **Azure CDN från Akamai** för närvarande.</span><span class="sxs-lookup"><span data-stu-id="aade5-136">**Purge all** and **Wildcard purge** not supported by **Azure CDN from Akamai** currently.</span></span>
   > > <span data-ttu-id="aade5-137">Enstaka URL-rensning`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span><span class="sxs-lookup"><span data-stu-id="aade5-137">Single URL purge `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span></span>  
   > > <span data-ttu-id="aade5-138">Frågesträng`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="aade5-138">Query string `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span></span>  
   > > <span data-ttu-id="aade5-139">Jokertecken Rensa `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span><span class="sxs-lookup"><span data-stu-id="aade5-139">Wildcard purge `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span></span> 
   > 
   > <span data-ttu-id="aade5-140">Flera **sökväg** textrutor visas när du har angett texten så att du kan skapa en lista över flera resurser.</span><span class="sxs-lookup"><span data-stu-id="aade5-140">More **Path** textboxes will appear after you enter text to allow you to build a list of multiple assets.</span></span>  <span data-ttu-id="aade5-141">Du kan ta bort tillgångar i listan genom att klicka på knappen med ellipstecknet (...).</span><span class="sxs-lookup"><span data-stu-id="aade5-141">You can delete assets from the list by clicking the ellipsis (...) button.</span></span>
   > 
5. <span data-ttu-id="aade5-142">Klicka på den **Rensa** knappen.</span><span class="sxs-lookup"><span data-stu-id="aade5-142">Click the **Purge** button.</span></span>
   
    ![Rensa knappen](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> <span data-ttu-id="aade5-144">Rensa begäranden tar cirka 2-3 minuter att bearbeta med **Azure CDN från Verizon** (Standard och Premium) och cirka 7: e minut med **Azure CDN från Akamai**.</span><span class="sxs-lookup"><span data-stu-id="aade5-144">Purge requests take approximately 2-3 minutes to process with **Azure CDN from Verizon** (Standard and Premium), and approximately 7 minutes with **Azure CDN from Akamai**.</span></span>  <span data-ttu-id="aade5-145">Azure CDN är begränsad till 50 samtidiga Rensa begäranden samtidigt.</span><span class="sxs-lookup"><span data-stu-id="aade5-145">Azure CDN has a limit of 50 concurrent purge requests at any given time.</span></span> 
> 
> 

## <a name="see-also"></a><span data-ttu-id="aade5-146">Se även</span><span class="sxs-lookup"><span data-stu-id="aade5-146">See also</span></span>
* [<span data-ttu-id="aade5-147">Läsa in tillgångar för en Azure CDN-slutpunkt i förväg</span><span class="sxs-lookup"><span data-stu-id="aade5-147">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="aade5-148">Azure CDN REST API-referens - rensa eller i förväg läsa in en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="aade5-148">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

