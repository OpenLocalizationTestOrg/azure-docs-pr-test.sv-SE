---
title: "Introduktion till Stream Analytics-fönstrets funktioner | Microsoft Docs"
description: "Läs mer om de tre funktionerna i fönstret i Stream Analytics (rullande hopping, glidande)."
keywords: "rullande fönster glidande fönstret hopping fönster"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 09915ad747153210f655b152355c551046066a88
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-stream-analytics-window-functions"></a><span data-ttu-id="3f97f-104">Introduktion till Stream Analytics-fönstrets funktioner</span><span class="sxs-lookup"><span data-stu-id="3f97f-104">Introduction to Stream Analytics Window functions</span></span>
<span data-ttu-id="3f97f-105">I många realtid strömning scenarier, är det nödvändigt att utföra åtgärder endast på de data som finns i den temporala windows.</span><span class="sxs-lookup"><span data-stu-id="3f97f-105">In many real time streaming scenarios, it is necessary to perform operations only on the data contained in temporal windows.</span></span> <span data-ttu-id="3f97f-106">Inbyggt stöd för fönsterhantering funktioner är en nyckelfunktion i Azure Stream Analytics som flyttar nålen på utvecklarproduktivitet i authoring komplexa dataströmmen bearbetar jobb.</span><span class="sxs-lookup"><span data-stu-id="3f97f-106">Native support for windowing functions is a key feature of Azure Stream Analytics that moves the needle on developer productivity in authoring complex stream processing jobs.</span></span> <span data-ttu-id="3f97f-107">Stream Analytics gör att utvecklare kan använda [ **rullande**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) och [ **glidande** ](https://msdn.microsoft.com/library/dn835051.aspx) windows utföra temporala åtgärder på strömmande data.</span><span class="sxs-lookup"><span data-stu-id="3f97f-107">Stream Analytics enables developers to use [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) and [**Sliding**](https://msdn.microsoft.com/library/dn835051.aspx) windows to perform temporal operations on streaming data.</span></span> <span data-ttu-id="3f97f-108">Det är värt att nämna som alla [fönstret](https://msdn.microsoft.com/library/dn835019.aspx) operations utdata resultat på den **end** i fönstret.</span><span class="sxs-lookup"><span data-stu-id="3f97f-108">It is worth noting that all [Window](https://msdn.microsoft.com/library/dn835019.aspx) operations output results at the **end** of the window.</span></span> <span data-ttu-id="3f97f-109">Utdata från fönstret blir enskild händelse baserat på mängdfunktionen används.</span><span class="sxs-lookup"><span data-stu-id="3f97f-109">The output of the window will be single event based on the aggregate function used.</span></span> <span data-ttu-id="3f97f-110">Händelsen har tidsstämpeln för slutet av fönstret och alla Windows-funktioner har definierats med en fast längd.</span><span class="sxs-lookup"><span data-stu-id="3f97f-110">The event will have the time stamp of the end of the window and all Window functions are defined with a fixed length.</span></span> <span data-ttu-id="3f97f-111">Slutligen är det viktigt att notera att alla Windows-funktioner kan användas i en [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) satsen.</span><span class="sxs-lookup"><span data-stu-id="3f97f-111">Lastly it is important to note that all Window functions should be used in a [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) clause.</span></span>

![Stream Analytics fönstret fungerar begrepp](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a><span data-ttu-id="3f97f-113">Rullande fönster</span><span class="sxs-lookup"><span data-stu-id="3f97f-113">Tumbling Window</span></span>
<span data-ttu-id="3f97f-114">Rullande fönster funktioner som används för att segmentera en dataström i distinkta tid segment och utföra en funktion mot dem, till exempel i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="3f97f-114">Tumbling window functions are used to segment a data stream into distinct time segments and perform a function against them, such as the example below.</span></span> <span data-ttu-id="3f97f-115">Viktiga skillnaderna i en rullande fönster är att de upprepar, inte överlappar och en händelse får inte tillhöra mer än en rullande fönster.</span><span class="sxs-lookup"><span data-stu-id="3f97f-115">The key differentiators of a Tumbling window are that they repeat, do not overlap and an event cannot belong to more than one tumbling window.</span></span>

![Stream Analytics-fönstrets funktioner rullande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a><span data-ttu-id="3f97f-117">Hoppande fönster</span><span class="sxs-lookup"><span data-stu-id="3f97f-117">Hopping Window</span></span>
<span data-ttu-id="3f97f-118">Hoppande fönster funktioner hopp framåt genom en bestämd tid.</span><span class="sxs-lookup"><span data-stu-id="3f97f-118">Hopping window functions hop forward in time by a fixed period.</span></span> <span data-ttu-id="3f97f-119">Det kan vara enkelt att se dem som rullande fönster som kan överlappa så händelser kan tillhöra mer än en resultatmängd i Hopping-fönstret.</span><span class="sxs-lookup"><span data-stu-id="3f97f-119">It may be easy to think of them as Tumbling windows that can overlap, so events can belong to more than one Hopping window result set.</span></span> <span data-ttu-id="3f97f-120">Om du vill ha ett Hopping fönster som en rullande ange fönster en bara hoppstorleken för att vara samma som fönsterstorleken.</span><span class="sxs-lookup"><span data-stu-id="3f97f-120">To make a Hopping window the same as a Tumbling window one would simply specify the hop size to be the same as the window size.</span></span> 

![Stream Analytics fönstret fungerar Hoppande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a><span data-ttu-id="3f97f-122">Hoppande fönster</span><span class="sxs-lookup"><span data-stu-id="3f97f-122">Sliding Window</span></span>
<span data-ttu-id="3f97f-123">Glidande fönstrets funktioner, till skillnad från rullande eller Hopping windows, skapa ett utgående **endast** när en händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="3f97f-123">Sliding window functions, unlike Tumbling or Hopping windows, produce an output **only**  when an event occurs.</span></span> <span data-ttu-id="3f97f-124">Alla fönster har minst en händelse och fönstret flyttar kontinuerligt fram en € (epsilon).</span><span class="sxs-lookup"><span data-stu-id="3f97f-124">Every window will have at least one event and the window continuously moves forward by an € (epsilon).</span></span> <span data-ttu-id="3f97f-125">Som Hopping Windows kan händelser tillhöra mer än ett glidande fönster.</span><span class="sxs-lookup"><span data-stu-id="3f97f-125">Like Hopping Windows, events can belong to more than one Sliding Window.</span></span>

![Stream Analytics-fönstrets funktioner glidande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a><span data-ttu-id="3f97f-127">Få hjälp med att fönstrets funktioner</span><span class="sxs-lookup"><span data-stu-id="3f97f-127">Getting help with Window functions</span></span>
<span data-ttu-id="3f97f-128">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="3f97f-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f97f-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f97f-129">Next steps</span></span>
* [<span data-ttu-id="3f97f-130">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3f97f-130">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="3f97f-131">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3f97f-131">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="3f97f-132">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="3f97f-132">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="3f97f-133">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="3f97f-133">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="3f97f-134">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="3f97f-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

