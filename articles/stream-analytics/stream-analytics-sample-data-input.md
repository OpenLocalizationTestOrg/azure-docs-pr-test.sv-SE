---
title: Provtagning indata i Azure Stream Analytics | Microsoft Docs
description: "Identifiera problem när du felsöker Stream Analytics-jobb."
keywords: "Felsöka inkommande, inkommande provtagning"
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
ms.openlocfilehash: 0bb66090b5025d57f5ca8f30713aef4e444fa8e7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-stream-analytics-input-stream-sampling"></a><span data-ttu-id="38afe-104">Azure Stream Analytics indata-stream provtagning</span><span class="sxs-lookup"><span data-stu-id="38afe-104">Azure Stream Analytics input-stream sampling</span></span>

<span data-ttu-id="38afe-105">Du kan prova inkommande händelser som kommer från en fil och testa frågor i portalen utan att behöva starta eller stoppa ett jobb med hjälp av Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="38afe-105">By using Azure Stream Analytics, you can sample input events that come from a file and test queries in the portal without needing to start or stop a job.</span></span>

## <a name="testing-your-query"></a><span data-ttu-id="38afe-106">Testa din fråga</span><span class="sxs-lookup"><span data-stu-id="38afe-106">Testing your query</span></span>

<span data-ttu-id="38afe-107">I informationsfönstret för Stream Analytics-jobbet öppna den **frågeredigeraren** bladet genom att klicka på frågans namn under **frågan**.</span><span class="sxs-lookup"><span data-stu-id="38afe-107">In the Stream Analytics job details pane, open the **Query editor** blade by clicking the query name under **Query**.</span></span> <span data-ttu-id="38afe-108">(I vårt Exempelscenario, eftersom ingen fråga har skapats ännu, klickar du på den **< >** platshållaren.)</span><span class="sxs-lookup"><span data-stu-id="38afe-108">(In our example scenario, because no query has been created yet, click the **< >** placeholder.)</span></span>

![Stream Analytics frågeredigeraren](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

<span data-ttu-id="38afe-110">Precis som i den tidigare versionen visas bladet omfattande redigerare för att skapa frågan.</span><span class="sxs-lookup"><span data-stu-id="38afe-110">The rich editor blade for creating your query is displayed as it was in the previous release.</span></span> <span data-ttu-id="38afe-111">Nu har bladet uppdaterats med en ny vänster som visar in- och utgångar som används av frågan och beskrivning för det här jobbet.</span><span class="sxs-lookup"><span data-stu-id="38afe-111">Now the blade has been updated with a new left pane that shows the inputs and outputs that are used by the query and defined for this job.</span></span>

![Stream Analytics query editor inmatningar och matar ut listor](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

<span data-ttu-id="38afe-113">Dessutom visas är ytterligare indata och utdata, som inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="38afe-113">Also shown are an additional input and output, which are not defined.</span></span> <span data-ttu-id="38afe-114">De kommer från den nya fråga mall som du börjar med.</span><span class="sxs-lookup"><span data-stu-id="38afe-114">They come from the new query template that you start with.</span></span> <span data-ttu-id="38afe-115">De ändrar eller försvinna även helt, när du redigerar frågan.</span><span class="sxs-lookup"><span data-stu-id="38afe-115">They change, or even disappear altogether, as you edit the query.</span></span> <span data-ttu-id="38afe-116">Du kan ignorera dem just nu.</span><span class="sxs-lookup"><span data-stu-id="38afe-116">You can safely ignore them for now.</span></span>

<span data-ttu-id="38afe-117">Högerklicka på någon av dina inmatningar för att testa med exempeldata i indata, och välj sedan **ladda upp exempeldata från filen**.</span><span class="sxs-lookup"><span data-stu-id="38afe-117">To test with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

![Stream Analytics query editor överför exempeldata från fil (kommando)](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

<span data-ttu-id="38afe-119">När överföringen är klar klickar du på **testa** att testa den här frågan mot exempeldata som du angav.</span><span class="sxs-lookup"><span data-stu-id="38afe-119">After the upload is complete, click **Test** to test this query against the sample data you have just provided.</span></span>

![Knappen Stream Analytics query editor Test](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

<span data-ttu-id="38afe-121">Om du vill spara testet utdata för senare användning visas utdata från frågan i webbläsaren med en länk till nedladdningsresultaten.</span><span class="sxs-lookup"><span data-stu-id="38afe-121">If you want to save the test output for later use, the output of your query is displayed in the browser with a link to the download results.</span></span> <span data-ttu-id="38afe-122">Du kan nu enkelt och upprepade gånger ändra din fråga och testa det flera gånger för att se hur utdata ändras.</span><span class="sxs-lookup"><span data-stu-id="38afe-122">You can now easily and iteratively modify your query and test it repeatedly to see how the output changes.</span></span>

![Stream Analytics query editor exempel på utdata](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

<span data-ttu-id="38afe-124">En andra utdata har lagts till i föregående bild, kallas **HighAvgTempOutput**.</span><span class="sxs-lookup"><span data-stu-id="38afe-124">In the preceding image, a second output has been added, called **HighAvgTempOutput**.</span></span>

<span data-ttu-id="38afe-125">Du kan se resultaten för varje utdata separat och växla mellan dem när du använder flera utdata i en fråga.</span><span class="sxs-lookup"><span data-stu-id="38afe-125">When you use multiple outputs in a query, you can see the results for each output separately and easily toggle between them.</span></span>

<span data-ttu-id="38afe-126">När du är nöjd med resultaten du spara frågan, starta jobbet, tillbaka sitta och titta på Magiskt tal för Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="38afe-126">After you are satisfied with the results, you can save your query, start your job, sit back and watch the magic of Stream Analytics.</span></span>

## <a name="get-help"></a><span data-ttu-id="38afe-127">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="38afe-127">Get help</span></span>

<span data-ttu-id="38afe-128">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="38afe-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="38afe-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="38afe-129">Next steps</span></span>
* [<span data-ttu-id="38afe-130">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="38afe-130">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="38afe-131">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="38afe-131">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="38afe-132">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="38afe-132">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="38afe-133">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="38afe-133">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="38afe-134">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="38afe-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
