---
title: "aaaHandling händelse ordning och lateness med Azure Stream Analytics | Microsoft Docs"
description: "Läs mer om hur Stream Analytics fungerar med out-ordning eller försenade händelser i dataströmmar."
keywords: "i ordning, Sen, händelser"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 87c028662fbafbf4f72f57f215d017f65bb649f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a><span data-ttu-id="226d3-104">Azure Stream Analytics ordning händelsehantering</span><span class="sxs-lookup"><span data-stu-id="226d3-104">Azure Stream Analytics event order handling</span></span>

<span data-ttu-id="226d3-105">Varje händelse registreras i en temporal dataström för händelser med hello tid att hello-händelsen har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="226d3-105">In a temporal data stream of events, each event is recorded with hello time that hello event is received.</span></span> <span data-ttu-id="226d3-106">Vissa villkor kan orsaka händelseströmmar toooccasionally tar emot vissa händelser i en annan ordning än som de skickades.</span><span class="sxs-lookup"><span data-stu-id="226d3-106">Some conditions might cause event streams toooccasionally receive some events in a different order than which they were sent.</span></span> <span data-ttu-id="226d3-107">En enkel TCP skickar eller även en förskjutning av klockan mellan hello skickar enheten och ta emot händelsehubb hello kan leda till att den här toooccur.</span><span class="sxs-lookup"><span data-stu-id="226d3-107">A simple TCP retransmit, or even a clock skew between hello sending device and hello receiving event hub might cause this toooccur.</span></span> <span data-ttu-id="226d3-108">Händelser som ”skiljetecken” också läggs tooreceived händelseströmmar tooadvance hello tid händelse mottagna hello saknas.</span><span class="sxs-lookup"><span data-stu-id="226d3-108">“Punctuation” events also are added tooreceived event streams, tooadvance hello time in hello absence of event arrivals.</span></span> <span data-ttu-id="226d3-109">Dessa krävs scenarier som ”Avisera mig när inga inloggningar inträffar för 3 minuter”.</span><span class="sxs-lookup"><span data-stu-id="226d3-109">These are needed in scenarios like “Notify me when no logins occur for 3 minutes."</span></span>

<span data-ttu-id="226d3-110">Antingen är inkommande dataströmmar som inte är i ordning:</span><span class="sxs-lookup"><span data-stu-id="226d3-110">Input streams that are not in order are either:</span></span>
* <span data-ttu-id="226d3-111">Sorterad (och därför **fördröjd**).</span><span class="sxs-lookup"><span data-stu-id="226d3-111">Sorted (and therefore **delayed**).</span></span>
* <span data-ttu-id="226d3-112">Justeras hello filsystem, enligt tooa användardefinierade princip.</span><span class="sxs-lookup"><span data-stu-id="226d3-112">Adjusted by hello system, according tooa user-specified policy.</span></span>


## <a name="lateness-tolerance"></a><span data-ttu-id="226d3-113">Lateness tolerans</span><span class="sxs-lookup"><span data-stu-id="226d3-113">Lateness tolerance</span></span>
<span data-ttu-id="226d3-114">Stream Analytics kan tolerera dessa typer av scenarier.</span><span class="sxs-lookup"><span data-stu-id="226d3-114">Stream Analytics tolerates these types of scenarios.</span></span> <span data-ttu-id="226d3-115">Stream Analytics har hantering för ”out ordning” och ”sent” händelser.</span><span class="sxs-lookup"><span data-stu-id="226d3-115">Stream Analytics has handling for "out-of-order" and "late" events.</span></span> <span data-ttu-id="226d3-116">Den hanterar dessa händelser i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="226d3-116">It handles these events in hello following ways:</span></span>

* <span data-ttu-id="226d3-117">Händelser som anländer utanför ordning men hello Ange tolerans är **ordnas om av tidsstämpel**.</span><span class="sxs-lookup"><span data-stu-id="226d3-117">Events that arrive out of order but within hello set tolerance are **reordered by timestamp**.</span></span>
* <span data-ttu-id="226d3-118">Händelser som kommer senare än tolerans är **släpptes eller justeras**.</span><span class="sxs-lookup"><span data-stu-id="226d3-118">Events that arrive later than tolerance are **dropped or adjusted**.</span></span>
    * <span data-ttu-id="226d3-119">**Justeras**: justerade tooappear toohave som anlänt på hello senaste acceptabel tid.</span><span class="sxs-lookup"><span data-stu-id="226d3-119">**Adjusted**: Adjusted tooappear toohave arrived at hello latest acceptable time.</span></span>
    * <span data-ttu-id="226d3-120">**Bort**: ignoreras.</span><span class="sxs-lookup"><span data-stu-id="226d3-120">**Dropped**: Discarded.</span></span>

![Stream Analytics händelsehantering](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-hello-number-of-out-of-order-events"></a><span data-ttu-id="226d3-122">Minska hello antalet händelser som out-ordning</span><span class="sxs-lookup"><span data-stu-id="226d3-122">Reduce hello number of out-of-order events</span></span>

<span data-ttu-id="226d3-123">Eftersom Stream Analytics gäller en temporal omvandling när den bearbetar inkommande händelser (till exempel för fönsteraggregeringar eller temporala kopplingar), sorterar Stream Analytics inkommande händelser efter tidsstämpel ordning.</span><span class="sxs-lookup"><span data-stu-id="226d3-123">Because Stream Analytics applies a temporal transformation when it processes incoming events (for example, for windowed aggregates or temporal joins), Stream Analytics sorts incoming events by timestamp order.</span></span>

<span data-ttu-id="226d3-124">När hello ”tidsstämpel av” nyckelord är **inte** används, hello Azure Event Hubs sätta händelsetid används som standard.</span><span class="sxs-lookup"><span data-stu-id="226d3-124">When hello “timestamp by” keyword is **not** used, hello Azure Event Hubs event enqueue time is used by default.</span></span> <span data-ttu-id="226d3-125">Händelsehubbar garanterar monotonicity av hello tidsstämpeln på varje partition för hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="226d3-125">Event Hubs guarantees monotonicity of hello timestamp on each partition of hello event hub.</span></span> <span data-ttu-id="226d3-126">Det garanterar även sammanfogas händelser från alla partitioner i tidsstämpel ordning.</span><span class="sxs-lookup"><span data-stu-id="226d3-126">It also guarantees that events from all partitions will be merged in timestamp order.</span></span> <span data-ttu-id="226d3-127">Dessa två Händelsehubbar garanterar att inga händelser för out-ordning.</span><span class="sxs-lookup"><span data-stu-id="226d3-127">These two Event Hubs guarantees ensure no out-of-order events.</span></span>

<span data-ttu-id="226d3-128">Ibland är det viktigt att du toouse hello avsändarens tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="226d3-128">Sometimes, it’s important for you toouse hello sender’s timestamp.</span></span> <span data-ttu-id="226d3-129">I så fall väljs en tidsstämpel från hello händelsenyttolast med hjälp av ”tidsstämpel av”.</span><span class="sxs-lookup"><span data-stu-id="226d3-129">In that case, a timestamp from hello event payload is chosen by using “timestamp by.”</span></span> <span data-ttu-id="226d3-130">En eller flera källor för händelsen misorder kan införas i följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="226d3-130">In these scenarios, one or more sources of event misorder might be introduced:</span></span>

* <span data-ttu-id="226d3-131">Händelsen producenter har jämföra.</span><span class="sxs-lookup"><span data-stu-id="226d3-131">Event producers have clock skews.</span></span> <span data-ttu-id="226d3-132">Detta är vanligt när producenter kommer från olika datorer, så de har olika klockor.</span><span class="sxs-lookup"><span data-stu-id="226d3-132">This is common when producers are from different computers, so they have different clocks.</span></span>
* <span data-ttu-id="226d3-133">Det uppstår en fördröjning för nätverk från hello källan för hello händelser toohello mål händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="226d3-133">There's a network delay from hello source of hello events toohello destination event hub.</span></span>
* <span data-ttu-id="226d3-134">Jämföra finns mellan event hub partitioner.</span><span class="sxs-lookup"><span data-stu-id="226d3-134">Clock skews exist between event hub partitions.</span></span> <span data-ttu-id="226d3-135">Stream Analytics sorterar först händelser från alla event hub partitioner genom att sätta händelsetid.</span><span class="sxs-lookup"><span data-stu-id="226d3-135">Stream Analytics first sorts events from all event hub partitions by event enqueue time.</span></span> <span data-ttu-id="226d3-136">Sedan undersöks hello dataströmmen för misordered händelser.</span><span class="sxs-lookup"><span data-stu-id="226d3-136">Then, it examines hello data stream for misordered events.</span></span>

<span data-ttu-id="226d3-137">På fliken Konfiguration hello visas hello följande standardinställningar:</span><span class="sxs-lookup"><span data-stu-id="226d3-137">On hello configuration tab, you see hello following defaults:</span></span>

![Stream Analytics out ordning hantering](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

<span data-ttu-id="226d3-139">Om du använder 0 sekunder som hello out ordning tolerans fönster, du garanterar att alla händelser som är i ordning alla hello tid.</span><span class="sxs-lookup"><span data-stu-id="226d3-139">If you use 0 seconds as hello out-of-order tolerance window, you are asserting that all events are in order all hello time.</span></span> <span data-ttu-id="226d3-140">Angivna hello tre källor misordered händelser, är det osannolikt att detta är sant.</span><span class="sxs-lookup"><span data-stu-id="226d3-140">Given hello three sources of misordered events, it’s unlikely that this is true.</span></span> 

<span data-ttu-id="226d3-141">Du kan ange ett nollskiljd out ordning tolerans fönster tooallow Stream Analytics toocorrect misorder en händelse.</span><span class="sxs-lookup"><span data-stu-id="226d3-141">tooallow Stream Analytics toocorrect an event misorder, you can specify a non-zero out-of-order tolerance window.</span></span> <span data-ttu-id="226d3-142">Stream Analytics buffertar händelser in toothat fönster och sorterar om dem med hjälp av hello tidsstämpel som du har valt.</span><span class="sxs-lookup"><span data-stu-id="226d3-142">Stream Analytics buffers events up toothat window, and then reorders them by using hello timestamp you chose.</span></span> <span data-ttu-id="226d3-143">Den gäller sedan hello temporal omvandling.</span><span class="sxs-lookup"><span data-stu-id="226d3-143">It then applies hello temporal transformation.</span></span> <span data-ttu-id="226d3-144">Du kan börja med ett fönster för 3 sekunder och finjustera hello värdet tooreduce hello antalet händelser som är tiden justeras.</span><span class="sxs-lookup"><span data-stu-id="226d3-144">You can start with a 3-second window, and tune hello value tooreduce hello number of events that are time-adjusted.</span></span> 

<span data-ttu-id="226d3-145">En sidoeffekt av hello buffring är att hello utdata är **skjutas upp med hello samma tid**.</span><span class="sxs-lookup"><span data-stu-id="226d3-145">A side effect of hello buffering is that hello output is **delayed by hello same amount of time**.</span></span> <span data-ttu-id="226d3-146">Du kan finjustera hello värdet tooreduce hello antalet out ordning händelser och håll nere hello jobbet svarstid.</span><span class="sxs-lookup"><span data-stu-id="226d3-146">You can tune hello value tooreduce hello number of out-of-order events, and keep hello job latency low.</span></span>

## <a name="get-help"></a><span data-ttu-id="226d3-147">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="226d3-147">Get help</span></span>
<span data-ttu-id="226d3-148">Mer hjälp, försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="226d3-148">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="226d3-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="226d3-149">Next steps</span></span>
* [<span data-ttu-id="226d3-150">Introduktion tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="226d3-150">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="226d3-151">Kom igång med Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="226d3-151">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="226d3-152">Skala Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="226d3-152">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="226d3-153">Språkreferens för Stream Analytics-fråga</span><span class="sxs-lookup"><span data-stu-id="226d3-153">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="226d3-154">Strömma Analytics management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="226d3-154">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)