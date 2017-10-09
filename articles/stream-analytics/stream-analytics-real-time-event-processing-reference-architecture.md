---
title: "aaaReal tid för händelsebearbetning med Stream Analytics händelsebearbetning | Microsoft Docs"
description: "Lär dig hur en uppsättning Azure-tjänster kan samverka för att aktivera realtidsskydd händelsebearbetning och analyser."
keywords: "realtidsbearbetning, händelsebearbetning, Referensarkitektur"
services: stream-analytics,event-hubs,storage,sql-database
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: 
ms.assetid: 11af48bc-313c-4527-8c80-91088dc9f3c6
ms.service: stream-analytics
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: jeffstok
ms.openlocfilehash: a43c503d709609ba61e9932822d30bc2208906ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a><span data-ttu-id="5c98c-104">Referera arkitektur: realtid händelsebearbetning med Microsoft Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="5c98c-104">Reference architecture: Real-time event processing with Microsoft Azure Stream Analytics</span></span>
<span data-ttu-id="5c98c-105">hello Referensarkitektur för realtid händelsebearbetning med Azure Stream Analytics är avsedda tooprovide en allmän plan för att distribuera en realtid plattform som en tjänst (PaaS) stream-bearbetning med Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5c98c-105">hello reference architecture for real-time event processing with Azure Stream Analytics is intended tooprovide a generic blueprint for deploying a real-time platform as a service (PaaS) stream-processing solution with Microsoft Azure.</span></span>

## <a name="summary"></a><span data-ttu-id="5c98c-106">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5c98c-106">Summary</span></span>
<span data-ttu-id="5c98c-107">Traditionellt baseras Analyslösningar på funktioner som ETL (extract, transform, load) och datalagring, där data är lagrade tidigare tooanalysis.</span><span class="sxs-lookup"><span data-stu-id="5c98c-107">Traditionally, analytics solutions have been based on capabilities such as ETL (extract, transform, load) and data warehousing, where data is stored prior tooanalysis.</span></span> <span data-ttu-id="5c98c-108">Ändra krav, inklusive mer snabbt inkommande data trycker den befintliga modell toohello gränsen.</span><span class="sxs-lookup"><span data-stu-id="5c98c-108">Changing requirements, including more rapidly arriving data, are pushing this existing model toohello limit.</span></span> <span data-ttu-id="5c98c-109">När den inte är en ny funktion hello-metoden har inte tagits brett över alla branschen lodräta annonser hello möjlighet tooanalyze data i glidande dataströmmar tidigare toostorage är en lösning.</span><span class="sxs-lookup"><span data-stu-id="5c98c-109">hello ability tooanalyze data within moving streams prior toostorage is one solution, and while it is not a new capability, hello approach has not been widely adopted across all industry verticals.</span></span> 

<span data-ttu-id="5c98c-110">Microsoft Azure tillhandahåller en omfattande katalog analytics tekniker som stöder ett flertal olika lösningsscenarier och krav.</span><span class="sxs-lookup"><span data-stu-id="5c98c-110">Microsoft Azure provides an extensive catalog of analytics technologies that are capable of supporting an array of different solution scenarios and requirements.</span></span> <span data-ttu-id="5c98c-111">Att välja vilka Azure-tjänster toodeploy för en slutpunkt till slutpunkt-lösning kan vara en utmaning beroende erbjudanden hello bredd.</span><span class="sxs-lookup"><span data-stu-id="5c98c-111">Selecting which Azure services toodeploy for an end-to-end solution can be a challenge given hello breadth of offerings.</span></span> <span data-ttu-id="5c98c-112">Det här dokumentet är utformad toodescribe hello funktioner och interoperation för hello olika Azure-tjänster som stöder en lösning för direktuppspelning av händelsen.</span><span class="sxs-lookup"><span data-stu-id="5c98c-112">This paper is designed toodescribe hello capabilities and interoperation of hello various Azure services that support an event-streaming solution.</span></span> <span data-ttu-id="5c98c-113">Här förklaras också hello scenarier där kunder kan dra nytta av den här typen av metoden.</span><span class="sxs-lookup"><span data-stu-id="5c98c-113">It also explains some of hello scenarios in which customers can benefit from this type of approach.</span></span>

## <a name="contents"></a><span data-ttu-id="5c98c-114">Innehåll</span><span class="sxs-lookup"><span data-stu-id="5c98c-114">Contents</span></span>
* <span data-ttu-id="5c98c-115">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5c98c-115">Executive Summary</span></span>
* <span data-ttu-id="5c98c-116">Introduktion tooReal tid Analytics</span><span class="sxs-lookup"><span data-stu-id="5c98c-116">Introduction tooReal-Time Analytics</span></span>
* <span data-ttu-id="5c98c-117">Molntjänsters realtidsdata i Azure</span><span class="sxs-lookup"><span data-stu-id="5c98c-117">Value Proposition of Real-Time Data in Azure</span></span>
* <span data-ttu-id="5c98c-118">Vanliga scenarier för Realtidsanalys</span><span class="sxs-lookup"><span data-stu-id="5c98c-118">Common Scenarios for Real-Time Analytics</span></span>
* <span data-ttu-id="5c98c-119">Arkitektur och komponenter</span><span class="sxs-lookup"><span data-stu-id="5c98c-119">Architecture and Components</span></span>
  * <span data-ttu-id="5c98c-120">Datakällor</span><span class="sxs-lookup"><span data-stu-id="5c98c-120">Data Sources</span></span>
  * <span data-ttu-id="5c98c-121">Dataintegrering lager</span><span class="sxs-lookup"><span data-stu-id="5c98c-121">Data-Integration Layer</span></span>
  * <span data-ttu-id="5c98c-122">Realtidsanalys lager</span><span class="sxs-lookup"><span data-stu-id="5c98c-122">Real-time Analytics Layer</span></span>
  * <span data-ttu-id="5c98c-123">Data lagringsskikt</span><span class="sxs-lookup"><span data-stu-id="5c98c-123">Data Storage Layer</span></span>
  * <span data-ttu-id="5c98c-124">Presentation / förbrukning lager</span><span class="sxs-lookup"><span data-stu-id="5c98c-124">Presentation / Consumption Layer</span></span>
* <span data-ttu-id="5c98c-125">Slutsats</span><span class="sxs-lookup"><span data-stu-id="5c98c-125">Conclusion</span></span>

<span data-ttu-id="5c98c-126">**Skapad av:** Charles Feddersen, Lösningsarkitekt, insikter Datacenter av utmärkt, Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="5c98c-126">**Author:** Charles Feddersen, Solution Architect, Data Insights Center of Excellence, Microsoft Corporation</span></span>

<span data-ttu-id="5c98c-127">**Publicerad:** januari 2015</span><span class="sxs-lookup"><span data-stu-id="5c98c-127">**Published:** January 2015</span></span>

<span data-ttu-id="5c98c-128">**Revision:** 1.0</span><span class="sxs-lookup"><span data-stu-id="5c98c-128">**Revision:** 1.0</span></span>

<span data-ttu-id="5c98c-129">**Hämta:** [realtid händelsebearbetning med Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)</span><span class="sxs-lookup"><span data-stu-id="5c98c-129">**Download:** [Real-Time Event Processing with Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)</span></span>

## <a name="get-help"></a><span data-ttu-id="5c98c-130">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="5c98c-130">Get help</span></span>
<span data-ttu-id="5c98c-131">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="5c98c-131">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c98c-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c98c-132">Next steps</span></span>
* [<span data-ttu-id="5c98c-133">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="5c98c-133">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="5c98c-134">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="5c98c-134">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="5c98c-135">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="5c98c-135">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="5c98c-136">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="5c98c-136">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="5c98c-137">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="5c98c-137">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

