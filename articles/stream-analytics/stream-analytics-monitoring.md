---
title: "aaaUnderstanding Stream Analytics-jobbet övervakning | Microsoft Docs"
description: "Förstå övervakning Stream Analytics-jobbet"
keywords: "Övervakare för frågan"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 36819025c7b2ddbf4b9694522f1b4820407ca5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-toomonitor-queries"></a><span data-ttu-id="d87b0-104">Förstå övervakning av Stream Analytics-jobb och hur toomonitor frågor</span><span class="sxs-lookup"><span data-stu-id="d87b0-104">Understand Stream Analytics job monitoring and how toomonitor queries</span></span>

## <a name="introduction-hello-monitor-page"></a><span data-ttu-id="d87b0-105">: Hello övervakaren introduktionssida</span><span class="sxs-lookup"><span data-stu-id="d87b0-105">Introduction: hello monitor page</span></span>
<span data-ttu-id="d87b0-106">hello Azure-portalen både yta nyckeltal som kan använda toomonitor och felsöka din fråga och jobbet prestanda.</span><span class="sxs-lookup"><span data-stu-id="d87b0-106">hello Azure portal both surface key performance metrics that can be used toomonitor and troubleshoot your query and job performance.</span></span> <span data-ttu-id="d87b0-107">toosee de här måtten Bläddra toohello Stream Analytics-jobbet som du är intresserad av mätvärden för och visa hello **övervakning** avsnitt på översiktssidan för hello.</span><span class="sxs-lookup"><span data-stu-id="d87b0-107">toosee these metrics, browse toohello Stream Analytics job you are interested in seeing metrics for and view hello **Monitoring** section on hello Overview page.</span></span>  

![Övervaka länk](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

<span data-ttu-id="d87b0-109">hello fönster visas som visas:</span><span class="sxs-lookup"><span data-stu-id="d87b0-109">hello window will appear as shown:</span></span>

![Övervaka jobb instrumentpanelen](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a><span data-ttu-id="d87b0-111">Mått som är tillgängliga för Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d87b0-111">Metrics available for Stream Analytics</span></span>
| <span data-ttu-id="d87b0-112">Mått</span><span class="sxs-lookup"><span data-stu-id="d87b0-112">Metric</span></span>                 | <span data-ttu-id="d87b0-113">Definition</span><span class="sxs-lookup"><span data-stu-id="d87b0-113">Definition</span></span>                               |
| ---------------------- | ---------------------------------------- |
| <span data-ttu-id="d87b0-114">SU % användning</span><span class="sxs-lookup"><span data-stu-id="d87b0-114">SU % Utilization</span></span>       | <span data-ttu-id="d87b0-115">hello användning av hello strömning enhet(er) tilldelas tooa jobb från hello skala för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="d87b0-115">hello utilization of hello Streaming Unit(s) assigned tooa job from hello Scale tab of hello job.</span></span> <span data-ttu-id="d87b0-116">Denna indikator når 80% eller ovan är hög sannolikhet att händelsebearbetning kan fördröjas eller stoppats framsteg.</span><span class="sxs-lookup"><span data-stu-id="d87b0-116">Should this indicator reach 80%, or above, there is high probability that event processing may be delayed or stopped making progress.</span></span> |
| <span data-ttu-id="d87b0-117">Inkommande händelser</span><span class="sxs-lookup"><span data-stu-id="d87b0-117">Input Events</span></span>           | <span data-ttu-id="d87b0-118">Mängden data som tagits emot av hello Stream Analytics-jobbet, i antal händelser.</span><span class="sxs-lookup"><span data-stu-id="d87b0-118">Amount of data received by hello Stream Analytics job, in number of events.</span></span> <span data-ttu-id="d87b0-119">Detta kan vara används toovalidate att händelser skickas toohello Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="d87b0-119">This can be used toovalidate that events are being sent toohello input source.</span></span> |
| <span data-ttu-id="d87b0-120">Utdatahändelserna</span><span class="sxs-lookup"><span data-stu-id="d87b0-120">Output Events</span></span>          | <span data-ttu-id="d87b0-121">Mängden data som skickas av hello Stream Analytics-jobbet toohello utdata mål, i antal händelser.</span><span class="sxs-lookup"><span data-stu-id="d87b0-121">Amount of data sent by hello Stream Analytics job toohello output target, in number of events.</span></span> |
| <span data-ttu-id="d87b0-122">Out-ordning händelser</span><span class="sxs-lookup"><span data-stu-id="d87b0-122">Out-of-Order Events</span></span>    | <span data-ttu-id="d87b0-123">Antalet händelser som mottagits i felaktig ordningsföljd som antingen tagits bort eller ges en justerade timestamp, baserat på hello händelse ordning princip.</span><span class="sxs-lookup"><span data-stu-id="d87b0-123">Number of events received out of order that were either dropped or given an adjusted timestamp, based on hello Event Ordering Policy.</span></span> <span data-ttu-id="d87b0-124">Detta kan påverkas av hello konfigurationen av hello Out of Tolerance fönstret inställningen.</span><span class="sxs-lookup"><span data-stu-id="d87b0-124">This can be impacted by hello configuration of hello Out of Order Tolerance Window setting.</span></span> |
| <span data-ttu-id="d87b0-125">Data Konverteringsfel</span><span class="sxs-lookup"><span data-stu-id="d87b0-125">Data Conversion Errors</span></span> | <span data-ttu-id="d87b0-126">Antal data Konverteringsfel drabbar Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="d87b0-126">Number of data conversion errors incurred by a Stream Analytics job.</span></span> |
| <span data-ttu-id="d87b0-127">Körningsfel</span><span class="sxs-lookup"><span data-stu-id="d87b0-127">Runtime Errors</span></span>         | <span data-ttu-id="d87b0-128">hello Totalt antal fel som inträffar under körningen av ett Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="d87b0-128">hello total number of errors that happen during execution of a Stream Analytics job.</span></span> |
| <span data-ttu-id="d87b0-129">Inkommande händelser</span><span class="sxs-lookup"><span data-stu-id="d87b0-129">Late Input Events</span></span>      | <span data-ttu-id="d87b0-130">Antal händelser anländer sent från hello-källa som har antingen tagits bort eller tidsstämpel har baserat på hello händelse ordning principkonfigurationen hello sen ankomst tolerans fönstret inställning justerats.</span><span class="sxs-lookup"><span data-stu-id="d87b0-130">Number of events arriving late from hello source which have either been dropped or their timestamp has been adjusted, based on hello Event Ordering Policy configuration of hello Late Arrival Tolerance Window setting.</span></span> |
| <span data-ttu-id="d87b0-131">Funktionen begäranden</span><span class="sxs-lookup"><span data-stu-id="d87b0-131">Function Requests</span></span>      | <span data-ttu-id="d87b0-132">Antal anrop toohello Azure Machine Learning-funktionen (om sådan finns).</span><span class="sxs-lookup"><span data-stu-id="d87b0-132">Number of calls toohello Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="d87b0-133">Funktionsfel begäranden</span><span class="sxs-lookup"><span data-stu-id="d87b0-133">Failed Function Requests</span></span> | <span data-ttu-id="d87b0-134">Antal misslyckade Azure Machine Learning-funktionsanrop (om sådan finns).</span><span class="sxs-lookup"><span data-stu-id="d87b0-134">Number of failed Azure Machine Learning function calls (if present).</span></span> |
| <span data-ttu-id="d87b0-135">Funktionshändelser</span><span class="sxs-lookup"><span data-stu-id="d87b0-135">Function Events</span></span>        | <span data-ttu-id="d87b0-136">Antal händelser skickas toohello Azure Machine Learning-funktionen (om sådan finns).</span><span class="sxs-lookup"><span data-stu-id="d87b0-136">Number of events sent toohello Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="d87b0-137">Händelse-byte</span><span class="sxs-lookup"><span data-stu-id="d87b0-137">Input Event Bytes</span></span>      | <span data-ttu-id="d87b0-138">Mängden data som tagits emot av hello Stream Analytics-jobbet, i byte.</span><span class="sxs-lookup"><span data-stu-id="d87b0-138">Amount of data received by hello Stream Analytics job, in bytes.</span></span> <span data-ttu-id="d87b0-139">Detta kan vara används toovalidate att händelser skickas toohello Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="d87b0-139">This can be used toovalidate that events are being sent toohello input source.</span></span> |


## <a name="customizing-monitoring-in-hello-azure-portal"></a><span data-ttu-id="d87b0-140">Anpassa övervakning i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d87b0-140">Customizing Monitoring in hello Azure portal</span></span>
<span data-ttu-id="d87b0-141">Du kan justera hello typ av diagram, mått som visas, och tidsintervallet i inställningarna för hello redigera diagram.</span><span class="sxs-lookup"><span data-stu-id="d87b0-141">You can adjust hello type of chart, metrics shown, and time range in hello Edit Chart settings.</span></span> <span data-ttu-id="d87b0-142">Mer information finns i [hur tooCustomize övervakning](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="d87b0-142">For details, see [How tooCustomize Monitoring](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

  ![Frågan övervaka tid diagram](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="get-help"></a><span data-ttu-id="d87b0-144">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="d87b0-144">Get help</span></span>
<span data-ttu-id="d87b0-145">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="d87b0-145">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d87b0-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d87b0-146">Next steps</span></span>
* [<span data-ttu-id="d87b0-147">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d87b0-147">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="d87b0-148">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d87b0-148">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="d87b0-149">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="d87b0-149">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="d87b0-150">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="d87b0-150">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="d87b0-151">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="d87b0-151">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

