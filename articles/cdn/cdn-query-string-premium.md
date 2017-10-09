---
title: "aaaControl Azure CDN cachelagringsbeteendet med frågesträngar - Premium | Microsoft Docs"
description: "Azure CDN cachelagring av frågesträng styr hur filer är toobe cachelagras när de innehåller frågesträngar."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5c97cf0230ae13fd378bfce49481f3135a5ef101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a><span data-ttu-id="1e54f-103">Kontrollen Azure CDN cachelagring av frågesträngar med frågesträngar - Premium</span><span class="sxs-lookup"><span data-stu-id="1e54f-103">Control Azure CDN caching behavior with query strings - Premium</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1e54f-104">Standard</span><span class="sxs-lookup"><span data-stu-id="1e54f-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="1e54f-105">Azure CDN Premium från Verizon</span><span class="sxs-lookup"><span data-stu-id="1e54f-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="1e54f-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="1e54f-106">Overview</span></span>
<span data-ttu-id="1e54f-107">Cachelagring av frågesträng styr hur filer är toobe cachelagras när de innehåller frågesträngar.</span><span class="sxs-lookup"><span data-stu-id="1e54f-107">Query string caching controls how files are toobe cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e54f-108">hello Standard och Premium CDN produkter har hello samma fråga sträng cachelagring funktioner, men hello användargränssnittet skiljer sig åt.</span><span class="sxs-lookup"><span data-stu-id="1e54f-108">hello Standard and Premium CDN products provide hello same query string caching functionality, but hello user interface differs.</span></span>  <span data-ttu-id="1e54f-109">Det här dokumentet beskriver hello gränssnitt för **Azure CDN Premium från Verizon**.</span><span class="sxs-lookup"><span data-stu-id="1e54f-109">This document describes hello interface for **Azure CDN Premium from Verizon**.</span></span>  <span data-ttu-id="1e54f-110">För frågan sträng cachelagring med **Azure CDN Standard från Akamai** och **Azure CDN Standard från Verizon**, se [styr cachelagring beteendet för CDN-begäranden med frågesträngar](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="1e54f-110">For query string caching with **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**, see [Controlling caching behavior of CDN requests with query strings](cdn-query-string.md).</span></span>
> 
> 

<span data-ttu-id="1e54f-111">Det finns tre lägen:</span><span class="sxs-lookup"><span data-stu-id="1e54f-111">Three modes are available:</span></span>

* <span data-ttu-id="1e54f-112">**standard-cache**: det här är standardläget för hello.</span><span class="sxs-lookup"><span data-stu-id="1e54f-112">**standard-cache**:  This is hello default mode.</span></span>  <span data-ttu-id="1e54f-113">hello CDN kantnod skickar hello frågesträng från hello begärande toohello ursprung på hello första begäran och cache hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="1e54f-113">hello CDN edge node will pass hello query string from hello requestor toohello origin on hello first request and cache hello asset.</span></span>  <span data-ttu-id="1e54f-114">Alla efterföljande förfrågningar för den tillgången som hämtas från hello kantnod ignoreras hello frågesträngen tills hello cachelagrade tillgången upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="1e54f-114">All subsequent requests for that asset that are served from hello edge node will ignore hello query string until hello cached asset expires.</span></span>
* <span data-ttu-id="1e54f-115">**no-cache**: I det här läget begäranden med frågesträngar inte cachelagras på hello CDN kantnod.</span><span class="sxs-lookup"><span data-stu-id="1e54f-115">**no-cache**:  In this mode, requests with query strings are not cached at hello CDN edge node.</span></span>  <span data-ttu-id="1e54f-116">hello kantnod hämtar hello tillgången direkt från hello ursprung och skickar den begärande toohello med varje begäran.</span><span class="sxs-lookup"><span data-stu-id="1e54f-116">hello edge node retrieves hello asset directly from hello origin and passes it toohello requestor with each request.</span></span>
* <span data-ttu-id="1e54f-117">**Unik cache**: det här läget behandlar varje begäran med en frågesträng som en unik tillgång med sin egen cache.</span><span class="sxs-lookup"><span data-stu-id="1e54f-117">**unique-cache**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="1e54f-118">Till exempel hello svar från hello ursprung för en begäran om *foo.ashx?q=bar* skulle cachelagras på hello kantnod och returneras för efterföljande med samma fråga strängen.</span><span class="sxs-lookup"><span data-stu-id="1e54f-118">For example, hello response from hello origin for a request for *foo.ashx?q=bar* would be cached at hello edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="1e54f-119">En begäran om *foo.ashx?q=somethingelse* skulle cachelagras som en separat tillgång med sin egen toolive tid.</span><span class="sxs-lookup"><span data-stu-id="1e54f-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time toolive.</span></span>

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a><span data-ttu-id="1e54f-120">Ändra inställningar för premium CDN profiler för cachelagring av frågesträng</span><span class="sxs-lookup"><span data-stu-id="1e54f-120">Changing query string caching settings for premium CDN profiles</span></span>
1. <span data-ttu-id="1e54f-121">Klicka på hello hello CDN-profilbladet **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="1e54f-121">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![CDN-profilbladet hantera knappen](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    <span data-ttu-id="1e54f-123">hello CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="1e54f-123">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="1e54f-124">Hovra över hello **HTTP stora** fliken och sedan hovra över hello **cacheinställningarna** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="1e54f-124">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="1e54f-125">Klicka på **cachelagring av frågesträng**.</span><span class="sxs-lookup"><span data-stu-id="1e54f-125">Click on **Query-String Caching**.</span></span>
   
    <span data-ttu-id="1e54f-126">Frågesträng cachelagringsalternativ visas.</span><span class="sxs-lookup"><span data-stu-id="1e54f-126">Query string caching options are displayed.</span></span>
   
    ![CDN-frågesträng cachelagringsalternativ](./media/cdn-query-string-premium/cdn-query-string.png)
3. <span data-ttu-id="1e54f-128">När du har gjort ditt val klickar du på hello **uppdatering** knappen.</span><span class="sxs-lookup"><span data-stu-id="1e54f-128">After making your selection, click hello **Update** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e54f-129">hello ändringarna kan inte visas omedelbart, som det tar tid för hello registrering toopropagate via hello CDN.</span><span class="sxs-lookup"><span data-stu-id="1e54f-129">hello settings changes may not be immediately visible, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="1e54f-130">För profiler av typen <b>Azure CDN från Verizon</b> slutförs spridningen vanligtvis inom 90 minuter, men i vissa fall kan det ta längre tid.</span><span class="sxs-lookup"><span data-stu-id="1e54f-130">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

