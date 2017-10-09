---
title: aaaPurge en Azure CDN-slutpunkt | Microsoft Docs
description: "Lär dig hur toopurge alla cachelagrat innehåll från en Azure CDN-slutpunkt."
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
ms.openlocfilehash: a09f4a49aa1e2d7655ecae44b5126c11c28fd599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a><span data-ttu-id="eabbc-103">Rensa en Azure CDN-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="eabbc-103">Purge an Azure CDN endpoint</span></span>
## <a name="overview"></a><span data-ttu-id="eabbc-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="eabbc-104">Overview</span></span>
<span data-ttu-id="eabbc-105">Azure CDN edge noder cachelagras tillgångar tills hello tillgången time to live (TTL) upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="eabbc-105">Azure CDN edge nodes will cache assets until hello asset's time-to-live (TTL) expires.</span></span>  <span data-ttu-id="eabbc-106">När hello tillgången TTL upphör att gälla, när en klient begär hello tillgångsinformation från hello kantnod, hämtar en ny uppdaterad kopia av hello tillgången tooserve hello klientbegäran hello kantnod och lagra hello cacheuppdatering.</span><span class="sxs-lookup"><span data-stu-id="eabbc-106">After hello asset's TTL expires, when a client requests hello asset from hello edge node, hello edge node will retrieve a new updated copy of hello asset tooserve hello client request and store refresh hello cache.</span></span>

<span data-ttu-id="eabbc-107">hello bästa praxis toomake till användarna alltid få hello senaste kopian av dina tillgångar är tooversion dina tillgångar för varje uppdatera och publicera dem som nya URL: er.</span><span class="sxs-lookup"><span data-stu-id="eabbc-107">hello best practice toomake sure your users always obtain hello latest copy of your assets is tooversion your assets for each update and publish them as new URLs.</span></span>  <span data-ttu-id="eabbc-108">CDN kommer omedelbart att hämta hello nya tillgångar för hello nästa klientbegäranden.</span><span class="sxs-lookup"><span data-stu-id="eabbc-108">CDN will immediately retrieve hello new assets for hello next client requests.</span></span>  <span data-ttu-id="eabbc-109">Ibland kanske du vill toopurge cachelagrat innehåll från alla noder i kant och tvinga dem alla tooretrieve nya uppdaterade tillgångar.</span><span class="sxs-lookup"><span data-stu-id="eabbc-109">Sometimes you may wish toopurge cached content from all edge nodes and force them all tooretrieve new updated assets.</span></span>  <span data-ttu-id="eabbc-110">Detta kan bero på att tooupdates tooyour webbprogram eller tooquickly uppdatering tillgångar som innehåller felaktig information.</span><span class="sxs-lookup"><span data-stu-id="eabbc-110">This might be due tooupdates tooyour web application, or tooquickly update assets that contain incorrect information.</span></span>

> [!TIP]
> <span data-ttu-id="eabbc-111">Observera att rensa endast rensar hello cachelagrat innehåll på hello CDN edge-servrar.</span><span class="sxs-lookup"><span data-stu-id="eabbc-111">Note that purging only clears hello cached content on hello CDN edge servers.</span></span>  <span data-ttu-id="eabbc-112">Alla underordnade cacheminnen, till exempel proxyservrar och lokala browser cacheminnen kan fortfarande håller en cachelagrad kopia av hello-filen.</span><span class="sxs-lookup"><span data-stu-id="eabbc-112">Any downstream caches, such as proxy servers and local browser caches, may still hold a cached copy of hello file.</span></span>  <span data-ttu-id="eabbc-113">Det är viktigt tooremember detta när du ställer in en fil time-to-live.</span><span class="sxs-lookup"><span data-stu-id="eabbc-113">It's important tooremember this when you set a file's time-to-live.</span></span>  <span data-ttu-id="eabbc-114">Du kan tvinga en underordnad klienten toorequest hello senaste versionen av filen genom att ge den ett unikt namn varje gång du uppdaterar den eller genom att utnyttja [fråga cachelagring av frågesträngar](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="eabbc-114">You can force a downstream client toorequest hello latest version of your file by giving it a unique name every time you update it, or by taking advantage of [query string caching](cdn-query-string.md).</span></span>  
> 
> 

<span data-ttu-id="eabbc-115">Den här självstudiekursen vägleder dig genom att rensa tillgångar från alla noder i kanten av en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="eabbc-115">This tutorial walks you through purging assets from all edge nodes of an endpoint.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="eabbc-116">Genomgång</span><span class="sxs-lookup"><span data-stu-id="eabbc-116">Walkthrough</span></span>
1. <span data-ttu-id="eabbc-117">I hello [Azure Portal](https://portal.azure.com), bläddra toohello CDN-profil som innehåller hello-slutpunkt som du vill toopurge.</span><span class="sxs-lookup"><span data-stu-id="eabbc-117">In hello [Azure Portal](https://portal.azure.com), browse toohello CDN profile containing hello endpoint you wish toopurge.</span></span>
2. <span data-ttu-id="eabbc-118">Från hello CDN-profilbladet Klicka hello Rensa.</span><span class="sxs-lookup"><span data-stu-id="eabbc-118">From hello CDN profile blade, click hello purge button.</span></span>
   
    ![CDN-profilbladet](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    <span data-ttu-id="eabbc-120">hello Rensa blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="eabbc-120">hello Purge blade opens.</span></span>
   
    ![CDN Rensa bladet](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. <span data-ttu-id="eabbc-122">Rensa bladet på hello, Välj hello-adress som du vill toopurge hello URL listrutan.</span><span class="sxs-lookup"><span data-stu-id="eabbc-122">On hello Purge blade, select hello service address you wish toopurge from hello URL dropdown.</span></span>
   
    ![Rensa formulär](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > <span data-ttu-id="eabbc-124">Du kan också få toohello Rensa bladet genom att klicka på hello **Rensa** hello CDN-slutpunkten bladet-knappen.</span><span class="sxs-lookup"><span data-stu-id="eabbc-124">You can also get toohello Purge blade by clicking hello **Purge** button on hello CDN endpoint blade.</span></span>  <span data-ttu-id="eabbc-125">I så fall hello **URL** fältet kommer att vara förifyllda med hello-adress för den specifika slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="eabbc-125">In that case, hello **URL** field will be pre-populated with hello service address of that specific endpoint.</span></span>
   > 
   > 
4. <span data-ttu-id="eabbc-126">Välj vilka resurser du vill toopurge från hello edge noder.</span><span class="sxs-lookup"><span data-stu-id="eabbc-126">Select what assets you wish toopurge from hello edge nodes.</span></span>  <span data-ttu-id="eabbc-127">Om du inte vill tooclear alla tillgångar, klickar du på hello **Rensa alla** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="eabbc-127">If you wish tooclear all assets, click hello **Purge all** checkbox.</span></span>  <span data-ttu-id="eabbc-128">Annars ange hello sökväg för varje resurs du vill toopurge i hello **sökväg** textruta.</span><span class="sxs-lookup"><span data-stu-id="eabbc-128">Otherwise, type hello path of each asset you wish toopurge in hello **Path** textbox.</span></span> <span data-ttu-id="eabbc-129">Under format stöds i hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="eabbc-129">Below formats are supported in hello path.</span></span>
    1. <span data-ttu-id="eabbc-130">**Den enda URL Rensa**: Rensa enskilda tillgångar genom att ange hello fullständiga URL-adress med eller utan hello filtillägget, t.ex.`/pictures/strasbourg.png`;`/pictures/strasbourg`</span><span class="sxs-lookup"><span data-stu-id="eabbc-130">**Single URL purge**: Purge individual asset by specifying hello full URL, with or without hello file extension, e.g.,`/pictures/strasbourg.png`; `/pictures/strasbourg`</span></span>
    2. <span data-ttu-id="eabbc-131">**Jokertecken Rensa**: Asterisk (\*) kan användas som jokertecken.</span><span class="sxs-lookup"><span data-stu-id="eabbc-131">**Wildcard purge**: Asterisk (\*) may be used as a wildcard.</span></span> <span data-ttu-id="eabbc-132">Rensa alla mappar, undermappar och filer på en slutpunkt med `/*` hello sökväg eller rensa alla undermappar och filer under en viss mapp genom att ange hello mappen följt av `/*`, t.ex.`/pictures/*`.</span><span class="sxs-lookup"><span data-stu-id="eabbc-132">Purge all folders, sub-folders and files under an endpoint with `/*` in hello path or purge all sub-folders and files under a specific folder by specifying hello folder followed by `/*`, e.g.,`/pictures/*`.</span></span>  <span data-ttu-id="eabbc-133">Observera att rensa jokertecken inte stöds av Azure CDN från Akamai för närvarande.</span><span class="sxs-lookup"><span data-stu-id="eabbc-133">Note that wildcard purge is not supported by Azure CDN from Akamai currently.</span></span> 
    3. <span data-ttu-id="eabbc-134">**Roten domän Rensa**: Rensa hello rot hello slutpunkt med ”/” i hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="eabbc-134">**Root domain purge**: Purge hello root of hello endpoint with "/" in hello path.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="eabbc-135">Sökvägar måste anges för rensning och måste vara en relativ URL som passar hello följande [reguljärt uttryck](https://msdn.microsoft.com/library/az24scfc.aspx).</span><span class="sxs-lookup"><span data-stu-id="eabbc-135">Paths must be specified for purge and must be a relative URL that fit hello following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx).</span></span> <span data-ttu-id="eabbc-136">**Rensa alla** och **jokertecken Rensa** stöds inte av **Azure CDN från Akamai** för närvarande.</span><span class="sxs-lookup"><span data-stu-id="eabbc-136">**Purge all** and **Wildcard purge** not supported by **Azure CDN from Akamai** currently.</span></span>
   > > <span data-ttu-id="eabbc-137">Enstaka URL-rensning`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span><span class="sxs-lookup"><span data-stu-id="eabbc-137">Single URL purge `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span></span>  
   > > <span data-ttu-id="eabbc-138">Frågesträng`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="eabbc-138">Query string `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span></span>  
   > > <span data-ttu-id="eabbc-139">Jokertecken Rensa `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span><span class="sxs-lookup"><span data-stu-id="eabbc-139">Wildcard purge `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span></span> 
   > 
   > <span data-ttu-id="eabbc-140">Flera **sökväg** textrutor visas när du har angett texten tooallow toobuild en lista över flera resurser.</span><span class="sxs-lookup"><span data-stu-id="eabbc-140">More **Path** textboxes will appear after you enter text tooallow you toobuild a list of multiple assets.</span></span>  <span data-ttu-id="eabbc-141">Du kan ta bort tillgångar hello listan genom att klicka på ellipsknappen för hello (...).</span><span class="sxs-lookup"><span data-stu-id="eabbc-141">You can delete assets from hello list by clicking hello ellipsis (...) button.</span></span>
   > 
5. <span data-ttu-id="eabbc-142">Klicka på hello **Rensa** knappen.</span><span class="sxs-lookup"><span data-stu-id="eabbc-142">Click hello **Purge** button.</span></span>
   
    ![Rensa knappen](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> <span data-ttu-id="eabbc-144">Rensa begäranden tar cirka 2 – 3 minuter tooprocess med **Azure CDN från Verizon** (Standard och Premium) och cirka 7: e minut med **Azure CDN från Akamai**.</span><span class="sxs-lookup"><span data-stu-id="eabbc-144">Purge requests take approximately 2-3 minutes tooprocess with **Azure CDN from Verizon** (Standard and Premium), and approximately 7 minutes with **Azure CDN from Akamai**.</span></span>  <span data-ttu-id="eabbc-145">Azure CDN är begränsad till 50 samtidiga Rensa begäranden samtidigt.</span><span class="sxs-lookup"><span data-stu-id="eabbc-145">Azure CDN has a limit of 50 concurrent purge requests at any given time.</span></span> 
> 
> 

## <a name="see-also"></a><span data-ttu-id="eabbc-146">Se även</span><span class="sxs-lookup"><span data-stu-id="eabbc-146">See also</span></span>
* [<span data-ttu-id="eabbc-147">Läsa in tillgångar för en Azure CDN-slutpunkt i förväg</span><span class="sxs-lookup"><span data-stu-id="eabbc-147">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="eabbc-148">Azure CDN REST API-referens - rensa eller i förväg läsa in en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="eabbc-148">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

