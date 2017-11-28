---
title: "aaaAzure Application Insights telemetri datamodellen - måttet telemetri | Microsoft Docs"
description: "Application Insights datamodellen för mått telemetri"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: 005e218a8451007458185f1e457a20cee93fa630
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="metric-telemetry-application-insights-data-model"></a><span data-ttu-id="a0051-103">Mått telemetri: Application Insights-datamodell</span><span class="sxs-lookup"><span data-stu-id="a0051-103">Metric telemetry: Application Insights data model</span></span>

<span data-ttu-id="a0051-104">Det finns två typer av mått telemetri som stöds av [Programinsikter](app-insights-overview.md): enkel mätning och före aggregerade mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-104">There are two types of metric telemetry supported by [Application Insights](app-insights-overview.md): single measurement and pre-aggregated metric.</span></span> <span data-ttu-id="a0051-105">Enskild mätning är bara ett namn och värde.</span><span class="sxs-lookup"><span data-stu-id="a0051-105">Single measurement is just a name and value.</span></span> <span data-ttu-id="a0051-106">Före aggregerade mått anger lägsta och högsta värdet för hello mått i hello aggregeringsintervall och standardavvikelsen för den.</span><span class="sxs-lookup"><span data-stu-id="a0051-106">Pre-aggregated metric specifies minimum and maximum value of hello metric in hello aggregation interval and standard deviation of it.</span></span>

<span data-ttu-id="a0051-107">Före aggregerade mått telemetri förutsätter aggregering perioden har en minut.</span><span class="sxs-lookup"><span data-stu-id="a0051-107">Pre-aggregated metric telemetry assumes that aggregation period was one minute.</span></span>

<span data-ttu-id="a0051-108">Det finns flera välkända mått namn som stöds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a0051-108">There are several well-known metric names supported by Application Insights.</span></span> 

<span data-ttu-id="a0051-109">Måttet som representerar system och processen räknare:</span><span class="sxs-lookup"><span data-stu-id="a0051-109">Metric representing system and process counters:</span></span>

| <span data-ttu-id="a0051-110">**.NET-namn**</span><span class="sxs-lookup"><span data-stu-id="a0051-110">**.NET name**</span></span>             | <span data-ttu-id="a0051-111">**Plattform storleksoberoende namn**</span><span class="sxs-lookup"><span data-stu-id="a0051-111">**Platform agnostic name**</span></span> | <span data-ttu-id="a0051-112">**REST API-namn**</span><span class="sxs-lookup"><span data-stu-id="a0051-112">**REST API name**</span></span> | <span data-ttu-id="a0051-113">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="a0051-113">**Description**</span></span>
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | <span data-ttu-id="a0051-114">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-114">Work in progress...</span></span> | [<span data-ttu-id="a0051-115">processorCpuPercentage</span><span class="sxs-lookup"><span data-stu-id="a0051-115">processorCpuPercentage</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | <span data-ttu-id="a0051-116">totala antalet datorer CPU</span><span class="sxs-lookup"><span data-stu-id="a0051-116">total machine CPU</span></span>
| `\Memory\Available Bytes`                 | <span data-ttu-id="a0051-117">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-117">Work in progress...</span></span> | [<span data-ttu-id="a0051-118">memoryAvailableBytes</span><span class="sxs-lookup"><span data-stu-id="a0051-118">memoryAvailableBytes</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | <span data-ttu-id="a0051-119">minne tillgängligt på disk</span><span class="sxs-lookup"><span data-stu-id="a0051-119">memory available on disk</span></span>
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | <span data-ttu-id="a0051-120">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-120">Work in progress...</span></span> | [<span data-ttu-id="a0051-121">processCpuPercentage</span><span class="sxs-lookup"><span data-stu-id="a0051-121">processCpuPercentage</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | <span data-ttu-id="a0051-122">CPU processens hello värd hello program</span><span class="sxs-lookup"><span data-stu-id="a0051-122">CPU of hello process hosting hello application</span></span>
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | <span data-ttu-id="a0051-123">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-123">Work in progress...</span></span> | [<span data-ttu-id="a0051-124">processPrivateBytes</span><span class="sxs-lookup"><span data-stu-id="a0051-124">processPrivateBytes</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | <span data-ttu-id="a0051-125">minne som används av hello process som är värd för programmet hello</span><span class="sxs-lookup"><span data-stu-id="a0051-125">memory used by hello process hosting hello application</span></span>
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | <span data-ttu-id="a0051-126">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-126">Work in progress...</span></span> | [<span data-ttu-id="a0051-127">processIOBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="a0051-127">processIOBytesPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | <span data-ttu-id="a0051-128">antalet i/o-åtgärder körs med processen som är värd hello program</span><span class="sxs-lookup"><span data-stu-id="a0051-128">rate of I/O operations runs by process hosting hello application</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | <span data-ttu-id="a0051-129">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-129">Work in progress...</span></span> | [<span data-ttu-id="a0051-130">requestsPerSecond</span><span class="sxs-lookup"><span data-stu-id="a0051-130">requestsPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | <span data-ttu-id="a0051-131">antalet begäranden som bearbetas av programmet</span><span class="sxs-lookup"><span data-stu-id="a0051-131">rate of requests processed by application</span></span> 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | <span data-ttu-id="a0051-132">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-132">Work in progress...</span></span> | [<span data-ttu-id="a0051-133">exceptionsPerSecond</span><span class="sxs-lookup"><span data-stu-id="a0051-133">exceptionsPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | <span data-ttu-id="a0051-134">antal undantag som utlöses av program</span><span class="sxs-lookup"><span data-stu-id="a0051-134">rate of exceptions thrown by application</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | <span data-ttu-id="a0051-135">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-135">Work in progress...</span></span> | [<span data-ttu-id="a0051-136">requestExecutionTime</span><span class="sxs-lookup"><span data-stu-id="a0051-136">requestExecutionTime</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | <span data-ttu-id="a0051-137">begäranden om Genomsnittlig körningstid</span><span class="sxs-lookup"><span data-stu-id="a0051-137">average requests execution time</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | <span data-ttu-id="a0051-138">Pågående arbete...</span><span class="sxs-lookup"><span data-stu-id="a0051-138">Work in progress...</span></span> | [<span data-ttu-id="a0051-139">requestsInQueue</span><span class="sxs-lookup"><span data-stu-id="a0051-139">requestsInQueue</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | <span data-ttu-id="a0051-140">antalet begäranden som väntar hello i en kö</span><span class="sxs-lookup"><span data-stu-id="a0051-140">number of requests waiting for hello processing in a queue</span></span>

## <a name="name"></a><span data-ttu-id="a0051-141">Namn</span><span class="sxs-lookup"><span data-stu-id="a0051-141">Name</span></span>

<span data-ttu-id="a0051-142">Namn för hello mått du vill att toosee i Application Insights-portalen och Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a0051-142">Name of hello metric you'd like toosee in Application Insights portal and UI.</span></span> 

## <a name="value"></a><span data-ttu-id="a0051-143">Värde</span><span class="sxs-lookup"><span data-stu-id="a0051-143">Value</span></span>

<span data-ttu-id="a0051-144">Enskilt värde för mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-144">Single value for measurement.</span></span> <span data-ttu-id="a0051-145">Summan av enskilda mått för hello aggregering.</span><span class="sxs-lookup"><span data-stu-id="a0051-145">Sum of individual measurements for hello aggregation.</span></span>

## <a name="count"></a><span data-ttu-id="a0051-146">Antal</span><span class="sxs-lookup"><span data-stu-id="a0051-146">Count</span></span>

<span data-ttu-id="a0051-147">Tjänstmåttets vikt för hello samman mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-147">Metric weight of hello aggregated metric.</span></span> <span data-ttu-id="a0051-148">Ska inte anges för ett mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-148">Should not be set for a measurement.</span></span>

## <a name="min"></a><span data-ttu-id="a0051-149">Min</span><span class="sxs-lookup"><span data-stu-id="a0051-149">Min</span></span>

<span data-ttu-id="a0051-150">Minimivärdet för hello samman mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-150">Minimum value of hello aggregated metric.</span></span> <span data-ttu-id="a0051-151">Ska inte anges för ett mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-151">Should not be set for a measurement.</span></span>

## <a name="max"></a><span data-ttu-id="a0051-152">Max.</span><span class="sxs-lookup"><span data-stu-id="a0051-152">Max</span></span>

<span data-ttu-id="a0051-153">Maximivärdet för hello samman mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-153">Maximum value of hello aggregated metric.</span></span> <span data-ttu-id="a0051-154">Ska inte anges för ett mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-154">Should not be set for a measurement.</span></span>

## <a name="standard-deviation"></a><span data-ttu-id="a0051-155">Standardavvikelse</span><span class="sxs-lookup"><span data-stu-id="a0051-155">Standard deviation</span></span>

<span data-ttu-id="a0051-156">Standardavvikelsen för hello samman mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-156">Standard deviation of hello aggregated metric.</span></span> <span data-ttu-id="a0051-157">Ska inte anges för ett mått.</span><span class="sxs-lookup"><span data-stu-id="a0051-157">Should not be set for a measurement.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="a0051-158">Anpassade egenskaper</span><span class="sxs-lookup"><span data-stu-id="a0051-158">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a><span data-ttu-id="a0051-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0051-159">Next steps</span></span>

- <span data-ttu-id="a0051-160">Lär dig hur toouse [Application Insights API för anpassade händelser och mått](app-insights-api-custom-events-metrics.md#trackmetric).</span><span class="sxs-lookup"><span data-stu-id="a0051-160">Learn how toouse [Application Insights API for custom events and metrics](app-insights-api-custom-events-metrics.md#trackmetric).</span></span>
- <span data-ttu-id="a0051-161">Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="a0051-161">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="a0051-162">Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a0051-162">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
