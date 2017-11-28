---
title: "aaa Azure Stream Analytics datadrivna felsökning med hjälp av hello jobbet diagram | Microsoft Docs"
description: "Felsöka Stream Analytics-jobbet med hello jobbet diagram och mått."
keywords: 
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
ms.date: 05/01/2017
ms.author: jeffstok
ms.openlocfilehash: 1af884d485bebb06b034da01a13f7f8240516571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-driven-debugging-by-using-hello-job-diagram"></a><span data-ttu-id="c6727-103">Datadrivna felsökning med hjälp av hello jobbet diagram</span><span class="sxs-lookup"><span data-stu-id="c6727-103">Data-driven debugging by using hello job diagram</span></span>

<span data-ttu-id="c6727-104">hello jobbet diagram som visar hello **övervakning** bladet i hello Azure-portalen kan hjälpa dig att visualisera dina jobb pipeline.</span><span class="sxs-lookup"><span data-stu-id="c6727-104">hello job diagram on hello **Monitoring** blade in hello Azure portal can help you visualize your job pipeline.</span></span> <span data-ttu-id="c6727-105">Den visar indata, utdata och frågesteg.</span><span class="sxs-lookup"><span data-stu-id="c6727-105">It shows inputs, outputs, and query steps.</span></span> <span data-ttu-id="c6727-106">Du kan använda hello jobbet diagram tooexamine hello mätvärden för varje steg, toomore Isolera snabbt hello orsaken till ett problem när du felsöker problem.</span><span class="sxs-lookup"><span data-stu-id="c6727-106">You can use hello job diagram tooexamine hello metrics for each step, toomore quickly isolate hello source of a problem when you troubleshoot issues.</span></span>

## <a name="using-hello-job-diagram"></a><span data-ttu-id="c6727-107">Med hjälp av hello jobbet diagram</span><span class="sxs-lookup"><span data-stu-id="c6727-107">Using hello job diagram</span></span>

<span data-ttu-id="c6727-108">I hello Azure-portalen när den är i ett Stream Analytics-jobb **stöd + felsökning**väljer **jobbet diagram**:</span><span class="sxs-lookup"><span data-stu-id="c6727-108">In hello Azure portal, while in a Stream Analytics job, under **SUPPORT + TROUBLESHOOTING**, select **Job diagram**:</span></span>

![Jobbet diagram med - plats](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

<span data-ttu-id="c6727-110">Markera varje fråga steg toosee hello motsvarande avsnitt i en fråga Redigera fönstret.</span><span class="sxs-lookup"><span data-stu-id="c6727-110">Select each query step toosee hello corresponding section in a query editing pane.</span></span> <span data-ttu-id="c6727-111">Ett mått diagram för hello steg visas i nedre fönstret på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="c6727-111">A metric chart for hello step is displayed in a lower pane on hello page.</span></span>

![Jobbet diagram med - grundläggande jobb](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

<span data-ttu-id="c6727-113">toosee hello partitioner i hello Azure Event Hubs indata, Välj **...**</span><span class="sxs-lookup"><span data-stu-id="c6727-113">toosee hello partitions of hello Azure Event Hubs input, select **. . .**</span></span> <span data-ttu-id="c6727-114">En snabbmeny visas.</span><span class="sxs-lookup"><span data-stu-id="c6727-114">A context menu appears.</span></span> <span data-ttu-id="c6727-115">Du kan också se hello inkommande fusion.</span><span class="sxs-lookup"><span data-stu-id="c6727-115">You also can see hello input merger.</span></span>

![Jobbet diagram med - Utöka partition](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

<span data-ttu-id="c6727-117">toosee hello mått diagram för endast en partition, Välj hello partition nod.</span><span class="sxs-lookup"><span data-stu-id="c6727-117">toosee hello metric chart for only a single partition, select hello partition node.</span></span> <span data-ttu-id="c6727-118">hello mått som visas på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="c6727-118">hello metrics are shown at hello bottom of hello page.</span></span>

![Jobbet diagram med - flera mått](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

<span data-ttu-id="c6727-120">toosee hello mått diagram för en fusion väljer hello fusion nod.</span><span class="sxs-lookup"><span data-stu-id="c6727-120">toosee hello metrics chart for a merger, select hello merger node.</span></span> <span data-ttu-id="c6727-121">hello följande diagram visar att inga händelser släpptes eller justeras.</span><span class="sxs-lookup"><span data-stu-id="c6727-121">hello following chart shows that no events were dropped or adjusted.</span></span>

![Jobbet diagram med - rutnätet](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

<span data-ttu-id="c6727-123">toosee hello information om hello värde och tid, punkt toohello diagram.</span><span class="sxs-lookup"><span data-stu-id="c6727-123">toosee hello details of hello metric value and time, point toohello chart.</span></span>

![Jobbet diagram med - hovra](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a><span data-ttu-id="c6727-125">Felsöka med mått</span><span class="sxs-lookup"><span data-stu-id="c6727-125">Troubleshoot by using metrics</span></span>

<span data-ttu-id="c6727-126">Hej **QueryLastProcessedTime** mått som anger när ett specifikt steg tog emot data.</span><span class="sxs-lookup"><span data-stu-id="c6727-126">hello **QueryLastProcessedTime** metric indicates when a specific step received data.</span></span> <span data-ttu-id="c6727-127">Genom att titta på hello topologi arbeta du bakåt från hello utdata processor toosee vilka steg inte tar emot data.</span><span class="sxs-lookup"><span data-stu-id="c6727-127">By looking at hello topology, you can work backward from hello output processor toosee which step is not receiving data.</span></span> <span data-ttu-id="c6727-128">Om ett steg inte blir data, går toohello frågesteg innan.</span><span class="sxs-lookup"><span data-stu-id="c6727-128">If a step is not getting data, go toohello query step just before it.</span></span> <span data-ttu-id="c6727-129">Kontrollera om hello föregående frågesteg har ett tidsfönster och om tillräckligt med tid har förflutit för den toooutput data.</span><span class="sxs-lookup"><span data-stu-id="c6727-129">Check whether hello preceding query step has a time window, and if enough time has passed for it toooutput data.</span></span> <span data-ttu-id="c6727-130">(Observera då windows är fästa element toohello timme.)</span><span class="sxs-lookup"><span data-stu-id="c6727-130">(Note that time windows are snapped toohello hour.)</span></span>
 
<span data-ttu-id="c6727-131">Om hello föregående frågesteg har ett indata-processor avsedda användning hello inkommande mått toohelp svar hello följande frågor.</span><span class="sxs-lookup"><span data-stu-id="c6727-131">If hello preceding query step is an input processor, use hello input metrics toohelp answer hello following targeted questions.</span></span> <span data-ttu-id="c6727-132">De kan hjälpa dig att avgöra om ett jobb hämtar data från dess indatakällor.</span><span class="sxs-lookup"><span data-stu-id="c6727-132">They can help you determine whether a job is getting data from its input sources.</span></span> <span data-ttu-id="c6727-133">Granska varje partition om hello frågan är partitionerad.</span><span class="sxs-lookup"><span data-stu-id="c6727-133">If hello query is partitioned, examine each partition.</span></span>
 
### <a name="how-much-data-is-being-read"></a><span data-ttu-id="c6727-134">Hur mycket data läses?</span><span class="sxs-lookup"><span data-stu-id="c6727-134">How much data is being read?</span></span>

*   <span data-ttu-id="c6727-135">**InputEventsSourcesTotal** är hello antalet enheter läsa.</span><span class="sxs-lookup"><span data-stu-id="c6727-135">**InputEventsSourcesTotal** is hello number of data units read.</span></span> <span data-ttu-id="c6727-136">Till exempel hello antal blobbar.</span><span class="sxs-lookup"><span data-stu-id="c6727-136">For example, hello number of blobs.</span></span>
*   <span data-ttu-id="c6727-137">**InputEventsTotal** är hello antalet händelser som läsa.</span><span class="sxs-lookup"><span data-stu-id="c6727-137">**InputEventsTotal** is hello number of events read.</span></span> <span data-ttu-id="c6727-138">Det här måttet är tillgängligt per partition.</span><span class="sxs-lookup"><span data-stu-id="c6727-138">This metric is available per partition.</span></span>
*   <span data-ttu-id="c6727-139">**InputEventsInBytesTotal** är hello antalet lästa byte.</span><span class="sxs-lookup"><span data-stu-id="c6727-139">**InputEventsInBytesTotal** is hello number of bytes read.</span></span>
*   <span data-ttu-id="c6727-140">**InputEventsLastArrivalTime** uppdateras med tiden för alla mottagna händelser i kö.</span><span class="sxs-lookup"><span data-stu-id="c6727-140">**InputEventsLastArrivalTime** is updated with every received event's enqueued time.</span></span>
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a><span data-ttu-id="c6727-141">Är tiden går vidare?</span><span class="sxs-lookup"><span data-stu-id="c6727-141">Is time moving forward?</span></span> <span data-ttu-id="c6727-142">Om faktiska händelser läses kanske inte skiljetecken utfärdas.</span><span class="sxs-lookup"><span data-stu-id="c6727-142">If actual events are read, punctuation might not be issued.</span></span>

*   <span data-ttu-id="c6727-143">**InputEventsLastPunctuationTime** anger när en skiljetecken utfärdades tookeep tid flyttning framåt.</span><span class="sxs-lookup"><span data-stu-id="c6727-143">**InputEventsLastPunctuationTime** indicates when a punctuation was issued tookeep time moving forward.</span></span> <span data-ttu-id="c6727-144">Om skiljetecken inte är utfärdat kan dataflöde blockeras.</span><span class="sxs-lookup"><span data-stu-id="c6727-144">If punctuation is not issued, data flow can get blocked.</span></span>
 
### <a name="are-there-any-errors-in-hello-input"></a><span data-ttu-id="c6727-145">Finns det några fel i hello indata?</span><span class="sxs-lookup"><span data-stu-id="c6727-145">Are there any errors in hello input?</span></span>

*   <span data-ttu-id="c6727-146">**InputEventsEventDataNullTotal** antal händelser som har null-data.</span><span class="sxs-lookup"><span data-stu-id="c6727-146">**InputEventsEventDataNullTotal** is a count of events that have null data.</span></span>
*   <span data-ttu-id="c6727-147">**InputEventsSerializerErrorsTotal** antal händelser som inte gick att deserialisera korrekt.</span><span class="sxs-lookup"><span data-stu-id="c6727-147">**InputEventsSerializerErrorsTotal** is a count of events that could not be deserialized correctly.</span></span>
*   <span data-ttu-id="c6727-148">**InputEventsDegradedTotal** antal händelser som har haft problem än med deserialisering.</span><span class="sxs-lookup"><span data-stu-id="c6727-148">**InputEventsDegradedTotal** is a count of events that had an issue other than with deserialization.</span></span>
 
### <a name="are-events-being-dropped-or-adjusted"></a><span data-ttu-id="c6727-149">Är händelser tas bort eller justeras?</span><span class="sxs-lookup"><span data-stu-id="c6727-149">Are events being dropped or adjusted?</span></span>

*   <span data-ttu-id="c6727-150">**InputEventsEarlyTotal** är hello antalet händelser som har ett Programtidsstämpel innan hello övre gräns.</span><span class="sxs-lookup"><span data-stu-id="c6727-150">**InputEventsEarlyTotal** is hello number of events that have an application timestamp before hello high watermark.</span></span>
*   <span data-ttu-id="c6727-151">**InputEventsLateTotal** är hello antalet händelser som har ett Programtidsstämpel efter hello övre gräns.</span><span class="sxs-lookup"><span data-stu-id="c6727-151">**InputEventsLateTotal** is hello number of events that have an application timestamp after hello high watermark.</span></span>
*   <span data-ttu-id="c6727-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** är hello antalet händelser tas bort innan hello jobbets starttid.</span><span class="sxs-lookup"><span data-stu-id="c6727-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** is hello number events dropped before hello job start time.</span></span>
 
### <a name="are-we-falling-behind-in-reading-data"></a><span data-ttu-id="c6727-153">Vi sjunker vid läsning av data?</span><span class="sxs-lookup"><span data-stu-id="c6727-153">Are we falling behind in reading data?</span></span>

*   <span data-ttu-id="c6727-154">**InputEventsSourcesBackloggedTotal** får du reda på hur många fler meddelanden behöver toobe läsa för Event Hubs och Azure IoT Hub-indata.</span><span class="sxs-lookup"><span data-stu-id="c6727-154">**InputEventsSourcesBackloggedTotal** tells you how many more messages need toobe read for Event Hubs and Azure IoT Hub inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="c6727-155">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="c6727-155">Get help</span></span>
<span data-ttu-id="c6727-156">Mer hjälp, försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="c6727-156">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6727-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6727-157">Next steps</span></span>
* [<span data-ttu-id="c6727-158">Introduktion tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="c6727-158">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="c6727-159">Kom igång med Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="c6727-159">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="c6727-160">Skala Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="c6727-160">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="c6727-161">Språkreferens för Stream Analytics-fråga</span><span class="sxs-lookup"><span data-stu-id="c6727-161">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="c6727-162">Strömma Analytics management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="c6727-162">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
