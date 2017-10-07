---
title: "aaaControl Azure CDN cachelagringsbeteendet med frågesträngar | Microsoft Docs"
description: "Azure CDN cachelagring av frågesträng styr hur filer är toobe cachelagras när de innehåller frågesträngar."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e7a138b2decec624a29eb703ad9a291d19c44ee8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a><span data-ttu-id="c4064-103">Kontrollen Azure CDN cachelagring av frågesträngar med frågesträngar</span><span class="sxs-lookup"><span data-stu-id="c4064-103">Control Azure CDN caching behavior with query strings</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c4064-104">Standard</span><span class="sxs-lookup"><span data-stu-id="c4064-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="c4064-105">Azure CDN Premium från Verizon</span><span class="sxs-lookup"><span data-stu-id="c4064-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="c4064-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="c4064-106">Overview</span></span>
<span data-ttu-id="c4064-107">Cachelagring av frågesträng styr hur filer är toobe cachelagras när de innehåller frågesträngar.</span><span class="sxs-lookup"><span data-stu-id="c4064-107">Query string caching controls how files are toobe cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4064-108">hello Standard och Premium CDN produkter har hello samma fråga sträng cachelagring funktioner, men hello användargränssnittet skiljer sig åt.</span><span class="sxs-lookup"><span data-stu-id="c4064-108">hello Standard and Premium CDN products provide hello same query string caching functionality, but hello user interface differs.</span></span>  <span data-ttu-id="c4064-109">Det här dokumentet beskriver hello gränssnitt för **Azure CDN Standard från Akamai** och **Azure CDN Standard från Verizon**.</span><span class="sxs-lookup"><span data-stu-id="c4064-109">This document describes hello interface for **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**.</span></span>  <span data-ttu-id="c4064-110">För att fråga sträng cachelagring med **Azure CDN Premium från Verizon**, se [styr cachelagring beteendet för CDN-begäranden med frågesträngar - Premium](cdn-query-string-premium.md).</span><span class="sxs-lookup"><span data-stu-id="c4064-110">For query string caching with **Azure CDN Premium from Verizon**, see [Controlling caching behavior of CDN requests with query strings - Premium](cdn-query-string-premium.md).</span></span>
> 
> 

<span data-ttu-id="c4064-111">Det finns tre lägen:</span><span class="sxs-lookup"><span data-stu-id="c4064-111">Three modes are available:</span></span>

* <span data-ttu-id="c4064-112">**Ignorera frågesträngar**: det här är standardläget för hello.</span><span class="sxs-lookup"><span data-stu-id="c4064-112">**Ignore query strings**:  This is hello default mode.</span></span>  <span data-ttu-id="c4064-113">hello CDN kantnod skickar hello frågesträng från hello begärande toohello ursprung på hello första begäran och cache hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="c4064-113">hello CDN edge node will pass hello query string from hello requestor toohello origin on hello first request and cache hello asset.</span></span>  <span data-ttu-id="c4064-114">Alla efterföljande förfrågningar för den tillgången som hämtas från hello kantnod ignoreras hello frågesträngen tills hello cachelagrade tillgången upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="c4064-114">All subsequent requests for that asset that are served from hello edge node will ignore hello query string until hello cached asset expires.</span></span>
* <span data-ttu-id="c4064-115">**Kringgå cachelagring för URL med frågesträngar**: I det här läget begäranden med frågesträngar inte cachelagras på hello CDN kantnod.</span><span class="sxs-lookup"><span data-stu-id="c4064-115">**Bypass caching for URL with query strings**:  In this mode, requests with query strings are not cached at hello CDN edge node.</span></span>  <span data-ttu-id="c4064-116">hello kantnod hämtar hello tillgången direkt från hello ursprung och skickar den begärande toohello med varje begäran.</span><span class="sxs-lookup"><span data-stu-id="c4064-116">hello edge node retrieves hello asset directly from hello origin and passes it toohello requestor with each request.</span></span>
* <span data-ttu-id="c4064-117">**Cachelagra varje unik URL**: det här läget behandlar varje begäran med en frågesträng som en unik tillgång med sin egen cache.</span><span class="sxs-lookup"><span data-stu-id="c4064-117">**Cache every unique URL**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="c4064-118">Till exempel hello svar från hello ursprung för en begäran om *foo.ashx?q=bar* skulle cachelagras på hello kantnod och returneras för efterföljande med samma fråga strängen.</span><span class="sxs-lookup"><span data-stu-id="c4064-118">For example, hello response from hello origin for a request for *foo.ashx?q=bar* would be cached at hello edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="c4064-119">En begäran om *foo.ashx?q=somethingelse* skulle cachelagras som en separat tillgång med sin egen toolive tid.</span><span class="sxs-lookup"><span data-stu-id="c4064-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time toolive.</span></span>

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a><span data-ttu-id="c4064-120">Ändra inställningar för standard CDN-profiler för cachelagring av frågesträng</span><span class="sxs-lookup"><span data-stu-id="c4064-120">Changing query string caching settings for standard CDN profiles</span></span>
1. <span data-ttu-id="c4064-121">Klicka på hello CDN-slutpunkt som du vill toomanage hello CDN-profilbladet.</span><span class="sxs-lookup"><span data-stu-id="c4064-121">From hello CDN profile blade, click hello CDN endpoint you wish toomanage.</span></span>
   
    ![CDN-profilen bladet-slutpunkter](./media/cdn-query-string/cdn-endpoints.png)
   
    <span data-ttu-id="c4064-123">hello CDN-slutpunkten blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="c4064-123">hello CDN endpoint blade opens.</span></span>
2. <span data-ttu-id="c4064-124">Klicka på hello **konfigurera** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4064-124">Click hello **Configure** button.</span></span>
   
    ![CDN-profilbladet hantera knappen](./media/cdn-query-string/cdn-config-btn.png)
   
    <span data-ttu-id="c4064-126">hello CDN Configuration blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="c4064-126">hello CDN Configuration blade opens.</span></span>
3. <span data-ttu-id="c4064-127">Välj en inställning i hello **cachelagring av frågesträngar** listrutan.</span><span class="sxs-lookup"><span data-stu-id="c4064-127">Select a setting from hello **Query string caching behavior** dropdown.</span></span>
   
    ![CDN-frågesträng cachelagringsalternativ](./media/cdn-query-string/cdn-query-string.png)
4. <span data-ttu-id="c4064-129">När du har gjort ditt val klickar du på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4064-129">After making your selection, click hello **Save** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4064-130">hello ändringarna kan inte visas omedelbart, som det tar tid för hello registrering toopropagate via hello CDN.</span><span class="sxs-lookup"><span data-stu-id="c4064-130">hello settings changes may not be immediately visible, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="c4064-131">För profiler av typen <b>Azure CDN från Akamai</b> slutförs spridningen vanligtvis inom en minut.</span><span class="sxs-lookup"><span data-stu-id="c4064-131">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="c4064-132">För profiler av typen <b>Azure CDN från Verizon</b> slutförs spridningen vanligtvis inom 90 minuter, men i vissa fall kan det ta längre tid.</span><span class="sxs-lookup"><span data-stu-id="c4064-132">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

