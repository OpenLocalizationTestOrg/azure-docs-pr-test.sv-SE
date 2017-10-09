---
title: "aaaIntroduction tooStream Analytics fönstrets funktioner | Microsoft Docs"
description: "Läs mer om hello tre fönstrets funktioner i Stream Analytics (rullande hopping, glidande)."
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
ms.openlocfilehash: 7fc36eb9afb2b28e2791d925d26923145eb38074
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toostream-analytics-window-functions"></a><span data-ttu-id="25083-104">Introduktion tooStream Analytics fönstrets funktioner</span><span class="sxs-lookup"><span data-stu-id="25083-104">Introduction tooStream Analytics Window functions</span></span>
<span data-ttu-id="25083-105">I många realtid strömning scenarier, är det nödvändigt tooperform endast åtgärder på hello data i den temporala windows.</span><span class="sxs-lookup"><span data-stu-id="25083-105">In many real time streaming scenarios, it is necessary tooperform operations only on hello data contained in temporal windows.</span></span> <span data-ttu-id="25083-106">Inbyggt stöd för fönsterhantering funktioner är en nyckelfunktion i Azure Stream Analytics som flyttar hello nålen på utvecklarproduktivitet i authoring komplexa dataströmmen bearbetar jobb.</span><span class="sxs-lookup"><span data-stu-id="25083-106">Native support for windowing functions is a key feature of Azure Stream Analytics that moves hello needle on developer productivity in authoring complex stream processing jobs.</span></span> <span data-ttu-id="25083-107">Stream Analytics gör det möjligt för utvecklare toouse [ **rullande**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) och [ **glidande** ](https://msdn.microsoft.com/library/dn835051.aspx) windows tooperform temporala åtgärder på strömmande data.</span><span class="sxs-lookup"><span data-stu-id="25083-107">Stream Analytics enables developers toouse [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) and [**Sliding**](https://msdn.microsoft.com/library/dn835051.aspx) windows tooperform temporal operations on streaming data.</span></span> <span data-ttu-id="25083-108">Det är värt att nämna som alla [fönstret](https://msdn.microsoft.com/library/dn835019.aspx) operations utdata resultat på hello **end** av hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="25083-108">It is worth noting that all [Window](https://msdn.microsoft.com/library/dn835019.aspx) operations output results at hello **end** of hello window.</span></span> <span data-ttu-id="25083-109">hello utdata från hello fönstret blir enskild händelse baserat på hello mängdfunktion används.</span><span class="sxs-lookup"><span data-stu-id="25083-109">hello output of hello window will be single event based on hello aggregate function used.</span></span> <span data-ttu-id="25083-110">hello händelse har hello tidsstämpeln för hello slutet av hello fönstret och alla Windows-funktioner har definierats med en fast längd.</span><span class="sxs-lookup"><span data-stu-id="25083-110">hello event will have hello time stamp of hello end of hello window and all Window functions are defined with a fixed length.</span></span> <span data-ttu-id="25083-111">Slutligen är det viktigt toonote som alla Windows-funktioner som ska användas i en [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) satsen.</span><span class="sxs-lookup"><span data-stu-id="25083-111">Lastly it is important toonote that all Window functions should be used in a [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) clause.</span></span>

![Stream Analytics fönstret fungerar begrepp](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a><span data-ttu-id="25083-113">Rullande fönster</span><span class="sxs-lookup"><span data-stu-id="25083-113">Tumbling Window</span></span>
<span data-ttu-id="25083-114">Rullande fönster funktioner är används toosegment en dataström i distinkta tid segment och utför en funktion mot dem, till exempel hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="25083-114">Tumbling window functions are used toosegment a data stream into distinct time segments and perform a function against them, such as hello example below.</span></span> <span data-ttu-id="25083-115">hello viktiga skillnaderna i en rullande fönster är att de upprepar, inte överlappar och en händelse kan inte tillhöra toomore än en rullande fönster.</span><span class="sxs-lookup"><span data-stu-id="25083-115">hello key differentiators of a Tumbling window are that they repeat, do not overlap and an event cannot belong toomore than one tumbling window.</span></span>

![Stream Analytics-fönstrets funktioner rullande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a><span data-ttu-id="25083-117">Hoppande fönster</span><span class="sxs-lookup"><span data-stu-id="25083-117">Hopping Window</span></span>
<span data-ttu-id="25083-118">Hoppande fönster funktioner hopp framåt genom en bestämd tid.</span><span class="sxs-lookup"><span data-stu-id="25083-118">Hopping window functions hop forward in time by a fixed period.</span></span> <span data-ttu-id="25083-119">Det kan vara enkelt toothink av dem som rullande fönster som kan överlappa så händelser kan höra toomore än Hopping fönstret resultatuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="25083-119">It may be easy toothink of them as Tumbling windows that can overlap, so events can belong toomore than one Hopping window result set.</span></span> <span data-ttu-id="25083-120">toomake ett Hopping fönster hello samma som en rullande fönster något att ange hello hopp storlek toobe hello samma som hello fönsterstorlek.</span><span class="sxs-lookup"><span data-stu-id="25083-120">toomake a Hopping window hello same as a Tumbling window one would simply specify hello hop size toobe hello same as hello window size.</span></span> 

![Stream Analytics fönstret fungerar Hoppande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a><span data-ttu-id="25083-122">Hoppande fönster</span><span class="sxs-lookup"><span data-stu-id="25083-122">Sliding Window</span></span>
<span data-ttu-id="25083-123">Glidande fönstrets funktioner, till skillnad från rullande eller Hopping windows, skapa ett utgående **endast** när en händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="25083-123">Sliding window functions, unlike Tumbling or Hopping windows, produce an output **only**  when an event occurs.</span></span> <span data-ttu-id="25083-124">Alla fönster har minst en händelse och hello fönstret flyttar kontinuerligt fram en € (epsilon).</span><span class="sxs-lookup"><span data-stu-id="25083-124">Every window will have at least one event and hello window continuously moves forward by an € (epsilon).</span></span> <span data-ttu-id="25083-125">Händelser kan höra toomore än ett glidande fönster som Hopping Windows.</span><span class="sxs-lookup"><span data-stu-id="25083-125">Like Hopping Windows, events can belong toomore than one Sliding Window.</span></span>

![Stream Analytics-fönstrets funktioner glidande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a><span data-ttu-id="25083-127">Få hjälp med att fönstrets funktioner</span><span class="sxs-lookup"><span data-stu-id="25083-127">Getting help with Window functions</span></span>
<span data-ttu-id="25083-128">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="25083-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="25083-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25083-129">Next steps</span></span>
* [<span data-ttu-id="25083-130">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="25083-130">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="25083-131">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="25083-131">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="25083-132">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="25083-132">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="25083-133">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="25083-133">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="25083-134">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="25083-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

