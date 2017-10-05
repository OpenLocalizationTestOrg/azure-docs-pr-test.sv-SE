---
title: "Kontrollera Azure CDN cachelagring av frågesträngar med frågesträngar - Premium | Microsoft Docs"
description: "Azure CDN cachelagring av frågesträng styr hur filer ska kunna cachelagras när de innehåller frågesträngar."
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
ms.openlocfilehash: 145067c2ce50b41c4783f4de4052c0e7cb529fc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a><span data-ttu-id="7b6fc-103">Kontrollen Azure CDN cachelagring av frågesträngar med frågesträngar - Premium</span><span class="sxs-lookup"><span data-stu-id="7b6fc-103">Control Azure CDN caching behavior with query strings - Premium</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7b6fc-104">Standard</span><span class="sxs-lookup"><span data-stu-id="7b6fc-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="7b6fc-105">Azure CDN Premium från Verizon</span><span class="sxs-lookup"><span data-stu-id="7b6fc-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="7b6fc-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="7b6fc-106">Overview</span></span>
<span data-ttu-id="7b6fc-107">Cachelagring av frågesträng styr hur filer ska kunna cachelagras när de innehåller frågesträngar.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-107">Query string caching controls how files are to be cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b6fc-108">Standard- och Premium CDN produkter ger samma frågesträngen cachelagring funktioner, men användargränssnittet skiljer sig åt.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-108">The Standard and Premium CDN products provide the same query string caching functionality, but the user interface differs.</span></span>  <span data-ttu-id="7b6fc-109">Det här dokumentet beskriver gränssnittet för **Azure CDN Premium från Verizon**.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-109">This document describes the interface for **Azure CDN Premium from Verizon**.</span></span>  <span data-ttu-id="7b6fc-110">För frågan sträng cachelagring med **Azure CDN Standard från Akamai** och **Azure CDN Standard från Verizon**, se [styr cachelagring beteendet för CDN-begäranden med frågesträngar](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="7b6fc-110">For query string caching with **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**, see [Controlling caching behavior of CDN requests with query strings](cdn-query-string.md).</span></span>
> 
> 

<span data-ttu-id="7b6fc-111">Det finns tre lägen:</span><span class="sxs-lookup"><span data-stu-id="7b6fc-111">Three modes are available:</span></span>

* <span data-ttu-id="7b6fc-112">**standard-cache**: det här är standardläget.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-112">**standard-cache**:  This is the default mode.</span></span>  <span data-ttu-id="7b6fc-113">CDN-kantnod skickar frågesträngen från begäranden till ursprunget på den första begäran och cacheposten tillgången.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-113">The CDN edge node will pass the query string from the requestor to the origin on the first request and cache the asset.</span></span>  <span data-ttu-id="7b6fc-114">Alla efterföljande förfrågningar för den tillgången som hämtas från kantnoden ignoreras frågesträngen tills cachelagrade tillgången upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-114">All subsequent requests for that asset that are served from the edge node will ignore the query string until the cached asset expires.</span></span>
* <span data-ttu-id="7b6fc-115">**no-cache**: I det här läget begäranden med frågesträngar inte cachelagras på kantnod CDN.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-115">**no-cache**:  In this mode, requests with query strings are not cached at the CDN edge node.</span></span>  <span data-ttu-id="7b6fc-116">Kantnoden hämtar tillgången direkt från ursprunget och skickar den till begäranden med varje begäran.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-116">The edge node retrieves the asset directly from the origin and passes it to the requestor with each request.</span></span>
* <span data-ttu-id="7b6fc-117">**Unik cache**: det här läget behandlar varje begäran med en frågesträng som en unik tillgång med sin egen cache.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-117">**unique-cache**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="7b6fc-118">Till exempel svaret från ursprung för en begäran om *foo.ashx?q=bar* skulle cachelagras på kantnoden och returneras för efterföljande med samma fråga strängen.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-118">For example, the response from the origin for a request for *foo.ashx?q=bar* would be cached at the edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="7b6fc-119">En begäran om *foo.ashx?q=somethingelse* skulle cachelagras som en separat tillgång med sin egen tid till live.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time to live.</span></span>

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a><span data-ttu-id="7b6fc-120">Ändra inställningar för premium CDN profiler för cachelagring av frågesträng</span><span class="sxs-lookup"><span data-stu-id="7b6fc-120">Changing query string caching settings for premium CDN profiles</span></span>
1. <span data-ttu-id="7b6fc-121">CDN-profilbladet klickar du på den **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-121">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![CDN-profilbladet hantera knappen](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    <span data-ttu-id="7b6fc-123">CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-123">The CDN management portal opens.</span></span>
2. <span data-ttu-id="7b6fc-124">Hovra över den **HTTP stora** och klicka sedan hovra över den **cacheinställningarna** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-124">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="7b6fc-125">Klicka på **cachelagring av frågesträng**.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-125">Click on **Query-String Caching**.</span></span>
   
    <span data-ttu-id="7b6fc-126">Frågesträng cachelagringsalternativ visas.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-126">Query string caching options are displayed.</span></span>
   
    ![CDN-frågesträng cachelagringsalternativ](./media/cdn-query-string-premium/cdn-query-string.png)
3. <span data-ttu-id="7b6fc-128">När du har gjort ditt val klickar du på den **uppdatering** knappen.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-128">After making your selection, click the **Update** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b6fc-129">Inställningsändringarna kanske inte syns direkt, eftersom det tar tid för registreringen ska spridas via CDN.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-129">The settings changes may not be immediately visible, as it takes time for the registration to propagate through the CDN.</span></span>  <span data-ttu-id="7b6fc-130">För profiler av typen <b>Azure CDN från Verizon</b> slutförs spridningen vanligtvis inom 90 minuter, men i vissa fall kan det ta längre tid.</span><span class="sxs-lookup"><span data-stu-id="7b6fc-130">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

