---
title: aaaReal statistik i Azure CDN | Microsoft Docs
description: "Realtid statistik innehåller data i realtid om Azure CDN hello prestanda när du levererar innehåll tooyour klienter."
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
ms.openlocfilehash: 68900a5092b767e45c1fdf9cef2cd03f55f38a6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a><span data-ttu-id="a21f6-103">Realtid statistik i Microsoft Azure CDN</span><span class="sxs-lookup"><span data-stu-id="a21f6-103">Real-time stats in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="a21f6-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a21f6-104">Overview</span></span>
<span data-ttu-id="a21f6-105">Det här dokumentet förklarar realtid statistik i Microsoft Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="a21f6-105">This document explains real-time stats in Microsoft Azure CDN.</span></span>  <span data-ttu-id="a21f6-106">Den här funktionen ger realtidsdata, såsom bandbredd, cache status och samtidiga anslutningar tooyour CDN-profilen när du levererar innehåll tooyour klienter.</span><span class="sxs-lookup"><span data-stu-id="a21f6-106">This functionality provides real-time data, such as bandwidth, cache statuses, and concurrent connections tooyour CDN profile when delivering content tooyour clients.</span></span> <span data-ttu-id="a21f6-107">Detta gör att kontinuerlig övervakning av hello hälsotillståndet för din tjänst när som helst, inklusive go live-händelser.</span><span class="sxs-lookup"><span data-stu-id="a21f6-107">This enables continuous monitoring of hello health of your service at any time, including go-live events.</span></span>

<span data-ttu-id="a21f6-108">följande diagram hello är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="a21f6-108">hello following graphs are available:</span></span>

* [<span data-ttu-id="a21f6-109">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="a21f6-109">Bandwidth</span></span>](#bandwidth)
* [<span data-ttu-id="a21f6-110">Statuskoder</span><span class="sxs-lookup"><span data-stu-id="a21f6-110">Status Codes</span></span>](#status-codes)
* [<span data-ttu-id="a21f6-111">Cache-status</span><span class="sxs-lookup"><span data-stu-id="a21f6-111">Cache Statuses</span></span>](#cache-statuses)
* [<span data-ttu-id="a21f6-112">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="a21f6-112">Connections</span></span>](#connections)

## <a name="accessing-real-time-stats"></a><span data-ttu-id="a21f6-113">Åtkomst till realtid statistik</span><span class="sxs-lookup"><span data-stu-id="a21f6-113">Accessing real-time stats</span></span>
1. <span data-ttu-id="a21f6-114">I hello [Azure Portal](https://portal.azure.com), bläddra tooyour CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="a21f6-114">In hello [Azure Portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>
   
    ![CDN-profilbladet](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. <span data-ttu-id="a21f6-116">Klicka på hello hello CDN-profilbladet **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="a21f6-116">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![CDN-profilbladet hantera knappen](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    <span data-ttu-id="a21f6-118">hello CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="a21f6-118">hello CDN management portal opens.</span></span>
3. <span data-ttu-id="a21f6-119">Hovra över hello **Analytics** fliken och sedan hovra över hello **realtid Stats** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="a21f6-119">Hover over hello **Analytics** tab, then hover over hello **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="a21f6-120">Klicka på **HTTP stort objekt**.</span><span class="sxs-lookup"><span data-stu-id="a21f6-120">Click on **HTTP Large Object**.</span></span>
   
    ![CDN-hanteringsportalen](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    <span data-ttu-id="a21f6-122">hello realtid stats diagram visas.</span><span class="sxs-lookup"><span data-stu-id="a21f6-122">hello real-time stats graphs are displayed.</span></span>

<span data-ttu-id="a21f6-123">Var och en av hello diagram visar realtid statistik för hello valda tidsintervallet startar när hello sidan läses in.</span><span class="sxs-lookup"><span data-stu-id="a21f6-123">Each of hello graphs displays real-time statistics for hello selected time span, starting when hello page loads.</span></span>  <span data-ttu-id="a21f6-124">hello diagram uppdateras automatiskt varje några sekunder.</span><span class="sxs-lookup"><span data-stu-id="a21f6-124">hello graphs update automatically every few seconds.</span></span>  <span data-ttu-id="a21f6-125">Hej **uppdatera diagrammet** knappen, om den finns raderar hello diagram, efter vilken visas endast hello valda data.</span><span class="sxs-lookup"><span data-stu-id="a21f6-125">hello **Refresh Graph** button, if present, will clear hello graph, after which it will only display hello selected data.</span></span>

## <a name="bandwidth"></a><span data-ttu-id="a21f6-126">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="a21f6-126">Bandwidth</span></span>
![Bandbredd-diagram](./media/cdn-real-time-stats/cdn-bandwidth.png)

<span data-ttu-id="a21f6-128">Hej **bandbredd** diagram som visar hello mängden bandbredd som används för hello aktuella plattformen över hello valda tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="a21f6-128">hello **Bandwidth** graph displays hello amount of bandwidth used for hello current platform over hello selected time span.</span></span> <span data-ttu-id="a21f6-129">hello skuggade delen av diagrammet hello anger bandbreddsanvändning.</span><span class="sxs-lookup"><span data-stu-id="a21f6-129">hello shaded portion of hello graph indicates bandwidth usage.</span></span> <span data-ttu-id="a21f6-130">hello exakta mängden bandbredd som används visas direkt under hello linjediagram.</span><span class="sxs-lookup"><span data-stu-id="a21f6-130">hello exact amount of bandwidth currently being used is displayed directly below hello line graph.</span></span>

## <a name="status-codes"></a><span data-ttu-id="a21f6-131">Statuskoder</span><span class="sxs-lookup"><span data-stu-id="a21f6-131">Status Codes</span></span>
![Diagram för status-kod](./media/cdn-real-time-stats/cdn-status-codes.png)

<span data-ttu-id="a21f6-133">Hej **statuskoder** diagram visar hur ofta vissa HTTP-svarskoder sker via hello valda tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="a21f6-133">hello **Status Codes** graph indicates how often certain HTTP response codes are occurring over hello selected time span.</span></span>

> [!TIP]
> <span data-ttu-id="a21f6-134">En beskrivning av varje alternativ för HTTP-status koden finns [Azure CDN HTTP-statuskoder](https://msdn.microsoft.com/library/mt759238.aspx).</span><span class="sxs-lookup"><span data-stu-id="a21f6-134">For a description of each HTTP status code option, see [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx).</span></span>
> 
> 

<span data-ttu-id="a21f6-135">En lista över HTTP-statuskoder visas direkt ovanför hello diagram.</span><span class="sxs-lookup"><span data-stu-id="a21f6-135">A list of HTTP status codes is displayed directly above hello graph.</span></span> <span data-ttu-id="a21f6-136">Den här listan visar varje statuskod som kan ingå i hello linjediagram och hello aktuellt antal händelser per sekund för denna statuskod.</span><span class="sxs-lookup"><span data-stu-id="a21f6-136">This list indicates each status code that can be included in hello line graph and hello current number of occurrences per second for that status code.</span></span> <span data-ttu-id="a21f6-137">Som standard visas en rad för var och en av dessa statuskoder i hello graph.</span><span class="sxs-lookup"><span data-stu-id="a21f6-137">By default, a line is displayed for each of these status codes in hello graph.</span></span> <span data-ttu-id="a21f6-138">Du kan dock välja tooonly övervakaren hello statuskoder som har särskild betydelse för din CDN-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a21f6-138">However, you can choose tooonly monitor hello status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="a21f6-139">toodo, markera önskad hello-statuskoder och avmarkera alla andra alternativ och klicka på **uppdatera diagrammet**.</span><span class="sxs-lookup"><span data-stu-id="a21f6-139">toodo this, check hello desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="a21f6-140">Du kan dölja loggdata för en viss statuskod tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="a21f6-140">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="a21f6-141">Klicka på hello statuskod som du vill toohide hello förklaringen direkt under hello diagram.</span><span class="sxs-lookup"><span data-stu-id="a21f6-141">From hello legend directly below hello graph, click hello status code you want toohide.</span></span> <span data-ttu-id="a21f6-142">hello-statuskoden ska vara dolda direkt från hello diagram.</span><span class="sxs-lookup"><span data-stu-id="a21f6-142">hello status code will be immediately hidden from hello graph.</span></span> <span data-ttu-id="a21f6-143">Om du klickar på den statuskoden igen kommer att alternativet toobe visas igen.</span><span class="sxs-lookup"><span data-stu-id="a21f6-143">Clicking that status code again will cause that option toobe displayed again.</span></span>

## <a name="cache-statuses"></a><span data-ttu-id="a21f6-144">Cache-status</span><span class="sxs-lookup"><span data-stu-id="a21f6-144">Cache Statuses</span></span>
![Diagram för cache-status](./media/cdn-real-time-stats/cdn-cache-status.png)

<span data-ttu-id="a21f6-146">Hej **Cache statusar** diagram visar hur ofta vissa typer av cache-status sker via hello valda tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="a21f6-146">hello **Cache Statuses** graph indicates how often certain types of cache statuses are occurring over hello selected time span.</span></span> 

> [!TIP]
> <span data-ttu-id="a21f6-147">En beskrivning av varje cache status kod alternativ finns [Azure CDN Cache-statuskoder](https://msdn.microsoft.com/library/mt759237.aspx).</span><span class="sxs-lookup"><span data-stu-id="a21f6-147">For a description of each cache status code option, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx).</span></span>
> 
> 

<span data-ttu-id="a21f6-148">En lista över cache statuskoder visas direkt ovanför hello diagram.</span><span class="sxs-lookup"><span data-stu-id="a21f6-148">A list of cache status codes is displayed directly above hello graph.</span></span> <span data-ttu-id="a21f6-149">Den här listan visar varje statuskod som kan ingå i hello linjediagram och hello aktuellt antal händelser per sekund för denna statuskod.</span><span class="sxs-lookup"><span data-stu-id="a21f6-149">This list indicates each status code that can be included in hello line graph and hello current number of occurrences per second for that status code.</span></span> <span data-ttu-id="a21f6-150">Som standard visas en rad för var och en av dessa statuskoder i hello graph.</span><span class="sxs-lookup"><span data-stu-id="a21f6-150">By default, a line is displayed for each of these status codes in hello graph.</span></span> <span data-ttu-id="a21f6-151">Du kan dock välja tooonly övervakaren hello statuskoder som har särskild betydelse för din CDN-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a21f6-151">However, you can choose tooonly monitor hello status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="a21f6-152">toodo, markera önskad hello-statuskoder och avmarkera alla andra alternativ och klicka på **uppdatera diagrammet**.</span><span class="sxs-lookup"><span data-stu-id="a21f6-152">toodo this, check hello desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="a21f6-153">Du kan dölja loggdata för en viss statuskod tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="a21f6-153">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="a21f6-154">Klicka på hello statuskod som du vill toohide hello förklaringen direkt under hello diagram.</span><span class="sxs-lookup"><span data-stu-id="a21f6-154">From hello legend directly below hello graph, click hello status code you want toohide.</span></span> <span data-ttu-id="a21f6-155">hello-statuskoden ska vara dolda direkt från hello diagram.</span><span class="sxs-lookup"><span data-stu-id="a21f6-155">hello status code will be immediately hidden from hello graph.</span></span> <span data-ttu-id="a21f6-156">Om du klickar på den statuskoden igen kommer att alternativet toobe visas igen.</span><span class="sxs-lookup"><span data-stu-id="a21f6-156">Clicking that status code again will cause that option toobe displayed again.</span></span>

## <a name="connections"></a><span data-ttu-id="a21f6-157">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="a21f6-157">Connections</span></span>
![Anslutningar diagram](./media/cdn-real-time-stats/cdn-connections.png)

<span data-ttu-id="a21f6-159">Det här diagrammet visar hur många anslutningar har etablerat tooyour kant-servrar.</span><span class="sxs-lookup"><span data-stu-id="a21f6-159">This graph indicates how many connections have been established tooyour edge servers.</span></span> <span data-ttu-id="a21f6-160">Varje begäran för en tillgång som passerar genom våra CDN resultat i en anslutning.</span><span class="sxs-lookup"><span data-stu-id="a21f6-160">Each request for an asset that passes through our CDN results in a connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a21f6-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a21f6-161">Next Steps</span></span>
* <span data-ttu-id="a21f6-162">Håll dig informerad med [aviseringar i realtid i Azure CDN](cdn-real-time-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="a21f6-162">Get notified with [Real-time alerts in Azure CDN](cdn-real-time-alerts.md)</span></span>
* <span data-ttu-id="a21f6-163">Gräva djupare med [avancerade http-rapporter](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="a21f6-163">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="a21f6-164">Analysera [användningsmönster](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="a21f6-164">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

