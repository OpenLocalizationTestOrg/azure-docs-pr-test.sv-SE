---
title: "Felsöka Azure Stream Analytics-frågor med hjälp av SELECT INTO | Microsoft Docs"
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
ms.openlocfilehash: b05222c6d6f4fc2c5b847dd75ff7e29352cd538c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="debug-queries-by-using-select-into-statements"></a><span data-ttu-id="34da4-103">Felsöka frågor med hjälp av SELECT INTO-instruktioner</span><span class="sxs-lookup"><span data-stu-id="34da4-103">Debug queries by using SELECT INTO statements</span></span>

<span data-ttu-id="34da4-104">I realtidsbearbetning av data, kan det vara bra att känna till hur data ser ut mitt i frågan.</span><span class="sxs-lookup"><span data-stu-id="34da4-104">In real-time data processing, knowing what the data looks like in the middle of the query can be helpful.</span></span> <span data-ttu-id="34da4-105">Eftersom indata- och stegen för ett Azure Stream Analytics-jobb kan läsa flera gånger, kan du skriva extra SELECT INTO-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="34da4-105">Because inputs or steps of an Azure Stream Analytics job can be read multiple times, you can write extra SELECT INTO statements.</span></span> <span data-ttu-id="34da4-106">Gör detta matar ut mellanliggande data till lagring och gör att du kan granska är korrekt av data, precis som *titta på variabler* göra när du felsöker ett program.</span><span class="sxs-lookup"><span data-stu-id="34da4-106">Doing so outputs intermediate data into storage and lets you inspect the correctness of the data, just as *watch variables* do when you debug a program.</span></span>

## <a name="use-select-into-to-check-the-data-stream"></a><span data-ttu-id="34da4-107">Använd SELECT INTO för att kontrollera dataströmmen</span><span class="sxs-lookup"><span data-stu-id="34da4-107">Use SELECT INTO to check the data stream</span></span>

<span data-ttu-id="34da4-108">Följande exempelfråga i Azure Stream Analytics-jobbet har en strömindata, två referens indata och utdata till Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="34da4-108">The following example query in an Azure Stream Analytics job has one stream input, two reference data inputs, and an output to Azure Table Storage.</span></span> <span data-ttu-id="34da4-109">Frågan kopplar ihop data från händelsehubb och två referens BLOB för att hämta information om namn och kategori:</span><span class="sxs-lookup"><span data-stu-id="34da4-109">The query joins data from the event hub and two reference blobs to get the name and category information:</span></span>

![SELECT INTO exempelfråga](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

<span data-ttu-id="34da4-111">Observera att jobbet körs, men inga händelser genereras i utdata.</span><span class="sxs-lookup"><span data-stu-id="34da4-111">Note that the job is running, but no events are being produced in the output.</span></span> <span data-ttu-id="34da4-112">På den **övervakning** ikonen som visas här, kan du se att indata är producera data, men du inte vet vilka steg i den **ansluta** orsakade alla händelser tas bort.</span><span class="sxs-lookup"><span data-stu-id="34da4-112">On the **Monitoring** tile, shown here, you can see that the input is producing data, but you don’t know which step of the **JOIN** caused all the events to be dropped.</span></span>

![Panelen övervakning](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
<span data-ttu-id="34da4-114">I så fall kan du lägga till några extra SELECT INTO-instruktioner för att ”logga” mellanliggande kopplingsresultatet och de data som läses från angivna indata.</span><span class="sxs-lookup"><span data-stu-id="34da4-114">In this situation, you can add a few extra SELECT INTO statements to “log” the intermediate JOIN results and the data that's read from the input.</span></span>

<span data-ttu-id="34da4-115">I det här exemplet har vi lagt till två nya ”tillfällig utdata”.</span><span class="sxs-lookup"><span data-stu-id="34da4-115">In this example, we've added two new “temporary outputs.”</span></span> <span data-ttu-id="34da4-116">De kan vara alla mottagare som du vill.</span><span class="sxs-lookup"><span data-stu-id="34da4-116">They can be any sink you like.</span></span> <span data-ttu-id="34da4-117">Här kan vi använda Azure Storage som exempel:</span><span class="sxs-lookup"><span data-stu-id="34da4-117">Here we use Azure Storage as an example:</span></span>

![Lägga till extra SELECT INTO-instruktioner](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

<span data-ttu-id="34da4-119">Du kan sedan skriva om frågan så här:</span><span class="sxs-lookup"><span data-stu-id="34da4-119">You can then rewrite the query like this:</span></span>

![Omskrivet SELECT INTO-frågan](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

<span data-ttu-id="34da4-121">Nu startar jobbet igen och låt den kör några minuter.</span><span class="sxs-lookup"><span data-stu-id="34da4-121">Now start the job again, and let it run for a few minutes.</span></span> <span data-ttu-id="34da4-122">Fråga sedan temp1 och temp2 med Visual Studio Cloud Explorer för att skapa följande tabeller:</span><span class="sxs-lookup"><span data-stu-id="34da4-122">Then query temp1 and temp2 with Visual Studio Cloud Explorer to produce the following tables:</span></span>

<span data-ttu-id="34da4-123">**temp1 tabell**
![SELECT INTO temp1 tabell](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span><span class="sxs-lookup"><span data-stu-id="34da4-123">**temp1 table**
![SELECT INTO temp1 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span></span>

<span data-ttu-id="34da4-124">**temp2 tabell**
![SELECT INTO temp2 tabell](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span><span class="sxs-lookup"><span data-stu-id="34da4-124">**temp2 table**
![SELECT INTO temp2 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span></span>

<span data-ttu-id="34da4-125">Som du kan se temp1 och temp2 har data och namnkolumnen fylls korrekt i temp2.</span><span class="sxs-lookup"><span data-stu-id="34da4-125">As you can see, temp1 and temp2 both have data, and the name column is populated correctly in temp2.</span></span> <span data-ttu-id="34da4-126">Men eftersom det fortfarande finns inga data i utdata, det är något fel:</span><span class="sxs-lookup"><span data-stu-id="34da4-126">However, because there is still no data in output, something is wrong:</span></span>

![SELECT INTO output1 tabell utan data](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

<span data-ttu-id="34da4-128">Genom att ta prov data vara du nästan säkert att problemet beror på den andra KOPPLINGEN.</span><span class="sxs-lookup"><span data-stu-id="34da4-128">By sampling the data, you can be almost certain that the issue is with the second JOIN.</span></span> <span data-ttu-id="34da4-129">Du kan hämta referensdata från blob och titta:</span><span class="sxs-lookup"><span data-stu-id="34da4-129">You can download the reference data from the blob and take a look:</span></span>

![SELECT INTO ref-tabell](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

<span data-ttu-id="34da4-131">Som du ser formatet för GUID i den här referensdata skiljer sig från formatet på den [kolumn i temp2 från].</span><span class="sxs-lookup"><span data-stu-id="34da4-131">As you can see, the format of the GUID in this reference data is different from the format of the [from] column in temp2.</span></span> <span data-ttu-id="34da4-132">Det är därför data inte kommer fram output1 som förväntat.</span><span class="sxs-lookup"><span data-stu-id="34da4-132">That’s why the data didn’t arrive in output1 as expected.</span></span>

<span data-ttu-id="34da4-133">Du kan åtgärda dataformatet, ladda upp den för att referera till blob och försök igen:</span><span class="sxs-lookup"><span data-stu-id="34da4-133">You can fix the data format, upload it to reference blob, and try again:</span></span>

![SELECT INTO temporära tabellen](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

<span data-ttu-id="34da4-135">Den här tiden kan data i utdata formateras och fyllts som förväntat.</span><span class="sxs-lookup"><span data-stu-id="34da4-135">This time, the data in the output is formatted and populated as expected.</span></span>

![SELECT INTO slutliga tabell](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a><span data-ttu-id="34da4-137">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="34da4-137">Get help</span></span>

<span data-ttu-id="34da4-138">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="34da4-138">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="34da4-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34da4-139">Next steps</span></span>

* [<span data-ttu-id="34da4-140">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="34da4-140">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="34da4-141">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="34da4-141">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="34da4-142">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="34da4-142">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="34da4-143">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="34da4-143">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="34da4-144">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="34da4-144">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

