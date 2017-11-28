---
title: "aaaDebug Azure Stream Analytics-frågor med SELECT INTO | Microsoft Docs"
description: "Data efter halva exempelfråga med SELECT INTO-instruktioner i Stream Analytics"
keywords: 
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 27e41af1a6ea06b4509d07a3a67087490d0ec1fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-queries-by-using-select-into-statements"></a><span data-ttu-id="af346-103">Felsöka frågor med hjälp av SELECT INTO-instruktioner</span><span class="sxs-lookup"><span data-stu-id="af346-103">Debug queries by using SELECT INTO statements</span></span>

<span data-ttu-id="af346-104">I realtid databearbetning verkar veta vilka hello-data hello mitten av hello frågan kan vara användbart.</span><span class="sxs-lookup"><span data-stu-id="af346-104">In real-time data processing, knowing what hello data looks like in hello middle of hello query can be helpful.</span></span> <span data-ttu-id="af346-105">Eftersom indata- och stegen för ett Azure Stream Analytics-jobb kan läsa flera gånger, kan du skriva extra SELECT INTO-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="af346-105">Because inputs or steps of an Azure Stream Analytics job can be read multiple times, you can write extra SELECT INTO statements.</span></span> <span data-ttu-id="af346-106">Gör det matar ut mellanliggande data till lagring och du kan inspektera hello är korrekt hello data, precis som *titta på variabler* göra när du felsöker ett program.</span><span class="sxs-lookup"><span data-stu-id="af346-106">Doing so outputs intermediate data into storage and lets you inspect hello correctness of hello data, just as *watch variables* do when you debug a program.</span></span>

## <a name="use-select-into-toocheck-hello-data-stream"></a><span data-ttu-id="af346-107">Använd SELECT INTO toocheck hello dataström</span><span class="sxs-lookup"><span data-stu-id="af346-107">Use SELECT INTO toocheck hello data stream</span></span>

<span data-ttu-id="af346-108">hello har följande exempelfråga i Azure Stream Analytics-jobbet en strömindata, två referens indata och en utgående tooAzure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="af346-108">hello following example query in an Azure Stream Analytics job has one stream input, two reference data inputs, and an output tooAzure Table Storage.</span></span> <span data-ttu-id="af346-109">hello frågan kopplar ihop data från hello händelsehubb och två blobbar tooget hello namn och kategori Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="af346-109">hello query joins data from hello event hub and two reference blobs tooget hello name and category information:</span></span>

![SELECT INTO exempelfråga](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

<span data-ttu-id="af346-111">Observera att hello jobbet körs, men inga händelser genereras i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="af346-111">Note that hello job is running, but no events are being produced in hello output.</span></span> <span data-ttu-id="af346-112">På hello **övervakning** ikonen som visas här, du kan se att hello indata ger data, men du inte vet vilka steg i hello **ansluta** orsakade alla hello händelser toobe bort.</span><span class="sxs-lookup"><span data-stu-id="af346-112">On hello **Monitoring** tile, shown here, you can see that hello input is producing data, but you don’t know which step of hello **JOIN** caused all hello events toobe dropped.</span></span>

![hello övervakning sida vid sida](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
<span data-ttu-id="af346-114">I det här fallet kan du lägga till några extra SELECT INTO-instruktioner för ”loggar” hello mellanliggande kopplingsresultatet och hello data som läses från hello indata.</span><span class="sxs-lookup"><span data-stu-id="af346-114">In this situation, you can add a few extra SELECT INTO statements too“log” hello intermediate JOIN results and hello data that's read from hello input.</span></span>

<span data-ttu-id="af346-115">I det här exemplet har vi lagt till två nya ”tillfällig utdata”.</span><span class="sxs-lookup"><span data-stu-id="af346-115">In this example, we've added two new “temporary outputs.”</span></span> <span data-ttu-id="af346-116">De kan vara alla mottagare som du vill.</span><span class="sxs-lookup"><span data-stu-id="af346-116">They can be any sink you like.</span></span> <span data-ttu-id="af346-117">Här kan vi använda Azure Storage som exempel:</span><span class="sxs-lookup"><span data-stu-id="af346-117">Here we use Azure Storage as an example:</span></span>

![Lägga till extra SELECT INTO-instruktioner](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

<span data-ttu-id="af346-119">Du kan sedan skriva om frågan hello så här:</span><span class="sxs-lookup"><span data-stu-id="af346-119">You can then rewrite hello query like this:</span></span>

![Omskrivet SELECT INTO-frågan](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

<span data-ttu-id="af346-121">Nu startar hello jobbet igen och låt den kör några minuter.</span><span class="sxs-lookup"><span data-stu-id="af346-121">Now start hello job again, and let it run for a few minutes.</span></span> <span data-ttu-id="af346-122">Frågan temp1 och temp2 med Visual Studio Cloud Explorer tooproduce hello följande tabeller:</span><span class="sxs-lookup"><span data-stu-id="af346-122">Then query temp1 and temp2 with Visual Studio Cloud Explorer tooproduce hello following tables:</span></span>

<span data-ttu-id="af346-123">**temp1 tabell**
![SELECT INTO temp1 tabell](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span><span class="sxs-lookup"><span data-stu-id="af346-123">**temp1 table**
![SELECT INTO temp1 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span></span>

<span data-ttu-id="af346-124">**temp2 tabell**
![SELECT INTO temp2 tabell](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span><span class="sxs-lookup"><span data-stu-id="af346-124">**temp2 table**
![SELECT INTO temp2 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span></span>

<span data-ttu-id="af346-125">Som du kan se temp1 och temp2 har data och hello namnkolumnen fylls korrekt i temp2.</span><span class="sxs-lookup"><span data-stu-id="af346-125">As you can see, temp1 and temp2 both have data, and hello name column is populated correctly in temp2.</span></span> <span data-ttu-id="af346-126">Men eftersom det fortfarande finns inga data i utdata, det är något fel:</span><span class="sxs-lookup"><span data-stu-id="af346-126">However, because there is still no data in output, something is wrong:</span></span>

![SELECT INTO output1 tabell utan data](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

<span data-ttu-id="af346-128">Hello datasampling du som kan vara nästan säkert att hello problemet är hello andra koppling.</span><span class="sxs-lookup"><span data-stu-id="af346-128">By sampling hello data, you can be almost certain that hello issue is with hello second JOIN.</span></span> <span data-ttu-id="af346-129">Du kan hämta hello referensdata från hello blob och titta:</span><span class="sxs-lookup"><span data-stu-id="af346-129">You can download hello reference data from hello blob and take a look:</span></span>

![SELECT INTO ref-tabell](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

<span data-ttu-id="af346-131">Som du ser skiljer hello format hello GUID i den här referensdata sig från hello [från]-kolumnen i temp2 hello format.</span><span class="sxs-lookup"><span data-stu-id="af346-131">As you can see, hello format of hello GUID in this reference data is different from hello format of hello [from] column in temp2.</span></span> <span data-ttu-id="af346-132">Det är därför hello data inte kommer fram output1 som förväntat.</span><span class="sxs-lookup"><span data-stu-id="af346-132">That’s why hello data didn’t arrive in output1 as expected.</span></span>

<span data-ttu-id="af346-133">Du kan åtgärda hello dataformat, ladda upp den tooreference blob och försök igen:</span><span class="sxs-lookup"><span data-stu-id="af346-133">You can fix hello data format, upload it tooreference blob, and try again:</span></span>

![SELECT INTO temporära tabellen](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

<span data-ttu-id="af346-135">Den här gången hello data i hello utdata formateras och fyllts som förväntat.</span><span class="sxs-lookup"><span data-stu-id="af346-135">This time, hello data in hello output is formatted and populated as expected.</span></span>

![SELECT INTO slutliga tabell](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a><span data-ttu-id="af346-137">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="af346-137">Get help</span></span>

<span data-ttu-id="af346-138">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="af346-138">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="af346-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af346-139">Next steps</span></span>

* [<span data-ttu-id="af346-140">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af346-140">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="af346-141">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af346-141">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="af346-142">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="af346-142">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="af346-143">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="af346-143">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="af346-144">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="af346-144">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

