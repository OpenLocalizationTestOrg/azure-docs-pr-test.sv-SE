---
title: Realtid statistik i Azure CDN | Microsoft Docs
description: "Realtid statistik ger data i realtid om Azure CDN prestanda när leverera innehåll till dina klienter."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e9b9522de6b2c54dc794b00100ffe358296ecfdd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a><span data-ttu-id="96d7b-103">Realtid statistik i Microsoft Azure CDN</span><span class="sxs-lookup"><span data-stu-id="96d7b-103">Real-time stats in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="96d7b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="96d7b-104">Overview</span></span>
<span data-ttu-id="96d7b-105">Det här dokumentet förklarar realtid statistik i Microsoft Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="96d7b-105">This document explains real-time stats in Microsoft Azure CDN.</span></span>  <span data-ttu-id="96d7b-106">Den här funktionen ger realtidsdata, till exempel bandbredd, cache status och samtidiga anslutningar till CDN-profilen när leverera innehåll till dina klienter.</span><span class="sxs-lookup"><span data-stu-id="96d7b-106">This functionality provides real-time data, such as bandwidth, cache statuses, and concurrent connections to your CDN profile when delivering content to your clients.</span></span> <span data-ttu-id="96d7b-107">Detta gör att kontinuerlig övervakning av hälsotillståndet för din tjänst när som helst, inklusive go live-händelser.</span><span class="sxs-lookup"><span data-stu-id="96d7b-107">This enables continuous monitoring of the health of your service at any time, including go-live events.</span></span>

<span data-ttu-id="96d7b-108">Följande diagram är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="96d7b-108">The following graphs are available:</span></span>

* [<span data-ttu-id="96d7b-109">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="96d7b-109">Bandwidth</span></span>](#bandwidth)
* [<span data-ttu-id="96d7b-110">Statuskoder</span><span class="sxs-lookup"><span data-stu-id="96d7b-110">Status Codes</span></span>](#status-codes)
* [<span data-ttu-id="96d7b-111">Cache-status</span><span class="sxs-lookup"><span data-stu-id="96d7b-111">Cache Statuses</span></span>](#cache-statuses)
* [<span data-ttu-id="96d7b-112">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="96d7b-112">Connections</span></span>](#connections)

## <a name="accessing-real-time-stats"></a><span data-ttu-id="96d7b-113">Åtkomst till realtid statistik</span><span class="sxs-lookup"><span data-stu-id="96d7b-113">Accessing real-time stats</span></span>
1. <span data-ttu-id="96d7b-114">I den [Azure Portal](https://portal.azure.com), bläddra till CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="96d7b-114">In the [Azure Portal](https://portal.azure.com), browse to your CDN profile.</span></span>
   
    ![CDN-profilbladet](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. <span data-ttu-id="96d7b-116">CDN-profilbladet klickar du på den **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="96d7b-116">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![CDN-profilbladet hantera knappen](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    <span data-ttu-id="96d7b-118">CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="96d7b-118">The CDN management portal opens.</span></span>
3. <span data-ttu-id="96d7b-119">Hovra över den **Analytics** och klicka sedan hovra över den **realtid Stats** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="96d7b-119">Hover over the **Analytics** tab, then hover over the **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="96d7b-120">Klicka på **HTTP stort objekt**.</span><span class="sxs-lookup"><span data-stu-id="96d7b-120">Click on **HTTP Large Object**.</span></span>
   
    ![CDN-hanteringsportalen](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    <span data-ttu-id="96d7b-122">Realtid stats diagram visas.</span><span class="sxs-lookup"><span data-stu-id="96d7b-122">The real-time stats graphs are displayed.</span></span>

<span data-ttu-id="96d7b-123">Varje diagram visar realtid statistik för det valda tidsintervallet startar när sidan läses in.</span><span class="sxs-lookup"><span data-stu-id="96d7b-123">Each of the graphs displays real-time statistics for the selected time span, starting when the page loads.</span></span>  <span data-ttu-id="96d7b-124">Diagrammen uppdateras automatiskt varje några sekunder.</span><span class="sxs-lookup"><span data-stu-id="96d7b-124">The graphs update automatically every few seconds.</span></span>  <span data-ttu-id="96d7b-125">Den **uppdatera diagrammet** knappen, om den finns tar bort diagrammet efter vilken visas endast valda data.</span><span class="sxs-lookup"><span data-stu-id="96d7b-125">The **Refresh Graph** button, if present, will clear the graph, after which it will only display the selected data.</span></span>

## <a name="bandwidth"></a><span data-ttu-id="96d7b-126">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="96d7b-126">Bandwidth</span></span>
![Bandbredd-diagram](./media/cdn-real-time-stats/cdn-bandwidth.png)

<span data-ttu-id="96d7b-128">Den **bandbredd** diagram visar mängden bandbredd som används för den aktuella plattformen över det valda tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-128">The **Bandwidth** graph displays the amount of bandwidth used for the current platform over the selected time span.</span></span> <span data-ttu-id="96d7b-129">Skuggade delen av diagrammet anger bandbreddsanvändning.</span><span class="sxs-lookup"><span data-stu-id="96d7b-129">The shaded portion of the graph indicates bandwidth usage.</span></span> <span data-ttu-id="96d7b-130">Den exakta mängden bandbredd som används visas direkt under linjediagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-130">The exact amount of bandwidth currently being used is displayed directly below the line graph.</span></span>

## <a name="status-codes"></a><span data-ttu-id="96d7b-131">Statuskoder</span><span class="sxs-lookup"><span data-stu-id="96d7b-131">Status Codes</span></span>
![Diagram för status-kod](./media/cdn-real-time-stats/cdn-status-codes.png)

<span data-ttu-id="96d7b-133">Den **statuskoder** diagram visar hur ofta vissa HTTP-svarskoder sker via det valda tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-133">The **Status Codes** graph indicates how often certain HTTP response codes are occurring over the selected time span.</span></span>

> [!TIP]
> <span data-ttu-id="96d7b-134">En beskrivning av varje alternativ för HTTP-status koden finns [Azure CDN HTTP-statuskoder](https://msdn.microsoft.com/library/mt759238.aspx).</span><span class="sxs-lookup"><span data-stu-id="96d7b-134">For a description of each HTTP status code option, see [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx).</span></span>
> 
> 

<span data-ttu-id="96d7b-135">En lista över HTTP-statuskoder visas direkt ovanför diagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-135">A list of HTTP status codes is displayed directly above the graph.</span></span> <span data-ttu-id="96d7b-136">Den här listan visar varje statuskod som kan ingå i linjediagrammet och det aktuella antalet förekomster per sekund för denna statuskod.</span><span class="sxs-lookup"><span data-stu-id="96d7b-136">This list indicates each status code that can be included in the line graph and the current number of occurrences per second for that status code.</span></span> <span data-ttu-id="96d7b-137">Som standard visas en rad för var och en av dessa statuskoder i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-137">By default, a line is displayed for each of these status codes in the graph.</span></span> <span data-ttu-id="96d7b-138">Du kan dock välja att endast övervaka statuskoder som har särskild betydelse för din CDN-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="96d7b-138">However, you can choose to only monitor the status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="96d7b-139">Om du vill göra detta, markera önskad statuskoder och avmarkera alla andra alternativ och klicka på **uppdatera diagrammet**.</span><span class="sxs-lookup"><span data-stu-id="96d7b-139">To do this, check the desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="96d7b-140">Du kan dölja loggdata för en viss statuskod tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="96d7b-140">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="96d7b-141">Klicka på statuskod som du vill dölja förklaringen direkt under diagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-141">From the legend directly below the graph, click the status code you want to hide.</span></span> <span data-ttu-id="96d7b-142">Statuskoden ska vara dolda direkt från diagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-142">The status code will be immediately hidden from the graph.</span></span> <span data-ttu-id="96d7b-143">Om du klickar på den statuskoden igen kommer alternativet för att visas igen.</span><span class="sxs-lookup"><span data-stu-id="96d7b-143">Clicking that status code again will cause that option to be displayed again.</span></span>

## <a name="cache-statuses"></a><span data-ttu-id="96d7b-144">Cache-status</span><span class="sxs-lookup"><span data-stu-id="96d7b-144">Cache Statuses</span></span>
![Diagram för cache-status](./media/cdn-real-time-stats/cdn-cache-status.png)

<span data-ttu-id="96d7b-146">Den **Cache statusar** diagram visar hur ofta vissa typer av cache-status sker via det valda tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-146">The **Cache Statuses** graph indicates how often certain types of cache statuses are occurring over the selected time span.</span></span> 

> [!TIP]
> <span data-ttu-id="96d7b-147">En beskrivning av varje cache status kod alternativ finns [Azure CDN Cache-statuskoder](https://msdn.microsoft.com/library/mt759237.aspx).</span><span class="sxs-lookup"><span data-stu-id="96d7b-147">For a description of each cache status code option, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx).</span></span>
> 
> 

<span data-ttu-id="96d7b-148">En lista över cache statuskoder visas direkt ovanför diagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-148">A list of cache status codes is displayed directly above the graph.</span></span> <span data-ttu-id="96d7b-149">Den här listan visar varje statuskod som kan ingå i linjediagrammet och det aktuella antalet förekomster per sekund för denna statuskod.</span><span class="sxs-lookup"><span data-stu-id="96d7b-149">This list indicates each status code that can be included in the line graph and the current number of occurrences per second for that status code.</span></span> <span data-ttu-id="96d7b-150">Som standard visas en rad för var och en av dessa statuskoder i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-150">By default, a line is displayed for each of these status codes in the graph.</span></span> <span data-ttu-id="96d7b-151">Du kan dock välja att endast övervaka statuskoder som har särskild betydelse för din CDN-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="96d7b-151">However, you can choose to only monitor the status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="96d7b-152">Om du vill göra detta, markera önskad statuskoder och avmarkera alla andra alternativ och klicka på **uppdatera diagrammet**.</span><span class="sxs-lookup"><span data-stu-id="96d7b-152">To do this, check the desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="96d7b-153">Du kan dölja loggdata för en viss statuskod tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="96d7b-153">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="96d7b-154">Klicka på statuskod som du vill dölja förklaringen direkt under diagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-154">From the legend directly below the graph, click the status code you want to hide.</span></span> <span data-ttu-id="96d7b-155">Statuskoden ska vara dolda direkt från diagrammet.</span><span class="sxs-lookup"><span data-stu-id="96d7b-155">The status code will be immediately hidden from the graph.</span></span> <span data-ttu-id="96d7b-156">Om du klickar på den statuskoden igen kommer alternativet för att visas igen.</span><span class="sxs-lookup"><span data-stu-id="96d7b-156">Clicking that status code again will cause that option to be displayed again.</span></span>

## <a name="connections"></a><span data-ttu-id="96d7b-157">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="96d7b-157">Connections</span></span>
![Anslutningar diagram](./media/cdn-real-time-stats/cdn-connections.png)

<span data-ttu-id="96d7b-159">Det här diagrammet visar hur många anslutningar har upprättats till edge-servrar.</span><span class="sxs-lookup"><span data-stu-id="96d7b-159">This graph indicates how many connections have been established to your edge servers.</span></span> <span data-ttu-id="96d7b-160">Varje begäran för en tillgång som passerar genom våra CDN resultat i en anslutning.</span><span class="sxs-lookup"><span data-stu-id="96d7b-160">Each request for an asset that passes through our CDN results in a connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="96d7b-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96d7b-161">Next Steps</span></span>
* <span data-ttu-id="96d7b-162">Håll dig informerad med [aviseringar i realtid i Azure CDN](cdn-real-time-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="96d7b-162">Get notified with [Real-time alerts in Azure CDN](cdn-real-time-alerts.md)</span></span>
* <span data-ttu-id="96d7b-163">Gräva djupare med [avancerade http-rapporter](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="96d7b-163">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="96d7b-164">Analysera [användningsmönster](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="96d7b-164">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

